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
  - 2026-10-05 CRO pass (ads NOT launching yet; Shoaib said fixes first):
    - LP deal copy synced (5f61ec6). `lib/lpConfig.ts` LP_PROMOTIONS is hand-kept and must follow promotion changes.
    - Reservations card payment label follows the deal (2416fcc).
    - Commit 4f634bb:
      - **Deals pay AFTER booking.** No screenshot needed at submit; the booking stays pending. The thank-you page has AdvancePaymentBox (bank details, upload via actions/paymentProof.ts, which emails the hotel, plus WhatsApp). The hotel confirms only after the transfer is verified.
      - Mobile booking form is fields-first, with a compact total.
      - Mobile reservations search bar collapses to "dates · Modify".
      - The thank-you payment box is untested end to end: it needs a real deal booking, cancelled afterwards, ideally together with the Meta test-event check.
    - Lighthouse can't run: Hostinger 403s it. Ad crawlers get 200.
    - Speed is fine: TTFB 0.28s, CLS 0.

**Ads launched 2026-10-05 (Google only):**
- Both hotels: Brand (Rs 300) + Non-brand (Rs 1,700) Search campaigns live; old campaigns paused.
- Ad copy corrected (prices, review counts, 25% deals; no 8.3/432/"best").
- Silver Sand reused Pre-Booking Demand v2 (id 24246154507) as Non-brand. Elegant got new campaigns 24327908878/24327909001 with 40 negatives and a call asset.
- **Meta NOT done:** the system-user token has no write permission on act_239008850511120 (error 4841020). Either Shoaib grants the Advertiser role, or he builds from `client-briefs/Meta-Build-Sheet-Oct-26.md` (adsbyshoaib folder, untracked).
- The 4 old Search-optimised Meta campaigns are still running until then.

**Meta live 2026-10-06:**
- The ABS Marketing app was published; the system user "ABS" got MANAGE on act_239008850511120.
- Both hotels now run WhatsApp (Rs 1,500 CBO, 2 ad sets) + Retargeting (Rs 500, InitiateCheckout) campaigns, created via the API. The old Search-optimised campaigns are paused.
- Gotchas:
  - Meta removed location_types / "travelling in".
  - IG Explore placement is deprecated.
  - adimages upload by URL is not allowed.
  - An app in development mode blocks creatives.
  - adsbyshoaib robots.txt now allows Meta crawlers on /privacy and /terms only (commit a584b91).

## CURRENT LIVE ADS STATE (end of 2026-10-06), both hotels
**Budget (Shoaib, daily):** per hotel, Google Rs 2,000 + Meta Rs 2,000. That is Rs 8,000/day for both, Rs 2.4 lakh/month.
Client brief PDF: `client-briefs/Hotel-Ads-Plan-Month-1.pdf` (adsbyshoaib folder, untracked; approved by the client).
Plan: KB Silver Sand `ads-plan-2026-10` (v2 final).

**Rule (Shoaib, 2026-10-06):** ads go to the website first; no direct WhatsApp or call from ads. See [[feedback-ads-website-first]].

### Google
- **Silver Sand** (customer 4063371094):
  - "HSS | Search | Brand | Oct-26" (24322025015), Rs 300, Max Clicks with a Rs 60 cap.
  - "HSS | Search | Hotel in Multan (Non-brand) | Oct-26" (24246154507, the former Pre-Booking Demand v2), Rs 1,700, Max Conversions.
  - "Arrival Intent v2" paused.
- **Elegant** (customer 6223250696 via MCC 8859347478):
  - "Elegant | Search | Brand | Oct-26" (24327908878), Rs 300, Rs 60 cap.
  - "Elegant | Search | Hotel in Multan (Non-brand) | Oct-26" (24327909001), Rs 1,700, Max Clicks with a Rs 90 cap. Ad groups: hotel-in-multan and gulgasht-family-suites.
  - 40 negatives added.
  - Old "Already in Multan" and "Planning to Travel" paused.
- All RSAs corrected:
  - Silver Sand: PKR 2,800 + GST, 845 reviews, Save 25%.
  - Elegant: Rs 6,000 + Tax, 631 reviews; no 8.3, no 432, no "best".
  - All 9 ads APPROVED.
- utm_content codes: SGB / SGN / EGB / EGN.
- CALL and BUSINESS_MESSAGE assets removed. Elegant's account-level call asset is paused.
- Bidding goals were already clean: GBP local actions are not biddable.
- **Watch:** 0 impressions on 5–6 Oct right after launch. If it is still 0, check bids and keywords.

### Meta (act_239008850511120, shared; system user "ABS" now has MANAGE; ABS Marketing app PUBLISHED; token rotated 2026-10-06)
**Live:**
- "Hotel Silver Sand | Website → Contact | Oct-26" (120250808596860504), Rs 1,500 CBO, optimise pixel CONTACT.
  - Ad sets: In Multan +40 km, and Planning a trip (7 cities).
  - LPs /lp/near-station and /lp/book-direct, utm SFC1/SFC2.
- "Hotel Elegant | Website → Contact | Oct-26" (120250808597270504), same setup.
  - Planners ad set adds UAE/KSA/UK. LP /lp/book, EFC1/EFC2.
- Retargeting, both hotels (HSS 120250807956920504, Elegant 120250807959570504): Rs 500, Sales/InitiateCheckout.
  - Audience: website visitors 30 days, minus purchasers 30 days. utm SFS1 / EFS1.
- The 4 website ads were in review at the end of the day; the retargeting ads are approved.

**Paused:**
- The old Search-optimised campaigns (Sep 11).
- The click-to-WhatsApp campaigns (120250807792300504 / …792570504), paused because of the website-first rule.

**Images:**
- Silver Sand: reception photo e3c667… (the video has a burned-in "Save 20% Today"; don't use it).
- Elegant: Executive King 01ae65… and Family Suite 5bb04f….

**Meta API gotchas:**
- Location types / "travelling in" have been removed.
- The IG Explore placement is deprecated.
- Image upload by URL is not allowed.
- An app in development mode blocks ad creatives.
- The Graph paging URLs leaked the token. A fix task was spawned; the token was rotated.

### Supporting changes
- adsbyshoaib `robots.txt` lets only Meta's crawlers read /privacy and /terms (a584b91). Search engines stay blocked.
- GA4 cleaned on both properties:
  - Silver Sand key events: booking_confirmed, whatsapp_click, call_click, directions_click, purchase.
  - Elegant key events: purchase, booking_created, booking_submitted, whatsapp_click, call_click.
  - Internal-traffic filters are ACTIVE.
  - The GA4 token now has analytics.edit.

### Next
1. Check this evening and tomorrow: Meta website ads approved? Google impressions starting?
2. Hands off for week 1. Monday report: spend, chats, calls, bookings by Ref code.
3. Week 3: scale the winners +20%, cut anything costing more than 2× target.
   - Target per confirmed booking: Silver Sand ≤ Rs 1,500, Elegant ≤ Rs 2,500.
4. Still pending:
   - 24 GBP draft replies awaiting Shoaib's approval.
   - Facebook page map pin is in Chennai.
   - GBP: remove Pool, set check-in to 24h.
   - Check Instagram / TikTok / OTA NAP.
   - A new Silver Sand video without the old offer.
