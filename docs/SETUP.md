# Setup and deployment

## 1. Supabase

Create a Supabase project. Run the entire `supabase/schema.sql` file in SQL Editor. It creates a private ledger, an atomic compare-and-swap function, and a transaction inspection view. Re-running it preserves data.

Copy the project URL and a backend secret key into `SUPABASE_URL` and `SUPABASE_SECRET_KEY`. The adapter supports `sb_secret_…` keys and legacy service-role JWT keys. Do not use the public/publishable key. Keep these values in server environment variables only. There are no public read/write policies on the ledger.

To inspect saved data in SQL Editor:

```sql
select reference, type, submitter, status, amount_cents, details
from public.finance_transactions order by reference;
```

## 2. Google Sheets

Create a Google Cloud project, enable the Google Sheets API and create a service account with a JSON key. Create a dedicated spreadsheet with tabs named exactly **Sales** and **Expenses**. Share it with the service-account email as **Editor**. Give instructors **Viewer** access, or use an appropriate view-only sharing link.

Set `GOOGLE_SERVICE_ACCOUNT_EMAIL` to the JSON `client_email`, and `GOOGLE_PRIVATE_KEY` to its `private_key`. The server accepts real PEM newlines or literal `\n` sequences. Set `GOOGLE_SHEET_ID` to the ID between `/d/` and `/edit` in the spreadsheet URL. Do not upload the JSON key to GitHub.

The application supplies headers, freezes the first row and formats monetary columns to two decimals. This spreadsheet is a read-only reporting copy for humans: do not sort, insert, delete or edit its source rows manually. Website and bot changes automatically overwrite the same stable rows. Supabase remains the source of truth.

## 3. Telegram

Create a bot with Telegram's **@BotFather** using `/newbot`. Set `TELEGRAM_BOT_TOKEN`, and set `TELEGRAM_BOT_USERNAME` to its username without `@`. Generate a random `TELEGRAM_WEBHOOK_SECRET` using only letters, digits, `_` and `-`.

The bot supports private chats only. Each participant opens the bot and sends `/start`. The reply shows their numeric Telegram user ID. The user appears in the manager's website setup area after refresh. Svetlana links that ID to an employee; bot users cannot assign themselves a role.

## 4. Vercel

Upload the repository to GitHub and import it into Vercel. Use **Other** as the framework, `npm run build` as the build command, and `public` as the output directory (already declared in `vercel.json`). Use Node.js 22 or newer. The `api` folder supplies Node functions.

Add all settings from `.env.example` in Vercel's server environment. Use:

- `DATA_MODE=supabase` and `MOCK_INTEGRATIONS=false`.
- A random `SESSION_SECRET` of at least 32 characters.
- A random `CRON_SECRET` for the retry endpoint.
- Your final canonical HTTPS site URL as `APP_URL`, without a trailing slash.
- Your name as `STUDENT_NAME` and your GitHub repository URL as `GITHUB_REPO_URL`.
- The provider credentials from steps 1–3.
- Optionally, `DEMO_ACCESS_CODE` to limit casual access to the role selector. Share it with your instructor separately.

Deploy. If you only learn the final domain after the initial deployment, update `APP_URL` and redeploy. Use that exact domain when testing; mutations from a different preview domain are rejected by the origin check. Hosting protection must allow Telegram to reach `/api/telegram` and the scheduled retry to reach `/api/cron`.

Copy the live values to a local, uncommitted `.env`, then run:

```sh
npm run telegram:setup
```

This registers the HTTPS webhook and bot commands. Re-run it after changing domains or webhook secrets. The API checks Telegram's secret header on every update.

## 5. First live check

Open the site as Svetlana. Start the bot from your own Telegram account and refresh the manager page. Link your numeric ID to Richard. Record S01 through the bot, then relink to Kevin and record E01 through the bot using the exact commands in TESTING.md. Verify confirmations, saved Supabase records, Sheets rows and decision messages. Relinking does not change the original submitter or chat of earlier bot transactions.

The website attempts delivery after every saved change. A daily Vercel cron also retries queued work, and the manager can use **Retry deliveries** whenever needed. Each pass handles a small batch; repeat until the queue is clear. If hosting does not support scheduled calls on your plan, the automatic post-save attempt and manager retry still operate.

For failure testing, use your own test integrations, not another person's resources. Keep a note of the original setting before temporarily changing it. Never erase the live ledger between assignment Test 1 and Test 2.

## Official implementation references

- [Vercel Node.js functions](https://vercel.com/docs/functions/runtimes/node-js)
- [Supabase database functions](https://supabase.com/docs/guides/database/functions) and [API keys](https://supabase.com/docs/guides/api/api-keys)
- [Telegram webhook API](https://core.telegram.org/bots/api#setwebhook)
- [Google service-account authorization](https://developers.google.com/identity/protocols/oauth2/service-account)
- [Sheets batch values updates](https://developers.google.com/workspace/sheets/api/reference/rest/v4/spreadsheets.values/batchUpdate)
