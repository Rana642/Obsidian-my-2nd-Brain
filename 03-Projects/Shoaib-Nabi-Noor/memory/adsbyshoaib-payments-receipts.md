---
name: adsbyshoaib-payments-receipts
description: "2026-10-13 (615e270) no payment gateway — bank details on invoice links + /pay, client uploads transfer receipt, Shoaib confirms by hand in /dashboard/receipts; SQL block must be run once"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-13T11:07:22.162Z
---

**Shoaib's decision (2026-10-13):** "Gateway to abhi nai hai". Payments are by bank transfer to the business account, which covers both national and international clients. The client uploads the receipt and Shoaib confirms it manually. For international payments he uses the Pakistan IBAN for now and will add a separate international account later.

**Built in commit 615e270:**
- **Settings → Payment accounts.** The `settings.payment_accounts` jsonb holds:
  - pk: title, bank, account, IBAN, branch;
  - intl: a "same as Pakistan" flag, its own fields, SWIFT and bank address;
  - a note.
  - Shoaib types the numbers himself in the dashboard (since 76dc704 this card is in Finance → Setup, not Settings). Never hard-code or store them in memory. The account number and IBAN were shared in chat; they are not to be written anywhere by Claude.
- **`lib/payment-accounts.ts` `paymentBlocks`** decides what payers see: PKR → Pakistan; other currencies → International; /pay → both.
- **Invoice link `/invoice/[token]`:** How to pay (with copy buttons), the receipts sent so far, and the upload form.
- **`/pay`:** noindex, revalidate 1 h.
- **APIs:**
  - `/api/pay/upload-url` presigns into R2 `receipts/<token|pay>/`.
  - `/api/pay/receipt` checks the object with `headObject`, inserts `payment_receipts` and emails Shoaib.
- **Dashboard:**
  - `/dashboard/receipts`: Confirm runs `applyPaymentToInvoice` and emails the payer; Reject requires a reason and emails the payer.
  - Receipts also show on the invoice page, and the Overview has a banner.

**SQL was run on 2026-10-13 and verified:** the `payment_receipts` table and the `settings.payment_accounts` column both exist.

**KB:** there is a pointer note in "Ads by Shoaib" at `marketing_doc/payment-accounts`. It holds the bank, the account title and where the numbers are shown. It does NOT hold the numbers themselves; Shoaib adds those himself.

**Filled by Shoaib on 2026-10-13.**
- Pakistan account and IBAN entered. The IBAN passes the mod-97 check.
- Same-as-Pakistan is ticked for international.
- Bank name is now "United Bank Limited (UBL)". Verified live on /pay on 2026-10-13.
- SWIFT is still empty. Shoaib will add it once UBL gives it to him.

**Live end-to-end test on 2026-10-13 (/pay path): passed.**
- presign → R2 PUT → receipt row → Shoaib pressed Reject in the dashboard.
- Both emails reached his Gmail: the "Payment receipt: PKR 1.00 · …" alert and the rejection with a retry link.
- The test row and its R2 file were then deleted.
- Scripts are in the scratchpad `paytest/`. The test used ref `TEST-E2E`.

**Not tested yet:**
- The Confirm path. It needs a sent (non-draft) invoice; on 2026-10-13 only 1 draft existed.
- The invoice-link upload path.
- Test both on the first real sent invoice. Never put a test payment on a real invoice.

**Shoaib's simplification (2026-10-13):**
- Bank details are one card with a **Pakistan / International toggle**.
- The receipt form asks **only name + amount + receipt** on /pay, and only amount + receipt on an invoice link. His words: "Name or Amount kafi hai".
- No email, phone, date, reference or note fields. The date defaults to today (PKT) and the API treats email as optional.
- So payers on /pay usually get **no confirm/reject email**.
- Don't add those fields back without asking him.

**Gotcha:** on this PC, every nested `/api/*` route returned an HTML 404 in `next dev` (even old ones such as `/api/billing/cron`). `rm -rf .next/dev` and a restart fixed it.

Related: [[adsbyshoaib-dashboard]], [[adsbyshoaib-client-portal]], [[adsbyshoaib-client-intake]].
