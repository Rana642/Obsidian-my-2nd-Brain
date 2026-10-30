---
name: invoice-addons-discount
description: "de4dd84 (2026-10-26) — invoices store their discount (not a negative line), add-ons (per-item discount/GST opt-in, retainer every-month/next-invoice-only), complimentary retainer lines; add-on share of payments = client money in Finance"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-27T07:48:00.929Z
---

Shoaib asked on 2026-10-26: the retainer invoice showed no original total or discount, it had no way to add extras (domain renewal, Claude subscription), and complimentary services were missing. He chose: one invoice for the client but separate records, add-ons kept apart from income, discount/GST chosen per add-on, and the receipt in the same format (the receipt is the same invoice link, showing "Paid").

What shipped (de4dd84; SQL run by Shoaib the same day):
- `invoices.discount_type/value/amount` and `addons_total`. `invoice_items.line_kind` (service|tool|addon) with `addon_discount` and `addon_tax`. `retainer_items.is_complimentary`. New table `retainer_addons` (recurring, apply_discount, apply_tax, billed_invoice_id; a one-off is billed once).
- Invoice `subtotal` now means every charged line before discount and GST: Subtotal − Discount + GST = Total. `DocumentPreview` shows the Add-ons group, the complimentary box, the discount under the lines, and "You save".
- `calculateTotals` gained an add-on bucket (`addonsSubtotal`, `addonsTotal`, `grossSubtotal`). Without add-ons the results are identical to the old maths (40k randomized checks).
- Manual invoices (`DocumentForm`) take a discount, a line type selector and "Add add-on". The invoice page has an "Add add-on" link (`edit?addon=1`). A quotation's discount now carries to its invoice.
- Finance: each payment's add-on share (`addons_total/total`) goes to clientMoneyIn, not income. New pass-through expense category `client_addons`.
- Fixed: the invoice edit page dropped `is_complimentary`.
- Data fix: the Ahmed Jahanzaib Shah (Meezab + Tad) retainer got 6 complimentary monthly lines. INV-2026-002 (draft) now has subtotal 40,000, discount 50% = 20,000, total 20,000, plus the 6 complimentary lines.

Shoaib's follow-ups the same day:
- No Rate column when every line's quantity is 1 (19b91d1). Rate just repeated Amount.
- On the invoice, charged lines are grouped as "Monthly retainer" (a retainer invoice's services), "One-time services" (description ends "(one-time)", first invoice only), "Tools" and "Add-ons" (93e2a37). A non-retainer invoice says "Services" and shows group labels only when there's more than one group.
- Totals with more than one group (Shoaib chose this 2026-10-26, 2c60d6a). A discounted group shows Subtotal, Discount (its proportional share) and "<group> total" under its own lines in the table. The bottom block lists each group's amount after its discount, then GST, then "Grand total", then "You save". A single group keeps Subtotal / Discount / Total.

- INV-2026-002 spelling fixed in the data (Claude, maintenance, Renewal); the typos were only in invoice_items.
- Print: an extension icon printed over an amount. Fixed in bf3e34e, see [[print-extension-overlay]].

A draft invoice's public `/invoice/[token]` returns 404 by design, so verify drafts in the dashboard.

See [[adsbyshoaib-proposal-agreement-funnel]], [[adsbyshoaib-dashboard-navigation]], [[adsbyshoaib-payments-receipts]].
