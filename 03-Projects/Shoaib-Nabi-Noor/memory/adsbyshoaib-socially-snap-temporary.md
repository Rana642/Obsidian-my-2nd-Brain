---
name: adsbyshoaib-socially-snap-temporary
description: "TEMPORARY: 'formerly Socially Snap' mentions on the site (footer, privacy policy, Organization JSON-LD) must be removed once Shoaib confirms his GBP rename is done — not a permanent brand fact"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-24T10:21:23.787Z
---

Shoaib is converting an old, already-verified Google Business Profile ("Socially Snap") into "Ads by Shoaib" instead of starting fresh, to skip Google's ~60-day new-profile wait for Business Profile API access. Google's anti-fraud system can suspend a profile whose name/site change with no prior public connection between old and new identity, so the plan (from a pasted AI Mode conversation) was: establish that connection on the website FIRST, then change the GBP name/website fields in a careful order (website URL first, wait, then name, wait) to avoid suspension.

**He explicitly confirmed (2026-08-24): this is purely a tactical, temporary measure — "seo ya indexing ya kisi bhi cheez se koi lena dena nahi" (nothing to do with SEO/indexing/anything else), and it should be reversed once the GBP details are actually changed.**

**Commit `69e3c0f` added "formerly Socially Snap" in 3 places — all need to come back out once he confirms the GBP rename is complete:**
1. `lib/schema.ts` — `alternateName: "Socially Snap"` on `organizationSchema()`
2. `components/layout/Footer.tsx` — copyright line "(formerly Socially Snap)"
3. `app/(marketing)/privacy/page.tsx` — intro sentence "formerly operated under the name Socially Snap"

**Action for a future session: don't remove this proactively — wait for Shoaib to say the GBP conversion is done, then revert exactly these 3 spots to their pre-`69e3c0f` wording.** He hasn't given a timeline; check in if it comes up, but don't ask unprompted either.
