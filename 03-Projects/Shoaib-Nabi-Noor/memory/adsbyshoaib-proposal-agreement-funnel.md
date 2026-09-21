---
name: adsbyshoaib-proposal-agreement-funnel
description: "Proposal -> Agreement -> Onboarding client-capturing funnel in the dashboard — shared accept/sign cascades, manual/offline controls, tiered catalog pricing"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-27T12:04:18.315Z
---

Built 2026-08-24 to 2026-08-27. Shoaib's positioning is "solution-based," not
fixed packages — Proposals carry narrative sections (situation, proposed
solution, scope of work) plus itemized investment, not just a quote.

**Funnel shape:** Proposal (dashboard, sent to prospect) -> prospect accepts
online at `/proposal/[token]` -> Agreement auto-generates and is emailed ->
prospect signs online at `/agreement/[token]` -> onboarding intake invite
fires -> client fills structured intake at `/onboarding/[token]`. These
public pages deliberately live outside `/dashboard` so `proxy.ts`'s auth
matcher never gates them.

**Shared cascade extraction (2026-08-27):** `performProposalAcceptance()`
(`lib/dashboard/proposal-acceptance.ts`) and `performAgreementSigning()`
(`lib/dashboard/agreement-signing.ts`) hold the actual side-effect logic
(client creation, agreement generation via `buildAgreementContent()`,
onboarding intake creation, emails). Both the public token-gated actions
(`proposal-public.ts`, `agreement-public.ts`) and the dashboard-authed
manual actions call these — never duplicate this logic inline again.

**Manual/offline controls added because Shoaib explicitly wanted to run
this himself for clients who don't engage over email:**
- Proposal detail page: "Mark accepted (confirmed offline)" — two-click
  `ConfirmActionButton`, calls `markProposalAccepted()` which runs the
  same cascade as the online accept (agreement generated as `sent`,
  emailed).
- Agreement detail page: "Mark signed (confirmed offline)" — same
  pattern, `markAgreementSigned()`.
- `/dashboard/agreements/new` + `AgreementForm.tsx`: pick any
  not-yet-accepted Proposal, generates the Agreement as `status: "draft"`
  with **no email sent** (`performProposalAcceptance(..., {agreementStatus:
  "draft", sendEmail: false})`) so Shoaib can review/edit delivery before
  sending it himself via the "Send to client" button on
  `AgreementActions.tsx` (which promotes draft -> sent on first send).
- Agreements table's `proposal_id` is `not null` — there is no such thing
  as an Agreement without a backing Proposal in this schema. A fully
  freeform agreement (no proposal at all) was considered and rejected;
  create the Proposal first (fast, has default content) then generate the
  Agreement from it.

**Pricing model (revised 2026-08-27, same day it first shipped):** first
built as catalog-level tiered pricing (`catalog_items.discounted_rate`,
"Standard Rate" / "Your Rate" per item) — Shoaib rejected it immediately:
"Your Rate Catalog mai add kerny ki zarorat nai hai... Total Prices per
discount add ker k Flexible bana do." **Lesson: don't assume a per-item
second price tier is what "flexible pricing for clients" means — ask
whether the flexibility belongs on the catalog or on the document
total.** Reverted same-session: `discounted_rate` column dropped
entirely, Catalog is single `default_rate` ("Standard Rate," Shoaib's own
reference figure) again.

In its place: `discount_enabled` / `discount_type` ('percentage'|'fixed')
/ `discount_value` / `discount_amount` on **Proposals and Quotations only**
(not Invoices, not Catalog). `calculateTotals()` in `lib/dashboard/
format.ts` now takes an optional `Discount` param and applies it
subtotal -> discount -> tax -> total (tax charged on the post-discount
amount). Invoices need no changes — `convertQuotationToInvoice` already
just copies the quote's final (discount-inclusive) `total`/`subtotal`/
`tax_amount` verbatim. Agreements likewise need no changes — the fee
quoted in `buildAgreementContent()` comes straight from `proposal.total`,
which already reflects any discount. DocumentForm.tsx is shared between
quotation/invoice via a `kind` prop; the discount UI and the DB payload's
discount fields are both gated to `kind === "quotation"` since the
`invoices` table has no discount columns at all.

The 11 real Core Services (Social Media Management, Brand Identity &
Handling, Content Writing, Meta Business Setup, Tracking & Conversion
Setup, Google & Meta Basic SEO Setup, and Media Buying split per-platform
across Google/Meta/YouTube/TikTok/LinkedIn) are seeded in the live
catalog with `default_rate` at 0 — Shoaib sets the Standard Rate himself
per service, and negotiates per client via the discount field on the
actual document, not via catalog tiers.

**Monthly Retainer vs One-Time/Fixed Cost (2026-08-27):** Proposal's
"Investment" section renamed "Service Charges"; each `proposal_items` row
now carries `billing_type` ('monthly'|'one_time'), inferred from the
catalog item's `unit` when filled from catalog (`unit === "month"` ->
monthly) but editable per-line. `ProposalPreview.tsx` groups items into
two labeled subsections with their own mini-subtotals when both types are
present (single combined Subtotal/Discount/Tax/Total block below,
unchanged — billing_type is presentational only, doesn't split the
actual maths). Quotations/Invoices don't have this field — scoped to
Proposals only, matching where Shoaib actually asked for it.

**Tools & Subscriptions + international transaction tax (2026-08-27,
same session):** `proposal_items.item_type` ('service'|'tool') is a
second, orthogonal axis to `billing_type` — a line can be a monthly-tool
or one-time-tool in principle, though the Tools UI in `ProposalForm.tsx`
doesn't expose billing_type for tool rows (defaults to 'monthly', not
shown). Tools get their own card in the form and their own section in
`ProposalPreview.tsx`/the public page, entirely separate from Service
Charges (no catalog-fill — catalog is Shoaib's own services, not
third-party tools). `proposals.tools_tax_enabled` / `tools_tax_rate`
(editable, defaults 18) / `tools_tax_amount` model the international
transaction surcharge Pakistani accounts get charged on foreign-currency
card purchases (Claude Pro, Canva, ad tools, etc.) — computed in
`calculateTotals()` on the **tools subtotal only**, added after tax, not
discountable, always shown with an on-document disclaimer that it's an
estimate. Verified live: PKR 30k service + PKR 7k tool -> 18% of 7k =
1,260 tax, total 38,260 — confirms it isn't accidentally applied to the
whole document.

**Revised again same day (2026-08-27):** Shoaib clarified the tools tax
must never blend into GST — `calculateTotals()` in `format.ts` now keeps
Discount and GST scoped to `subtotal` (services only, renamed in meaning
from "everything" to "services"), while tools get their own fully
separate `toolsSubtotal` -> `toolsTaxAmount` -> `toolsTotal` (inclusive)
that's added straight into `total` untouched by discount/GST. Zero schema
changes needed — `proposals.subtotal`/`tax_amount` already meant "the
right thing" once the function changed; old rows keep their frozen
pre-fix numbers (same "documents are snapshots" rule as ever).
`ProposalPreview.tsx`/`ProposalForm.tsx` show "Services subtotal" (only
when tools exist) -> GST -> "Tools & Subscriptions" (one inclusive
figure) -> Grand Total. Verified live: 30k service + 7k tool, 15% GST +
18% tools tax -> GST=4,500 (on 30k only), Tools&Subscriptions=8,260
(7k+1,260), Total=42,760.

**Multiple projects/companies per Proposal (2026-08-27, same session):**
One client relationship can cover 2+ distinct projects or even separate
companies. New `proposal_projects` table (id generated client-side via
`crypto.randomUUID()` in `ProposalForm.tsx` so line items can reference
it in the same submission, before it's ever saved — see
`replaceProposalProjects()` in `actions/proposals.ts`, which must run
*before* `replaceProposalItems()` for the FK to resolve). Each project:
name + optional short scope_of_work text. `proposal_items.project_id`
(nullable, `on delete cascade`) tags a line item to a project, or leaves
it "General" (null) if shared across projects. Confirmed explicitly by
Shoaib: pricing is still per-project (sums up project by project into
one combined subtotal) and he still lands on ONE final negotiated total
via the existing discount mechanism — projects are a presentation/
organization layer over line items, not a second pricing engine or a
per-project accept/sign flow. A proposal with zero projects defined
renders exactly as it always has (flat Service Charges/Tools, no
"General" heading) — this is the default for the vast majority of
proposals; multi-project is opt-in via an "Add project" button.
`ProposalPreview.tsx` extracted a `renderCharges(bucketItems, ...)`
helper reused per-project and for the flat/general case, rather than
duplicating the Service Charges + Tools table JSX three times. **Forgot
`alter table proposal_projects enable row level security;` in the first
migration draft** — Supabase's SQL editor flagged it before running;
every dashboard table needs this line (RLS enabled, zero policies,
service-role-only access is the whole security model) — always include
it for any brand-new table, don't rely on catching it after the fact.

**Emails are opt-in for offline confirmation (2026-08-27):**
`markProposalAccepted(id, sendEmail)` and `markAgreementSigned(id,
sendEmail)` now take an explicit `sendEmail: boolean` from a checkbox on
`ConfirmActionButton` (default unchecked) — Shoaib may be sharing the
resulting link himself over WhatsApp (new `WhatsAppShareLink.tsx`, a
`wa.me` link next to Proposal/Agreement actions) instead of through
Resend. `performAgreementSigning()` gained the same `options?: {
sendEmail }` pattern `performProposalAcceptance()` already had. The
ONLINE self-serve accept/sign flow (`proposal-public.ts`/
`agreement-public.ts`) is untouched — it still omits the option and
defaults to `sendEmail: true`, so a prospect who accepts/signs online
still gets their email automatically; only the offline dashboard-side
confirmation changed.

**Client-level Projects (2026-08-27):** New `client_projects` table
(client_id, name, notes) — managed on the Client detail page via
`ClientProjectsManager.tsx`. Fixes an earlier gap: `proposal_projects`
were only ever typed fresh per-proposal, so a repeat client with 2
companies meant retyping "Avenza Restaurant" every time. Now
`getProposalFormData()` returns `clientProjects: Record<clientId,
ClientProject[]>`, and `ProposalForm.tsx` shows the selected client's
saved projects as one-click chips that copy name+notes into a new
`proposal_projects` row (still its own snapshot, not live-linked — same
"documents are frozen" rule as catalog-fill). Copy corrected same day:
a Client is not necessarily "a company, or a person if there's no
company" — it can be a person or entity holding several *unrelated*
companies with no parent among them (Shoaib's real example: Ahmed holds
both Meezabz International and Tad Pharma). `ClientForm.tsx`'s Name
field is now "Client name" with a hint pointing multi-company clients at
Projects instead of cramming a second company into one Name field.

**Nested Service Charges per project (2026-08-27, still same day):**
First shipped Projects with a flat Service Charges list + a per-row
"Project" dropdown (`item.project_id` picked from a `<select>`). Shoaib
found that clunky and asked for the layout first before building it —
confirmed design: each Project block in `ProposalForm.tsx` now has its
own nested "Service Charges" mini-section with its own Add line button;
items get `project_id` implicitly from which project's Add-line button
created them, no dropdown. A separate "General / Shared Charges" card
covers ungrouped items. **Tools & Subscriptions stays flat on purpose**
(Shoaib's call: "jitne projects honge usi hisaab se costing kar loonga 1
hi jagah") — still has its own per-row Project dropdown, unchanged. Pure
UI reorganization in `ProposalForm.tsx` only — `renderServiceRows()` is
a shared row-markup helper reused for both the general list and each
project's nested list; no schema/data-shape change, since `project_id`
already existed on line items and `ProposalPreview.tsx`'s per-project
grouping (built earlier) already reads it correctly regardless of which
UI wrote it.

**"Email right away" checkbox on New Proposal (2026-08-27):** `createProposal`
already didn't auto-email (only the separate "Send to prospect" button
did) — added an explicit unticked-by-default checkbox so Shoaib can
create+send in one step when he wants to, instead of two. Implementation:
`createProposal` just calls the existing `sendProposal(created.id)`
internally when `send_immediately` is checked, after the proposal/
projects/items are all inserted, before its own `redirect()` —
`updateProposal` destructures the field out and ignores it (no
send-on-edit behavior). No changes to `sendProposal()` itself.

**Agreements now show the full pricing breakdown too (2026-08-27):**
Shoaib wanted the same Service Charges/Tools & Subscriptions/Totals
section that appears on the Proposal to also appear "completely" on the
Agreement, and wanted the international-transaction-tax narration living
next to the Tools subtotal it explains everywhere, not as a disconnected
footnote near the grand total. Extracted `ChargesBreakdown.tsx`
(proposal + items + projects in, renders the charges + totals) out of
`ProposalPreview.tsx` so both it and the Agreement pages (public
`/agreement/[token]` and dashboard `agreements/[id]`) render the
identical breakdown under an "Investment Summary" heading, fetched via
`agreement.proposal_id` (Agreements have no line items of their own —
`getAgreementByToken` now also returns `{proposal, items, projects}`).
Tools subtotal is now always shown tax-inclusive with its disclaimer
right there (`"Tools subtotal (incl. est. X% intl. tax): ..."` +
footnote), removed from the old spot next to Grand Total in both the
Preview and `ProposalForm.tsx`'s edit-side totals sidebar. Verified live
end-to-end: accepted a test proposal (30k service + 7k tool, 15% GST +
18% tools tax), confirmed the generated Agreement's Investment Summary
matches the Proposal exactly (Total PKR 42,760) and the legal text's fee
mention agrees with it.

**Catalog-level billing type + bundle value shown in documents
(2026-08-27, later same day):** Shoaib asked that picking a bundle into a
Proposal/Quotation/Invoice "reflect the same way" the Catalog's own
Bundle Services list does (combined member value vs bundle price), and
separately that each Single Service define its Monthly Retainer vs
One-time/Fixed billing type explicitly rather than only being inferred
per-line from `unit`. He explicitly said he wasn't sure bundles should
get that same billing-type field too, worried about complexity — judgment
call made and stated to him before building: **billing_type stays OFF
bundles**, Single Services only. `catalog_items.billing_type`
('monthly'|'one_time', not null) added; `CatalogForm.tsx` hides the field
entirely when `is_bundle` is checked, falling back to
`resolveBillingType()` (infers from `unit === "month"`) so the column
always gets a sensible value for bundles without a second recurrence
concept stacked on the bundle price/members already there.
`ProposalForm.tsx`'s `applyCatalogItem` now sets `billing_type:
source.billing_type` directly (was inferring from `unit` inline before).
For the bundle-value ask: `getProposalFormData()`/`getDocumentFormData()`
now also return `bundleTotals: Record<bundleId, number>` (summed member
rates, computed the same way the Catalog bundles page already did) passed
through `ProposalForm.tsx`/`DocumentForm.tsx` as a `bundleTotals` prop;
picking a bundle now auto-fills its description with "(combined value X,
bundled at Y)" appended — a text-only enhancement, deliberately no new
columns on `proposal_items`/`quotation_items`/`invoice_items`, chosen
specifically because Shoaib flagged complexity risk. Migration (not yet
run live as of this write-up):
```sql
alter table catalog_items add column if not exists billing_type text not null default 'one_time'
  check (billing_type in ('monthly','one_time'));
update catalog_items set billing_type = 'monthly' where unit = 'month';
```
Verified live (read-only, pre-migration): computed the enrichment against
the real "Media Buying Services" bundle and got exactly `"Media Buying
Services — includes: Google Ads, Meta Ads, YouTube Ads, TikTok Ads,
LinkedIn Ads (combined value PKR 50,000.00, bundled at PKR 15,000.00)"`.

**Offline accept locks the deal, doesn't auto-send the Agreement; Agreements
become editable clauses (2026-08-27, later same day):** Shoaib tested "Mark
accepted (confirmed offline)" and found it inconsistent with intent: it
was auto-generating the Agreement as `status: "sent"` immediately (only
the email itself was opt-in via a checkbox), when what he wanted was
"accepting locks the deal, I'll send the Agreement myself when it's
actually needed." Fix: `markProposalAccepted(id)` (dropped the `sendEmail`
param entirely) now always calls `performProposalAcceptance(..., {
agreementStatus: "draft", sendEmail: false })` — same defaults
`createManualAgreement()` already used. The `ConfirmActionButton` on the
Proposal page lost its email checkbox. **Explicitly confirmed scope: online
self-serve accept (`proposal-public.ts`) is untouched** — still
auto-generates `sent` + emails immediately, since there's no one in that
loop to send it manually; this change only applies to the offline
dashboard action.

Separately, Shoaib wanted the Agreement's Investment Summary to sit next
to Fees & Payment "taake relevancy rahe aur professional lage" (instead of
a disconnected block above the whole document), and wanted every clause
editable and addable — "Sary Clause editable bhi hony chaiye aur as per
requirement add hony chaiye," confirmed to mean **always** editable, even
after signing (no lock). `agreements.content text` (was `not null`) is now
nullable; new `agreements.clauses jsonb` holds an array of `{title, body,
showInvestmentSummary?}`. `buildAgreementContent()` → `buildAgreementClauses()`
in `agreement-template.ts` (same wording, split into 12 clause objects —
Preamble, 1–10, Signatures — with "2. Fees & Payment" flagged
`showInvestmentSummary: true`). New shared `components/dashboard/
AgreementBody.tsx` renders either shape: clauses present → loop them,
inserting the Investment Summary right after whichever clause carries the
flag (falls back to the end if none does, e.g. after editing removes it);
clauses null (**legacy agreements, frozen forever**) → exact old
rendering, Investment Summary above the `content` blob, untouched. Takes
a `wrapped` prop (`Card` vs plain `div`) since the dashboard page has no
outer card of its own but the public `/agreement/[token]` page already
wraps everything in one white document card — nesting a second `Card`
inside would double the border/padding.

New `/dashboard/agreements/[id]/edit` + `AgreementClausesForm.tsx` —
repeatable clause list (title input + body textarea + a checkbox per
clause that acts as a radio, only one clause can anchor the Investment
Summary at a time), Add/Remove clause, saves via `updateAgreementClauses()`.
Only reachable when `agreement.clauses` is non-null (`notFound()` on
legacy rows, and `AgreementActions.tsx`'s new optional `editHref` prop is
only passed when clauses exist) — there's nothing structured to edit on a
pre-migration agreement. `catalog_bundle_members`-editing precedent
(radio-via-checkbox pattern) reused here for the summary anchor: checking
one clause's box clears the flag on all others in local state before
submit, so exactly zero or one clause ever carries it.

Also fixed same session: printed/PDF Agreements and Proposals were
breaking a Service Charges table, the Tools block, or the Totals block
across a page boundary (heading stranded on one page, table on the next).
Added a `.avoid-break { break-inside: avoid; page-break-inside: avoid; }`
utility (plus a blanket `tr` rule) to `globals.css`'s existing
`@media print` block, applied to each charges group / tools section /
totals div in `ChargesBreakdown.tsx` and each clause / investment-summary
block in `AgreementBody.tsx`. **Browser print header/footer (URL, date,
page number) is a Chrome print-dialog setting, not fixable from the
page** — told Shoaib to toggle "Headers and footers" off under "More
settings" in the print dialog.

Verified live (throwaway proposal + client + two test agreements — one
with `clauses`, one legacy `content`-only — via the dashboard's own
service-role scripts, browsed through the *local dev server*, not
production, since the deployed site doesn't have same-day uncommitted
code): confirmed the Investment Summary lands immediately after "2. Fees
& Payment" and before "3. Term & Termination" on the new-style agreement,
and the legacy agreement renders byte-for-byte as it did before this
change. All test rows deleted after.

**Testing gotcha:** `lib/dashboard/proposal-acceptance.ts` and
`agreement-signing.ts` transitively import `"use server"` action files
(for `generateNumber`) which pull in `next/navigation` — this breaks under
plain `tsx` outside the Next runtime ("React.createContext is not a
function"). Can't smoke-test these cascades via a standalone script.
Verify instead by driving the public `/proposal/[token]` and
`/agreement/[token]` pages through a browser against a throwaway DB row,
then cleaning up — that exercises the identical shared functions the
offline dashboard actions call. See [[adsbyshoaib-dashboard]].
