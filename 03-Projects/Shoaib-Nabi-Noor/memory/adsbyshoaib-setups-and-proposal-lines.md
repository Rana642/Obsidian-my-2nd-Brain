---
name: adsbyshoaib-setups-and-proposal-lines
description: "Home-PC work 2026-09-28 (f1b8f2b..de80e8a) — 13 single \"setup\" services (site /setups + catalog), keyword/solution pages, and proposal/quotation/invoice line features (post-count stepper, complimentary lines, duplicate project, multi-add)"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-28T11:21:51.956Z
---

Built on the home PC on 2026-09-28 and pushed as 21 commits (f1b8f2b..de80e8a). This PC learned about it afterwards from the git log.

**Single setup services (fixed scope, one-time):**
- Website data is in `lib/setups.ts`, shown on `/setups` and `/setups/[slug]`. Keyword titles were checked in Keyword Planner.
- `price` and `delivery` are set by Shoaib only. When one is null, the page says it is confirmed per project. Never invent a number.
- The order form is `components/forms/SetupOrderForm.tsx`. It posts to `/api/contact` with `setup`, and the lead is labelled "Setup order: <name>".
- The same services are `catalog_items` rows (sort 200–275, `unit` project, `billing_type` one_time). They were added directly in the DB, so they are not in code.
- The social setups (Facebook Page, Instagram, LinkedIn Page, TikTok, YouTube) are all PKR 2,911.62. Shoaib's words for the reason: "filhal Facebook wali" (for now, the same as the Facebook one).
- Other prices: Pixel+CAPI, GA4+GTM and Meta Verified are PKR 5,000. GBP, Google Ads, Meta BM, Merchant Center and Landing-page copy are PKR 15,000.
- Scope for the social setups came from Fiverr gig copy. The seller's own claims ("7 years / 700+ projects") were deliberately left out.

**Other marketing pages:**
- `/solutions/[slug]`: problem-first pages, data in `lib/solutions.ts`.
- `/hotel-marketing` and `/google-business-profile-management`.
- `lib/service-seo.ts`, plus a home hero with a cutout portrait.
- Placeholder testimonials were removed.

**Proposal, quotation and invoice lines:**
- **Post-count stepper:** `catalog_items.count_label` / `count_default` (Social Media Management and Post Design are 16 posts).
  - The count lives in the line's description text; see `lib/dashboard/countable.ts` and `CountStepper`.
  - On proposals, Qty stays 1 and the rate stays at the catalog price, because deals are settled with the total discount. On quotations and invoices the rate stays editable.
- **Complimentary lines:** `is_complimentary` on proposal, quotation and invoice items.
  - The value is shown struck through and never charged; `calculateTotals` skips these lines.
  - They appear in a separate table after the totals, and the flag survives quotation→invoice conversion.
- **Proposal editing:**
  - Duplicate a project with all its lines, or use "Duplicate as…" one of the client's saved projects.
  - "Add services" multi-picker from the catalog, and drag-to-reorder lines.
- Hostinger was removed from proposal and agreement documents.
- The SQL for all of this was verified as run on 2026-09-28.

See [[adsbyshoaib-proposal-agreement-funnel]], [[adsbyshoaib-dashboard]] and [[shoaib-two-pc-workflow]].

**Retainer, complimentary and dates (a8c4b92, 2026-09-28, this PC).** Shoaib's rules:
- Complimentary services are shown inside each project, not in a separate section at the bottom.
- Tools and subscriptions are "client ki marzi" (the client's choice): optional, and never part of the deal.

What was built:
- `calculateTotals` now also returns `servicesTotal`, `monthlyTotal` and `oneTimeTotal`. Discount and tax are prorated by subtotal share.
- `proposals.total` is stored as services only. A re-base SQL was given.
- `ChargesBreakdown` recomputes the totals from the lines and shows:
  - Monthly Retainer (big), then One-time.
  - A dashed "Optional: Tools & Subscriptions" line.
- The agreement Fees clause (`buildAgreementClauses` `fees:{monthly,oneTime,tools,toolsMonthly}`) says the tools are optional. This applies to NEW agreements only.
- `proposal_date` / `valid_until` are editable in the form. A null `proposal_date` falls back to `created_at`.
  - Saves retry without the new columns if they are missing (PGRST204).
  - The list now selects `*` for the same reason.
- Offline accept/sign takes a date through `ConfirmActionButton` `datePicker`.
  - `SignedDateEditor` lets Shoaib change the date later, but only for OFFLINE signatures (`lib/dashboard/offline-dates.ts` `isOfflineSignature`).
  - Online signatures with an IP are never edited.
