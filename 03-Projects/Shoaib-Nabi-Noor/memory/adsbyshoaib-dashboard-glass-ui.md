---
name: adsbyshoaib-dashboard-glass-ui
description: "Dashboard glassmorphism + collapsible sidebar architecture, and two Tailwind-v4/browser CSS gotchas that will recur"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-27T12:54:21.077Z
---

Shipped 2026-08-27. Shoaib asked for a glass UI, a collapsible sidebar,
and a leads fix. **Glass is scoped to the dashboard only** — the public
brand site's design system is locked (see [[adsbyshoaib-color-swap]] /
CLAUDE.md), so glass there would violate the brand. Confirmed scope by
proposing it; when Shoaib didn't answer, went with dashboard-only.

**Glass architecture:**
- `DashboardShell.tsx` (new client component) wraps the whole authed area:
  owns the sidebar's collapsed state (localStorage `dashboard-sidebar-collapsed`,
  restored in an effect after mount to avoid SSR hydration mismatch),
  renders an ambient colour wash (3 blurred citrus/cobalt blobs, fixed,
  `z-0`, `pointer-events-none`, reusing the existing `ambient-blob*`
  keyframes) behind everything so the glass has something to frost over,
  and drives the `<main>` left-padding from the same collapsed state. The
  server `layout.tsx` just renders `<DashboardShell email>{children}</DashboardShell>`.
- `.glass-surface` (light) / `.glass-dark` (sidebar) in `globals.css`
  carry only the translucent fill / border / shadow. `Card` gained a
  `variant?: "glass" | "solid"` prop, **default "glass"** — every dashboard
  card is glass automatically. Pass `variant="solid"` on client-facing
  surfaces with no ambient backdrop: currently just `OnboardingIntakeForm`
  (public onboarding page). Document previews (ProposalPreview, public
  agreement/proposal pages) use their own `bg-white` divs, not `Card`, so
  they're unaffected.
- **Print guard**: `@media print { .glass-surface { background:#fff!important;
  backdrop-filter:none!important; ... } }` — a translucent blurred panel
  prints muddy grey, unprofessional on a client PDF. The `!important`
  overrides the Tailwind backdrop-blur utility sitting on the same element.

**GOTCHA 1 — Tailwind v4 / Lightning CSS strips raw `backdrop-filter`:**
A `backdrop-filter: blur(16px)` declaration written in custom CSS (inside
OR outside `@layer utilities`) is silently DROPPED from the compiled
stylesheet — the `.glass-surface` rule shipped with bg/border/shadow but
no backdrop-filter, so nothing frosted. Fix: don't write raw
backdrop-filter; apply Tailwind's own `backdrop-blur-xl backdrop-saturate-150`
utility classes on each element (Card glass variant + the sidebar
surfaces). Tailwind keeps its own generated backdrop utilities. Verified
via `getComputedStyle(el).backdropFilter` === "blur(24px) saturate(1.5)".

**GOTCHA 2 — width/padding transitions to `calc(var(--spacing)*n)` endpoints
stall:** `w-64`/`w-20`/`lg:pl-64` compile to `calc(var(--spacing)*n)`. A
`transition-[width]` (or padding) animating between two such calc()/var()
endpoints can freeze at the START value in some browsers — the sidebar
stayed 256px instead of collapsing to 80px. Fix: use literal-rem arbitrary
classes for animated dimensions — `w-[16rem]`/`w-[5rem]`,
`lg:pl-[16rem]`/`lg:pl-[5rem]` — which compile to plain rem and interpolate
fine. (Separately, an OFFSCREEN preview pane doesn't composite frames, so
CSS transitions don't visually advance there at all — verify end-states
with `el.style.transition='none'` then measure; don't trust
mid-transition readings. Real displayed browsers animate normally.)

**Collapsible sidebar:** `Sidebar.tsx` takes `collapsed` + `onToggle`.
Desktop `<aside>` toggles `w-[16rem]`↔`w-[5rem]`; a `renderNav(rail)`
helper (mobile drawer passes `rail=false`, desktop passes `collapsed`)
hides labels, centres icons, shrinks the wordmark to "a.", shows a
`PanelLeftClose`/`PanelLeftOpen` toggle, and adds `title=` tooltips in
rail mode. The Services Catalog dropdown can't render legibly in the
rail, so collapsed it becomes a single icon linking to
`/dashboard/catalog`. Sidebar surfaces are `glass-dark` +
`backdrop-blur-xl`.

**Print: keep the investment summary on one page (2026-08-27):** Shoaib's
2-project proposal printed with the charges splitting across a page break.
Fix: wrap the whole `ChargesBreakdown` (all projects + tools + totals) in
`ProposalPreview.tsx` in a `print:break-inside-avoid` div so it relocates
as one unit to the next page rather than splitting — but the summary was
~979px vs a ~986px printable A4 page (20/16mm margins), razor-thin, so a
`@media print { td,th { padding-top/bottom: 0.5rem !important } }` rule
compacts the itemized rows (~72px saved → 80px headroom). `DocumentPreview`
(quotations/invoices) got the same on its totals block. Also bumped the
`@page` top margin to 20mm so the document header clears the browser's own
print header, and `leading-none` → `leading-tight` on the header number so
tall italic-serif glyphs don't clip. The browser's print-header
date/title/url is still a print-dialog setting ("Headers and footers") the
user turns off — not fixable from CSS.

**Leads section fix:** There was no code bug — the page reads contact-form
submissions (`contacts`) + footer newsletter signups (`subscribers`) and
renders them fine. It only *looked* broken because it was full of leftover
Aug-22 dev/test rows (table-test@example.com, "Redeploy Check 2", etc.)
with no way to remove them. Added `deleteContact`/`deleteSubscriber` in
`lib/dashboard/actions/leads.ts` and wired two-click `DeleteButton`s onto
each contact card + a new actions column in the subscribers table
(per-item inline `"use server"` closures over the row id, same pattern as
the proposals page). Then deleted the 4 test contacts + 1 test subscriber
from live DB — section is now an empty slate for real leads. See
[[adsbyshoaib-dashboard]] and [[adsbyshoaib-verify-via-local-dev-server]].
