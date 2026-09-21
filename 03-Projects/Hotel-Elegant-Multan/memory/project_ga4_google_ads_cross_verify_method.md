---
name: project-ga4-google-ads-cross-verify-method
description: "How to cross-verify Google Ads conversions against GA4 — use gclid auto-tagging (sessionSource/sessionMedium/sessionCampaignName), not UTM params, not GA4-imported conversion actions"
metadata: 
  node_type: memory
  type: project
  originSessionId: fb776024-2ed0-44c1-a91d-40c7ca3ba0dd
  modified: 2026-09-18T06:57:19.825Z
---

For Hotel Elegant, GA4 (property 546101152) is linked to Google Ads (customer 6223250696) via an active `googleAdsLinks` connection (created 2026-08-04, `adsPersonalizationEnabled: true`) — see [[reference_google_ads_account_id]]. This link means every Google Ads click's `gclid` gets auto-recognized by GA4 without any manual UTM setup: GA4 auto-labels that session `sessionSource: "google"`, `sessionMedium: "cpc"`, and `sessionCampaignName: "<the real Google Ads campaign name>"`.

**Decision (2026-09-18):** Use this gclid auto-tagging as the cross-verification method between GA4 and Google Ads — query GA4 (`ga4_run_report`) with dimensions `date`, `eventName`, `sessionSource`, `sessionMedium`, `sessionCampaignName`, filtered to the events that matter (`whatsapp_click`, `call_click`, `book_now_click`, `contact_intent_submitted`, `booking_created`, `booking_submitted`), and compare the `google`/`cpc` rows against what Google Ads' own conversion_action reporting shows for the same window. This is how the AW-18202393540 vs AW-18370206861 bug and its ongoing effects were caught and confirmed fixed.

**Explicitly rejected as methods, per the user:**
- **UTM parameters** — not needed; auto-tagging (gclid) already does this natively with zero setup/maintenance, and adding manual UTM suffixes on top would be one more thing to configure correctly and one more thing that could silently drift out of sync.
- **Re-activating GA4-imported conversion actions in Google Ads** (found REMOVED/HIDDEN in this account — `Hotel Elegant Executive Suite (web) whatsapp_click/call_click/booking_created/...`) — the user does not want this turned into a second live conversion-counting pipeline; GA4's role here is strictly a cross-check/audit tool, not a parallel source Google Ads bids on. See [[feedback_gads_direct_connection_only]] — the only pipeline that should ever feed real Google Ads conversions is the direct gtag one.

**How to apply:** Every time Google Ads conversion health is audited going forward, pull the same GA4 dimension set above for the window in question and compare against Google Ads' `segments.conversion_action_name` / `metrics.all_conversions` for the identical date range — don't declare Google Ads conversions "missing" or "working" from one source alone (ties into [[feedback_cross_verify_tracking]]).
