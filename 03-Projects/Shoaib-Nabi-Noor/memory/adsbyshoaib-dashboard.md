---
name: adsbyshoaib-dashboard
description: "Business operations dashboard at /dashboard — Supabase-backed clients, catalog, quotations, invoices; separate from Sanity which keeps website content"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-27T11:12:13.263Z
---

Shoaib's vision for "one dashboard" (2026-08-22) is a **business operations tool**, not a content CMS — quotations, branded invoices, a priced services catalog, and a backend that other apps on subdomains will connect to later. Claude initially built only a Sanity CMS after he said "sab kuch ek jagah"; he pushed back, correctly. **Lesson: when he says "everything in one place," ask what operations he runs, not just what content he edits.**

**Split of responsibilities (his decision, refined by Claude's recommendation):**
- **Sanity** (`/studio`): blog posts, case studies, website Services page copy, FAQs — anything that is a public page with long-form text, images, and SEO value. Case studies specifically stay because they're structurally identical to blog posts; moving them would mean rebuilding the rich-text + image editor that keeping blogs in Sanity was meant to avoid.
- **Dashboard** (`/dashboard`): clients, services catalog (priced line items — distinct from the website's Services pages despite the shared name), quotations, invoices, payments, leads, settings.
- Sidebar links out to Sanity so there's one entry point.

**Key design decisions to preserve:**
- Document numbering (`INV-2026-001`) uses the `next_document_number` Postgres function with an atomic upsert — never generate numbers in JS.
- `tax_enabled`, `tax_rate`, and `currency` are **snapshots stored per document**. Changing Settings must never alter an already-sent document's totals.
- RLS is enabled with **no policies**; all access is server-side with `service_role` behind auth. `lib/dashboard/db.ts` imports `server-only` to prevent client bundling.
- Every server action re-checks auth — middleware (`proxy.ts`) alone doesn't protect server actions.
- `amount_paid` is recomputed from the `payments` table, never incremented.
- PDFs are browser print-to-PDF via print CSS on `DocumentPreview`, not a separate export path.

**Setup completed 2026-08-22:** schema run in Supabase, Shoaib created his own login user (Claude must never create accounts or handle passwords). Verified end-to-end via the service-role REST API: all 9 tables present, settings seeded, numbering function correct sequentially AND under 5 parallel calls (5 distinct numbers, no collision), full client → quotation → invoice → partial payment flow with relationships intact, and the FK restriction correctly refusing to delete a client that has documents (error 23503, which `deleteClient` translates into plain language). All test data was cleaned up afterwards — the tables are empty. Production `/dashboard` redirects to login correctly and carries `noindex`.

Logo is the current citrus-dot mark; he'll supply a final one later. See [[adsbyshoaib-cms-architecture]] and [[adsbyshoaib-build-status]].

The dashboard later grew a Proposal -> Agreement -> Onboarding
client-capturing funnel with tiered catalog pricing — see
[[adsbyshoaib-proposal-agreement-funnel]] for that architecture.

**Service bundles (2026-08-27):** `catalog_items.is_bundle` + join table
`catalog_bundle_members` lets a catalog entry package several existing
services under one name and one package price (its own field, not a sum
of members' rates). A bundle is still just a `catalog_items` row, so it
drops into every existing "fill from catalog" dropdown (Proposals,
Quotations, Invoices) with no changes to line-item storage or totals —
only the dropdown groups Services vs Bundles via `<optgroup>`, and
picking a bundle auto-fills the description with what's included
(`lib/dashboard/proposals.ts` and `documents.ts`'s `getProposalFormData`/
`getDocumentFormData` compute a `bundleMembers: Record<id, string[]>` map
passed down for this). A bundle can't include another bundle (enforced
in the app, not the DB). `CatalogForm.tsx`'s member checklist and price
field are controlled state (not defaultChecked/defaultValue) so a live
summary can show "combined total if bought separately" vs the bundle
price plus the savings, updating as services are checked/unchecked.

Catalog list was first split into two sections on one page, then Shoaib
asked for that to go further — now it's two *separate pages*:
`/dashboard/catalog` (Single Services) and `/dashboard/catalog/bundles`
(Bundle Services), sharing `CatalogTable.tsx` and a
`getCatalogSplitByBundle()` helper (`lib/dashboard/catalog.ts`).
Sidebar's "Services Catalog" is the first (only) expandable nav group in
`Sidebar.tsx` — a `NavItem` discriminated union (`{href,...} |
{label,icon,children}`) with a chevron-toggle button instead of a Link;
auto-expands whenever the route is under `/dashboard/catalog`. "Add
bundle" pre-checks the bundle toggle on the shared create form via
`/dashboard/catalog/new?type=bundle`; create/update/cancel/back links all
route back to whichever list an item actually belongs to. The Bundle
Services list itself also shows the strikethrough combined-services
total next to the bundle's own rate (same comparison as the create/edit
form's live summary) — `getCatalogSplitByBundle()` returns a
`bundleTotals: Map<bundleId, number>` alongside `membersByBundle` for
this.
