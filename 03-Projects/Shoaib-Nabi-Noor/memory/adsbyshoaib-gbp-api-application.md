---
name: adsbyshoaib-gbp-api-application
description: Google Business Profile API access request status — real support case ID and review timeline
metadata: 
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-18T11:56:43.648Z
---

Shoaib first tried applying for Google Business Profile API access on 2026-08-24 (see [[adsbyshoaib-privacy-policy-api-compliance]] for the privacy-policy work done ahead of that). 25 days later (2026-09-18) there was still no response, which looked like it should have been resolved — Google's own confirmation had said ~7 days.

**CORRECTED same session (2026-09-18) — the "never submitted" theory below was wrong, don't repeat it.** Initially, `support.google.com/business/workflow/16726127`'s "Continue where you left off" resumed at only 33% with empty fields, which looked like proof the original application had never gone anywhere — so I had Shoaib fill it out again, reaching a genuine submission (Case ID `8-8588000042022`, "7-10 business days"). But Shoaib then checked Google's own "Recent cases" list (visible under "Need more help?" on support.google.com pages) and found the **real, original case was already there and already "In Progress": Case ID `5-2799000041097`, last updated 4 days before 2026-09-18 (~2026-09-14)** — meaning Google genuinely had been working on it, the 33%-draft form was a stale/unrelated resumption state, not evidence the real request was missing. Submitting a second time via the form was unnecessary and created a redundant duplicate case.

**Correct current status:** the real, active request is **Case ID `5-2799000041097`** — track this one. The duplicate `8-8588000042022` can be ignored (harmless, but not the one that matters). **The generalized lesson stands even though the specific conclusion was wrong: verify against the platform's own case-tracking list (not just a resumable-form's draft state) before concluding an application never went through** — a form showing a stale/incomplete draft does not prove the underlying request doesn't exist elsewhere in the account.

**How to follow up:** don't resubmit again. Just wait/check Case `5-2799000041097`'s status periodically (Business Profile Manager → Support, or Google's "Recent cases" widget). If a future session is asked "GMB/Business Profile API status," check whether Shoaib has heard back before assuming it's still pending.

**Waiting plan set 2026-09-18:** Case `5-2799000041097` had already gone 4 days without an update as of this conversation. Shoaib's explicit instruction: wait **3 more days** (so check back around **2026-09-21**) before doing anything else about it — don't suggest resubmitting, don't suggest alternate paths, just hold. Only revisit sooner if Shoaib brings it up himself or an email/status change arrives.

**Status check 2026-09-29:** both cases still "In Progress" (8-8588000042022 updated ~1 day ago, 5-2799000041097 ~2 days ago — Google touching the cases, not a restart). GCP project "Ads by Shoaib CRM": My Business Account Management API enabled but **Requests per minute quota = 0 → NOT approved yet**. Account Mgmt, Business Information, Verifications APIs enabled; told Shoaib to also enable Google My Business API (v4: reviews/posts/media), Business Profile Performance, Notifications, Place Actions, Lodging. No code uses GBP APIs yet. Approval signal = that quota turning to 300.

**APPROVED 2026-09-29 (evening, email on case 5-2799000041097):** project 875327223530 allowlisted for the Google Business Profile API, default quota 300 QPM. Google's reminder: no public statements/press implying Google partnership or endorsement without written approval. Next: confirm the quota shows 300 in GCP, make sure the v4 Google My Business API + Performance/Notifications APIs are enabled, then build the GBP integration (none exists in code yet).

**Project mismatch found 2026-09-29:** "Ads by Shoaib CRM" is project number 946914911619 (ID ads-by-shoaib-crm) — NOT the approved 875327223530. Its quota staying 0 is expected. The GBP integration must use whichever GCP project is 875327223530 (likely the one from the original Aug application), or Google must be asked (reply on case 5-2799000041097) to move the approval to 946914911619.
**Resolved same day:** the approved project 875327223530 is **"Socially Snap" (project ID socially-snap)** — ties to the Socially Snap GBP website/SaaS ([[sociallysnap-site]], [[socially-snap-saas-plan]]). Build the GBP integration on this project's OAuth client, not ads-by-shoaib-crm.
**Confirmed 2026-09-29:** Socially Snap project shows Requests per minute = 300 on My Business Account Management API — live.
