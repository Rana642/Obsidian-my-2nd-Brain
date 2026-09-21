---
name: adsbyshoaib-build-status
description: "Build progress on adsbyshoaib.com — Phases 1-3 done, Phase 4 (other pages) is next"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-24T07:14:16.753Z
---

**STALE — kept for history only.** As of 2026-08-24 the site is live with real content
throughout, the Sanity CMS and the Supabase business dashboard are both shipped (see
[[adsbyshoaib-cms-architecture]] and [[adsbyshoaib-dashboard]]), case studies were
rewritten with real client data, and an aesthetic/visual-hierarchy audit fixed several
bugs (see [[adsbyshoaib-aesthetic-audit-2026-08-24]]). Everything below this line
describes the 2026-08-21/22 state only.

As of 2026-08-21, adsbyshoaib.com build status (repo: https://github.com/Rana642/Shoaib-Portfolio-Website, app at repo root, Next.js 16.3.2 + Tailwind v4, tokens in `app/globals.css` @theme):

- **Done:** Phase 1 (foundation), Phase 2 (layout/UI kit), Phase 3 (12-section home page, approved by Shoaib), Phase 4 (all inner pages: Services + 4 details, About, Case Studies via MDX pipeline with 5 placeholder studies, Blog with placeholder data module shaped like Sanity schema, Resume with [Placeholder] CV entries + /cv redirect, Contact with RHF+Zod form and stub /api/contact). Pages live under `app/(marketing)/` route group so /studio and /api stay outside nav/footer.
- **Done Phase 5 (on Sonnet 5, per Shoaib's switch):** Sanity Studio embedded at `/studio`, `post` schema, `lib/sanity/*` client+queries, `lib/posts.ts` now queries Sanity live with graceful fallback to 2 placeholder posts until Shoaib creates a project. Hit two real build issues, both fixed: (1) importing NextStudio into the RSC graph trips a Turbopack/`swr` export-condition conflict in the `sanity` package — fixed by making the studio page client-only with `next/dynamic({ssr:false})`; (2) `@sanity/sdk-react` ships untranspiled JSX in its dist — fixed via `transpilePackages: ["@sanity/sdk-react"]` in next.config.ts. Don't remove that config without retesting `npm run build`.
- **Done Phase 6 (Sonnet 5):** per-page metadata via `lib/seo.ts` pageMetadata() (title/description/canonical/OG/twitter), JSON-LD via `lib/schema.ts` (Organization site-wide, Person on About+Resume, Service on Services pages, Article on blog+case studies, FAQPage on home+blog posts, BreadcrumbList everywhere), `app/sitemap.ts` + `app/robots.ts`, dynamic OG images at `app/api/og/route.tsx` (next/og, nodejs runtime — edge is deprecated in this Next version).
- **Done Phase 7 (Sonnet 5):** Supabase (`lib/supabase.ts`), Resend (`lib/resend.ts` + `lib/email-templates.ts`), Meta CAPI (`lib/meta-capi.ts`), GA4/GTM/Pixel (`components/shared/Analytics.tsx`), Vercel Analytics, Calendly (`components/shared/CalendlyButton.tsx`) — all gated on env vars, same graceful-fallback pattern as Sanity. `/api/contact` and `/api/newsletter` wired to Supabase + Resend + CAPI.
- **Indexing BLOCKED on purpose (2026-08-22, per Shoaib):** `SITE_IS_LIVE = false` in `lib/seo.ts` — every page renders `noindex, nofollow` and `/robots.txt` disallows everything. Shoaib wants correct info to hit the internet exactly once, not while copy/case-studies/testimonials are still placeholders. **Do not flip `SITE_IS_LIVE` to true without Shoaib explicitly confirming the site is final** — that single line re-enables indexing site-wide.
- **Partial Phase 9 done:** branded 404 (`app/not-found.tsx`), Privacy/Terms pages (fixed 2 pre-existing 404 links in Footer), branded favicon (`app/icon.tsx`/`apple-icon.tsx`), a11y pass (verified via browser: no mobile horizontal scroll, 44x44 hamburger touch target, all images have alt text, one h1/page, no unlabeled buttons).
- **Live infrastructure confirmed working end-to-end (2026-08-22):** Vercel deploy + domain attached (adsbyshoaib.com, nameservers now ns1/ns2.vercel-dns.com — DNS for this domain must be managed in Vercel's dashboard, NOT Hostinger, which is inactive for this domain despite still showing the zone). Supabase inserts confirmed on production (verified via direct REST query with the service_role key). Resend confirmed sending on production after domain verification (DKIM + SPF CNAMEs `rsend`/`send` added in Vercel DNS) — both notification and auto-reply emails verified "delivered" via Resend's API.
- **Resume page updated with real CV data (2026-08-22)** — see [[adsbyshoaib-resume-data]]. Work experience (4 jobs + 6 remote projects) still pending from Shoaib.
- **Next: Phase 8** content (real case-study data, blog posts), then finish 9 polish, 10 deploy (which includes flipping SITE_IS_LIVE once Shoaib signs off).
- **Accounts Shoaib has created (2026-08-22):** Vercel, Supabase, Resend. Supabase project ref `ugbahdlprhuozcteegtl` → URL `https://ugbahdlprhuozcteegtl.supabase.co`, already in `.env.local` along with the anon/publishable key.
- **Still needed from Shoaib:** `SUPABASE_SERVICE_ROLE_KEY` (secret, from Supabase Settings → API → Reveal), the `contacts`/`subscribers` tables created via Supabase SQL Editor (SQL given in chat 2026-08-22), `RESEND_API_KEY`, and Resend domain verification for adsbyshoaib.com (Shoaib chose to verify now rather than use the resend.dev test domain — needs him to add Resend's DNS records at his domain registrar). Also still pending: a real Sanity project (sanity.io — Claude cannot create accounts), real CV data for Resume placeholders, Calendly URL, testimonials, real case-study outcomes, logo, and the 3 final copy files.
- **Copy is DRAFT** in components marked with `DRAFT COPY` comments — Shoaib's 3 final copy files (adsbyshoaib-home-conversion-copy.md etc.) were missing; he approved Claude-drafted copy pending replacement. Placeholders: testimonials, case study outcomes, trust signal certifications.
- Headshot lives at `public/images/shoaib.png` (orange background — pairs with citrus accent, see [[adsbyshoaib-color-swap]]). Use on hero, About, Resume.
- Dev server: `.claude/launch.json` → "adsbyshoaib-dev" (npm run dev, port 3000).
