---
name: hotel-silver-sand-knowledge-base
description: Hotel Silver Sand Multan KB built 2026-10-05 + parity plan with the client's other hotel; repo/DB/ad-account pointers and open owner decisions
metadata:
  type: project
---

Hotel Silver Sand Multan = same client (Muhammad Ajmal) as Hotel Elegant. On 2026-10-05 Shoaib asked to bring Silver Sand to the same standard as Elegant (all rules + implementations), then plan ads for both. KB project "Muhammad Ajmal — Hotel Silver Sand Multan" filled the same day:
- Docs: nap, brand_position, icp, pain_points, graphic_rules, system_rules, memory.
- Marketing docs: offers-and-policies, competitor-and-keyword-research, ads-audit-2026-10, nap-consistency-audit, fix-checklist.
- 4 room products and 7 assets.

Pointers:
- Repo github.com/Rana642/Hotel-Silver-Sand; local `D:\Rana Shoaib\My Projects Website\Hotel Silver Sand Multan`.
- Next.js 16 on **Vercel**. Supabase qjarifqmmfeggmkrxmbm is PRODUCTION: never create test bookings. Shoaib runs SQL migrations himself.
- GBP location 3905396707891391632 (connected in /dashboard/gbp).
- Google Ads 4063371094 (own account). Meta act_239008850511120 (shared with Elegant; filter by Silver Sand campaigns, pixel 1336120438462344).
- GA4 542541529.
- Vault project notes: `Obsidian-my-2nd-Brain/03-Projects/Hotel-Silver-Sand-Multan/`. Read its hss-non-negotiables first: locked colours, no staff names, Booking.com link stays as a subtle fallback.

Audit findings (2026-10-05):
- NAP mismatch vs GBP: the site adds "near Aziz Hotel Chowk, Cantt"; phone shows "0300-872-0939" instead of GBP's "0300 8720939".
- Review count 837 → 845.
- Booking bar shows a static "From PKR 3,000" before JS loads.
- No tracking guard on /admin. No WhatsApp ref code.
- Last Minute deal expired 2026-09-30. Deal engine picks by priority, not biggest discount.
- Google Ads: GBP local actions are primary conversions.
- GA4: page_view, view_item_list and first_visit are key events; legacy "WhatsApp Clicks" still fires.
- Meta ad sets optimise on SEARCH: Rs 40.6k for 3 purchases in 30 days.
- DB: 11 bookings since Aug, 4 of them no-shows.

Already in place (no work needed):
- Purchase fires at submit (Pixel + CAPI, deduped).
- Google Ads booking conversion with enhanced conversions.
- Direct gtag, no GTM.
- /reservations one-page flow with a compact form.

Open owner decisions:
- Booking.com standard and Genius rates (for rate parity).
- Tax: keep GST 16% only (settled Sept 2026).
- Deal percentages and whether to renew Last Minute.
- Deals: full advance after booking, or stay pay-at-hotel.
- Bank details.

Boundary note: [[hotel-elegant-knowledge-base]]. Silver Sand's old vault rule says never to mention Elegant in Silver Sand reports. Shoaib now wants parity and an ads plan for both, so using Elegant as the implementation reference is fine. Keep each hotel's performance reports separate unless he asks for a combined view.

**Progress 2026-10-05 (Shoaib: "complete all phases without stopping, analyse what's left"):**
- Phase 1 live (f253b8a):
  - NAP = GBP.
  - Review count 845.
  - Fallback prices fixed.
  - /admin tracking guard (`hss_internal`).
  - First-touch WhatsApp "Ref:" code. A global wa.me/tel listener fires the Ads conversions.
  - Expired promos hidden.
  - Biggest discount wins.
- Phase 3 live (526b817): room-card dates popup → /reservations?room= highlighted; mobile one-line search; mobile total above Book Now.
- docs/TRACKING.md added (b83750b).
- Dev launcher: `hotel-silver-sand-dev` on port 3030 (`.claude/hotel-silver-sand-dev.cjs` in the adsbyshoaib repo).
- The repo needed a local git identity. Set to Shoaib's; older commits were by "Rana642". Vercel deployed fine.
- Google Ads correction: bidding is already clean. GBP local actions are NOT biddable; real conversions are 4 WhatsApp + 1 Call. No change made.
- 24 GBP review replies queued as **draft** (nothing sent).
- Facebook page map pin is in **Chennai** (13.079, 80.261). Owner must fix it in FB settings.
- Phase 2 waits on owner decisions. Note: deals currently STACK on the offer price. Switching to the Elegant model (deal off standard, only if it beats the offer) waits for the new deal percentages.

**Phase 2, live 2026-10-05.** Shoaib's answers:
- Offer = Booking.com standard −20%.
- Tax-exclusive: rates are pre-tax, +16% GST at checkout.
- Deals 25/25/30, with the deal % taken off the standard rate. A deal applies only if it beats the offer; the biggest wins.
- **No advance payment on Silver Sand**. He stopped it mid-build ("Silver Sand mai Advance payment wala na add kero abhi"); the advance flow was removed. Deals pay at the hotel too.
- Every booking: free cancellation, 100% refund.

Rates (standard → offer): King 3,500→2,800 · Double 8,500→6,800 · Triple 7,500→6,000 · Twin 7,000→5,600.
Last Minute: check-in today or tomorrow, Thu–Sat, booked 3 pm–midnight, until 2026-12-31.

Commits:
- Code 5f52bdf: lib/pricing.ts addGst, priceWithDeal, coupons don't stack with deals, DEAL:<name> marker.
- DB switched after the deploy, then an empty-commit rebuild (b359234).

**Gotcha:** home, /rooms and the LPs are static. They refresh only on an admin room save (revalidatePath) or a redeploy, so after any direct DB price edit, redeploy.

**Ads launched 2026-10-05 (Google only):**
- Both hotels: Brand (Rs 300) + Non-brand (Rs 1,700) Search campaigns live; old campaigns paused.
- Ad copy corrected (prices, review counts, 25% deals; no 8.3/432/"best").
- Silver Sand reused Pre-Booking Demand v2 (id 24246154507) as Non-brand. Elegant got new campaigns 24327908878/24327909001 with 40 negatives and a call asset.
- (superseded 2026-10-06, Meta now live) Meta NOT done: the system-user token has no write permission on act_239008850511120 (error 4841020). Either Shoaib grants the Advertiser role, or he builds from `client-briefs/Meta-Build-Sheet-Oct-26.md` (adsbyshoaib folder, untracked).
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
