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

- **2026-10-05 rate parity (live, commit 0637d79):** website is tax-exclusive like Booking.com. Standard = Booking.com standard, offer = Genius 3 (-20- **2026-10-05 deals and payment (live):**
  - Early Booking 25% (≥7 days ahead).
  - Long Stay 25% (3+ nights).
  - Last Minute 30%: check-in today or tomorrow, Thu–Sat, booked 3 pm–midnight, until 2026-12-31.
  - The biggest discount wins, and a deal applies only if it beats the 20% offer.
  - Payment: regular rate pays at the hotel (advance optional); deals are paid in full in advance.
  - Free cancellation and a 100% refund at any time on both.
  - Full detail: `docs/PRICING.md` in the website repo.
- **2026-10-05 one-page booking (live, Zehneria-style):**
  - Room cards open a dates popup that lands on /reservations with the chosen room first.
  - Book Now opens the compact guest form on the same page:
    - Full name, phone + email, Terms popup.
    - "Book Now & Pay at Hotel / in Advance".
    - Sidebar "Your Booking Details" with Pay Now / Balance.
  - Commits 0b12714, c231548, 2def6f6.
  - Details: `docs/PRICING.md` → Booking flow.
- **2026-10-05 CRO pass (ads on hold until fixes are done):**
  - Deals now pay **after** booking. The thank-you page shows "Complete Your Payment": bank details, screenshot upload (emails the hotel) or WhatsApp. The booking stays pending until the transfer is verified.
  - Mobile booking form is fields-first, with a compact total.
  - Mobile reservations search collapses to "dates · Modify".
  - LP deal copy synced.
  - Commits 5f61ec6, 2416fcc, 4f634bb.
  - The payment box still needs one real test booking.

