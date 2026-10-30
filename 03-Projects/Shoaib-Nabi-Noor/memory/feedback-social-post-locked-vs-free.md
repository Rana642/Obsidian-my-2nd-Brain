---
name: feedback-social-post-locked-vs-free
description: "AI-generated social posts (Tad Pharma, likely every KB brand) — lock only content, logo, original product, footer strip, brand colours; let the AI design scene/background/layout"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-02T10:26:37.692Z
---

Shoaib's rule, set 2026-09-30 after testing Tad Day 11 in ChatGPT: "Jo cheezain Exact hain wo sirf yeah…".

**Locked:**
1. The written content, character for character.
2. The logo.
3. The original product. It must look identical even when the AI re-renders it.
4. The footer strip, plus the vet line **on product posts only** (see below).
5. The brand colours.

**Caution line only on product-related posts — a GENERAL rule (Shoaib, 2026-10-02)**
- What he said:
  - "ye line sirf product se posts mai ani chaiye Baki Type ki posts mai iski zarorat nai";
  - then, when I asked about a single day: "kisi 1 Date ki baat nai… sirf Product se related posts per aye".
  - So don't ask him day by day — apply the rule yourself.
- The line is "Vet — Not for human use. Veterinary use only.", and the same goes for any caution or disclaimer line.
- **Product-related** means any of:
  - the post has a `product` field;
  - a product photo is in `attach` (Tad's Education days have only the photo);
  - the image text names one of the project's products (Meezab Day 9 storage tip).
- **Everything else gets no caution line**, in either the image or the caption: brand, certifications, general tips, dealer/B2B, Jummah/occasions, greetings, events.
- **Global rule** `product-caution-lines` in kb_global_docs, plus a line in `KB_SERVER_INSTRUCTIONS` (aa5ba59), cover every project and every post type, including planner captions I write myself.
- Code (b7bc5ad) — `designBriefBlocks`:
  - for a non-product post, sets the lock's `vet_line` to "NONE";
  - STEP 3 says "NO vet line";
  - drops the vet line from the caption.
- KB data updated the same day:
  - lock rules 1 and 4 and `vet_line` in the Tad, Meezab and REEFCO locks;
  - the calendar "Locked" bullets;
  - 16 non-product calendar captions;
  - REEFCO `gradients-and-strips`;
  - global `content-calendar-day-requests` step 5.
  - Script: scratchpad `vetfix.cjs`.

**Free:** "Baki Manzar Back Ground Styling kerny do issy". The AI decides the scene, background, layout and styling, and the product may stand inside the scene.

**Why:** the first batch, which had a strict layout and no scenery, looked generic and not poultry. The Day 11 test with an AI farm scene looked premium, even though it broke the "product below the band" rule.

**How to apply:**
- In design locks and calendars, keep hard rules only for the 5 locked items.
- Write layouts and backgrounds as suggestions (`layout_idea`, `background_idea`).
- Keep a short safety `do_not`: no extra text or other brands, no sick or deformed animals, no invented claims.
- When reviewing a generated post, flag only locked-item deviations and safety issues, not layout choices.
- On 2026-09-30 ("dono ka Pora Calender dobara banao isi tarah Day 1 se") both Tad and Meezab were rebuilt this way with scratchpad `tadposts/rebuild.cjs`:
  - Shared lock builder.
  - Every day 1–30 has a poultry or farm `background_idea`. Livestock appears only where the post's own text names it.
  - The script refuses to save if text, captions or attachments change.
- The reference look is Tad Day 11.

**Variety rule (2026-09-30).** The first brief posts all came out as one template: scene on top, product on the right, text on white. Shoaib: "Professional Graphic designer ki tarah Different layout ... Product kis problem ka Solution hai wo relection".
- Both locks now have 8 `layout_styles`:
  - A: hero centre
  - B: problem→solution split
  - C: dark editorial
  - D: callouts
  - E: scientific
  - F: typographic
  - G: bento tiles
  - H: circle frame
- Every brief post and calendar day has a `style` and a problem/solution `background_idea`. Neighbouring calendar days never share a style.
- Illustrations are flat and in brand colours, with no labels or extra text.
- White text is allowed on dark brand panels.
- Script: scratchpad `tadposts/styles.cjs`.
- **Never write one identical layout_idea for many posts again.**

**Product brief posts (2026-09-30):**
- One social post per product, separate from the calendar. Shoaib said the carousel comes later.
- Only the important points from the PDF brochure; English for now.
- Stored as marketing_doc `product-brief-posts` with the JSON `{english:[...], urdu:[], mix:[]}`. Categories are extensible.
- Builder: scratchpad `tadposts/briefs.cjs`.
- Shoaib approved the Aminotox sample ("Thek hai Baki bhi bana lo"). The same day all 26 were written, each from its brochure pages: Tad 17, Meezab 9.
- Content source: scratchpad `tadposts/briefcontent.cjs`.
- Tool: `kb_get_product_post` (commit e80db61) shares `designBriefBlocks` with `kb_get_social_post`.
- Content rules followed:
  - Tad FCR is only ever "increases", so it is left out where the English page says "improves".
  - E-Se 100's "Bitter Diseases" wording is avoided because it is pending.
  - Niflorise's Colistin write-up is left out, because Colistin is not in the composition.
  - Urdu-only lines are never used.
- Local handler test: `npx tsx --conditions=react-server --env-file=.env.local scratchpad/tadposts/tooltest.ts` with a dummy `MCP_OAUTH_SIGNING_KEY`, which exists only on Vercel.

Related: [[tadpharma-knowledge-base]], [[meezab-knowledge-base]].

**Product crop (Shoaib, 2026-10-03):**
- Product spotlight and product brief posts show the whole pack.
- Other posts (brand, education, tips, range, dealer) may crop it: half the pack entering from an edge, or a zoom on part of the label.
- What shows stays identical, and the product name stays readable.
- This is in the global rule `layout-alignment` and in the Tad lock.
