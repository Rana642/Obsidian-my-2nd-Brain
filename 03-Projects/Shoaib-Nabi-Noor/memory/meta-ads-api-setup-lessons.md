---
name: meta-ads-api-setup-lessons
description: "Building/editing Meta ads via meta_ads_* MCP (ABS system user) — per-account + per-Page assignment, Lead Ads ToS, CBO same-goal rule, attribution locked, 30 s edit limit; Choppers Studio Pink October build 2026-10-30"
metadata:
  node_type: memory
  type: reference
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-30T12:47:34.606Z
---

**The token.** The `meta_ads_*` MCP tools use the **ABS system user** (id 122099356329477125), whose token sits in the API Vault. Its scopes include `ads_management`. Having the scope is not enough to edit.

**What ABS needs before it can edit:**
- **The ad account assigned** with "Manage ad account". Without it, the account is read-only and edits fail with "Can't manage ads: missing permission" (#200).
- **The Facebook Page assigned**, or creating the ad set fails with "Insufficient Page Permission to Run Ads".
- Shoaib does both in Business Settings (Socially Snap, or the Page's own portfolio). Reads work without either assignment.

**Leads-goal ads for WhatsApp:**
- `optimization_goal: LEAD_GENERATION`, `destination_type: WHATSAPP`, attribution 1-day click.
- The Page must accept the Lead Ads Terms at facebook.com/ads/leadgen/tos?page_id=<id>. **Pick the Page in the dropdown, not the personal profile.**

**Meta rules that block edits:**
- The attribution window can't change after an ad set is created, so a CONVERSATIONS ad set can't be switched to LEAD_GENERATION. Create a new ad set.
- In a CBO (lowest-cost) campaign, every ad set must share the same optimization goal. Different goal = new campaign.
- Ad set edits are limited to about 1 call per 30 s (#613). Space the calls out.
- Ads Manager drafts that the user has open can overwrite API changes when they press Publish. Tell them to discard the draft first.

**The Choppers Studio "Pink October" build (2026-10-30)**, in ad account Salon | Choppers (act_267230435722441):
- New campaign 120253162568800704, Leads, CBO PKR 800/day, left PAUSED for Shoaib to switch on.
- Ad set 120253162570130704:
  - a pin at 30.245293, 71.494111 with a 12 mi radius (from his April Choppers Studio ad);
  - Women, 22–50, Advantage+ audience off;
  - FB + IG + WhatsApp Status;
  - runs 30 Oct 18:30 to 3 Nov 23:59.
- Ad 120253162571840704 uses creative 2906195653086103:
  - primary text and headline;
  - WhatsApp greeting with the flow title "Pink October Package Booking" and CTA "Book my package";
  - overlays, text optimizations and enhance-CTA opted out.
- The WhatsApp lead-form questions (WABA flow 2299347806918374) can't be edited through our tools. Ads Manager only.
- **Image left as is** (Shoaib: "image ko choro"). It is 561×800, WhatsApp-compressed, with a Pinterest "Save" button baked in.
- The old campaign 120253162248050704 (Conversations) stays paused. Its ad set got the new targeting too.
- `meta_ads_get` now redacts the token in paging URLs (7d6a4d4).

Related: [[choppers-knowledge-base]], [[meta-insights-organic-vs-ads]].
