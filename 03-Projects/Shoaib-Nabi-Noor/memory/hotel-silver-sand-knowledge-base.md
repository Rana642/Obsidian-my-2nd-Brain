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
- **Meta NOT done:** the system-user token has no write permission on act_239008850511120 (error 4841020). Either Shoaib grants the Advertiser role, or he builds from `client-briefs/Meta-Build-Sheet-Oct-26.md` (adsbyshoaib folder, untracked).
- The 4 old Search-optimised Meta campaigns are still running until then.
