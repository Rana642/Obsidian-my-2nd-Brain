---
name: meezab-knowledge-base
description: Meezab Z. International KB (same client as Tad Pharma) — built at office PC in Supabase, not git; status and what's left
metadata:
  type: project
---

Meezab Z. International (project f15de242-fe7f-433f-b549-f68439888be0, client Ahmed Jahanzaib Shah — same client as Tad Pharma, see [[adsbyshoaib-tad-pharma-knowledge-base]]) was built on the office PC on 2026-09-29 entirely in the dashboard DB (knowledge base), NOT in any git repo. Its own KB `memory` doc + `system_rules` doc hold the details (website product data is NOT a source; keep printed values as-is; NAP from meezabz.com: 061-2111031, WhatsApp +92 333 6875033, info@meezabz.com).

Built: nap/brand_position/icp/pain_points/graphic_rules/brand_kit (Meezab Teal Strip)/system_rules, 9 products (Aminoreef Sol, Diureef Plus, Fegatosim, Laxi-Zyme Plus, Mentofresh, Recal Fos, Reeftox Sol, Reevit AD₃EK Sol, Reevit E-Se 150), 28 A4 presentation JPEGs (no Design canvas), datasheet-template, social-post-design-lock, content-calendar-launch-30 (English). All APPROVED by Shoaib 2026-09-29 (docs, 28 pages, design lock, calendar).

Done this PC 2026-09-29: CDN publish of 67 images (kb assets repo commit c75b542, path meezab-z/), calendar links swapped → kb_get_social_post returns images; Urdu re-proofread word by word for all 9 (fixes in Recal Fos, Diureef, AD₃EK), then Shoaib approved → VERIFIED; page 3 of all 9 now typed Jameel Noori (generator scratchpad mzp/gen.mjs, CDN a325914).

Also 2026-09-29: Shoaib dropped raw pack photos + brand kit in `Shoaib Nabi Noor/Meezab Knowledge base/` (untracked, don't commit). Added as reference_image + "Pack label (real product)" sections (script scratchpad mz_pack_kb.ts, run from repo root); hi-res logo asset added; Design canvas https://claude.ai/artifact/JiFMXopaE369488ASsSnW4 holds all 28 pages.

Later 2026-09-29 Shoaib ruled: "Jo information Raw products aur pdf per hai hamesha wo use kero" — pack + brochure are both sources, **pack wins on conflict** (applied: Recal Fos composition, Fegatosim MgSO4 40,000, Laxi-Zyme 10⁹ counts, AD3EK 1 L + 5 L). And "logo yehi hai isi ko apny mutabiq bana lo" — built clean mark + 3 lockups (horizontal light/dark, stacked) from the real mark. 8 pages rebuilt with scratchpad mzp2/gen.mjs (a faithful port of the office layout, spec.json per page); CDN b8ddf72; canvas boards updated.

Rule (Shoaib 2026-09-29, Meezab only): a product counts only if its PDF brochure exists; pack-photo-only products (e.g. the one in "New folder (6)") are ignored. Tad Pharma is not affected.

Still open: meezabz.com product pages wrong (site code not on this PC).
