---
name: project-marketing-tracking
description: "Silver Sand Meta Ads + Google Ads + GA4 tracking architecture, current campaign state, and open threads (Sep 2026)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1d648d7d-fd46-4ccf-80ea-bbb3bb48c07f
  modified: 2026-09-17T06:59:37.322Z
---

**Goal (user's own words, repeated often): "bookings chahiye har haal mein"** — real
guest bookings, not proxy metrics. Everything below serves that. See
[[project-hotel-website]] for the site itself and [[feedback-hotel-elegant-boundary]]
for the account-sharing boundary rule (never touch/mention Hotel Elegant, a separate
business sharing the same Meta/Google Ads accounts).

## Access mechanism
- **Meta**: NOT an MCP tool — direct Graph API via node fetch scripts, using
  `META_MARKETING_API_TOKEN` (System User token) + `META_AD_ACCOUNT_ID` from
  `.env.local`. User declined MCP/OAuth setup for this.
- **Google Ads**: `mcp__google-ads__*` MCP tools (query/mutate/keyword_ideas/list_accounts).
  Customer ID `4063371094` = Silver Sand's own account (not an MCC). Always
  `dry_run: true` before `dry_run: false` on mutate.
- **GA4**: `mcp__e41c058f...__ga4_run_report` / `ga4_admin_request` MCP tools.
  Property ID `542541529` ("Hotel Silver Sand Multan"), Measurement ID
  `G-TFTT1WXL5L`. **This connector's token is READ-ONLY for Admin API writes**
  (confirmed: creating/deleting conversion events both 403'd with
  `ACCESS_TOKEN_SCOPE_INSUFFICIENT`) — any GA4 config change (new key event,
  etc.) must be walked through manually in the GA4 UI (Admin → Events), not
  attempted via `ga4_admin_request` POST/DELETE.

## Tracking architecture (as of 2026-09-17)
All three destinations are **direct installs, no GTM** (GTM fully removed
2026-09-16 — was the last indirect hop, previously fanning GA4 events out via
`dataLayer`). One `gtag.js` loader in `layout.tsx` configs both
`NEXT_PUBLIC_GOOGLE_ADS_ID` (AW-18025732902) and `NEXT_PUBLIC_GA4_ID`
(G-TFTT1WXL5L); Meta Pixel loads its own `fbevents.js` separately.
`trackEvent()` in `analytics.ts` now calls `gtag('event', ...)` directly.

- **Meta**: dual-signal — browser Pixel (`trackMetaPixel`) + server CAPI
  (`sendMetaEvent` in `meta-capi.ts`), paired by shared `eventId` so Meta
  dedupes. Funnel: Search (date-pick, HeroBookingBar/RoomSearchBar) →
  ViewContent (room detail) → Schedule/Contact (PreContactModal, qualified =
  `intent === "book"` alone, dates optional) → InitiateCheckout (booking form)
  → Lead + Purchase (confirmed booking, `booking.ts`). Search also logs to a
  `search_intents` Supabase table (migration-phase16.sql) surfaced at
  `/admin/search-intents` for human visibility.
- **Google Ads**: primary conversions = Call Click / WhatsApp Click / Booking
  Request (correct — matches the real call/WhatsApp booking model). Added a
  **secondary, non-primary** "Website Search" conversion action (fires
  alongside Meta's Search) purely for extra Smart Bidding signal volume —
  doesn't touch primary optimization.
- **GA4**: fixed 2026-09-17 — GTM-era conversion event was named "WhatsApp
  Clicks" (capitalized, legacy GTM tag override); the site's own code always
  sent `whatsapp_click` (lowercase). Created `whatsapp_click` as a new GA4 key
  event and un-marked "WhatsApp Clicks" as a key event (didn't delete the
  event definition itself, just its key-event flag) — done manually in the
  GA4 UI since the connector can't write. `call_click` was already correctly
  named both places, no fix needed there.

## Meta ad sets (act_239008850511120, pixel 1336120438462344)
- **Already in Multan** (`120250365195160504` campaign) — presence-only,
  Multan, PKR 700/day.
- **Planning to Travel** (`120250365195640504` campaign) — presence-or-interest,
  Multan+Lahore+Karachi+Islamabad, PKR 1,100/day.
- Both originally optimized on `SCHEDULE` (low volume, Learning Phase
  starved) — **switched to `SEARCH`** 2026-09-11. Meta doesn't allow editing
  `promoted_object` on a published ad set, so this required creating new ad
  sets (`120250390130510504` / `120250390134910504`) inside the same
  campaigns and pausing the old ones, not editing in place.
- 2026-09-14: "Already in Multan" ad's creative claimed "Save 20% Today /
  Last Minute Deal" but that promo only applies Thu/Fri/Sat (DB
  `weekdays: [4,6,5]`, `promotions` table) — misleading on other days. Fixed
  by creating a new creative with neutral copy (ad renamed "... - ad v2").
  Meta blocks editing creative text on a published ad via API — had to be
  done through Ads Manager's own edit flow (which silently swaps in a new
  creative under the hood; confirmed via API afterward that it worked).
  **Still open**: the underlying video has "Save 20% Today" burned into the
  video overlay graphic itself — ad copy is fixed but the video asset isn't;
  needs a new video without that text whenever there's time.
- 2026-09-16: "Planning to Travel" real booking landed — `HSS-96D9TP`, guest
  Irfan Khan, Deluxe King Room, PKR 3,629 (first Purchase from the new
  Search-optimized ad set).

## Google Ads campaigns (customer 4063371094)
- **Arrival Intent v2** (`24246154504`) — Multan-only, PKR 500/day, phrase
  match SKAGs. Was at literally 0 impressions for 6 weeks (keyword volume
  ~10/mo each on Keyword Planner) — added 3 new arrival-intent keyword
  variants 2026-09-14 (railway station / cantt station / budget hotel),
  reusing the existing verified-facts RSA. Still low volume as of 2026-09-17,
  still in Learning.
- **Pre-Booking Demand v2** (`24246154507`) — Multan+5 other cities, PKR
  2,200/day, SKAGs incl. brand keyword "hotel silver sand multan". **Exited
  Learning Phase as of 2026-09-17** (`primary_status: ELIGIBLE`, no blocking
  reasons) — the healthy one of the two.
- **Open watch item**: brand keyword "hotel silver sand multan" has Quality
  Score 10/10 (perfect) but ~36% absolute-top-impression-share and
  unusually high CPC (up to PKR 117/click) despite being the hotel's own
  name — pattern consistent with an OTA (e.g. Booking.com) also bidding on
  the brand term. 0 conversions through ~10 clicks so far — sample too small
  to be alarming yet, but worth re-checking; if it stays at 0 past ~15-20
  clicks, dig further (was mid-investigation when last checked).
- Both v2 campaigns replaced older v1 campaigns (`24212446722`,
  `24207015269`, `24212446218` — now paused) from an earlier rebuild.

## Standing process notes
- Monitoring is manual/on-demand (user declined cloud-scheduled routines) —
  triggers on phrases like "ads check karo" / "check ads". When checking,
  cross-reference all three: Meta insights, Google Ads GAQL, and real
  Supabase data (`bookings`, `inquiries`, `search_intents`) — the real
  success metric is bookings/inquiries landing, not platform-reported
  conversions alone.
- Any live ad-account mutation (Meta or Google) gets a dry-run/preview shown
  to the user before going live, even though the user has been giving fairly
  open-ended "just do it" authorization lately — keep surfacing the exact
  before/after rather than silently acting, per the pattern the user has
  consistently approved throughout this engagement.
