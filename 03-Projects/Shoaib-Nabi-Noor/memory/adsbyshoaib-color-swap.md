---
name: adsbyshoaib-color-swap
description: Citrus is now the 8% brand accent and Cobalt the 2% highlight — reversed from the original CLAUDE-CODE-INSTRUCTIONS.md
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-21T12:26:31.956Z
---

On 2026-08-21 Shoaib swapped the accent roles in the adsbyshoaib design system:
**Citrus #EAB308 = 8% brand accent** (was Cobalt), **Cobalt #1E40AF = 2% highlight** (was Citrus). Cloud #FAFAFA background and Ink #0F0F14 text unchanged.

**Why:** Shoaib's explicit design decision; the swap note is documented at the top of the Design System section in CLAUDE-CODE-INSTRUCTIONS.md.

**How to apply:** Wherever the build doc says "Cobalt" as accent (tags, secondary buttons, icons, animated lines, selection), use Citrus instead. Citrus fails text contrast on Cloud — use it only for badges/underlines/icons/decorative elements; text-level accents stay Ink or Cobalt. Confirm per-element choices with Shoaib. See [[shoaib-working-preferences]].
