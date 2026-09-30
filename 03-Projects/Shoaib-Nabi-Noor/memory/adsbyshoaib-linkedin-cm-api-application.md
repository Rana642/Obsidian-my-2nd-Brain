---
name: adsbyshoaib-linkedin-cm-api-application
description: LinkedIn Community Management API access request (submitted ~2026-09-19) plus the data-use restrictions any LinkedIn code must obey
metadata: 
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-18T19:33:39.088Z
---

Organic Company Page posting via LinkedIn's API needs the **Community Management API** (Development Tier), whose access form requires a registered active business (LinkedIn cross-checks legal name + address against public registries; no upload field, no explicit NTN box). Shoaib had no registration, so it was blocked until he registered as a **sole proprietor and got an NTN (~2026-09-19)**. "Share on LinkedIn" was ruled out: it only grants `w_member_social` (personal profile), not Company Pages. Third-party relay (Make.com free tier / Ayrshare $149+/mo) was researched as a fallback but Shoaib chose the direct route.

**Submitted ~2026-09-19** on the app "Ads by Shoaib" (Client ID `77dikbiti3z1fg`, LinkedIn Page verified 2026-09-13, privacy URL set): primary use case **Agency**, other use cases **Page management + Page analytics** (NOT Profile management), business email `info@adsbyshoaib.com`, website adsbyshoaib.com. Response pending; no case/reference number was captured. Do not resubmit; wait for LinkedIn's email/decision. Later, if the planner becomes a public SaaS, LinkedIn must be told and a "Platform" tier requested — agency-only is the truthful use today.

**Restrictions to obey when writing LinkedIn code** (from LinkedIn's "Restricted Uses of LinkedIn Marketing APIs and Data" page): member social-activity data (posts, comments, reactions) may be stored **max 48 hours**, profile data 24 hours; member data can't be exported/forwarded to customers; can't be combined with other data; only for managing Pages, never CRM/sales/ads/recruiting; no "social feed" widget on a website. So any future LinkedIn Insights feature must not persist engagement data past 48h and must not feed client reports/exports.

Existing code (`lib/social-linkedin.ts`, `connectLinkedInAccount` paste-a-token flow, `saveLinkedInOrgMappings`) is built but was never run against a real LinkedIn account — expect API-shape surprises on first real post.

**Status 2026-09-29:** Community Management API (Development Tier) shows "Review in progress — 1 of 2. Access Form Review". LinkedIn: may email the business email (info@adsbyshoaib.com) within 10–14 business days for extra documentation → expect contact/decision roughly 2026-10-03 to 10-09. All other products are greyed out ("Request access" disabled) — expected, CM API must be the only product on its app. Step 2 (after form review) is presumably the business/legal verification.
