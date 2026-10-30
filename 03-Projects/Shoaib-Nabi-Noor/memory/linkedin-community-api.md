---
name: linkedin-community-api
description: LinkedIn posting for client Company Pages waits on the Community Management API review (identity vetting since ~2026-10-24); why other products are greyed out; what Shoaib should keep ready
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-28T10:14:23.107Z
---

On 2026-10-24 Shoaib shared his LinkedIn Developers screen. The app's **Community Management API** request had passed the application form and was in **"Identity Vetting"**, where LinkedIn checks the business is real. LinkedIn says it may ask for documents by email within 10–14 business days.

- Until it is approved, Connections shows "LinkedIn login: Awaiting approval", and dashboard posting to Company Pages can't work.
- **Other products are greyed out on purpose.** LinkedIn doesn't allow any other product (Share on LinkedIn, Advertising API…) in an app that has Community Management. If the Ads API is ever needed, it has to be a separate app.
- **What Shoaib should do:**
  - watch his business email, including spam, and reply fast;
  - keep ready: business registration or NTN, proof of address (bill or bank statement), and adsbyshoaib.com showing the same name and address as LinkedIn;
  - make sure the "Ads by Shoaib" Company Page is verified in the app settings;
  - **not** re-request or create a new app, because that can restart the review.
- When approval or a document request arrives, he will send a screenshot. Then wire the posting.

**2026-10-28: vetting passed.** The "Ads by Shoaib" app (client 77dikbiti3z1fg) now lists Community Management API as **Development Tier** under Added products. Shoaib submitted the upgrade request. LinkedIn's toast says to fill the upgrade form from the "Access request form" link on the Products tab **within 21 days, so by about 2026-11-18**.
- The current code is not ready for a Standard-tier demo:
  - the Company Page login is a pasted member token (no OAuth route);
  - `lib/social-linkedin.ts` posts through the old v2 `/ugcPosts` and was never tested.
- The plan I proposed: an OAuth connect + callback, Import Company Pages, posting through `/rest/posts` + `/rest/images`. Test on Dev Tier with his own Page, then he records the screencast for the form.
- **Built and live, 1db24d4 (2026-10-28).**
  - Vault service `linkedin_company`.
  - Callback `https://adsbyshoaib.com/api/social/linkedin-callback`, which must be added in the app's Auth tab.
  - Scopes `rw_organization_admin w_organization_social r_organization_social`.
  - `LinkedIn-Version` 202610. Versions live about a year, and 202511 sunsets 2026-11-16.
  - Captions must be escaped for the "little" text format. An unescaped `(`, `@`, `_` or `*` can cut or garble the post.
  - Untested against a real token until he connects.
- **Standard-tier form answers drafted** (in chat, 2026-10-28):
  - Agency use case; Page management + Page analytics;
  - Asia; 11–100 clients; about 25% B2B; small and medium businesses;
  - partnerships: Facebook and Instagram (Meta Tech Provider);
  - privacy answers Yes, Yes, No, No.
- **Privacy policy is live** (4e2adcb, approved): LinkedIn in the connected-accounts list and the processors table, plus a "LinkedIn API disclosure" section.
- **Reviewer test login:** a portal **Team member** login on client "Shoaib Nabi Noor" (projects "Ads by Shoaib" + "Personal Profiles"), limited to the planner and uploads, and to the Ads by Shoaib project only.
  - Shoaib creates it himself in Dashboard → Clients → Shoaib Nabi Noor → Client portal, and sets the password himself.
  - Portal uploads land as pending_caption, so nothing a reviewer uploads posts until it is captioned.
  - The Ads by Shoaib project also has FB, TikTok and YouTube, so a captioned upload goes to all of them.
- **Still needed:**
  - the legal name: the privacy page says "Shoaib Nabi Noor's independent practice, formerly operated under the name Socially Snap", so it is likely Shoaib Nabi Noor, with Socially Snap as the alternate (he must confirm);
  - the registered address.
- Never give the reviewer the admin login: it holds every client, the vault and invoices.

**Connected and live, 2026-10-28.**
- 11 Company Pages imported onto projects: Ads by Shoaib (145027023), Tad, Meezab, Avalon, Eventia, Elegant, Silver Sand, Danish, Avenza Land, Avenza Avenue, Choppers Salon.
- The test post went out at 15:00 PKT (urn:li:share:7521148785645789184).
- Shoaib asked for 68 backfill posts that same day, LinkedIn-only rows reusing the image + caption:
  - Tad 24, Meezab 6, Silver Sand 14, Avalon 15, Elegant 9, at 20–75 minute gaps;
  - left out: the Tad "dealer" design and all Jummah posts;
  - the 14 IG-only scheduled posts got "linkedin" added to their targets.
- **Development Tier daily limits are small and per resource.** `/rest/organizations` hit "Resource level throttle APPLICATION_AND_MEMBER DAY limit" (HTTP 429) after a few Import Pages opens, so the dialog showed URNs.
  - Fixes: one BATCH_GET for names (4502ba3); 429s are held as kind `rate_limited`, retried hourly with no banner (f2d1e46); image checks cut to 2.
  - Limits reset at 00:00 UTC (05:00 PKT). The per-endpoint limits are shown in the LinkedIn app's Analytics tab.
- MCP tool `linkedin_get_page` (0ef6625) gives a read-only Page report. A new MCP tool needs the ABS connector toggled off/on (Shoaib does it; the classifier blocked me).

Related: [[instagram-login-connect]], [[connections-import-pages]].
