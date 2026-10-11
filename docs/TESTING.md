# Assignment test procedure

Start with an empty live Supabase ledger. Do not run the local seed against a live assessment. Amounts below are EUR; split order is always Richard / Anastasia / Jean-Claude. Exact descriptions are included in `fixtures/assignment.mjs`.

## Test 1 — live bot and first decisions

Send `/start` to the configured bot. On the website select Svetlana and link your numeric user ID to Richard. Send:

```text
/sale S01 | Olivia Rose | A | One proud uncle and an emotional grandmother | 1000 | 50/30/20
```

Check that the bot confirms only after the sale is saved. Relink the same user to Kevin and send:

```text
/expense E01 | Rented suit and fake pearl necklace for the relatives | 120 | Materials | A
```

Use the website to enter the other records:

| Ref | Employee | Customer / description | Type | Amount | Project / proposed allocation | Proposed split |
|---|---|---|---|---:|---|---|
| S02 | Anastasia | Daniel King — University friends, dancing, and the stripping performance | Sale | 2000.00 | B | 0 / 50 / 50 |
| E02 | Kevin | Taxi for the grandmother; Kevin selected the wrong project | Travel | 80.00 | B | — |
| E03 | Kevin | Monthly company website subscription | Other | 100.00 | overhead | — |

Before approval, income and commissions are zero, both project results are zero and the company result is **−300.00**. E01/E02 await allocation; E03 is already overhead. Pending sales total 3000.00. In Sheets, the sales' approved split cells are blank and earned commissions are zero.

As Svetlana, approve S01 at 50/30/20; approve S02 at **20/40/40**; allocate E01 to A and move E02 from B to **A**. Confirm the S01 and E01 decision messages arrive in the original bot chat despite relinking. Original proposals must remain unchanged.

| Metric | Project A | Project B | Company |
|---|---:|---:|---:|
| Approved income | 1000.00 | 2000.00 | 3000.00 |
| Commission | 100.00 | 200.00 | 300.00 |
| Allocated project expenses | 200.00 | 0.00 | 200.00 |
| Result | **700.00** | **1800.00** | **2400.00** |

Company overhead is 100.00. Commissions earned: Richard **90.00**, Anastasia **110.00**, Jean-Claude **100.00**. Check that existing Sheets rows were updated, not duplicated.

## Test 2 — keep all Test 1 records

Add these through the appropriate website roles:

| Ref | Employee | Customer / description | Type | Amount | Project / proposed allocation | Proposed split |
|---|---|---|---|---:|---|---|
| S03 | Jean-Claude | Emma Stonebridge — Premium relatives, including an uncle presented as a surgeon | Sale | 1500.00 | A | 40 / 40 / 20 |
| S04 | Richard | Lucas Green — Small group of loud university friends | Sale | 800.00 | B | 25 / 25 / 50 |
| S05 | Richard | Mia Brooks — Extra guests and an embarrassing speech | Sale | 600.00 | B | 100 / 0 / 0 |
| E04 | Kevin | Replacement costumes after an enthusiastic dance performance | Materials | 250.00 | B | — |
| E05 | Kevin | Minibus for university friends; Kevin selected the wrong project again | Travel | 90.00 | A | — |
| E06 | Kevin | Company telephone subscription | Other | 60.00 | overhead | — |
| E07 | Kevin | Emergency replacement clothing; project allocation still needs checking | Materials | 140.00 | A | — |

Link your Telegram account to Jean-Claude **before approving S03**, then approve S03 at **20/30/50**. Verify its decision notification includes the change and final amounts. Approve S04 at 25/25/50. Link your Telegram account to Kevin, allocate E04 to B and move E05 from A to **B**. Verify the allocation change notification. Leave S05 and E07 pending.

| Metric | Project A | Project B | Company |
|---|---:|---:|---:|
| Approved income | 2500.00 | 2800.00 | 5300.00 |
| Commission | 250.00 | 280.00 | 530.00 |
| Allocated project expenses | 200.00 | 340.00 | 540.00 |
| Result | **2050.00** | **2180.00** | **3930.00** |

Overhead **160.00**; awaiting expense allocation **140.00**; all expenses **840.00**; pending sales **600.00**. Earned commissions: Richard **140.00**, Anastasia **175.00**, Jean-Claude **215.00**. Reconciliation: 2050 + 2180 − 160 − 140 = 3930.

## Invalid actions and resilience

1. Try a split totaling 110%, a missing amount and a zero amount. The server must reject each without saving a transaction.
2. As Richard, attempt a manager approval; as Kevin, attempt a sale. The UI omits these actions; the automated tests also call the processing layer directly and expect rejection.
3. Submit an existing reference again, including different letter case. It must be rejected. Repeat a saved approval: totals and job counts must remain unchanged.
4. Temporarily remove the service account's spreadsheet edit access. Submit a new uniquely referenced item. It must remain in Supabase, with Sheets failure shown on the website. Restore access and press manager retry. Verify one row, updated status, and unchanged financial totals during retry.
5. Block the bot from a previously linked test account and approve a new item. The decision must remain saved while notification status becomes failed. Unblock/start the bot and retry. Confirm delivery and unchanged totals.
6. Test an employee without a Telegram link: save/approval succeeds and clearly reports no recipient. No success delivery claim should appear.
7. Add instructor-chosen records with new references and different amounts/splits. The same formulas and workflow must work; the dashboard is calculated from saved data, not hardcoded fixture totals.

After each decision, inspect Supabase, refresh the website and inspect both Sheets tabs. Capture your live bot messages and final totals as course evidence. The repository's unit tests use synthetic Telegram updates and mocked providers; these are not substitutes for this live procedure.
