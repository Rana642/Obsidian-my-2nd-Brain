---
name: project-hotel-website
description: Hotel Silver Sand Multan — Next.js site + Supabase admin panel, phased build
metadata:
  node_type: memory
  type: project
  originSessionId: 1d648d7d-fd46-4ccf-80ea-bbb3bb48c07f
  modified: 2026-09-17T06:59:04.251Z
---

Hotel Silver Sand Multan — Next.js 16 + React 19 + TS + Tailwind v4 site, deployed on Vercel (`https://hotel-silver-sand.vercel.app`, custom domain hotelsilversandmultan.com connect hoga end mein). Repo: github.com/Rana642/Hotel-Silver-Sand. Backend: Supabase (project ref `qjarifqmmfeggmkrxmbm`). Admin at `/admin`, admin user `admin@hotelsilversandmultan.com` (role=admin).

**Model:** pay-at-hotel (no online payment), booking = request confirmed via WhatsApp. Timezone Asia/Karachi.

**Tax (Sep 2026, settled — don't re-open):** direct website booking = **GST 16% only** (`rooms.gst_percent = 16`). Booking.com ka 26% = GST 16 + City Tax 10; city tax sirf OTA channel par lagta hai. User ne saaf kaha tax "jo pehly wohi rehny dy". Maine tajweez di thi ke "direct book karein — city tax nahi lagta" copy mein daal dein; **user ne mana kar diya — ye message site par NAHI dalna.**

**Rates = owner-managed.** Booking.com ka public page post-discount rate dikhata hai, standard rack rates extranet mein hain. User khud admin dashboard se set karta hai. `scripts/sync-rooms-from-booking.mjs` ab `--force` ke baghair chalta hi nahi (warna rates overwrite ho jayen).

**Booking.com = source of truth** room names/sizes/beds/occupancy ke liye (Deluxe King, Deluxe Double, Deluxe Triple, Budget Twin). **Google Business Profile = 3.8 ★ from 837 reviews** — yehi figure site par publish hota hai (Booking.com ka 6.2/19 nahi, sample bohot chhota hai). GBP par do galtiyan hain jo owner ko theek karni hain: "Pool" listed hai (pool nahi hai) aur check-in 11 AM likha hai (asal mein 24-hour hai). Email = Resend (working). Live domain www.hotelsilversandmultan.com. SQL migrations user Supabase SQL editor mein khud run karta hai (main DDL run nahi kar sakta).

**Inquiries RE-ENABLED (Sep 2026, supersedes the Aug "deactivated" note below).** `CONTACT_FORM_ENABLED` flipped back on — PreContactModal (Quick-Details modal before Call/WhatsApp) is live again and writes to the `inquiries` table via `createInquiry()`. `/admin/inquiries` had no sidebar/nav link for a while after re-enabling (real leads were landing there invisibly) — fixed 2026-09-16, now in the secondary nav as "Inquiries". Separate from **Contacts** `/admin/contacts` (booked guests, not raw leads) which still exists alongside it. See [[project-marketing-tracking]] for the Meta/Google Ads work that made qualifying these leads matter (PreContactModal's `qualified` flag feeds the ad platforms' optimization signal).
<details>
Old note (Aug 2026, now stale): Inquiry lead-capture turned OFF (not deleted). Call/WhatsApp act directly — contact page used direct Call/WhatsApp/Email CTAs only.
</details>

**Reservations flow + Deals (Sep 2026):** `/reservations` = Simplotel/Zehneria-style booking page (availability bar, room list with Rates/Amenities/Photos tabs, rate plan + inclusions, Guest Info + booking summary sidebar, pay-at-hotel). Hero "Book Now" + all header/BookingBar "Book Now" link here; PromotionsSideTab hidden on it. Dashboard-managed **rate deals** (`rate_deals` table, migration-phase9.sql — MUST be run by user): admin `/admin/deals` (DealsEditor) creates date-window discounts (name, discount%, check-in date range, room_id null=all, refundable+free_cancel_days, priority). src/lib/deals.ts pickDeal/applyDeal. Reservations page + createBooking apply the matching deal for the check-in date (server-authoritative). Crash-safe before migration. Deal types like Early Bird/Last Minute are just deals with different date windows (no hardcoded thresholds).

**Availability (phase8, Aug 2026):** multi-unit inventory model. `rooms.total_units` = inventory per type; per-day available = total − bookings(from bookings table, non-cancelled/no_show) − manual_holds(OTA/walk-in/maintenance). Per-date cap override in `inventory_overrides`. Website blocks booking when available≤0 (server-side `checkNightsAvailable` in src/lib/availability.ts). Admin at `/admin/availability` (14-day grid). Old single-unit `availability_blocks` model retired (table still exists, no longer written/read). Channel manager (future) will auto-write manual_holds. **migration-phase8.sql must be run by user in Supabase** for it to activate (code is crash-safe before migration — defaults to 1 unit).

**Done:** Phase 1 (core booking ops: statuses, notes, financials snapshot, extend stay, receipt, walk-in entry), Phase 2 (inquiries lead capture + admin/reception roles via is_staff()/is_admin()).

**IMPORTANT — Coupons abhi bhi banana hai (user ne kaha skip NAHI karna).** Discount coupon jo booking total kam kare (payment phir bhi hotel pe). Reports + Activity Log batch ke baad karna hai.

**Why:** Large phased build; user wants coupons despite earlier "discount just mention" note.
**How to apply:** After Reports+ActivityLog, build coupons (code, atomic usage counter RPC, apply at booking, reflect in receipt/admin). See [[feedback-concise]] for response style.
