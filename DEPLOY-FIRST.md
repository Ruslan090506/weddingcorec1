# New repository: upload this whole folder's CONTENTS

This is the complete connected application, not a patch. Upload every file and subfolder at the repository root. package.json, vercel.json, api/, lib/, and public/ must be at the same level. Do not mix in the old four-file standalone app.

## Vercel build
Framework: Other
Build command: npm run build
Output directory: public
Node: 22 or newer

## Required before opening a workspace
A new Vercel project does NOT inherit settings from your old project or from .env.example. Add these to Production Environment Variables:

- SESSION_SECRET: a securely generated private value of at least 32 characters. The separately supplied VERCEL-ONLY.env contains a generated value; copy only its value into Vercel. DO NOT upload that file to GitHub.
- SUPABASE_URL: https://admikgrrzysmocdzzeho.supabase.co
- SUPABASE_SECRET_KEY: the backend secret/service-role key for that existing project, never a public key.
- DATA_MODE: supabase
- MOCK_INTEGRATIONS: false
- CRON_SECRET: a separate securely generated random private value.
- APP_URL: your final HTTPS website origin, without a path. If omitted, the code uses Vercel's trusted production URL when provided. Never retain the URL of an old project here.

Keep existing Supabase records. Do not seed, reset or recreate the database. Save the settings and redeploy. A missing SESSION_SECRET cannot be repaired safely by a public GitHub file: it is deliberately required server configuration.

## Complete the real integrations
Follow START-HERE.md and docs/SETUP.md for Telegram bot token, webhook secret, actual bot username, Google service-account credentials, actual Sheet ID, Viewer sharing, and GITHUB_REPO_URL for THIS new repository. No fabricated links, tokens or mock messages are included. The code is ready for configuration; real live delivery is not claimed verified.

All secrets remain in Vercel. Reviewers only use website controls, the public bot, and Viewer ledger. The demonstration role selector is not personal authentication. Use fictional data only.
