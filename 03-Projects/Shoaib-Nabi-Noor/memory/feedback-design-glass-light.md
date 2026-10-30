---
name: feedback-design-glass-light
description: "ALL projects' posts/ads/graphics — light, never dark; NO glass of any kind (banned 2026-10-09); text straight on a calm part of the photo, no cards/bars/pills; generate scenes with negative space for the text"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-29T08:43:35.977Z
---

**Exception: an established brand look wins (Shoaib, 2026-10-29).** These are GENERAL defaults. A brand that already runs on its own branding keeps it. Shoaib: "kuch brand k liye hum rules change ker sakty hain magar jiska… pehly se apni branding se chal raha hai isko continue kerna hai". First case: **Choppers stays black/charcoal + golden yellow + white**, with its yellow footer bar ([[choppers-knowledge-base]]). The rules below apply where a brand has no look of its own yet. This doesn't settle REEFCO by itself: it had no brand kit, and only the client asked for dark.

**Current rule (2026-10-09), for every project's posts, ads and graphics:**
- **Light, never dark.** No dark backgrounds, panels or overlays. Dark colours are for text only. Brand colours are accents.
- **No glass of any kind.** No frosted or glass cards, glass bars, transparent shades, blur behind text, code "calm space" blur or blurred light-lift. Shoaib: "Glass effect 1 dafa k liye bilkul khatam ker do".
- **Text sits straight on the photo**, with no cards, bars or pills around the edges. Only the locked footer strip, the vet/caution line and small icons are allowed as shapes.
- **Readability comes from the scene.** Generate it with a real calm, light, empty area where the text will go. Then apply the size floor, colour rules and contrast gate in [[feedback-text-readability]].

**Writing the scene prompt (from 2026-10-08):**
- Say where the text goes, e.g. "right 55% calm, light, almost plain, no objects, lines or strong colours; top band calm for the logo".
- Keep the busy story (drinkers, birds, equipment) on the product's side or in the soft distance.
- If a generated scene is too busy, fix it with one edit that clears the text area before designing. Ask about the cost first ([[feedback-ask-before-spend]]).
- Shoaib, 2026-10-08: "Shapes Zayada na use kero Main image per likhny ki try kero … isi tarah image generate krwaya kero ta k ap k lihny k liye feasible ho".

**Why:**
- Panels and bars make posts look like templates; text on a calm part of the photo looks premium.
- He wants a professional look that comes from each post's topic.

**Where it is enforced:**
- KB global rules `design-glass-light` and `text-readability` (both rewritten 2026-10-09). Every post prompt (kb_get_social_post, kb_get_product_post, kb_get_occasion_post) embeds the rule as `design_rule_every_project`.
- Overrides appended to the REEFCO, Tad and Meezab `social-post-design-lock` docs and to the Tad and Meezab `design_system`.
- adsbyshoaib 57d9236 (MCP wording); graphics-studio 1e4235c (GLOBAL_DESIGN_RULE).
- **Not covered:** the website dashboard's own glass UI ([[adsbyshoaib-dashboard-glass-ui]]) is a separate thing and stays.

**History, in short:**
- 2026-10-02/03: "light and glassy, never dark" for all projects, after the Tad samples kept getting a black panel (lock style "C_dark_editorial", since renamed). Shoaib: "tad nai har all projects k liye".
- 2026-10-08: glass was made optional ("Hard color k bijaye transparency kam ker di jaye").
- 2026-10-09: glass banned completely after the blurred light-lift on REEFCO DICLAREEF.
- 2026-10-03 Tad-only wording, still in force: "importer and distributor", never "sole"; no product count; no principals on brand posts (Tad's brand_position doc).

Related: [[feedback-cool-white-not-warm]], [[feedback-layout-alignment]], [[feedback-social-post-locked-vs-free]], [[feedback-plain-english]], [[hero-pack-label-swap]].
