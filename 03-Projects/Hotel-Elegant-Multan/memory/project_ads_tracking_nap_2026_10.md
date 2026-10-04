---
name: hotel-elegant-ads-tracking-nap-2026-10
description: "2026-10-04 — NAP locked to GBP, KB built, ads/website audits, tracking fixes shipped (GA4 + Google Ads + Meta), Meta Purchase at submit"
metadata:
  node_type: memory
  type: project
---

Work done on 2026-10-04 from the adsbyshoaib session. The full detail is in the knowledge base (adsbyshoaib MCP `kb_*`, project "Hotel Elegant Executive Suites Multan") and in `docs/TRACKING.md` in the website repo.

- **NAP is locked to the Google Business Profile.**
  - Name: Hotel Elegant Executive Suites Multan
  - Address: Hotel Elegant Executive Suites, 77A, A Block Gulgasht Colony, Multan, 60750
  - Phone: 0317 3330998
  - Every listing (FB, IG, OTAs, website) must match this. GBP itself is never changed. Mismatches are listed in KB `nap-consistency-audit`.
- **Goal:** real guests and bookings from Google Ads and Meta Ads.
- **KB audit docs:**
  - `ads-audit-2026-10`
  - `website-audit-2026-10`
  - `competitor-and-global-research`
  - `ads-playbook` (draft)
  - `fix-checklist` (with a progress log)
- **Tracking fixes are live**, in commits df979ca, e491dea, ba14736, e532ebd, ef3e97e and cc1f294:
  - Google Ads conversions now fire from every WhatsApp/Call button. The cause of the 0 conversions was TrackedLink.
  - Tracking init is inline in `<head>`, so mount-time events are no longer dropped.
  - /admin and staff browsers are excluded (`he_internal`).
  - The duplicate noscript PageView is removed.
  - WhatsApp messages carry "(Ref: GA/FB/GS/WEB)" codes.
  - Review numbers come from `lib/reviewStats.ts`. Booking.com links and "Five Star" are removed.
  - Phone is shown in the GBP format.
- **Meta Purchase fires at booking submit** (thank-you Pixel + CAPI, event_id booking-purchase-<ref>). A confirm-time Purchase was tried and reverted, because there are too few bookings to ever leave learning. Never move it.
- **Blocked:** conversion-goal cleanup.
  - Google: Booking Started / Booking Lead should be secondary.
  - Meta: our token has no Advertise permission on act_239008850511120.
- **Pending on Shoaib or the client:**
  - Booking.com rate parity.
  - FB/IG/OTA NAP fixes.
  - GA4 internal filter.
  - GSC access.
  - Stale claims in the live ads ("no advance payment" on the 20% offer, 432 reviews, 8.3 Booking.com).

**How to apply:** check `docs/TRACKING.md` and the KB `fix-checklist` before touching tracking or ads. Local dev uses the production DB, so never submit test bookings.
