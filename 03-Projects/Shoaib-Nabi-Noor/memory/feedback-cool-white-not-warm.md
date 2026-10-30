---
name: feedback-cool-white-not-warm
description: "2026-10-09 Shoaib wants scenes cool and white, not warm/beige — \"beige ki jaga shed White kero har jagah, Warm feel nai cool feel ani chaiye\"; grade whole photo cool, keep product/logo colours"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-12T07:43:03.740Z
---

On REEFCO FOSBAREEF T (2026-10-09), Shoaib first asked "is shed ko White kero zara warm k bajaye". After the wall and floor were neutralised, he asked again: "beige ki jaga shed White kero har jagah, Warm feel nai cool feel ani chaiye".

**Rule:** scenes look cool and white everywhere: walls, tables, ceiling and floor. No beige, cream, warm wood or golden-morning cast. The product label, the logo and other truly coloured things keep their real colours.

**How to apply:**
- **Generating:** ask for "cool, clean white daylight, neutral-to-cool white balance, white or light-grey surfaces, no beige, cream or warm tones". Don't use "warm morning light" or "birch wood". DICLAREEF's warm scene was approved before this rule.
- **Fixing a warm scene (free):** run `scratchpad/tadstyle/cool-grade.cjs <in> <out>`. Per pixel, the less saturated it is, the more of its warm cast is removed (keepChroma 0.12). Then it brightens ×1.04 and sets a cool white point (B ×1.035, R ×0.985). Saturated pixels such as the label, logo and combs are protected by a sat 0.32–0.55 smoothstep.
- This is a colour grade of the photo itself, not an overlay, so it doesn't break the no-glass rule ([[feedback-design-glass-light]]).
- **Scope: ALL projects.** Shoaib, same day: "sab projects k liye rule bana do aur DICLAREEF bhi cool kero". Where it is enforced:
  - appended to KB global rule `design-glass-light`, which rides in every post prompt and overrides warm wording in locks;
  - graphics-studio 88585d5 (`GLOBAL_DESIGN_RULE`);
  - adsbyshoaib 3f58f92 (`KB_SERVER_INSTRUCTIONS`).
- DICLAREEF was re-graded to v11-cool, which is now the KB reference asset.
- **2026-10-12, Tad Biofurose Urdu:** "Back Ground iska white keryn zara". A cool grey-blue scene also counts as not white enough.
  - Fix (free): `scratchpad/tadstyle/white-lift.cjs <in> <out> 0.6 0.4 150 215`. It lifts only light tones toward white and cuts their grey-blue cast to a faint cool hint. Combs and drinkers stay as they are.
- **Same post, he circled out-of-focus ceiling lamps (white bokeh blobs) in a corner beside the logo: "yeah kaya hai?".**
  - Check corners and edges for stray bright blobs before showing.
  - Fix (free): `scratchpad/tadstyle/fill-corner.cjs`, a per-row fill from the clean wall beside the blob, with feathering.
- **Same post: "ye gray nechy bura lag raha hai".** A grey table front and a dark under-table band behind the footer/vet line look bad.
  - Fix (free): `scratchpad/tadstyle/extend-surface.cjs <in> <out> <Y> <S0,S1> <feather>`. It continues the white table top to the frame bottom, using the per-column table colour with a gentle fall-off.
  - Biofurose settings: 1114 / 1088,1110 / 4. Put Y a few px ABOVE the table edge, or a faint seam stays.
  - In scene prompts, ask for the white surface to run to the bottom edge with no table front or dark gap.
