---
name: reefco-social-posts
description: "REEFCO (Muhammad Khalid, Jordan) social posts 2026-10-05 — product options A–D, brand/distributor set, Arabic header name rendered by code, real-photo compositing method; files in Reef-co/social"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-12T13:34:08.805Z
---

**REEFCO** (KB project "Muhammad Khalid — REEFCO"; local folder `E:\Rana Shoaib\My Projects Website\Reef-co`).

**Done on 2026-10-05:**
- **Product posts:**
  - DICLAREEF and FOSBAREEF T, 4 layouts each (A–D), in `social/options/`, with an options board per product.
  - Made because Shoaib said: "Client ko impress kerna hai ta kam mil jaye humain".
- **Brand / distributor posts:**
  - 4 posts in `social/brand/`, plus a board.
  - The copy is in the KB doc `brand-and-distributor-posts`, approved with one change: "pori range hi k hisab se branding, products lagany ki zarorat nai".
- **Arabic name on every post:**
  - The text: شركة الريف / لصناعة الأدوية البيطرية والمبيدات الزراعية.
  - Where: the top-right of the header band. Shoaib said "Arabic Version bhi use kero" and sent REEFCO's own bilingual artwork.
  - How: rendered by code with Amiri Bold, never by the image model.
  - The source is that artwork, because the catalogue's Arabic text layer is corrupt. REEFCO still has to confirm it.

**Method notes:**
- Real group photos are never redrawn.
- A code-made layout mock becomes the concept for the gpt-image-2 final, then code composites the photo into the mock's reserved panel. gpt-image-1-mini ignores "leave this area empty".
- The vet line goes only on product posts.
- ISO claims, turnover and named export markets stay off posts until REEFCO verifies or approves them.

**2026-10-09: bilingual hero series (Arabic first, no glass)**
- **DICLAREEF v10** was APPROVED and saved to the KB:
  - `product-brief-posts` → `mix` entry;
  - `social-post-design-lock` → "Approved hero style";
  - reference asset "DICLAREEF hero v10".
- **FOSBAREEF T v1** is made (`Reef-co/social/hero-test/REEFCO_FOSBAREEF-T_hero-AR_v1.png`) and waits for Shoaib's verdict.
  - **Scene:** the mirror of DICLAREEF. The tub is on the right, on a light birch table, with a steel scoop of powder as the prop. Behind it is an airy ventilated house with fans (the respiratory topic). The text column is on the left. One Nano Banana Pro image, ≈ PKR 39.
  - **Label:** AI side micro-text was garbled, so the real label was put back with `tadstyle/tub-label-swap2.cjs`. It uses a rectangle mask and a luminance-only light match; per-channel gain tinted white text blue. Gotcha: a 1-channel raw buffer blurred by sharp comes back as 3 channels, so add `extractChannel(0)`.
  - **Copy:** the Arabic is verbatim from catalogue p.92, in `rf-copy.json` → fosbareef. Layout script: `rf-hero7.ts`. The company name uses deep green, because Forest was 2.99:1 on the ceiling.
- **FOSBAREEF T v3** (cool-white grade, Dubai font) was accepted as "much better" and saved to the KB in `product-brief-posts` → `mix`.
- **Font trial (2026-10-09):** IBM Plex Sans Arabic, Tajawal, Almarai and Amiri were tried against Dubai at matched visual size.
  - All are OFL, and their files are in `scratchpad/fonts/`. Use `FONT_SET=plex|tajawal|almarai|amiri` on `rf-hero7.ts`.
  - My pick: Plex, then Almarai.
  - Shoaib: "Chalo filhal Current hi rakho phr mazeed dekhty hain". So **Dubai stays for now**; revisit fonts later.
  - Board: `Reef-co/social/hero-test/font-options/`.
- **Open questions for Shoaib:** should cool-white become a global rule for all projects, and should DICLAREEF v10 be re-graded to cool?
- **2026-10-10: client feedback reset the direction.** On WhatsApp the client wrote "the brochures you made for Meezab are more beautiful". Shoaib said "total dobara se banao, scene change, attractive + brand awareness" and "english mai bhi, 2 Versions".
  - **v2 series** is in `Reef-co/social/v2-meezab-style/`, made with `scratchpad/tadstyle/rf-series.ts <dic|fos|dicEn|fosEn|brand1|brand2>`. Design:
    - solid REEFCO-green shapes: a curved band with a leaf line, and a name ribbon;
    - spaced caps;
    - Aref Ruqaa calligraphic Arabic name, or a Great Vibes English script accent;
    - short text, balanced line breaks (`wrapB`), and the contrast gate.
  - **New AI scenes,** 3 × Nano Banana Pro (≈ PKR 117):
    - DICLAREEF with day-old chicks;
    - FOSBAREEF T in a tunnel-ventilated house (label swapped by code);
    - a GMP lab scientist for brand post 1.
  - Brand post 2 is a grid of REEFCO's real group photos plus a lucide wheat tile.
  - **Versions:** all 4 posts have AR and EN versions (8 files plus a board in `v2-meezab-style/`).
  - **Brand-post Arabic source:** exact clauses from the catalogue's Arabic About page, لمحة عن ريفكو (catalogue part 1, PDF p.4), used via the `brand1ar` / `brand2ar` entries in `rf-copy.json`.
    - The text layer is corrupt, so the page was rendered with npm `mupdf` (`scratchpad/pdftools/render.mjs`) and read visually.
    - So the AR brand posts say what the catalogue says, not a translation of the EN copy. For example, point 3 is "experienced staff" in Arabic but "132 products" in English.
  - Draft WhatsApp message to Khaled asking for inspiration examples given to Shoaib 2026-10-10.
- **Client reply, same day:**
  - Khaled called the Meezab work "Nice and Upscale".
  - He wants DARK backgrounds, because white "shows that it is AI work". This conflicts with the global never-dark / cool-white rules. Make no dark design until Shoaib OKs a REEFCO-only exception.
  - REEFCO has no brand guideline kit, so Shoaib will build one.
  - Shoaib asked REEFCO for a master folder: brochures, labels, HD product photos, certifications, factory, events and shipping images.
  - All of this is recorded in the REEFCO KB `memory` and `graphic_rules` ("CLIENT PREFERENCES").
- **Shoaib approved both, 2026-10-10 ("ok"):**
  1. **The REEFCO-only DARK exception.** It is written into the REEFCO `social-post-design-lock` as an override; every other project stays light.
     - Implementation: `DARK=1` on `rf-series.ts`.
       - A low-key spotlight grade: an ellipse around the hero stays lit, and the text side is forced fully dark.
       - Text is white / leaf, with a leaf ribbon carrying dark text, leaf icon discs, and the white reversed logo `reefco/logo-white.png`.
     - The code-spotlight samples were rejected: Shoaib said "Darkness thek hai lakin aesthetics thek nai, AI se dobara".
     - The replacement uses 2 AI dark-studio scenes (Nano Banana Pro, ≈ PKR 78): `scenes/diclareef-dark-raw.png` and `fosbareef-dark-fix.png`.
       - The FOSBAREEF scene had "Made in Jordan" re-cased by the AI; the real "Made In Jordan" was patched back with tub-label-swap2 on a small rect.
     - Render with `DARK=1 NOSPOT=1`. NOSPOT skips the spotlight grade and only applies a soft top fall-off, so the header reads over the ceiling lights. The FOS column is `R=522` with rows from 868.
     - All 4 dark posts (DIC/FOS × AR/EN) pass, plus a board, in `v2-meezab-style/dark-samples/`. The brandbook PDF was rebuilt with them.
  2. **The brand guideline draft.**
     - `Reef-co/brand-guidelines/REEFCO_Brand_Guidelines_DRAFT_v0.1.pdf`: 9 A4 pages, 1.2 MB, also uploaded to the KB as a document.
     - Pages: cover, essence, logo, colours, type, elements, posts, dark style, photo + do/don't.
     - Built by `tadstyle/reefco-brandbook.ts` (resvg pages) and `pdftools/images-to-pdf.mjs` (mupdf). Pages are embedded as JPG; the PNG version was 61 MB.
- **Leaf-green variant (2026-10-10).** Shoaib said "Back ground mai agar yeah color dal k dekhyn" about the ribbon's leaf green, then "ai se naya scene banao leaf green mein… Product se reletive scene".
  - **First AI pair (≈ PKR 78), rejected:** chicks spilled into the text side, and the FOSBAREEF tub and label were REDRAWN, including a wrong logo.
  - **Second pair (≈ PKR 78), labels correct:**
    - DICLAREEF: chicks drinking from a white drinker (the product goes in drinking water);
    - FOSBAREEF T: a healthy broiler behind the tub, with a blurred ventilated house and fan.
    - Files: `scenes/*-leaf2-raw.png`.
  - **Gotcha:** the AI still drifted the products toward the centre. The fix was free reframing: DICLAREEF zoomed to 0.48 and anchored left with edge-copy extension; FOSBAREEF shifted 44 px.
  - **Render:** `LEAF=1` (darker text greens: forest #0C4521, deep #06180B, which pass on the leaf backdrop), `COLR=482`, `TOPBLEND=1` for FOSBAREEF.
    - FOSBAREEF info rows start at 806 because the leaf backdrop darkens downward.
  - All 4 (AR/EN) pass. Output is in `v2-meezab-style/leaf-samples/` with a board.
  - **FOSBAREEF follow-up fixes (Shoaib: "Resize", "Product per line a rai hai"):**
    - The tub was shrunk: the scene zoomed to h=1080, right-anchored, with top (140) and bottom (130) extensions blurred from edge-copy. The frame file is `scenes/fosbareef-leaf2-frame-small.png`.
    - Rendered with `COLR=556`.
    - The tub now sits fully above the band's leaf line.
  - **Brand posts in leaf (free):** "brand awareness posts bhi leaf green mein banao".
    - `LEAF=1` on `brand1`/`brand1ar`/`brand2`/`brand2ar`.
    - Leaf gradient canvas. The lab photo sits in a white-edged rounded frame on the right (not graded, so the white coat stays white), and brand2 tiles are white.
    - 4 posts plus `REEFCO_leaf_brand_board.png`.
    - **Brand2 packs:** Shoaib said "Background remove nai kiya".
      - `tadstyle/remove-bg.cjs` flood-fills the plain #F7F7F7 studio background from the border, producing `reefco/grp-*-cut.png`.
      - On LEAF, the cut-out packs stand straight on the green, bottom-aligned with a soft ellipse shadow, with no white cards. Feed Additives is a large icon disc.
  - **Prompt lesson:** state "Everything stays in the LEFT/RIGHT HALF, the other half completely EMPTY" and spell out the pack's exact shape and label elements. Even then, check the composition and reframe.
- **Mother's Day (2026-10-10).** Shoaib: "Mothers day ki bhi banao … meezab jaisi … insano wali".
  - Layout follows the Meezab Jummah post (layout only, NOT a Jummah post): a faint calligraphy watermark, a big script heading (Great Vibes for EN, Aref Ruqaa for AR), a message between thin leaf lines, a solid green "FROM THE TEAM AT REEFCO" / "مع تحيات عائلة ريفكو" block flush to the edge, a soft-3D hero in the opposite corner, and the footer. No product and no vet line.
  - **Hero images (Nano Banana Pro):** first a mother hen with chicks (≈ PKR 39, replaced), then a mother in a leaf-green hijab hugging her child (≈ PKR 39). The hero is generated on a plain #F4F9EE background, and the post background is sampled from it so there is no seam.
  - Script: `tadstyle/rf-mothersday.ts <en|ar> <hero> <out>`. Output: `Reef-co/social/occasions/REEFCO_MothersDay_{EN,AR}.png`.
  - **Copy is a draft.** The Arabic greeting عيد أم سعيد / إلى كل أم تمنح الحب والرعاية بلا حدود is standard Arabic, not from REEFCO documents, so Khaled should confirm it.
  - **Date:** Jordan/Arab Mother's Day is 21 March (2027-03-21). The international date is the 2nd Sunday of May.
- **Become a Distributor (2026-10-10, free).** Script `tadstyle/rf-distributor.ts <d1|d2> <en|ar>`, output in `v2-meezab-style/distributor/` with a board.
  - **d1 (invitation):** a code-drawn abstract supply-network globe (meridians, hub pin and dashed route arcs; no country shapes) and a CTA pill.
  - **d2 (complete range):** REEFCO's real full-range photo with the background removed (`remove-bg.cjs` with `POCKETS=600` to also clear enclosed bg pockets) and a soft shadow.
  - EN = KB-approved copy (posts 3 and 4). AR = catalogue clauses, plus 4 TYPED lines for Khaled to confirm: فرصة شراكة · كن موزعاً لريفكو · مطلوب موزعون · تواصل معنا اليوم.
- **Next products:** reuse the approach. Generate a 4:5 scene with named calm zones, measure the bands, lay out with the gate, and check the label at 100%.

- **Hero posts (2026-10-12, free, code only).** Shoaib: "Ab in heroes se REEFCO ki post banao, kuch shapes … professional".
  - Script: `tadstyle/rf-hero-post.ts <dic|fos> <ar|en> <out>`. Output: `Reef-co/social/hero-posts/` (4 posts and a board).
  - Background: the deep REEFCO-green studio (the dark exception).
  - Shapes:
    - a lit halo disc, a dashed ring and a leaf arc with an end dot;
    - a dot grid and leaf line-dots;
    - a curved bottom band with a leaf edge;
    - a floor glow plus contact shadow under the MASTER HERO cut-out (see [[hero-packshot-ai-topaz]]).
  - Layout:
    - AR: pack on the left, text right-aligned;
    - EN: pack on the right, script accent.
  - Copy is the approved rf-copy.json text. Arabic is RLE-wrapped.
  - The vet line is two runs, because mixed AR+EN in one RTL string flipped the English full stop.
  - Numbers stay glued to their unit, because "٢٥٠ غم" broke across lines.
  - All 4 posts pass the gate.

- **Scene posts — the CLIENT LIKED them (2026-10-12).** Khaled: "That's Better, thank you".
  - The style: an AI scene, a Topaz realism pass, the master hero cut-out and the dark green text side. Files: `hero-posts/scene/`; built per [[hero-packshot-ai-topaz]].
  - The 4 posts are saved to the KB as reference images, and the feedback is in the KB memory.
  - Shoaib asked Khaled "do i lock this layout for future posts?". The answer is pending, so don't lock `social-post-design-lock` until he confirms.

**Next:** share the boards with the REEFCO WhatsApp group (Raneem, Khalid), let them pick a style, then use that style for the next products.

Related: [[feedback-content-ours]], [[feedback-no-batch-without-ask]], [[graphics-studio-project]].
