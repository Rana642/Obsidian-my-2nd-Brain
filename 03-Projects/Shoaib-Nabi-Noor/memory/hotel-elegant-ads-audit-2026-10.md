---
name: hotel-elegant-ads-audit-2026-10
description: Hotel Elegant goal = Google+Meta ads bookings; 2026-10-04 audit findings (Meta optimises SEARCH event, GAds web conversions 0) and account IDs
metadata:
  type: project
---

Shoaib's main goal for Hotel Elegant (2026-10-04) is real hotel guests and maximum bookings from Google Ads and Meta Ads. Calendar/content work is not needed for now. The full audit is in KB marketing_doc `ads-audit-2026-10`.

Key findings:
- **Meta:** the active Elegant ad sets optimise for the pixel SEARCH event. Since 11 Sep they spent Rs 61.6k for 1 purchase and 8 WhatsApp chats, with about 5 s sessions.
- **Google Ads:** website conversion actions have recorded 0 since June, even though the labels match the code. Max Conversions is running blind, and search terms are full of competitor names.
- **Bookings table:** 22 bookings Jul–Oct, and every fb/paid booking was a no-show. WhatsApp bookings aren't attributed to any source.

Account IDs:
- Google Ads 6223250696 needs loginCustomerId **8859347478** (MCC "Ad By Shoaib"). Calling it without the MCC id gives a 403.
- Meta act_239008850511120 is shared with Silver Sand. Elegant pixel 27407654508906433.
- GA4 property 546101152.
- Hotel bookings are readable read-only via the hotel repo's .env.local service key.

**Why:** The previous ads were "working" on paper but produced no real guests.

**How to apply:**
- Fix tracking and optimisation signals first, then restructure the campaigns.
- Every ads change needs Shoaib's approval.
- Cross-verify GA4 + Meta + Google Ads together, as his vault rule requires.
- See [[hotel-elegant-nap-locked]] and [[hotel-elegant-knowledge-base]].

**2026-10-04 research follow-up:**
- A draft playbook was written to KB marketing_doc `ads-playbook`, and `icp` was rewritten. Shoaib has to approve both.
- The playbook covers:
  - Google: Brand, "In Multan" calls, "Travellers" website bookings.
  - Meta: WhatsApp & Calls, Website bookings optimising InitiateCheckout, Retargeting.
  - Purchase is sent only via CAPI when admin marks the booking confirmed. The Search event is never used for optimisation.
- **Rate parity is broken:** on Booking.com the King Room is PKR 7,000 + tax = 8,820, against Rs 10,773 on the website (15 Oct check).
- **Conflict rule:** Shoaib also runs Hotel Avalon Suites (79-A Gulgasht, next door) and Silver Sand. Never bid on or target each other; add both as negatives.
- Competitor and global research is in KB `competitor-and-global-research`. Findings on "hotel in multan":
  - The Google paid ads are only Expedia and Booking.com; no Multan hotel runs its own ads.
  - Elegant's own Google Hotels page lists only Trip.com and Bluepillow prices. The website is not listed, even though repo feeds `/api/google/hotel-list.xml` and `rates.xml` exist — Hotel Center is not live.
  - Google shows check-in as 11:30 AM instead of 24 h.
- Meta Ad Library needs a login, so competitor Meta ads are still unchecked.
- Website audit 2026-10-04 is in KB `website-audit-2026-10`.
  - WhatsApp clicks 1,751 and calls 1,053 against 21 web bookings, so WhatsApp and calls are the real channel.
  - GBP brought about 600 calls and 1,550 directions in 3 months, more than all ads.
  - Admin pages are tracked in GA4; Meta ad URLs carry a literal `fbclid=fbclid`.
  - The King room page title claims "Five Star"; /lp/book makes an OTA-fee claim while the OTA is cheaper; review numbers on the site are stale.
  - CAPI Purchase fires at submit (`app/actions/metaCapi.ts`), not at confirmation.
  - GSC for sc-domain:elegant-suite.com is siteUnverifiedUser for our account, so access must be re-granted.
- **Phase 1 + trust/SEO fixes shipped 2026-10-04** (Hotel Elegant repo commits df979ca, e491dea; live-verified). Details are in KB `fix-checklist` → Progress.
  - Google Ads root cause: TrackedLink never fired Ads conversions.
  - Tracking init is now inline in `<head>`; /admin and staff traffic are excluded via `he_internal`.
  - Duplicate noscript PageView removed.
  - WhatsApp "(Ref: …)" codes added.
  - Review stats live in `lib/reviewStats.ts`.
  - Booking.com links removed from the site; "Five Star" removed.
  - Gotcha: Meta product-param events are sent as a hidden form POST, invisible in resource timing. Verify with Events Manager, not performance entries.
  - Local dev: launch config "hotel-elegant-dev" (port 3020) via `.claude/hotel-elegant-dev.cjs`. It must chdir into the repo, otherwise this repo's Tailwind v4 config breaks the build.
  - Local dev uses the production DB, so never submit test bookings.
  - Phone display format on the website = GBP "0317 3330998" / "+92 317 3330998" (commit ba14736, plus settings.hotel_phone in the DB). tel:, wa.me and schema keep +923173330998.
  - **Meta Purchase fires at booking submit** (thank-you Pixel + server CAPI, event_id booking-purchase-<ref>; commit ef3e97e). A confirm-time Purchase (cb2d240) was tried and REVERTED the same day: Shoaib said too few bookings would keep Meta stuck in learning and slow confirmations would lose signal. Never move Purchase to confirm again. Staff-entered bookings also send Purchase, without staff cookies/IP. StayCompleted fires on completion.
  - Fixed in commit e532ebd: ContactIntentButton only sent GA4 whatsapp_click/call_click when the caller passed onClick (LP CTAs). Now every site button sends them. Live-verified on 2026-10-04: room page view_room; Call/WhatsApp → GA4 + Ads + Meta Contact; Book Now → GA4 + Meta InitiateCheckout. Not live-verifiable without a real booking: the thank-you events and server CAPI Lead/Purchase/StayCompleted (needs a Meta Test Events code).
  - **2026-10-05 rate parity, live (website commit 0637d79):** the website is tax-EXCLUSIVE like Booking.com (lib/pricing.ts). The DB rooms table holds pre-tax rates:

    | Room | Standard | Offer |
    |---|---|---|
    | King | 7,500 | 6,000 |
    | Family | 14,000 | 11,200 |
    | Triple | 12,000 | 9,600 |
    | Presidential | 16,000 | 12,800 |
    | Junior | 13,000 | 10,400 |

    - Standard = Booking.com standard; offer = Genius 3 (-20%).
    - A promotion deal now applies only if it beats the offer.
    - Deploy gotcha: a push made during a DNS blip never reached Hostinger; an empty commit re-triggered it.
    - Rollout gotcha: DB prices and the pricing code must flip together. While old code was live with new prices, the site would have undercharged 26%, so the rates were reverted until the deploy landed.
    - Watcher gotcha: React splits numbers with comment nodes, so grep for a static label, not "(26%)".
  - 2026-10-05: Early Booking and Long Stay deals were raised to 25% off standard (offer = 20%, so the deals beat it). The booking page payment wording now follows the active deal (commit 253d0cc).
  - 2026-10-05 payment/cancellation LOCKED: regular rate = pay at the hotel, advance optional; deals = FULL payment in advance by bank transfer; both get free cancellation and a 100% refund at any time. Site wording unified (commits d238e5f, 2f3648d). Last Minute 30% renewed to 2026-12-31 (Thu–Sat check-in, booked 3 pm–midnight PKT). Deal priority Early > Long Stay > Last Minute.
  - 2026-10-05: deal selection now picks the highest discount % (priority only breaks ties), in lib/deals.ts. Side effect: Last Minute (30%, no lead-time limit) wins any Thu–Sat check-in booked 3 pm–midnight, even weeks ahead.
  - 2026-10-05 final: Last Minute is limited to check-in today/tomorrow (DB lead_time_type=last_minute, lead_time_days=1), plus Thu–Sat and 3 pm–midnight PKT, until 2026-12-31. Promotions label reads 'Check-in today or tomorrow' (commit 945c3c7).
  - 2026-10-05 one-page booking (commit 0b12714, live-verified):
    - Room cards (home, /rooms, LP) and the room-page Reservation button open ReservationModal for that room; it lands on /reservations?room=<slug>, with the room listed first under a 'Your selected room' banner. 'View Room' is a separate link.
    - Book Now on /reservations swaps the list for a selected-room row (with Modify) plus the BookingForm inline (ReservationsFlow.tsx); ?book=<id> keeps the form open.
    - /booking still works for direct links.
    - Dev gotcha: 'Invalid hook call / useContext null' after hot reload is an HMR glitch; restart the dev server.
  - 2026-10-05 compact booking form (commit c231548): Full name (split for CAPI), Phone + Email on one row, terms checkbox that also confirms Multan, 'Book Now & Pay at Hotel / in Advance' button, sidebar 'Your Booking Details' with Pay Now / Balance and the saving. BookingForm takes an 'embedded' prop that hides the in-form summary on /reservations.
  - 2026-10-05: Terms & Conditions open in a popup on the booking form (7 hotel terms; 'I Agree' ticks the checkbox), commit 2def6f6. The booking flow is documented in docs/PRICING.md of the hotel repo.
