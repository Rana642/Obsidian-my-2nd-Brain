---
name: social-design-system
description: Social Design System (Tad first, structure for all projects) — code-drawn post skeleton v3, pending decisions, what the 5 design videos + 2 repos added, DESIGN.md as the final format
metadata:
  type: project
---
**Goal (Shoaib, 2026-10-03):** "Pehly Social media Design System final kerty hain Ta k posts Start ker skyn".

**State on 2026-10-03:** OpenAI API credits are at zero, so all of this was built with code at Rs 0.
- The prototype is in `graphics-studio/scripts/social-design/` (commit 9429052):
  - `design-system.ts` renders v3 post A (logo top-left) and post B (logo top-centre), plus the glass tint sheet;
  - `footer.ts` draws the exact footer strip.
- **Fonts:** static Archivo, whose resvg family name is **"Archivo SemiBold"** (weights 700/800/900); plain "Archivo" silently falls back to Inter. Inter 400 and 600.

**Decisions Shoaib made**
- Glass may come in other brand tints: white, pearl, blush, red (one key chip) and mist.
- The logo goes only at the top (top-left or top-centre), never in the footer, and without the white pill.
- Crop policy (also in the Tad lock): spotlight and brief posts show the whole pack; brand, education, tips, range and dealer posts may crop it.

**Decided on 2026-10-03:**
- Logo: BOTH places.
  - A (top-left, on the 72 px margin): spotlight, brief, education, tips, range, dealer.
  - B (top-centre): Jummah, days, events, brand awareness.
- Layout AND scene are made by AI for variety ("Layout aur scenes dono ai per chor dyty hain ho sakta hai verety aye har bar"). Only the logo, footer, vet line, real product and exact words are fixed.
- Shapes and icons must be at professional-designer level: one line-icon set in red circles, no clichés, geometric purposeful shapes, one light source.
- Where this lives:
  - KB global rule `layout-alignment`: "Who decides the layout" and "Shapes and icons" sections;
  - the Tad lock: logo item 2 rewritten for A/B, plus a new `design_system` key;
  - KB doc **design_system/main** = the Tad DESIGN.md;
  - occasion prompts carry `logo_place` + `design_system` (d4eb5ca);
  - scratchpad generate4 has SHAPES_ICONS and logoCentred().

**What the 5 design videos added** (into the KB global rules `layout-alignment` and `presentation-ideas`):
- rule-of-thirds composition types;
- think-first (audience, one message, REAL-only social proof — the video suggested fake reviews, rejected);
- every element earns its place;
- one light source;
- tonal shading;
- a podium and a glow behind the hero;
- a blob-headline pattern (now 9 patterns).

**Repos reviewed**
- **Anil-matcha/Open-AI-Design-Agent:** a thin FastAPI proxy to the paid MuAPI hosted agent. The planner is not in the repo, so there is nothing to self-host. Skip it; the idea plan → route model per asset → approve/reject jobs is already in Graphic Studio.
- **VoltAgent/awesome-design-md:** DESIGN.md files (Google Stitch format: YAML tokens + overview, colours, type, layout, depth, shapes, components, do's/don'ts, iteration guide, known gaps) for web brands. Use the FORMAT for each project's social design system doc in the KB. Never copy another brand's identity.

**Next:** when the OpenAI credits are back, make one sample per content type with generate4 (AI layout + scene; code logo A/B + footer + vet line), Draft → approve → Final.

Related: [[feedback-layout-alignment]], [[feedback-presentation-ideas]], [[feedback-design-glass-light]], [[graphics-studio-project]].

**AI-written "senior designer rules" guide (pasted by Shoaib, 2026-10-03)**

Adopted into `layout-alignment`:
- the 3-second rule;
- headline about 3× the body size;
- 60-30-10 (Tad v3 measured 7.5% red, 84% neutral);
- 9:16 safe zones (250 px top and bottom, no logo bottom-right on Reels);
- a mono logo only if the client supplies an official one.

NOT adopted, because each conflicts with a locked decision:
- **Dark footer as the anchor:** our rule is no dark; the Tad strip is a solid red gradient, which already anchors.
- **Footer of max 3 items (web, social, CTA):** the footer is locked to phone, WhatsApp, email and web; for Pakistani B2B buyers, phone and WhatsApp convert.
- **10% margin:** we use 72 px (6.7%), and the Instagram 3:4 grid crop only takes about 34 px per side.
- **Logo 5–8%:** ours is 2.8% of the height and about 0.8% of the area, so it is already smaller.
- **"Dark aesthetic" prompt:** never.

Open item: ask Tad Pharma for an official white/mono logo version.
