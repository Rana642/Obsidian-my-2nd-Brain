---
name: adsbyshoaib-privacy-policy-api-compliance
description: "Privacy policy (app/(marketing)/privacy/page.tsx) now discloses Google Business Profile API/OAuth use ahead of Shoaib's API application; only claims things actually implemented"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-24T09:35:27.551Z
---

Shoaib is applying for Google Business Profile API access (2026-08-24) to manage client Business Profiles — posts, reviews, business info — via OAuth 2.0, framed to Google as a freelance/independent consultant (not an "agency," since his site/copy deliberately avoids that framing — see the brand-voice rules in CLAUDE-CODE-INSTRUCTIONS.md). One application covers all his clients via OAuth per-client authorization, not a separate application per client.

**Privacy policy updated** (`app/(marketing)/privacy/page.tsx`, commit `d061bc3`) to carry the disclosure Google's API reviewers check for — including the required phrase "Google API Services User Data Policy, including the Limited Use requirements" verbatim. Also added a "Data sharing" (no-sale) section and a revoke-access line.

**Important lesson:** Shoaib pasted an AI-generated draft privacy policy (from a Google AI Mode conversation) and initially asked to use it directly. It was **not used as-is** — it made false claims: a "LinkedIn Insight Tag" (not implemented anywhere in the codebase — grepped, only a plain LinkedIn profile link exists in the footer), an already-built "internal single-user dashboard" for Google Business Profile data (doesn't exist — the real `/dashboard` is Supabase business-ops only, unrelated), infrastructure claims like "data never exposed to external servers" (false — Vercel/Supabase/Sanity are all external servers), and `contact@adsbyshoaib.com` (the site's real, consistently-used address is `hello@adsbyshoaib.com`). Rewrote the additions from scratch in the site's real first-person voice, checking every claim against actual code (e.g. confirmed via grep that Meta CAPI really does SHA-256-hash emails in `lib/meta-capi.ts`, so that claim stayed). **Rule: never paste externally-drafted legal/compliance copy onto the site verbatim — verify every factual claim against the actual codebase first, same as the no-fabrication rule already in place for case-study outcomes and resume links.**

If Shoaib later adds LinkedIn Ads/Insight Tag or actually ships the Google Business Profile management dashboard, the privacy policy should be revisited — the LinkedIn section was deliberately left out until it's real.

**Restructured into a full 13-section page (2026-08-24, commit `04d52d1`)** — Shoaib shared Buffer's real "Policies and procedures" page (screenshots) as a structural reference and asked for something equally professional/complete "hamary mutabiq" (in our own voice/scope). Used it purely for structure, not content — did NOT copy Buffer's actual legal text (referral partner program, Delaware corporate law, DPA/sub-processor agreements are all irrelevant to a solo practice with no registered company entity). Added: a Table-of-Contents card with anchor links (`Reveal` now takes an optional `id` prop for this), a proper Service/Purpose/Data table for sub-processors, GDPR legal-basis section, data security, international transfers, and children's privacy. Same rule applies going forward: reference-only, verify-every-claim, never paste external legal copy wholesale.
