---
name: hero-packshot-ai-topaz
description: "Reusable master hero packshot recipe (2026-10-12, REEFCO test) — GPT Image 2.5 re-renders the pack, code puts the REAL label back, Topaz 2x upscale + Topaz matting cut-out; Topaz API gotchas"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-12T13:18:12.785Z
---

Shoaib, 2026-10-12: "Reef-co per test kerty hain … AI se packaging dobara banao … Topaz aur AI image ka use kerty huay ta k in future use kiya ja saky". The output is a master hero packshot per product, reused in every post and in print.

**Recipe (≈ PKR 19 + 2 Topaz credits per product):**
1. **Real label artwork, sharpest source available.**
   - DICLAREEF label PDF rendered at 300 dpi with `pdftools/render.mjs`, front panel cropped (946×1415).
   - FOSBAREEF T has only a 1600 px JPG, so Topaz "Text Refine" 2x was used (1 credit). Small text came out visibly sharper and the words were unchanged.
2. **AI pack** with `tadstyle/openai-gen.mjs` (GPT Image 2.5 Sunburst high, 1024×1536, refs = pack render + label; ≈ $0.067).
   - Prompt: product fidelity, front-on, plain cool-white seamless sweep, contact shadow, no props.
   - Prompts: `scratchpad/reefhero/*-prompt.txt`.
   - The AI redraws label details: icon order and side micro-text were wrong.
3. **Topaz 2x** with "High Fidelity V2" on the AI image (1 credit), giving 2048×3072.
4. **Real label put back at 2x**, so the label sits at native resolution, not upscaled:
   - flat front label → `tadstyle/label-flat-swap.cjs`: auto rect from the label colour, light match on a coarse 6×9 grid of cell means so no AI label ghosts through;
   - round tub → `tadstyle/wrap-label.cjs`: a copy of tadlabels/wrap.cjs with PNG output. FOSBAREEF config: `reefhero/fos-wrap.json`.
     - Inputs measured from the image: cx and R from the red extent, yTop from the top stripe, bBottom from how far the bottom stripe rises at the sides, labelCenterX = the middle icon centre.
     - Trim the JPG's print border (row 1730).
5. **Cut-out:** use Topaz matting (`/matting/async`, model "Object", 1 credit, about 75 s).
   - DICLAREEF came out clean: cap ribs, rounded base, no floor sliver.
   - The free `cut-pack.cjs` left corner/base slivers on the FOSBAREEF tub, so Topaz matting is the better option.

**Files:**
- `Reef-co/products/<PRODUCT>/hero/`: `*_Hero_Studio_2048x3072.png`, `*_Hero_Cutout_transparent.png` (DICLAREEF), and the label masters.
- Side-by-side preview: `Reef-co/products/_REEFCO_hero_packshots.jpg`.

**Scene posts (2026-10-12, after Shoaib asked "Product k hisab se koi manzar bhi add kiya kero" and "topaz se kuch behteri"):**
- **2 AI scenes,** GPT Image 2.5 high, about $0.043 each. Each is low-key, with an empty floor or counter spot on the left and a dark right side for text. Prompts: `reefhero/scene-*-prompt.txt`.
  - DICLAREEF: chicks drinking at a chick drinker (the product goes in drinking water).
  - FOSBAREEF T: a tunnel-ventilated house with fans and a charcoal counter.
- **Topaz realism pass:** `topaz.mjs <in> <out> gen Redefine` with OUTW=1536, CREATIVITY=2, TEXTURE=2 and a plain-description PROMPT. It costs 1 credit per scene (up to about 4 MP output).
  - Clearly more real: chick down and feather detail, sharp combs.
  - Scenes only, never the pack: generative models could redraw the label.
  - Endpoint: `/enhance-gen/async`. Models: Redefine | Wonder | "Recovery V2" | "Standard MAX".
- **Render:** `rf-hero-post.ts` with `SCENE=<path> SPOTX=0.32 CROPTOP=…`.
  - The English version uses the scene mirrored (flop).
  - The text side melts into deep green through a gradient grade (no panel).
  - The hero pack is shaded with a key-light multiply, drawn INTO its alpha with the `in` blend; joinChannel had made a grey box.
  - `TXEN=700` darkens the English text side earlier when the scene sits behind the heading (the FOSBAREEF fans did).
  - Output: `Reef-co/social/hero-posts/scene/` (4 posts plus a board); all pass the gate.
- **Credits used so far:** 7 of 20; 13 left.

**Topaz API gotchas:**
- `/download/{id}` returns `download_url` (plus `head_url`), not `url`. Graphic Studio's `lib/enhancers/topaz.ts` read only `url`, so the Enhance button failed; fixed in 615addf (pushed).
- CLI helper: `scratchpad/tadstyle/topaz.mjs <in> <out> enhance|remove <model> [scale]`. Run from graphics-studio with `--env-file=.env.local`.
- The job status JSON shows `credits` per job: enhance 2x = 1, matting = 1.
- Credits: 20 bought 2026-10-12; 5 used on this test (FOSBAREEF matting approved separately), 15 left.
- **Matting gotcha:** on a white lid against a white background, Topaz leaves a wispy half-alpha haze above the lid top. The free fix:
  - fit a shallow arc (y = a + b·dx², robust two-pass) to the per-column top edge;
  - clear alpha above it with a 1.5 px ramp;
  - leave the rounded lid corners as Topaz cut them.
  - Done inline for FOSBAREEF; output `fos-hero-2x-cut-final.png`.
- Both cut-outs are in each product's `hero/` folder; preview `Reef-co/products/_REEFCO_hero_cutouts.jpg`.
- **Saved to the REEFCO KB on 2026-10-12.** Upload script: `reefhero/kb-upload.sh`.
  - The studio hero is now the main photo of DICLAREEF (Liquid) and FOSBAREEF T (Powder); the old render is kept as an extra photo.
  - Both cut-outs are stored as product_image; both label masters as reference_image.
  - Also written to the KB memory note and the `graphic_rules` section "Product images — master hero packshots" (use the cut-out in every post; never let AI redraw the pack).

Related: [[hero-pack-label-swap]], [[reefco-social-posts]], [[feedback-ask-before-spend]], [[image-model-choice]].
