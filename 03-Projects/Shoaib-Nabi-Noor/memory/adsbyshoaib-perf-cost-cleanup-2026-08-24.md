---
name: adsbyshoaib-perf-cost-cleanup-2026-08-24
description: ISR revalidate is now 1h (was 60s) to cut Vercel/Sanity request volume; dead MDX pipeline + unused Card/Container/image-url removed
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-24T09:51:41.041Z
---

Shoaib asked (2026-08-24) to remove unused code, minify, and specifically cut Supabase/Vercel usage so free-tier limits/quotas aren't hit as fast, mobile speed especially. Commit `f2f6100`.

**Biggest lever: `sanityFetch()`'s ISR window (`lib/sanity/client.ts`) is now `revalidate: 3600` (1 hour), was 60s.** At 60s, almost any real visit to a content page could trigger a background regeneration — a fresh Vercel function invocation *and* a fresh Sanity API read — on nearly every request, for a low-traffic portfolio site where content changes rarely. Confirmed in the build output: every Sanity-backed route went from a 1m to a 1h revalidate window. **If Shoaib ever wants Studio edits to reflect faster without going back to a short polling window, the real fix is on-demand ISR revalidation via a Sanity webhook → a Next.js revalidation route — not shortening this number again.**

**Dead code removed** (all zero-reference-confirmed before deleting):
- `@next/mdx`, `@mdx-js/loader`, `@mdx-js/react` — never configured in `next.config.ts`, never imported. Leftover from the original build-doc dependency list that was never actually wired up.
- `gray-matter`, `next-mdx-remote` — only used by the case-studies MDX-file fallback in `lib/case-studies.ts`, which read from `content/case-studies/` — a directory deleted back when case studies moved fully into Sanity (see [[adsbyshoaib-cms-architecture]]). `lib/case-studies.ts` is now Sanity-only, no fallback; the case-study detail page has one render path (PortableText) instead of two.
- `components/ui/Card.tsx`, `components/ui/Container.tsx` — Phase-2 scaffold, zero imports anywhere; real pages hand-style cards/containers inline instead.
- `lib/sanity/image.ts` + the `@sanity/image-url` package — `urlFor()` was never called; GROQ queries already resolve `coverImage` to a plain URL via `.asset->url`.
101 packages removed from node_modules total.

**Checked and already fine, no change needed:** `next/image` usages all have correct `sizes` (no oversized mobile image requests); `Analytics.tsx` already uses `strategy="afterInteractive"` and renders nothing at all today since GA4/GTM/Meta Pixel env vars aren't set yet; `proxy.ts`'s auth middleware matcher is already scoped to `/dashboard/:path*` only, not site-wide; the dashboard's Supabase client (`lib/dashboard/db.ts`) is already a module-level singleton, not recreated per request.

**Note:** `coverImage` is fetched by the case-study/post Sanity queries but never actually rendered anywhere on the frontend yet — left the schema field alone (Shoaib may want it later, it's a real content field), just didn't build display for it since that's a feature addition, not a cleanup.
