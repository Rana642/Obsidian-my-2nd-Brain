---
name: adsbyshoaib-client-portal
description: "Client portal (/portal) — Owner + team roles and features; since 2026-09-30 it uses the dashboard's glass shell and the Planner calendar in client mode"
metadata:
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-30T05:53:52.985Z
---

`/portal` is the client-facing area (separate login, `proxy.ts` guard, `lib/portal/auth.ts` scopes everything to the client). Features per client (`clients.portal_features`): intakes, planner, uploads, credentials, reports (coming soon), team. Owners get the client's features; an Owner with "team" can add members with a subset, optionally limited to some projects (`lib/portal/features.ts`).

**Redesign 2026-09-30 (commit c8b1b9d, Shoaib chose option "b": Owner and team both get it):**
- `components/portal/PortalShell.tsx` renders the portal inside `DashboardShell` (`portal` prop): same glass sidebar, "Client portal" caption, own nav (Home, Planner, Intake forms, Team for Owners), sign-out → /portal/login, no PWA, separate collapse key. Old `PortalNav`/`SignOutButton` deleted.
- `/portal/planner` uses `PlannerCalendar` with `mode="client"`: Week/Month calendar, client status labels (Being prepared / Scheduled / Posted; failed hidden), "+ Add" only with the uploads feature (opens `PlannerUploader` in a modal, with a note), no drag/bulk/captions, can remove only their own still-pending uploads (`deletePortalUpload`).

**Sidebar (same commit):** `components/dashboard/Sidebar.tsx` takes `nav`, `homeHref`, `areaLabel`, `loginHref`, `showStudio`. Nav items can be `{ section: "..." }` headings. The dashboard is grouped Overview / Sales / Clients & billing / Marketing (Planner, Insights, Meta Ads, Google Business, Connections) / Admin, and the nav scrolls on its own (`.sidebar-scroll` in globals.css) with the sign-out pinned.

**Why:** Shoaib wanted clients and their team to see the same UI as his dashboard. **How to apply:** new portal pages go under `app/portal/(app)/` and get a nav entry in `PortalShell` gated by a feature; keep the client-mode restrictions in PlannerCalendar (admin server actions check `getAdminUser` anyway). Related: [[adsbyshoaib-dashboard-glass-ui]], [[adsbyshoaib-client-intake]].
