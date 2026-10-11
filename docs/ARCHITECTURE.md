# Implementation notes

## Data and permissions

The browser sends commands to Node functions. The server derives the acting employee from an HMAC-signed HttpOnly cookie, validates the role, applies the command and saves it in Supabase. Supplying a different actor in a request body cannot grant permission. The role selector deliberately issues a session for any of the five classroom roles; it is not real authentication. The optional classroom access code gates that selector.

Salespeople submit sales; Kevin submits expenses; Svetlana approves, allocates, links Telegram users and retries delivery. Employees see their own submissions; the manager sees the ledger and dashboard. All financial validation also runs for bot commands.

The private singleton JSONB ledger is a deliberate small-homework design. Every transaction, decision, Telegram update ID and notification job is saved in one database update. A version check prevents lost updates; a conflict retries the command against the newest state. The read-only SQL view makes individual transactions inspectable. Larger workloads would require normalized tables, dedicated queues and authenticated users.

## Accounting

Amounts are integer euro cents. Split percentages are integer hundredths of a percent. The commission pool is the sale amount × 10%, rounded half up to cents. Each share is independently rounded half up; any remaining cent difference goes to the largest percentage, with ties resolved Richard, Anastasia, Jean-Claude. Integer arithmetic avoids floating-point rounding drift.

Pending sales contribute no income and no commission. Approval preserves the proposed split and records the final split, approver, time and earnings. Company expenses count when submitted. Project expenses count after allocation; overhead counts immediately without a manager decision.

Project result = approved project income − approved project commissions − allocated project expenses.

Company result = all approved income − all approved commissions − all recorded expenses.

Equivalently: project A result + project B result − overhead − expenses awaiting allocation. Pending sales are displayed separately.

## Idempotency and notifications

References are unique across all transactions, case-insensitively. Repeat approval of an already decided item is a no-op. Telegram update IDs are recorded atomically with their command (most recent 5,000 retained); reference uniqueness also prevents a repeated sale/expense from becoming a second record after an old update ID ages out.

Bot transactions keep their original numeric chat ID and submitting employee. Relinking that Telegram account later does not reroute their decisions. Website confirmations use the current link at submission; website decisions use the current employee link at decision time, with the original recipient as fallback. With no recipient, the record visibly says so. Linking later does not automatically resend an already skipped message.

Confirmation and decision jobs have stable IDs. Financial writes and jobs commit before dispatch. A leased worker prevents overlapping normal dispatches; provider timeouts bound requests. Failed jobs remain retryable; attempts are ordered fairly so one failed recipient cannot permanently block new jobs. A notification is marked sent only after Telegram reports success.

Telegram delivery is **at least once**: an exceptional crash after Telegram accepts a message but before its acknowledgement is saved can cause a repeated message on retry. Telegram provides no sendMessage idempotency key. This cannot repeat the financial decision.

## Sheets synchronization

Transactions remain in insertion order. Filtering that ledger by type provides a stable reference-to-row mapping in each tab. Every financial change writes the current snapshot with RAW input, overwriting existing rows rather than appending duplicates. Proposed and approved fields are separate; pending sales have blank approved percentages and zero commissions. RAW prevents entered descriptions from being interpreted as spreadsheet formulas.

Only a successfully written revision is marked synchronized. If new data arrives during synchronization, the newer revision stays pending. A failure preserves all financial state and records a visible error. The next saved action, manager retry or scheduled call retries it. Manual changes to the reporting tabs are unsupported and may be overwritten.

## Boundaries

The demo contains no tax, payroll, refunds, transaction editing/deletion, or real identity authentication. Local mode persists to `.local/ledger.json` and is disabled on Vercel. Local mock integrations intentionally report disconnected instead of pretending to send messages. Secrets never appear in public configuration or the browser bundle.
