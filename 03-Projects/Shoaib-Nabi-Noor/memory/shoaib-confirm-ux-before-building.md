---
name: shoaib-confirm-ux-before-building
description: "Shoaib wants layout/UX restructuring described and confirmed before implementation, not built straight away"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-27T09:34:21.330Z
---

For a meaningful UI/UX restructuring (not a small copy tweak or bug fix), describe the proposed
layout/flow in text first and wait for confirmation before writing code — even when the
underlying feature intent is already clear.

**Why:** During the adsbyshoaib.com dashboard work (2026-08-27), after building a flat Service
Charges list with a per-row "Project" dropdown, Shoaib said it was clunky and explicitly asked
to merge Projects and Service Charges differently — ending with "pehly batao kaisa hai phr poch
k implement kerna" ("first tell me how it looks, then ask, then implement"). He had a specific
nested-list mental model in mind that a text description surfaced and let him correct
(confirming Tools & Subscriptions should stay flat) before any code was written, avoiding a
second throwaway implementation.

**How to apply:** For layout changes, new multi-section forms, or reorganizing how existing UI
pieces relate to each other, write a short structural description (or ask a clarifying question)
first. Small isolated fixes (a mislabeled field, a copy tweak, a straightforward bug) don't need
this — go ahead and implement those directly, matching this project's overall fast iterate-and-
verify pace. See [[adsbyshoaib-proposal-agreement-funnel]] for the specific feature this came up
on.
