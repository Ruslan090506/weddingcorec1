# Verification status

Checked locally on 4 October 2026.

| Check | Result |
|---|---|
| Node syntax/build check | Passed |
| Automated suite | 21 tests passed, 0 failed |
| Test 1 pending/approved calculations | Passed |
| Test 2 cumulative totals and pending records | Passed |
| Rounding, invalid splits/amounts, role denials | Passed |
| Unique references, repeat approval and Telegram update deduplication | Passed |
| Immutable original bot recipient after relinking | Passed |
| Missing recipient, delivery failure/retry, concurrency and sync revision handling | Passed with test adapters |
| Signed session tamper rejection and same-origin write check | Passed |
| Supabase adapter request shape and key handling | Passed with mocked HTTP |
| Sheets stable RAW rows, pending fields and two-decimal formatting requests | Passed with mocked HTTP |
| Website manager dashboard | Opened in browser; expected 3930.00 / 2050.00 / 2180.00 results verified visually |
| Live Supabase SQL migration and persistence | Not run: requires your project |
| Actual Telegram webhook and message delivery | Not run: requires your bot |
| Actual Google Sheets synchronization | Not run: requires your spreadsheet and service account |
| Vercel deployment and scheduled retry | Not run: requires your account |

Run `npm test` and `npm run build` again after extracting. Complete the live procedure in TESTING.md before presenting the assignment as an end-to-end deployed system. No API credentials, live user data or local seeded database are included in the ZIP.
