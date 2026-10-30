---
name: adsbyshoaib-hostinger-partner
description: "Hostinger Partner integration on adsbyshoaib.com — badge placements, floating widget, coupon SEO page, and the referral details"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-31T06:53:30.888Z
---

Shoaib became a verified **Hostinger Partner** (2026-08-28) and wanted the
badge on every trust surface, a coupon SEO page, and a floating widget.

**Referral details (single source: `lib/hostinger.ts`):**
- Coupon code: **NAWAL20** (clients save 20%)
- Referral link: `https://www.hostinger.com/pk?REFERRALCODE=NAWAL20`
- Official badge PNGs live in `public/png/` (Hostinger's download — 16
  variants with a `×` char in the horizontal filenames). The two used are
  copied to clean ASCII names: `hp-badge-dark.png` (purple pill, 320×120)
  and `hp-badge-sq-dark.png` (square). The purple "brand_dark" badge is
  self-contained so it reads on both light and dark backgrounds.

**Hostinger badge rules (from their dashboard modal — must follow):** use
the badge UNMODIFIED (no recolour/resize/retext), never on a cluttered
background, don't combine with other logos, stop using it if partner status
lapses. So I always render the official PNG as-is on its own clean bg —
never recreate it as an on-brand SVG.

**What shipped:**
- `components/shared/HostingerPartnerBadge.tsx` — reusable badge, official
  PNG unmodified, links to the referral with `rel="sponsored"` (affiliate).
  Placed on: `TrustSignals` section, `Footer` (brand column), `About` page
  (after practical bits), the `/shoaib-nabi-noor` resume (Certifications),
  and — via `asLink={false}` (plain credential image, NO referral link, so a
  client's own document isn't an affiliate funnel) — the footer of
  `ProposalPreview` and the end of `AgreementBody` (both client-facing docs
  Hostinger's rules explicitly allow: proposals/presentations).
- `components/shared/HostingerFloatingBadge.tsx` — quiet, dismissible
  floating widget on the **public marketing site only** (added in
  `app/(marketing)/layout.tsx`, not the dashboard). Collapsed = square badge
  chip; expanded = card with coupon + "Get the deal" → the coupon page.
  Collapse state in localStorage; hidden on `/hostinger-coupon` itself;
  mounts after a 900ms delay.
- `app/(marketing)/hostinger-coupon/page.tsx` — dedicated SEO landing page
  targeting "Hostinger coupon" intent: coupon box (`CouponCode.tsx`
  click-to-copy), what's-included, plan guidance, how-to steps, FAQ with
  `faqPageSchema` JSON-LD, affiliate disclosure. Added to `sitemap.ts`.
  Deliberately NO fabricated prices (they change) — 20% + plan types only.
- `components/ui/Button.tsx` gained an `external` prop (renders a plain
  `<a target="_blank" rel="sponsored noopener noreferrer">`) for the
  affiliate CTAs.
- **Sanity blog post** (slug `hostinger-coupon-code`, category "Funnels &
  Web", 4 FAQs) published via a one-off `createOrReplace` script with fixed
  `_id: "post-hostinger-coupon"` (idempotent + deletable), links back to the
  coupon page. The blog PortableText renderer has no `link`-mark component,
  so the body uses plain paragraphs/h2/bullets and cites the page URL as
  text, not a clickable mark. Script deleted after running (post now editable
  in Studio) — same lifecycle as the old migrate-to-sanity script.

**Untracked asset folders (checked 2026-08-28):** `public/png`, `public/svg`,
`public/webp` were all untracked in git — but they contain ONLY the Hostinger
badge download bundle (png/svg/webp of the same badges), nothing else. The
only `/png|/svg|/webp` references anywhere in the code are the two badge PNGs
in `lib/hostinger.ts`, both committed. So nothing is missing in production;
the other ~30 badge files were deliberately NOT committed (unused bloat) and
just sit untracked locally.

**Gotcha reused:** lazy-loaded `<img>` (next/image default) never fires load
in the offscreen preview pane, so badges showed `naturalWidth: 0` there —
but the optimizer served them 200 and a fresh `new Image()` loaded at w=320,
so it's the same non-composited-pane artifact, not a real bug. See
[[adsbyshoaib-dashboard-glass-ui]] and [[adsbyshoaib-verify-via-local-dev-server]].
