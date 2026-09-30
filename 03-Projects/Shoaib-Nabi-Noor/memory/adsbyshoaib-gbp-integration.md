---
name: adsbyshoaib-gbp-integration
description: "Google Business Profile integration built 2026-09-30 — /dashboard/gbp, gbp_connections table, gbp_* MCP tools, Socially Snap OAuth client via API Vault \"gmb\""
metadata:
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-29T19:21:11.470Z
---

Built 2026-09-30 (phase 1 = Connect + Reviews), after the API approval ([[adsbyshoaib-gbp-api-application]]). Shoaib chose the **Socially Snap** consent screen.

- `lib/gbp.ts`: OAuth (scopes openid, email, business.manage; offline + prompt=consent; state = project id), discoverLocations (Account Mgmt v1 + Business Information v1), v4 reviews list/reply/delete-reply. OAuth client = API Vault service **`gmb`** (client_id, client_secret) from the Socially Snap GCP project.
- Routes: `/api/dashboard/gbp/authorize?project_id=` → Google; callback **`/api/gbp/callback`** (must be an Authorized redirect URI on the OAuth client, prod + localhost:3000). Admin-only.
- Table `gbp_connections` (one per client project, refresh_token_enc via SOCIAL_TOKENS_ENCRYPTION_KEY) — migration already run.
- Page `/dashboard/gbp` (sidebar Social → Google Business): connect/reconnect/disconnect, location picker, rating summary, reviews with All / Needs reply filter, post/update/delete reply.
- MCP (`lib/mcp-gbp-tools.ts`, registered inside registerMarketingTools so local + remote both get it): gbp_list_connections, gbp_list_reviews, gbp_reply_review (confirm gate), gbp_request passthrough limited to GBP API hosts (confirm for non-GET).

**Why:** Shoaib wants GBP managed like Meta Ads, from the dashboard and via ChatGPT/Claude.
**How to apply:** next phases planned — posts (maybe tied to the Planner), performance metrics, profile info editing. Before first use Shoaib must create the Web OAuth client in the Socially Snap project and put it in the API Vault as `gmb`; a consent screen left in "Testing" makes refresh tokens expire after 7 days.

**Setup 2026-09-30:** gmb credential saved in API Vault (client id from project 875327223530, validated). Consent screen branded Socially Snap (logo = 120px S mark, home/privacy/terms on sociallysnap.adsbyshoaib.com, authorized domain adsbyshoaib.com), test user shoaib.nabi.noor@gmail.com, then **published to In production**. Google verification (logo + business.manage sensitive scope) NOT yet submitted — needs a demo video; until then users click Advanced → Go to Socially Snap. Shoaib's own Gmail manages every client's Business Profile, so he connects each project with that one account.
**Branding verification 2026-09-30:** first attempt failed (sociallysnap.adsbyshoaib.com 'not registered to you'). Shoaib then verified **adsbyshoaib.com as a Domain property** in Search Console (DNS TXT, shoaib.nabi.noor@gmail.com). Next: wait 24h, then Branding → Verify branding → 'I have fixed the issues'. Also told him to enable Business Information API + Google My Business API (v4) etc. in socially-snap — the first connect failed on Business Information API being disabled.
**LIVE 2026-09-30:** first real connect worked on production — Hotel Elegant Executive Suites Multan project connected via shoaib.nabi.noor@gmail.com, 20 locations visible, reviews load (4.6 avg, 624 reviews, 31 of latest 50 unreplied). Branding verification still pending (retry after 24h).
**f7d8a3b (2026-09-30):** one Google grant reused across projects — 'Use <email>' button + 'Link every project with a matching location' (matchLocation by project-name words), fresh connects auto-pick the matching location too.
**ad2c508 (2026-09-30): Planner + Connections.** Platform key `google_business` (lib/social-platforms.ts, hub tile links to /dashboard/gbp; hub rows come from gbp_connections, not client_social_accounts). Planner counts it connected when the project has a chosen location. Cron (app/api/social/cron) posts a v4 localPost (STANDARD, photo via mintFacebookMediaUrl proxy) via publishGbpPhotoPost → paceGbpWrite; on a pace refusal it saves result.live and keeps the post 'scheduled' so only Google retries next run. gbpCaption() drops lines with phone numbers (Google rejects them) and caps 1500 chars. Not yet exercised with a real post — watch the first one. social_post_now (MCP) does NOT include GBP.
