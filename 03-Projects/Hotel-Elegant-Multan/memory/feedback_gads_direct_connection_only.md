---
name: feedback-gads-direct-connection-only
description: "Google Ads must always connect to the website directly (gtag), never via GTM or any indirect/imported pipeline"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fb776024-2ed0-44c1-a91d-40c7ca3ba0dd
  modified: 2026-09-18T06:57:06.542Z
---

Google Ads' connection to the Hotel Elegant website must always stay **direct** — `gtag('config', 'AW-...')` + `gtag('event', 'conversion', {send_to: ...})` fired straight from the site's own code (`app/layout.tsx` + `lib/googleAdsPixel.ts`), the same way it is right now. Never reintroduce GTM or any other indirect/middleman layer between the site and Google Ads.

**Why:** This is the same direct-wiring principle the user set for the whole account early in this engagement (see [[feedback_meta_marketing_api_direct]] for the parallel Meta rule) — GTM was fully removed earlier for exactly this reason. Explicitly reinforced 2026-09-18 right after fixing the `AW-18202393540` vs `AW-18370206861` wrong-account-ID bug: the user does not want to be asked about this again or have the setup regress.

**How to apply:** Whenever touching Google Ads tracking code, keep it as direct `gtag()` calls only. Don't propose GTM, a tag-management layer, or any conversion pipeline that isn't the site firing directly to `AW-18370206861` (see [[reference_google_ads_account_id]] for the correct account/ID). GA4's own GA4-imported conversion actions in Google Ads (found REMOVED/HIDDEN during this audit) are explicitly NOT to be reactivated as a parallel conversion pipeline — see [[project_ga4_google_ads_cross_verify_method]] for what GA4↔Ads is used for instead.
