---
name: adsbyshoaib-color-swap
description: Brand colours — superseded by the 2026-09-15 official-logo rebrand (Citrus #FEC107, Cobalt #2196F3 decorative-only, Forest #3FA343); CLAUDE.md is the source of truth
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-25T07:50:14.577Z
---

**Current (rebrand 2026-09-15, done from Shoaib's other PC):** palette
re-hexed to match the designer's official logo — **Citrus #FEC107 = 8%
brand accent**, **Cobalt #2196F3 = decorative-only**, new **Forest
#3FA343 = sparing third accent**; Cloud #FAFAFA / Ink #0F0F14 unchanged.
Token names stayed the same (only hex values in `app/globals.css`). Citrus
AND Cobalt both fail text contrast on Cloud — text-level accents stay Ink.
The real logo is `public/brand/*.svg`, rendered as an image, never as a
CSS-text wordmark. Always read the repo's CLAUDE.md for the live rules.

History: on 2026-08-21 the accent roles were first swapped (Citrus #EAB308
as accent, Cobalt #1E40AF as highlight) — those hex values are now gone.

See [[shoaib-two-pc-workflow]] and [[shoaib-working-preferences]].
