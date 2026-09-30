---
name: adsbyshoaib-color-swap
description: Citrus/Cobalt/Forest hex values and roles as of the 2026-09-15 logo rebrand — see [[adsbyshoaib-logo-rebrand-2026-09-15]] for the full rebrand
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-14T21:55:52.735Z
---

Current (2026-09-15) token values in `app/globals.css`'s `@theme` block: **Citrus `#FEC107`** (8% brand accent, unchanged role), **Cobalt `#2196F3`** (was `#1E40AF` — now brighter, decorative-only), **Forest `#3FA343`** (new, sparing third accent). Cloud `#FAFAFA` background and Ink `#0F0F14` text unchanged. Token NAMES did not change — only hex values — so all existing `bg-citrus`/`text-cobalt` Tailwind usage recolored automatically.

History: on 2026-08-21 Shoaib swapped which named token carried the 8%-accent vs 2%-highlight role (Citrus became accent, Cobalt became highlight). On 2026-09-15 the designer delivered an official logo and Shoaib requested the exact new hex values applied everywhere, replacing the old ones under the same token names — see [[adsbyshoaib-logo-rebrand-2026-09-15]] for the full scope (logo image replacement, text-contrast audit, etc).

**Why:** Shoaib's explicit design decisions in both cases; matching the real designer-delivered brand logo in the second.

**How to apply:** Citrus AND Cobalt both fail text contrast on Cloud — use both only for badges/underlines/icons/decorative elements/backgrounds; text-level accents stay Ink only (not Cobalt anymore — this changed 2026-09-15, the new brighter Cobalt fails ~4.5:1). Forest is a sparing accent (a card border, an ambient blob) — not for body text, not overused. Confirm per-element choices with Shoaib for anything non-obvious. See [[shoaib-working-preferences]].
