---
name: sociallysnap-site
description: Socially Snap website — separate Next.js repo at D:\Rana Shoaib\My Projects Website\sociallysnap for sociallysnap.adsbyshoaib.com; built so the verified Socially Snap GBP has a website for GBP API access; palette taken from the real logo
metadata: 
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-14T10:08:40.251Z
---

**Why it exists (2026-09-13):** Shoaib's "Socially Snap" Google Business Profile has been verified for 60+ days but had no website (sociallysnap.com expired and someone else bought it; the Ads by Shoaib GBP is stuck in video verification). Google's GBP API application needs a website representing the business, so a landing page was built for the subdomain `sociallysnap.adsbyshoaib.com`. The future full SaaS dashboard is planned at `app.sociallysnap.adsbyshoaib.com` (brand subdomain for marketing, `app.` for the product).

**Location:** `D:\Rana Shoaib\My Projects Website\sociallysnap`. It's a separate repo from adsbyshoaib.com, per Shoaib ("Alag Dashboard Socially snap k liye banao ga mai"). Next 16.3.5 + Tailwind v4, all pages static. **Pushed to GitHub: [github.com/Rana642/sociallysnap](https://github.com/Rana642/sociallysnap)** (Shoaib created the repo himself — no `gh` CLI on this machine — and gave the URL; `git remote add origin` + push done, `main` tracks `origin/main`). Not yet on Vercel — that's the next step, then the `sociallysnap.adsbyshoaib.com` domain + DNS CNAME. Local preview uses the `sociallysnap-dev` config (port 3001) in adsbyshoaib's `.claude/launch.json`, which runs `npm --prefix ../sociallysnap`.

**Legal pages added (2026-09-13, commit `ab696f4`), researched per-platform rather than boilerplate:** `/privacy` (expanded), `/terms`, `/disclaimer`, `/data-deletion` — all built on a shared `components/legal.tsx` (header/TOC/section/cross-links). Requested by Shoaib specifically so the site is "eligible for every kind of API" (Meta, Google, LinkedIn). Key facts pulled from each platform's real developer policy (verify again if any of this is used elsewhere, policies change):
- **Meta Platform Terms:** privacy policy must explain what's processed and how users request deletion; must delete Platform Data when a user requests it or account closes; prohibits selling Platform Data, surveillance, eligibility-determination uses (credit/housing/employment), and building user profiles without consent. Also requires either a data-deletion **callback** (JSON: URL + confirmation code) or a **Data Deletion Instructions URL** — Socially Snap uses the instructions-URL route, hence the dedicated `/data-deletion` page (this is the URL to paste into Meta's App Dashboard "Data Deletion" field when submitting for App Review).
- **Google API Services User Data Policy:** the exact Limited Use disclosure sentence is already in `/privacy#google` (verbatim, required by reviewers). Prohibits use for ads/retargeting, selling to data brokers, credit-worthiness determination, and training generalized AI/ML models — all reflected as explicit "not used to..." statements.
- **Google Business Profile API policy:** third-party content caching capped at 30 days — reflected in `/privacy#google`.
- **LinkedIn API Terms:** privacy policy must be "at least as stringent as LinkedIn's" and disclose deletion mechanics; profile data must not be refreshed on an automated schedule (only when the member is actively using the app) and must be deleted immediately on request/account closure; prohibits ads use and credit/employment/housing eligibility uses.
- Governing-law clause in `/terms` reuses adsbyshoaib's existing agreement-template wording ("laws of Pakistan... courts of Multan, Punjab") for consistency.
- TikTok, Pinterest and X policies were also researched (site/domain verification rules, no-caching rules, automation-consent rules) but deliberately **not** hard-coded into the pages since those platforms aren't on the product roadmap — the privacy/terms pages instead use generic "connected platforms" language that already covers any platform added later without needing a rewrite each time.

**Brand:**
- **Name:** written "Socially Snap" (two words), as on the logo and the GBP. Code and the domain use "sociallysnap".
- **Logo:** black S mark with yellow triangles, heavy grotesque wordmark, and the tagline "A Digital Hub". Shoaib pasted the logo as PNGs in chat. They were extracted from the session transcript's base64 and cut with sharp (borrowed from adsbyshoaib's node_modules via NODE_PATH) into `public/brand/{mark,wordmark,logo-horizontal,logo-stacked}.png`, plus icon, apple-icon and OG image. If he provides the original vector files, replace these.
- **Palette (sampled from the logo):** yellow `#fac152` (the `snap` token) and black `#0d080a` (treated as ink `#0f0f14`). Yellow is used for fills only (buttons with ink text, highlights, badges), never as text on Cloud.
- **Type:** headings in Geist extrabold. No serif — the logo is a grotesque.

**Contact shown on the site:** hello@adsbyshoaib.com, **+92 311 6796858** (Socially Snap's own number — Shoaib's explicit call, 2026-09-14: "alag hi rakh lyna chaiye number", deliberately separate from Ads by Shoaib's +92 301 7461642), Multan. Matches the real GBP listing exactly (NAP consistency) — `lib/site.ts` is the single source, so a future number change only needs editing there.

**Live and deployed (2026-09-14):** `sociallysnap.adsbyshoaib.com` is up and serving 200s on every page (/, /privacy, /terms, /disclaimer, /data-deletion). GitHub: [github.com/Rana642/sociallysnap](https://github.com/Rana642/sociallysnap), Vercel project "sociallysnap" under the "Taking My Soul Home" Hobby team.

**Vercel gotcha hit during first deploy — if this recurs, don't waste time re-debugging, just recreate the project:** the first Vercel project (created via the dashboard's "Connect Git Repository" checklist item rather than the standard "Add New → Project → Import" flow) built successfully every time (`next build` produced a fully correct route manifest, "Deployment completed" logged, zero errors) but its production domain AND even the deployment's own canonical per-deployment URL returned a raw Vercel-edge `404 NOT_FOUND` for every path — confirmed this wasn't Deployment Protection (Hobby plan only protects preview/generated URLs, never the assigned production domain, per Vercel's own docs) and wasn't caching (fresh `X-Vercel-Id` each request). Three redeploys of the same project didn't fix it. **Fix: deleted the project entirely and re-imported the same repo via the standard "Add New → Project → Import Git Repository" flow** — the fresh project's very first deployment worked immediately, correctly public, no protection wall. Whatever broke was in that specific project's internal alias/routing state, not the code.

**GBP optimization pass (2026-09-14):** sent Shoaib a ready-to-paste business description + services list (grounded in the real feature set, matching the site's early-access/coming-next distinctions) and the site's cover image (`app/opengraph-image.png`) + transparent logo (`public/brand/logo-stacked.png`) as GBP photo uploads (photos on the profile were 859 days stale). Flagged for his decision, not yet resolved:
- **GBP category** is "Advertising agency" — suggested reviewing to "Software company" or "Internet marketing service" instead, since Socially Snap is the tool, not the agency service; he hasn't decided yet.
- **Facebook/Instagram profile links** — GBP's "Profiles" section only has LinkedIn linked; asked whether Socially Snap has its own FB/IG pages yet (unanswered as of this writing — don't assume either way).

**Indexing:** `SITE_IS_LIVE=false` in `lib/site.ts`, so every page is noindex until Shoaib approves the copy. Same rule as [[adsbyshoaib-no-indexing-until-final]].

**Honesty rules:**
- Features marked "In early access" are the ones actually running in the adsbyshoaib internal tool ([[adsbyshoaib-social-poster]]). GBP and LinkedIn are marked "Coming next".
- The privacy page carries Google's Limited Use sentence verbatim. Its claims must stay matched to real behaviour ([[adsbyshoaib-privacy-policy-api-compliance]]).

**Pending:**
1. ~~GitHub repo and push.~~ ✅ done.
2. ~~Vercel project, then the `sociallysnap.adsbyshoaib.com` domain and its DNS CNAME.~~ ✅ done and confirmed live 2026-09-14.
3. ~~Shoaib sets the GBP website field to the URL.~~ ✅ done — GBP now shows a "Website" button.
4. **Google Business Profile API application SUBMITTED 2026-09-14 — Case ID `5-2799000041097`, review time quoted as 7-10 business days.** New, dedicated Google Cloud project created for this — **Project ID `socially-snap`, Project Number `875327223530`**, signed in as `shoaib.nabi.noor@gmail.com` (kept separate from any Ads by Shoaib GCP project, since this is meant to grow into the SaaS's own OAuth app).

   **Correct order (learned the hard way — I initially told Shoaib to enable the APIs first, which is backwards):** create GCP project → **submit the access-request form FIRST** (APIs stay gated/restricted in the Library until approved — enabling errors out) → only after approval, enable the APIs in Basic Setup → then OAuth consent screen + client ID. The "Google My Business API" named in Google's own basic-setup doc no longer exists in the real Library — it was split into 8 real APIs (My Business Business Information/Account Management/Lodging/Notifications/Place Actions/Q&A/Verifications + **Business Profile Performance API**), all still gated pending this approval.

   **"Organization account"** (a GBP prereq for agencies managing *other* businesses' locations, at business.google.com/agencysignup) was deliberately skipped for this application — Shoaib's own account already directly owns the Socially Snap GBP, and joining an Organization requires the account NOT already own/manage locations directly, so it would conflict. Only revisit this if Google's review explicitly asks for it.

   **Check approval status:** Cloud Console → APIs & Services → the Business Profile APIs' quota page — 0 QPM = still pending, 300 QPM = approved. Once approved: enable the 8 APIs, then OAuth consent screen (privacy policy URL `https://sociallysnap.adsbyshoaib.com/privacy`) + OAuth client ID.

   **Still open, unrelated to this application:** a warning banner on Shoaib's support.google.com session said "one or more of your profiles needs to be reverified" — very likely the already-known Ads by Shoaib GBP video-verification issue, not Socially Snap (which is confirmed verified 60+ days), but Shoaib hadn't confirmed which profile it is as of this writing — check before assuming. Whether Socially Snap has its own FB/IG pages to link is also still open.

   **GBP category decision RESOLVED — do NOT change it, don't touch this again:** Shoaib tried changing the category from "Advertising agency" to "Software company" / "Internet marketing service" (2026-09-14) and Google auto-rejected it citing policy **"Business identity changed"** — the same anti-fraud sensitivity documented in [[adsbyshoaib-socially-snap-temporary]] (Google flags identity-field changes — name/category/website in close succession — as a possible hijack). This profile had already just had its phone number and website changed, so a category change on top of that tripped the flag. **We deliberately did NOT click "Appeal"** — appealing risks drawing manual review scrutiny onto this exact profile while the GBP API application (Case `5-2799000041097`) is pending review on it. Category stays "Advertising agency" (which is what it was when the API application was submitted, so no mismatch there). **Do not attempt any further identity-field edits (name, category) on this GBP until the API application is decided** — phone/website changes already went through fine and shouldn't be touched again either, just leave the profile alone for now.
5. Meta App Review: paste `https://sociallysnap.adsbyshoaib.com/data-deletion` into the app's Data Deletion Instructions URL field when submitting.

**Conflicts with [[adsbyshoaib-socially-snap-temporary]]:** that plan was to convert this GBP into Ads by Shoaib. It has now been abandoned — see that note.
