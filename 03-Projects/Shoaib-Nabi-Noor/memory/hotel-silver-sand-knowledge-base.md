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
