---
name: feedback-text-readability
description: "ALL projects, 2026-10-09: social post text must pass the readability gate — size floor 22–24px, deep brand colour for small text, soft feathered light-lift behind text, WCAG contrast check + 360px phone preview before showing"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-09T12:06:54.040Z
---

Shoaib, 2026-10-09, on REEFCO DICLAREEF v6: "ab kaam reh gaya Text Visibility aur Readability… best graphic designers ki best practices dekho". He approved the plan with "shuru kero". The result was v7 (`Reef-co/social/hero-test/REEFCO_DICLAREEF_hero-AR_v7.png`), which passed all 10 blocks; v6 had failed 5.

**Rule.** The KB global rule `text-readability` applies to every project.
- **Size floor (1080 canvas):** English 22–24 px at minimum, Arabic/Urdu body 30 px SemiBold, vet line 24 px. Wrap the text instead of shrinking it.
- **Colour:** small text uses the deepest brand colour, never grey. White text goes on dark photo areas.
- **Soft light behind text:** blur 8, saturation 0.75, toward white at about 0.48, through a text-following mask (padding 26, blur 38). It reads as natural light, not a card.
- **Grouping:** ingredient, dose and packs are icon rows on a calm surface.
- **Quality gate:** before showing a post, measure WCAG contrast per block. Text under 40 px needs 4.5:1, and 40 px or more needs 3:1. Use the darkest 10% of the background for dark text and the lightest 10% for light text. Also make a 360 px phone preview.

**Why:** at phone size, the 18–19 px grey English lines and the vet line were unreadable, and contrast went as low as 1.5–1.9:1. Shoaib also dislikes cards and white bands ([[feedback-design-glass-light]]), so the fix had to be invisible.

**UPDATE (same day):** the glass effect is banned for good ([[feedback-design-glass-light]]), so the light-lift is gone. Run rf-hero5 with `NO_LIFT=1`. With no lift, DICLAREEF's existing scene fails 7–9 of 10 blocks, because the right side has a water tank, red drinkers and hens. The fix is a scene with real empty light space, via an AI edit or a regeneration (paid, so ask first).

**Result (same day):** Shoaib approved one Nano Banana Pro edit ("kero edit, product samet… presentation professional"), which cost ≈ PKR 39.
- The edit used `scratchpad/tadstyle/gemini-edit2.mjs`, which takes several input images and an aspect ratio. The inputs were the label-fixed scene plus the real product render, with output at **4:5**. The prompt is in `Reef-co/social/hero-test/diclareef-scene-v2-edit-prompt.txt`.
- It produced a calm cream wall on the right and a clean steel top. The label came out faithful, so no label swap was needed.
- v10 was made with `rf-hero6.ts`. It has no glass, no glow and no shade. All 14 blocks pass. Text was placed by measuring the scene's brightness in bands:
  - the column on the wall (y 0–810);
  - the dose on the steel (845–990);
  - pack sizes in WHITE on the dark cart edge (1080–1160);
  - the vet line on the floor.
- **Lesson:** ask the AI for a 4:5 scene with named empty zones, measure the bands, then lay out the text.

**How to apply:**
- The reference script is `scratchpad/tadstyle/rf-hero5.ts`. Its `txt()` registers each block, and `LIFT` is set by an env var.
- Never show Shoaib a post that has a FAIL.
- If a post still fails at about 0.5 lift, the scene's text area is too busy. Fix the scene, don't push the lift harder.
