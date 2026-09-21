---
name: feedback-cross-verify-tracking
description: "When auditing Google Ads, Meta Ads, or GA4 tracking, always cross-verify all three together before declaring anything broken or fixed"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fb776024-2ed0-44c1-a91d-40c7ca3ba0dd
  modified: 2026-09-16T10:16:43.374Z
---

Whenever checking or auditing ad tracking (Google Ads conversions, Meta Pixel/CAPI, or GA4), always cross-verify across all three — don't conclude something is broken or working based on checking just one source or one environment in isolation.

**Why:** Caught making a false "broken" claim about GA4 Measurement Protocol (`GA4_API_SECRET`) — it was flagged as "never configured" based on checking only the local `.env.local` file. Production's actual Hostinger environment variables had it correctly set since 1 Aug 2026, and real bookings had been triggering it successfully the whole time. The user's exact words: "Ab jab bhi Ads check karne (Google ya Meta), GA4 se zaroor verify kar lena — aisa na ho website par cheezein sahi kaam kar rahi hon aur Meta Pixel ki tracking sahi na ho ya Google Ads ki. Cross-verify se asal haqiqat pehle milegi, phir uska hal kiya karenge." (Cross-verifying reveals the real truth first, then decide the fix.)

**How to apply:** Before asserting any ad-tracking finding (broken, fixed, misconfigured) for this project:
1. Check the actual live/production state, not just local config files (`.env.local` can differ from Hostinger's real environment variables — see [[project_ci_deploy_broken.md]] for the related pattern of local state not reflecting production).
2. Cross-check GA4, Meta (Pixel + CAPI), and Google Ads together for the same event/flow — e.g. if checking whether a booking conversion fired, look for it in GA4 events, Meta's dataset stats, and Google Ads conversion actions, not just one.
3. Only report something as "broken" after confirming it's actually absent across the cross-check, not just missing from one place you happened to look first.
4. When genuinely uncertain and no further verification is possible from this side (e.g. no GA4 Data API read access), say so explicitly and ask the user to confirm in the platform's own UI rather than asserting a conclusion.
