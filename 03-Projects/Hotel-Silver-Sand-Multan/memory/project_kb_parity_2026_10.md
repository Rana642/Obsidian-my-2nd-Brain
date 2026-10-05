---
name: project-kb-parity-2026-10
description: 2026-10-05 Silver Sand knowledge base built + parity plan with the client's standard (NAP=GBP, tracking, deals, booking CRO)
metadata:
  type: project
---

2026-10-05: KB project "Muhammad Ajmal — Hotel Silver Sand Multan" built (adsbyshoaib dashboard, kb_* MCP).
- Docs: NAP locked to GBP (514 Akbar Road, Railway Colony, Multan 60000 · 0300 8720939), brand, ICP, pains, graphic/system rules, offers, competitors, ads audit, NAP audit, fix-checklist.
- 4 room products and 7 assets.

Shoaib: same client as the other hotel, so apply the same rules and implementations, then plan ads for both.
- The old "never mention the other hotel" rule is relaxed for implementation work.
- Keep performance reports per hotel.

Fix checklist (KB `fix-checklist`):
- **Phase 1** (code, no decisions):
  - NAP = GBP.
  - Review count 845.
  - Fix the static "From PKR 3,000" fallback.
  - No tracking on /admin.
  - WhatsApp ref code.
  - Remove expired deal copy.
- **Phase 2** (owner decisions): Booking.com rate parity, deal percentages and Last Minute renewal, advance payment for deals.
- **Phase 3** (CRO):
  - Room-card dates popup.
  - Mobile form-first.
  - Collapsed search bar.
  - Terms popup.
  - Thank-you payment box, if needed.
- **Phase 4** (accounts):
  - Google Ads: local actions → secondary.
  - GA4: remove page_view, view_item_list, first_visit and begin_checkout as key events; find what still sends "WhatsApp Clicks".
  - Meta: move off Search optimisation; refresh creatives.
  - GBP: remove Pool, set check-in to 24h, reply to the 9 unreplied reviews.

Last 30 days:
- Meta: Rs 40.6k for 3 purchases and 92 WhatsApp conversations.
- Google: Rs 10k, of which 1 real Booking Request.
- DB: 11 bookings since Aug, 4 of them no-shows.

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
