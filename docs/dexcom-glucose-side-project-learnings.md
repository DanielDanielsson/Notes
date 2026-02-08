# Dexcom Glucose API Side Project - Main Learnings

## Project
- Build a backend-first foundation for a future macOS menubar app that shows glucose values.
- Keep it open source without exposing secrets.
- Use official Dexcom API if possible.

## What We Built
- Vercel-hosted TypeScript backend with:
  - OAuth start/callback endpoints
  - Cron sync endpoint
  - API key protected glucose endpoints
  - Postgres schema + migrations
- Deployed project URL:
  - `https://glucose-nu.vercel.app`
- Health endpoint is working:
  - `GET /api/health`

## Key Architecture Decisions
- Backend-first approach (good call):
  - Reusable for macOS app + web app later.
- Secrets stay server-side:
  - Dexcom client secret never in desktop client.
- Use Vercel + Postgres for easy operations.
- Region-aware Dexcom endpoints are required:
  - US: `api.dexcom.com`
  - EU/outside US: `api.dexcom.eu`

## Main Learnings
1. Dexcom app credentials are region-scoped in practice.
   - Our `client_id` worked on `.com` but failed on `.eu` with `Client not found`.
2. User account region also matters.
   - Sweden account + US-provisioned app causes `Account not supported`.
3. EU support is not just an endpoint switch.
   - You likely need Dexcom support/upgrade to provision app credentials for EU production access.
4. Vercel API-only projects can fail build if output settings are wrong.
   - Fixed by setting `"outputDirectory": "."` in `vercel.json`.
5. Production env mistakes can look like runtime hangs.
   - Missing/invalid env vars caused routes to fail/hang while health still passed.
6. CLI env values can accidentally include trailing newline.
   - This broke admin token validation until corrected.

## Important Fixes We Made
- Vercel deploy config fix:
  - `vercel.json` -> `"outputDirectory": "."`
- Dexcom OAuth endpoints updated to v3:
  - `/v3/oauth2/login`
  - `/v3/oauth2/token`
- Runtime DB stability improvements:
  - Switched DB client to `pg`
  - Added `connectionTimeoutMillis`
- Production env corrections:
  - Fixed Dexcom base URL values and admin token formatting.

## Current Blocker
- Official OAuth for EU account still blocked by Dexcom app provisioning mismatch:
  - Error examples:
    - `Client not found` on `api.dexcom.eu`
    - `Account not supported` for Sweden account

## Next Steps
1. Submit Dexcom support ticket requesting EU provisioning for this app/client.
2. Keep backend configured for EU while waiting:
   - `DEXCOM_BASE_URL=https://api.dexcom.eu`
   - `DEXCOM_OAUTH_BASE_URL=https://api.dexcom.eu`
3. After Dexcom confirms EU-enabled credentials:
   - Update `DEXCOM_CLIENT_ID` / `DEXCOM_CLIENT_SECRET`
   - Re-run OAuth flow
   - Run first sync + create client API key
4. Then start menubar app implementation against this backend.

## Side Project Summary
- This project is viable and technically solid.
- The major remaining risk is vendor-side app/account region provisioning, not backend engineering.
- Once EU credentials are enabled, the rest of the stack is ready to continue quickly.
