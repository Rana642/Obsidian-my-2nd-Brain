---
name: wacrm-hostinger-deployment
description: "How/where the wacrm WhatsApp CRM is deployed for adsbyshoaib, plus the Supabase key gotcha"
metadata: 
  node_type: memory
  type: project
  originSessionId: e75bd4f0-f22e-4066-9685-dfd468b3e2e2
---

Deploying the **wacrm** WhatsApp CRM template to Hostinger for the user (adsbyshoaib).

- **Fork:** github.com/Rana642/ABS-wacrm (forked from ArnasDon/wacrm). Hostinger Git-deploys this fork; env vars live in hPanel, not the repo.
- **Domain:** https://crm.adsbyshoaib.com (subdomain, so the apex marketing site stays separate).
- **Supabase project ref:** `stklhjcxmzihvvdupmwc` (https://stklhjcxmzihvvdupmwc.supabase.co). Schema applied by pasting `supabase/deploy_all_migrations.sql` (all 25 migrations, concatenated locally) into the SQL editor.
- **GOTCHA — Supabase keys:** the project's *new* `sb_publishable_`/`sb_secret_` keys did not work — the `sb_secret_` key returned 401 against both PostgREST and auth-admin. Switched to the **legacy anon + service_role JWT keys** (Settings → API → "Legacy anon, service_role API keys" tab), which authenticate fine. ⚠️ Do NOT click "Disable JWT-based API keys" — that breaks the app.
- **Secrets:** ENCRYPTION_KEY, AUTOMATION_CRON_SECRET, WHATSAPP_VERIFY_TOKEN were generated locally; all secrets live only in hPanel env + local gitignored `.env.local`.
- **Status (2026-06-27):** deployed; user reports Meta/WhatsApp integration done and fully functional. WhatsApp was Phase 2 (app deployed first).
- **SECURITY TODO:** the legacy `service_role` JWT (and the old `sb_secret_`) were pasted into the chat transcript — recommend rotating via Supabase "Reset JWT secret," then updating both Supabase env vars in hPanel + `.env.local` and redeploying. See [[wacrm-security-followups]].
