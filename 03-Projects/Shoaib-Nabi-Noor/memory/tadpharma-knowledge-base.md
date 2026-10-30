---
name: tadpharma-knowledge-base
description: "Tad Pharma knowledge base (kb_* MCP tools, Supabase project_products/project_assets/project_knowledge_docs) — FINAL as of 2026-09-29; approval lives in presentation_page notes; FCR wording rule"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-12T11:54:21.022Z
---

The Tad Pharma knowledge base is served by the site's MCP `kb_*` tools. Data lives in Supabase tables `project_products`, `project_assets` (files in R2) and `project_knowledge_docs`. Most of the work was done on the home PC; see [[shoaib-two-pc-workflow]].

**State on 2026-09-29, marked FINAL by Shoaib:**
- 17 products, each with 3 A4 presentation pages (1 Cover, 2 Data sheet, 3 Urdu): 51 pages, all "APPROVED by Shoaib (2026-09-28) — final".
- Approval is plain text in `project_assets.notes`. There is no status column and no MCP tool to edit notes. This PC changed the notes with a service-role PATCH script.
- Every product's typed Urdu is verified. Facts come only from the PDF brochures plus photos of the real packs (the `reference_image` assets, and the "## Pack label (real product)" section in each product).
- The home PC also added marketing docs `content-calendar-launch-30`, `social-post-design-lock`, `datasheet-template`, and `brand_kit/gradients-and-strips`.
- `kb_get_social_post` (da6a396) returns a calendar day's image prompt, caption, logo and product photo.
- 2026-09-29, poultry elements trial (office PC). Shoaib found the first ChatGPT batch (Day 1–8) too generic and not poultry-looking. No real farm photos exist, so the approach is "AI ka smart use".
  - Day 9–30 each got a `poultry_element`:
    - Education, tip and brand posts: an AI scene in a hero band from y=170 to 600 under the logo.
    - Product posts: a thin Tad Red line drawing on the left, between the bullets and the chips. Livestock motif only where the post's own text names it.
    - Day 23: none.
  - The lock got `poultry_element_rules` and `footer_strip.common_mistakes`. That batch had rounded/inset strips and icons above the text; only Day 7 and Day 8 strips were right.
  - Day 1–8 are unchanged. Shoaib generates a few first, then decides: keep, tune, or build an approved scene library composited in HTML.
- 2026-09-30: The Day 11 test passed. Shoaib then set the rule [[feedback-social-post-locked-vs-free]].
  - The lock was rewritten as `locked` (5 items), `creative_freedom`, and a safety `do_not`.
  - The old `layout_grid` / `premium_style` / `poultry_element_rules` are gone.
  - Calendar entries now carry `layout_idea` and `background_idea` (suggestions); `composition` / `poultry_element` were renamed.
  - Paste-ready prompt builder for when ChatGPT's connector fails: scratchpad `tadposts/prompts.cjs <days>`.
  - ChatGPT can't download the CDN images, so attach the files yourself.
- Numeric MCP args can't be passed from this client, so `kb_get_social_post` day=N fails with "expected number". Verify through REST (scratchpad `tadposts/verify.cjs`) instead.

**Shoaib's wording rules, recorded in the KB memory doc:**
- Aminotox, Li Tonic and Lysorox "increase FCR", exactly as the brochure's Urdu says. In English write "increases FCR", never "improves/better FCR".
- The Urdu FCR lines stay as printed.
- English text must have no spelling or grammar mistakes, and nothing may be added beyond the brochure.
- Product images are originals only, never AI.
- Niflorise is shown exactly as its PDF (including the Colistin write-up). Manufacturer confirmation is still open.

**Label print files (2026-10-01, office PC):**
- Shoaib sent flat label artwork for 11 products; more will follow. Still pending: Aminotox, Li Tonic, Lysorox, and the three Reevits.
- Each one was stored as a `reference_image`, "<Product> — label artwork (print file)". The upload went directly to R2 and `project_assets` using scratchpad `tadlabels/upload.cjs`.
- Each product got a "## Label artwork (print file, 2026-10-01)" section added: Reg. No, M.L. No 000934, and how the label compares with the brochure.
- **Open:** Necloxy-50 dosage disagrees across the brochure (1 g per 4 L / 1 g per 2 L), the label's English (1 ml per 4 L / 1 ml per 2 L) and the label's Urdu (1 g per 10 L / 1 g per 5 L). Tad Pharma must confirm.
- The Niflorise label still carries the old Jahanian address and phone 03216307810. Shoaib, 2026-10-01: leave the label as printed, including in packshots, but never use that address or number in any other content.
- Shoaib is confirming the Necloxy-50 dosage with Tad Pharma himself.
- The current product photos have garbled small text, e.g. Biofurose reads "Salamat House … Bukhar Colony … Muitan". Shoaib wants the packshots re-created with the crisp label artwork on them.
- Packshot re-creation, sample made 2026-10-01, waiting for Shoaib's verdict. Not uploaded to the KB yet.
  - Scratchpad `tadlabels/wrap.cjs <config.json>` wraps the print file around the jar in the existing packshot. The config gives cx, R, yTop, scale, and the white/dark reference rows.
  - The mapping is cylindrical: arc length gives the label x, and `bBottom` adds an ellipse sag toward the bottom.
  - Per-column light is measured from the photo as out = colour·a(x) + h(x), using white rows plus maroon rows, which keeps the gloss stripe.
  - The label is resized to 2× with lanczos, lightly sharpened, then 2×2 supersampled.
  - Biofurose v2 settings: scale 0.815 (print 1600px → front), cx 544, R 284, yTop 430, yEye 380, bBottom 70, edgeFade 0.6 from |sx| 0.8.
  - Shoaib's feedback on v1: side text looked like broken pieces and the bottom corners stuck out past the rounded base.
    - Fixes: horizontal supersampling that grows as 1/cos toward the edges, an edge fade, and a strong sag measured from the jar's bottom groove (the label's bottom edge rises about 70px at the sides).
    - Measure R from the actual silhouette; too big an R makes the label poke out of the jar.
  - Each pack shape needs its own measurements; bottles will need a different R and top.

**Gotchas on this PC:**
- The page generator (HTML → JPG) isn't on this PC; the pages were made at home.
- As of 2026-10-01, `.env.local` here does have the S3/R2 credentials (S3_*), so R2 reads and writes work through `@aws-sdk/client-s3`. The KB signed-URL key (MCP_OAUTH_SIGNING_KEY) is still missing.
- The Jameel Noori Nastaleeq font is installed for the user at `%LOCALAPPDATA%\Microsoft\Windows\Fonts`.

**2026-10-05: all 17 product brief posts**
- Made with generate4 (draft → final, one by one), started at Shoaib's request "Sary Product ki posts banao".
- Folder: `E:\Rana Shoaib\My Projects Website	ad-pharma-social\product-posts\`. It holds samples.json, overrides.json, captions.md, run-all.sh and run-log.txt.
- Prompts were fetched with a local kb_get_product_post harness. Set a dummy `MCP_OAUTH_SIGNING_KEY` (this PC has none) and swap the attach links for jsDelivr CDN photo and logo URLs.
- Necloxy-50 unit settled 2026-10-05: the pack says Water Soluble Powder 1000 gm, so doses are in gm, following Shoaib's rule "liquid = ml, powder = gm". The post uses the brochure figures, 1 gm / 4 L and 1 gm / 2 L. The label's Urdu 10 L / 5 L is still different.

**2026-10-05: 17 product posts DONE**
- The finals are in `tad-pharma-social\product-posts\final\` as TadPharma_<Product>.png, plus captions.md.
- **Pack check against the original photos:** gpt-image-2 re-drew two labels with garbled text.
  - Tilmorise: fixed by a re-render with an explicit label description.
  - Ampotad: fixed with the "empty podium" method. A gpt-image-2 edit removes the AI pack, then scratchpad `tadstyle/real-pack.ts` composites the REAL photo. It does a flood-fill cutout, fills holes per row, and cuts the base as a measured half-ellipse via env BASE="cx,yc,rx,ry" (Ampotad: 544.5,1213,294.5,121).
  - Use real-pack.ts whenever an AI pack is not identical.
- **Other fixes:**
  - Florise-23 and Enrolin-20 icons were syringes. The lock's do_not bans syringes, so they became a water drop, a drinker and a bottle.
  - The Gumbonil product name was missing.
  - The Niflorise photo showed goats; changed to poultry.
- Cost: about $1.8 (≈ PKR 500).

**2026-10-10: Urdu product post (Biofurose), scratchpad `tadstyle/tad-urdu.ts`, run from graphics-studio**
- The KB logo PNG (and scratchpad `tad-logo.png`) has an OPAQUE white box. On a photo it shows as a white rectangle, and Shoaib caught it ("logo dekho").
  - Use `tadstyle/tad-logo-clear.png`, made by `tadstyle/logo-transparent.cjs`. That script makes border-connected white and letter counters transparent, keeps the white PHARMA letters inside the red pill, and anti-aliases the edges.
- Jar cutouts with `cut-convex.cjs` had lumpy bottom corners (from floor shadow and reflection) and a box of white above the curved lid. Shoaib caught that too ("product cutting dekho").
  - Use `tadstyle/cut-jar-round.cjs` with `ELLIPSE=yc,ry`, `CORNER_Y=yc` and `BEVEL=y0,slope`. Biofurose: ELLIPSE 1191,118 and BEVEL 1174,0.5.
  - BEVEL is the moulded corner where the body rounds in before the ellipse starts. Measure it from a luminance grid of the edge pixels. Without it, a white sliver showed at both bottom corners (Shoaib caught that too).
  - A jar seen from slightly above ends in the LOWER HALF OF AN ELLIPSE. Measure it from the dark contact line per column.
  - The rounded-corner/flat-bottom model left a white "plate" of studio floor under the jar. Shoaib: "Product finishing … is not correct".
  - The script also sets a per-column top on the lid, prints the trim offset and writes a `-check.png` red outline to verify.
  - **Edges:** the jar body tapers a few px, so a symmetric width left a 1–4 px studio-white strip on the narrower side. That read as a white outline on the scene ("kinaray dekho product k").
    - The script now uses per-side detected edges when both sides agree, ERODE=1 px, and copies the colour from 2 px inside onto the 2 outer pixels. Alpha blur is 0.7.
    - Always check the cut-out on a MID-TONE background (e.g. #8a7a6a); white-on-white hides fringes.
- **2026-10-12: all 17 Urdu product posts DONE**
  - Files: `tad-pharma-social/product-posts/urdu/TadPharma_<Name>_Urdu.png`, plus contact sheets `_Tad_Urdu_posts_1/2.jpg`.
  - Shoaib asked for variety: "Posts mai variety honi chaiye Styles mai ... visual hierarchy improve hoti rahy".
  - `tad-urdu.ts` now has 4 layouts set via the copy's `layout`, never the same twice in a row, each used 4×:
    - A "right": the Biofurose layout.
    - B "left": mirror of A.
    - C "center": name + headline centred on top, pack in the centre, text wings ON the white table (`surfaceY`). Text over the flock failed at 3.1:1.
    - D "window": landscape scene in a rounded photo window (rounded with sharp's `in` blend; joinChannel left square corners, dest-in made them black), pack breaking out of the window.
  - Pipeline:
    - `tadurdu/gen-copies.cjs`: every Urdu string is a verified KB line or an asserted verbatim excerpt.
    - `scene-prompts.cjs`: prompts per layout.
    - `gen-one.sh <slug>`: GPT Image 2.5 Sunburst high; Gemini prepay credits ran out on 2026-10-12.
    - `make-final.cjs <slug>`:
      - feathers GPT's hard "panel" edge (`soften-edge.cjs`);
      - whiter grade;
      - in the center layout, lifts the scene so the table is at y=790. Override with `SURFACE=920` when the lift pushes scene objects under the headline (Seno).
      - `WINPOS=top` keeps a head or comb in the window crop.
    - `tadstyle/cut-pack.cjs`: automatic jar/bottle cutter (contact-line ellipse, per-side edges); the Lysorox sachet used `remove-bg.cjs`.
  - Cost: 16 scenes ≈ $0.69 (≈ PKR 191). Each scene was about PKR 12, against a PKR 22 estimate.
  - Saved to the KB on 2026-10-12 as each product's reference_image, and the `product-brief-posts` "ALL 17 DONE" section was appended. The upload script is `tadurdu/kb-upload.sh`.
  - Final folder Shoaib asked for: `tad-pharma-social/Final Posts - Urdu Product Posts/` (17 PNGs).
- **Label rule (Shoaib 2026-10-12):** never change the label on a packshot, even a wrong-looking address. Use the KB photo exactly as it is.
  - I briefly swapped in the label-wrap v2 packshot (Saltanat/Bukhari address). That was reverted: the post uses the KB photo.
  - In tad-urdu.ts, copy JSON `footprint` {cx, yc, a, ry} (cut-out px) drives a matching soft cast shadow and a contact (AO) shadow. `jarGrade` [0.965, 0.975, 0.99] tones the studio white down to the scene's cool daylight.

**Posting, 2026-10-21:**
- Tad's Instagram was connected from the Business portfolio (via Import Pages, 4c5c405/a684e4b).
- All 23 ready posts went out the same day, 2:00–11:10 PM, every 25 minutes, with the old approved captions. Each post also became a Story.
- Only the first 15 went to Google; the location takes about 20 writes a day.
- **The "Partner with Tad Pharma" design contains the word "dealer".** It was put last (11:10 PM) and Shoaib was told twice; he didn't stop it, so it went out. Future designs must not use "dealer" ([[feedback-no-dealer-word]]). Check designer creatives for it before scheduling.
- **Asked Shoaib to check:** Tad had old Meta-native schedules for 22 Oct–16 Nov. A failed draft delete is silent, so he should look in Meta Business Suite → Tad Pharma → Planner for stale old posts after 22 Oct. I offered to build a check tool (`/{page}/scheduled_posts`, see [[fb-native-schedule-feed-fix]]); no answer yet.
- 23 posts on one page in one day may lower reach per post. Spread future batches out.
