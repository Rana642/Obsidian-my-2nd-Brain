---
name: print-extension-overlay
description: bf3e34e (2026-10-26) — browser-extension icons printed over invoice amounts; print CSS now hides non-app nodes with display+visibility+opacity; headless-Chrome print-to-PDF test recipe
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-27T07:47:55.299Z
---

On 2026-10-26 Shoaib printed INV-2026-002 to PDF and a small purple extension icon (a text-selection helper) printed on top of an amount.

**Why the old rule failed:** `app/globals.css` @media print already had `display: none !important` for `html > :not(head):not(body)` and `body > :not([data-app-root])`. Extensions often put an inline `display: block !important` on their host element. Inline !important beats stylesheet !important, so the icon still printed.

**Fix (bf3e34e):** the same selectors also get `visibility: hidden !important; opacity: 0 !important`. This covers every printable page: invoices, quotations, proposals, agreements, reports and the letterhead.

**How to test a print issue without his browser:**
1. Write a small HTML file with the print rule and an injected element.
2. Run `chrome.exe --headless=new --no-pdf-header-footer --print-to-pdf=out.pdf file:///...`. Chrome is at `C:\Program Files\Google\Chrome\Application\chrome.exe`.
3. Read the PDF with the Read tool.

The browser repeating the table's DESCRIPTION/AMOUNT header on page 2 is normal and needs no fix.

See [[invoice-addons-discount]], [[adsbyshoaib-letterhead]].
