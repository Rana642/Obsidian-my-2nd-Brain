---
name: socially-snap-saas-plan
description: "Plan for a separate multi-tenant SaaS (\"Socially Snap\") — scope is Planner + Connections + WhatsApp API only; Studio and Knowledge Base stay out of it"
metadata:
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-23T19:56:45.174Z
---

Decided 2026-09-24, refined same day: the social Planner will NOT be moved out of adsbyshoaib.com yet. A new multi-tenant SaaS app (working name Socially Snap, the Meta-verified business) will be built fresh, reusing code patterns (platform registry `lib/social-platforms.ts`, OAuth flows, posting logic, Connections hub). It connects to the adsbyshoaib dashboard by API during the transition. Clients move over one at a time.

**Scope of the SaaS app — kept narrow on purpose:**
- Planner + Connections (FB, IG, LinkedIn, TikTok)
- WhatsApp Business Platform module (inbox, broadcasts, templates, chatbot, AiSensy-style)

**Explicitly NOT part of the Socially Snap SaaS app (Shoaib's call, 2026-09-24):**
- **Graphic Studio** (image/video generation) IS also a SaaS product (not personal-only, correction from earlier same-day note) — but it stays its OWN SEPARATE product at [[graphics-studio-project]], never merged into Socially Snap. Two distinct SaaS products, not one combined app. Keeping them apart protects each one's approvals/reviews from the other.
- **Knowledge Base** (per-client product literature, e.g. Tad Pharma) is the ONLY one of these three that's personal — it stays inside the adsbyshoaib **dashboard**, tied to `client_projects`. Not a SaaS feature at all, not in either SaaS product.

**Why:** pending reviews (Meta Access Verification/App Review, LinkedIn CM API, TikTok verified domain) are tied to adsbyshoaib.com. Moving now would risk them and the live hotel posts. Shoaib wants the SaaS to qualify for partner programs (Meta Tech Provider → Tech Partner, WhatsApp Embedded Signup, LinkedIn, TikTok, Google).

**How to apply:** keep the adsbyshoaib Planner running until approvals land. Build the SaaS narrow (Planner+Connections+WhatsApp only) and partner-program-ready from day one: per-tenant OAuth, webhooks, encrypted tokens, audit logs, privacy/terms/data-deletion pages on the SaaS domain. Do NOT add Studio or Knowledge Base features to it even if it seems convenient later — Shoaib separated these deliberately. Confirm architecture before building. See [[adsbyshoaib-social-poster]], [[graphics-studio-project]], [[shoaib-confirm-ux-before-building]].
