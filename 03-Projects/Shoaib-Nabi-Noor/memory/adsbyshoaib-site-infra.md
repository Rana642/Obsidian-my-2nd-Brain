---
name: adsbyshoaib-site-infra
description: "adsbyshoaib.com infrastructure facts that aren't obvious from the code — DNS lives in Vercel (not Hostinger), Sanity build gotchas, env-gated integrations; replaces the old build-status note"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-27T07:53:14.165Z
---

The site has been live since August 2026 with real content, the Sanity CMS ([[adsbyshoaib-cms-architecture]]) and the Supabase dashboard ([[adsbyshoaib-dashboard]]). The phase-by-phase build log from August was retired on 2026-10-27. Current work is in `docs/WORK-LOG.md`.

**Facts that are easy to get wrong:**
- **DNS for adsbyshoaib.com is managed in Vercel**, whose nameservers are ns1/ns2.vercel-dns.com. Hostinger still shows a zone for the domain but it is inactive, so never edit DNS there. Resend's DKIM/SPF records (`rsend`, `send`) are in Vercel DNS.
- **Sanity build gotchas (Aug 2026):**
  - `/studio` is client-only (`next/dynamic`, ssr:false), because NextStudio in the RSC graph trips a Turbopack/swr export-condition conflict.
  - `transpilePackages: ["@sanity/sdk-react"]` in `next.config.ts` is needed, because that package ships untranspiled JSX. Don't remove it without re-running `npm run build`.
- **Analytics** (GA4, GTM, Meta Pixel, Calendly, Meta CAPI) are all gated on env vars and render nothing when a var is missing.
- **Indexing is still blocked** (`SITE_IS_LIVE = false`). See [[adsbyshoaib-no-indexing-until-final]].
- **Dev server:** `.claude/launch.json` → "adsbyshoaib-dev" (port 3000). Browser-test uncommitted work there, see [[adsbyshoaib-verify-via-local-dev-server]].
- The headshot is `public/images/shoaib.png`.

Repo: github.com/Rana642/Shoaib-Portfolio-Website (app at the repo root). The repo is also worked on from a second PC, see [[shoaib-two-pc-workflow]].

**Supabase API outage, 2026-10-29 ~00:50–12:12 PKT.** Every REST call failed with `PGRST003 "Timed out acquiring connection from connection pool"`, so the dashboard and posting both went down. Auth and Storage still answered. The database itself was fine: SQL Editor worked, only 14 of 60 connections were used, and nothing was stuck in `pg_stat_activity`. That meant PostgREST itself was wedged. **Fix: Supabase → Project Settings → General → Restart project** (about 1 minute). Cause unknown (free plan). Afterwards, overdue posts run in `scheduled_at` order, so move any bulk backlog (e.g. LinkedIn copies) behind the clients' overdue posts. Built `/api/health` (0d8460b). It returns 503 when the DB API takes over 8 s or when unheld posts are more than 2 h late. Shoaib is to point a free UptimeRobot monitor at it (his own account) with email and app alerts.
