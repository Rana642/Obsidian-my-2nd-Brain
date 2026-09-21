---
name: hss-working-style
description: "How the client wants the build to proceed (phase-by-phase, plain-language explanations)"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 48f648cb-9c79-4134-90c6-bf52b9121542
---

The client (hotel owner, super-admin email shoaib.nabi.noor@gmail.com) gave explicit working instructions for this build.

**Why:** They are following along and testing each stage themselves; the old live site must keep running untouched until the new site is fully tested.

**How to apply:**
- Build phase by phase (per [[hss-build-status]] plan), pause at the end of each phase and let them test before starting the next.
- Before coding each phase, briefly explain in simple language what is being built and why.
- Ask before proceeding when a genuine client decision arises (e.g. the unconfirmed email address).
- Never modify or point anything at the old live site; domain switch happens only at launch.
- Content language: ALL site content in professional English (client decision 2026-07-04, overriding the Roman Urdu mix in the original content doc). Translate the doc's copy — keep the warm, inviting tone and all specific facts/prices. E.g. "WhatsApp Us — Confirm in 2 Minutes", "Book Direct With Us".
- Design language (client decision 2026-07-08, matched to their PREVIOUS website — supersedes the earlier gold-heading direction): headings = Poppins bold (font-heading, weight 600 via globals + font-bold where large), body = Poppins, accent subtitles = Playfair Display ITALIC in gold (SectionHeading subtitle style); headings WHITE on navy sections / NAVY on light (never gold); NO divider bars under headings; buttons FLAT gold bg-gold hover:bg-gold-rich (no gradients); "Discover Our Story" = navy pill (rounded-full bg-navy); amenities = navy section w/ royal cards + bare gold icons ("Premium Amenities"). Locked palette unchanged.
