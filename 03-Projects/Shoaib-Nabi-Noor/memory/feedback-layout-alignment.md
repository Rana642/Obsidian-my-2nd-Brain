---
name: feedback-layout-alignment
description: Global rule layout-alignment (2026-10-03) — one shared left edge, equal cards, no empty cards, logo ~30% width for wordmarks, self-check (quality gate) in every post prompt, all projects
metadata:
  type: feedback
---
Shoaib, 2026-10-03, on a Tad Product Spotlight draft: "Content alignment dekho zara aur content presentation". Earlier the same day: "Logo thora Chota kero skills mai se dekh lo kitna hona chaiye".

**The rule (KB global rule `layout-alignment`, for every project)**

Grid:
- one shared left edge for the headline, points, icons, chips and logo — or one centre line in a centred layout;
- equal-height cards with equal gaps;
- exactly as many cards as text items, never an empty card;
- headline at most 2 lines;
- points at most 3 lines;
- one icon set, with each icon clearly showing its object.

Logo:
- a wide wordmark is ~28–30% of the width (~300–320 px on 1080);
- a square or stacked logo is ~15–18%;
- clear space is at least the logo's height.

Self-check: score the image 0–100. Pass is 70+ with every word correct. If it fails, make it again once with "fix only: …" and show the better one.

**Where it lives**
- The KB MCP (commit 77dd740) adds `layout_rule_every_project` and `layout_plan` (counted from text_on_image) to every post prompt, next to `design_rule_every_project`.
- STEP 3 of those prompts is the self-check.
- The Tad lock got an ALIGNMENT line, and its stale "solid dark panel" lines were removed.

**Why:** the draft had a headline that did not line up with the logo, an empty card, a squeezed dose card, uneven points and an egg icon drawn as a "0" ring.

**How to apply:**
- Any post generator (Graphic Studio, ChatGPT through the KB MCP, scratchpad pipelines) must follow this rule and self-check before showing an image.
- Related: [[feedback-design-glass-light]], [[feedback-social-post-locked-vs-free]].

**2026-10-03 update (Shoaib's design videos, "kero")**

The rule now also carries:
- think first: audience, one purpose, one clear message, a psychology trigger;
- rule-of-thirds composition types;
- minimal: every element earns its place, one light source, tonal shading, don't overdo it;
- the product crop policy.

Self-check point 8 is "design thinking". The KB MCP self-check line names these (f535a40).

Social proof is REAL only. One video suggested fake reviews; that is refused.
