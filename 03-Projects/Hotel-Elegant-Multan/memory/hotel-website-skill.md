---
name: hotel-website-skill
description: A reusable Claude Code skill was distilled from this build for making future hotel websites
metadata: 
  node_type: memory
  type: reference
  originSessionId: 9511844e-13c0-42ff-86af-800dc65bec3e
---

The entire Hotel Elegant build was captured into a reusable **user-level Claude
Code skill** at `C:\Users\Abdul Ahad\.claude\skills\hotel-website\` (created
2026-07-20). It is available in **any** project, not just this one.

Purpose: when the user starts a new hotel/guesthouse website, the `hotel-website`
skill triggers, runs an **intake questionnaire** (hotel identity, brand, rooms,
policies, media, Supabase/GTM/GA4 keys, deployment target), then reproduces this
whole setup — no need to re-prompt everything from scratch.

Structure: **one single self-contained `SKILL.md`** (~560 lines) — the user
explicitly wanted everything in one file, not split into reference files. It
covers: intake questionnaire, 8 build phases, core architecture, tech stack +
structure, full tested Supabase schema + RLS + admin_users seeding, public
site/booking flow, SEO/schema/AEO, admin panel (incl. the mobile "app" shell:
bottom tab bar + app header + More bottom-sheet), signature features (offer price,
hero video, deep room content, gallery ring, blog), GTM/GA4 analytics,
deployment, and a consolidated 16-item gotcha list. If asked to add new
work, fold it into the matching section of this one file (do NOT re-introduce
separate reference files).

**Keep it in sync (user instruction, 2026-07-20):** The user wants this skill to
stay current with the live build. When they say to update the skill (they'll ask
explicitly — it's on-demand, not automatic), fold whatever new features or fixes
we've since made to the Hotel Elegant site into the matching `references/*.md`
file (or SKILL.md phases/gotchas), so future hotel builds don't miss later
implementations. **Why:** they'll keep improving this site and don't want the
reusable skill to drift behind it.

If the user wants to improve/extend it later, edit those files. See
[[security-and-admin-ops]] and [[project_hotel_elegant]] for the details it was
built from.
