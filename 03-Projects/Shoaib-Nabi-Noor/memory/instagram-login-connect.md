---
name: instagram-login-connect
description: "Instagram accounts not linked to a FB Page connect via Instagram Login (3cb145f, 2026-10-01) — tester role, API Vault \"instagram_login\", token_expires_at marks login accounts, graph.instagram.com"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-28T11:49:26.102Z
---

On 2026-10-01 Shoaib said some clients' Instagram accounts can't be linked to their Facebook Page ("nai judty hain"). He wants them on the Planner and wants Instagram audits. Built in 3cb145f.

**Setup**
- Runs on the "Ads by Shoaib" app's Instagram product. Instagram app ID 999378643116905 (not the FB app id 970252279417151).
- The app id and secret go in the dashboard API Vault, service **`instagram_login`**, fields `app_id` and `app_secret`. Shoaib enters the secret himself.
- Redirect URI to register under Business login settings: `https://adsbyshoaib.com/api/social/instagram-oauth-callback`.
- Scopes: `instagram_business_basic`, `instagram_business_content_publish`, `instagram_business_manage_insights`. `force_reauth=true` so a second account can be added.

**Access level**
- The app is unpublished (Standard Access). Each account must be added under App roles → **Instagram Testers** and accept the invite (instagram.com → Settings → Apps and websites → Tester invites).
- The account must be Business or Creator.
- Clients connecting accounts themselves would need Advanced Access, which means App Review plus business verification. Confirmed in Meta's docs on 2026-10-01.

**How it's stored and used**
- Login accounts are platform `instagram` rows in `client_social_accounts`. A non-null `token_expires_at` is the marker; Page-linked IG rows have null.
- `external_id` is `/me` `user_id`, the same IG id the Page import uses.
- Code lives in `lib/social-instagram-login.ts`: `isInstagramLoginAccount`, `getFreshInstagramLoginToken` (refreshes when <20 days are left and the token is >1 day old), `refreshExpiringInstagramLogins` (called from the social cron).
- Posting, insights and the GBP backfill pass `IG_LOGIN_GRAPH_BASE` (`graph.instagram.com/v26.0`).
- Duplicates are blocked both ways (login vs Page import).

**UI and status**
- Instagram tile on `/dashboard/social`: "Log in with Instagram". The result comes back as `?instagram=` or `?instagram_error=` and shows as a notice.
- Not yet tried with a real account as of the commit. Shoaib has to do the Meta-side setup first.

**First accounts (2026-10-01).** Both landed in Avenza Avenue because that project was still selected after the first login. Moved by REST: @avenzaland → Avenza Land, @danishconstructionco → Danish Constructions. Since 85a5882 the button reads "Log in with Instagram for <project>".

**Second Facebook login (512212a, 2026-10-01).** `social_connections` row 2 holds a second Facebook profile — Shoaib's wife's, which has access to some client Pages; she is added as a Tester on the Ads by Shoaib app.
- Needs SQL that widens `social_connections_id_check` to `id in (1,2)`.
- **Any profile that connects needs a role on the posting app (970252279417151).** The app is unpublished, so add the profile as a Tester and have it accept the invite. Without the role, the OAuth dialog shows "Feature unavailable — Facebook Login is currently unavailable for this app as we are updating additional details". Shoaib hit this on 2026-10-28 when he connected a new profile.
- Connect: `/api/dashboard/social/facebook/authorize?slot=2`. The slot travels in OAuth `state` (`slot2`).
- Import Pages merges both logins' Pages, deduped, with slot 1 winning.
- The same profile in both slots is refused (`fb_user_id` compare).
- The login cards show the profile name, fetched live via `/me`.
- Facebook callback errors redirect to `?fb_error=`.

Related: [[gbp-reply-queue]] (the social cron runs on Supabase pg_cron).
