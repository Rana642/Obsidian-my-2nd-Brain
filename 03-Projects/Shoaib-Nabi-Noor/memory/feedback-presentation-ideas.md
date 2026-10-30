---
name: feedback-presentation-ideas
description: Global rule presentation-ideas (2026-10-03) — 9 presentation patterns (incl. blob_headline_container) + shapes library from Shoaib's inspiration posts; every post prompt gets one, rotated with no back-to-back repeats
metadata:
  type: feedback
---
Shoaib, 2026-10-03, shared 8 animal-health posts from other brands as presentation inspiration: "Content nai Back grounds Shapes Content ko presentation kaisy kiya ja sakta". The brands were GowMithra, WIXBIO, AIMIL, a vaccination-profits post and Cheptel.

**What was built**
- KB global rule `presentation-ideas`: a ```json block with 8 patterns and a shapes library.
  - Patterns: hub_and_petals, hero_with_info_column, did_you_know_frame, big_type_icon_list, side_by_side_cards, framed_photo_icon_pills, thick_frame_ghost_background, carousel_series.
  - Each pattern has `best_for` content types: brand awareness, product spotlight, product brief, education, tip, range, dealer, carousel.
- KB MCP (8cebbd2, a32a1f2): every calendar and product post prompt gets `presentation_idea_this_time`.
  - The series is walked in order, each post one step further round the ring, and the previous post's idea is never taken.
  - Simulated on Tad: 0 back-to-back repeats across 30 days and 17 product posts.

**Translation rules**
- Take only the presentation idea, never the other brand's words or layout.
- Dark bands become white or glass.
- Brand colours only.
- The project's do_not wins (Tad: no QR codes, no sick animals, no before/after).
- An idea never adds words.

**Why:** Shoaib wants varied, professional presentation drawn from real-world references, not one template.

**How to apply:**
- When Shoaib shares more references, add patterns to the rule's JSON (key, name, best_for, structure, shapes).
- Never store the other brands' images or copy.
- Related: [[feedback-layout-alignment]], [[feedback-design-glass-light]].
