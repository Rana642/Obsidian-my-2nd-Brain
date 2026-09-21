---
name: adsbyshoaib-no-indexing-until-final
description: Never enable search engine indexing for adsbyshoaib.com until Shoaib explicitly confirms the site is final
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-22T07:19:09.605Z
---

Do not let adsbyshoaib.com become indexable by search engines until Shoaib explicitly says the site is final. This is enforced via `SITE_IS_LIVE = false` in `lib/seo.ts`, which drives both per-page `noindex, nofollow` meta tags (via `pageMetadata()` in the same file, used by every page) and `Disallow: /` in `app/robots.ts`.

**Why:** Shoaib's instruction (2026-08-22): correct information should hit the internet exactly once, not iterate in public while copy, case studies, and testimonials are still placeholders/drafts. Getting indexed with draft content risks Google caching outdated info that's hard to fully scrub later.

**How to apply:** Never flip `SITE_IS_LIVE` to `true` unless Shoaib explicitly confirms readiness — treat this the same as any other destructive/hard-to-reverse action requiring explicit go-ahead, even though it's a one-line code change. This applies even during deploy/launch work (Phase 10) — flipping it should be a deliberate, separate step Shoaib signs off on, not a side effect of "let's deploy now." See [[adsbyshoaib-build-status]].
