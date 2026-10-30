---
name: feedback-pack-edges-and-props
description: Before delivering any post with a composited/AI pack — zoom 100% on pack edges + base; never paste a real pack over an AI pack (fringes); remove the AI pack first (empty-spot edit) then composite; props must not imply wrong contents
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-07T06:18:09.790Z
---

On 2026-10-07 Shoaib rejected 5 Meezab finals. He marked them in red and said "Product k Edges clear nai hain sary paisy waste ker diye":

1. **Recal Fos, Reeftox Sol, Aminoreef Sol, Reevit AD3EK:** I pasted real pack photos OVER AI-drawn packs. White fringes from the studio floor showed at the bases, and parts of the AI pack peeked out. More code patching (clean plates, ellipse cuts) made it worse: visible rectangles, and the pack covered text.
2. **Fegatosim:** a steel scoop of feed stood in front of a LIQUID pack, which implies the pack contains feed. Props must never suggest wrong contents or form (liquid vs powder).

**Why:** I called the posts "checked" after only small contact-sheet views. Shoaib is paying per image, so a defect he finds means wasted money.

**How to apply:**
- Before calling anything final, zoom to 100% on every pack edge and base, every post. Fix it or hold it.
- When the AI's pack is wrong: first make the scene WITHOUT any pack, using an "empty spot" edit with gemini-3-pro-image (about PKR 37 per image; ask first). Then place the real cutout with a contact shadow.
- Never cover an AI pack with a real one.
- Props must match the product form: liquid products get water, a drinker or a measuring cup; powders get a scoop.
- Free code fixes are fine only for things like the text or name overlays.

Related: [[feedback-ask-before-spend]], [[feedback-product-presentation-variety]].

**Method that worked (2026-10-07, all 5 Meezab posts fixed, 5 edits ≈ PKR 186):**
1. Run `tadstyle/gemini-edit.mjs` with "remove the X pack completely, show the empty surface, keep everything else pixel for pixel". The text came through unchanged every time.
2. Place the real pack with `tadstyle/mz-place.cjs` and a spec of:
   - `contour`: the base edge measured on a 5× grid of the studio photo (polyline);
   - `body:[l,r]`: the measured body width. The flood fill keeps grey studio gradient, so a width measured from a row is too wide and squashes the pack;
   - `inset` 2–3 px, and `sideRef` above the contour;
   - then an opening, a 0.7 px feather, and a tight + soft contact shadow.
3. Check the base at 3–4× zoom and the whole post at full size before calling it final. Keep a gap between the pack and the chip bar.

**Gotcha:** sharp `.threshold()` returns 3 channels. Add `.extractChannel(0)` before using it as a mask.
