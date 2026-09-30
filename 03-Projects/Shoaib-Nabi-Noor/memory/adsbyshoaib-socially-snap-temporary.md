---
name: adsbyshoaib-socially-snap-temporary
description: "TEMPORARY 'formerly Socially Snap' mentions on adsbyshoaib.com (footer, privacy, JSON-LD) — the GBP-rename plan behind them was abandoned 2026-09-13 (Socially Snap stays its own brand), so the wording is now inaccurate; awaiting Shoaib's call to reword or remove"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-13T17:03:18.944Z
---

**Original plan (2026-08-24):** Shoaib was going to convert an old, already-verified Google Business Profile ("Socially Snap") into "Ads by Shoaib", instead of starting a fresh profile. The point was to skip Google's ~60-day new-profile wait for Business Profile API access.

Google's anti-fraud system can suspend a profile whose name or site changes with no prior public link between the old and new identity. So the plan (from a pasted AI Mode conversation) was:
1. Establish that link on the website FIRST.
2. Then change the GBP fields in a careful order — website URL first, wait, then the name, wait — to avoid suspension.

**He explicitly confirmed (2026-08-24):** this was purely a tactical, temporary measure. In his words: "seo ya indexing ya kisi bhi cheez se koi lena dena nahi" (nothing to do with SEO, indexing, or anything else). It was meant to be reversed once the GBP details had actually changed.

**Commit `69e3c0f` added "formerly Socially Snap" in 3 places:**
1. `lib/schema.ts` — `alternateName: "Socially Snap"` on `organizationSchema()`
2. `components/layout/Footer.tsx` — the copyright line "(formerly Socially Snap)"
3. `app/(marketing)/privacy/page.tsx` — the intro sentence "formerly operated under the name Socially Snap"

**Plan changed (2026-09-13):**
- The rename never happened. Socially Snap now stays its own brand — "a product of Ads by Shoaib" — and its verified GBP is being used for the GBP API application as-is.
- A website was built for it at sociallysnap.adsbyshoaib.com: see [[sociallysnap-site]].
- So "formerly Socially Snap" on adsbyshoaib.com no longer describes reality: Ads by Shoaib wasn't formerly Socially Snap, it is Socially Snap's parent.
- This was raised with Shoaib on 2026-09-13.

**Action:** don't touch the 3 spots until he decides. His options:
- reword them to the parent/product relationship (e.g. "Socially Snap is a product of Ads by Shoaib" — arguably helpful for Google, since it ties the two identities together), or
- revert them to their pre-`69e3c0f` wording.
