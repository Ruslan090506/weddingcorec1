# Start here — complete connected application

Upload ALL files and folders in this directory to your repository root. package.json, vercel.json, api/, lib/ and public/ must be directly at the root. Do not mix with the four-file browser-only version.

Vercel: Framework Other; Build npm run build; Output public; Node 22 or newer. This app needs server API functions and cannot run by opening index.html directly.

## Production environment settings (set in Vercel, never GitHub)
DATA_MODE=supabase
SUPABASE_URL=https://admikgrrzysmocdzzeho.supabase.co
SUPABASE_SECRET_KEY=your backend secret/service-role key for that project
SESSION_SECRET=a securely generated random value of at least 32 characters
CRON_SECRET=another securely generated random secret
MOCK_INTEGRATIONS=false
APP_URL=https://YOUR-ACTUAL-SITE.vercel.app
STUDENT_NAME=your name
GITHUB_REPO_URL=https://github.com/YOUR-ACCOUNT/YOUR-REPOSITORY
TELEGRAM_BOT_USERNAME=your real bot username
TELEGRAM_BOT_TOKEN=your real bot token
TELEGRAM_WEBHOOK_SECRET=a securely generated secret
GOOGLE_SHEET_ID=your fictional ledger spreadsheet ID
GOOGLE_SERVICE_ACCOUNT_EMAIL=your service-account email
GOOGLE_PRIVATE_KEY=your service-account private key

No credentials are included. If APP_URL is absent, Vercel's trusted production URL is used; no old wedding project hostname is hardcoded. An explicitly configured APP_URL must match the exact site being opened. Origin and custom-header checks remain enabled. After changing environment settings, redeploy.

## Existing persistent records
Use the existing Supabase project shown above. Its shared ledger has the 12 restored S01-S05/E01-E07 records and the Test 2 decisions. S05 and E07 remain pending. Do not run demo:seed, reset the database, or replace it with another project. These records are labeled assignment_restore, not Telegram submissions. They are not evidence of successful live delivery.

Expected company result EUR3930, Project A EUR2050, Project B EUR2180. Existing queued delivery statuses are honest: real external delivery is not claimed. Once configured, press Retry sync & notifications as Svetlana to copy the persistent ledger to the real Sheet.

## Bot and ledger
Follow docs/SETUP.md to register your real bot webhook at YOUR-ACTUAL-SITE/api/telegram, enable the Sheets API and share the spreadsheet with the service account as Editor. Give the reviewer Viewer access. Set the public link variables above so all three project links appear. These real resources must be created/configured by the owner; source files cannot substitute for accounts and secret credentials.

The website includes Manager setup instructions and linking controls. Reviewer sends /start to the bot, enters their own numeric ID as Svetlana, and links to an available salesperson. Switch the same ID to Kevin for expenses. Occupied target bindings and stale bindings are rejected. Old bot records retain their original recipient. No private manual linking or production credentials are needed by the reviewer.

## Live verification still required
Open the deployed URL, select a role and open the workspace. Confirm the restored records appear across fresh browsers. Submit a NEW uniquely referenced item through the real bot, correct/approve it as Svetlana, verify the actual Telegram response and exactly one updated Sheet row. This adds real test data; it should change totals accordingly, not silently reset them. Original required references must not be duplicated.

Employee workspaces filter records server-side and the database has no public read/write grants. However, the original coursework role selector is a demonstration, not real individual authentication: anyone permitted to select Svetlana can see manager data. Use only fictional records.

Local automated tests and build checks are provided. This package is not a deployed or live-integration-verified result. No remote accounts were changed while preparing it.
