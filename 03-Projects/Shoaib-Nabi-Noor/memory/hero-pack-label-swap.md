---
name: hero-pack-label-swap
description: "Higgsfield-style method (2026-10-07) — AI renders the whole real pack big + integrated in the scene, code swaps in the REAL label by perspective warp; plus the resvg variable-font weight gotcha"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-07T12:05:33.236Z
---

On 2026-10-07 Shoaib complained that the product looked too small, did not read as a hero, and looked pasted ("Baki post se alag hi nazar ati hai"). He asked for a Higgsfield-style test on Recal Fos. Spend: 2 Nano Banana Pro images, ≈ PKR 75.

**Method (free after the 1 generation):**
- **Step 1, generate.** gemini-3-pro-image, called through `tadstyle/gemini-gen.mjs`, gets the real pack photo as a ref plus a PRODUCT FIDELITY + HERO prompt:
  - the pack at about 60% of the height, low 50 mm camera, light direction matching the pack photo, contact shadow.
  - The pack comes out big and lit like the scene, but the small label text is ALWAYS garbled.
- **Step 2, swap the label.** `tadstyle/label-swap.cjs` replaces the garbled label with the real one:
  - it finds the purple label border in both images (largest purple component, mask dilated 3 px to join the thin lines, then a line fit for each edge);
  - computes a homography from the AI quad to the real quad;
  - warps the real label in, with per-channel gains on the white paper (3×3 grid) to match the scene light, and a 1 px feather.
  - The purple threshold must catch dark border pixels (r > 55).
- **Step 3, compose.**
  - Hero (text drawn by the AI from the wireframe): `meezab-gen.ts compose`.
  - Lifestyle (photo with no text): `tadstyle/mz-lifestyle.ts` draws frosted-glass cards (real backdrop blur), Bright Teal icon badges, the logo pill and the footer.
- Outputs are in `meezab-social/product-posts/hero-test/` (Recal-Fos_Hero.png, Recal-Fos_Lifestyle.png).

**Font gotcha:** resvg ignores the weight axis of the variable `graphics-studio/fonts/Inter.ttf`, so weight 800 renders as Regular.
- This is why the wireframe headlines, and therefore the AI's copies of them, came out thin.
- Fix: draw text as glyph outlines with fontkit (in adsbyshoaib node_modules) using `getVariation({wght, opsz})`, as mz-lifestyle.ts does.

Waiting on Shoaib's verdict: which style he prefers, and whether to roll it out to the other products (ask about spend first).

Related: [[feedback-pack-edges-and-props]], [[meezab-knowledge-base]], [[feedback-ask-before-spend]].
