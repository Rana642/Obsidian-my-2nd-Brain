---
name: feedback-google-friendly-pace
description: "Standing rule — Google Business Profile replies/posts/edits never in bulk; 5-min gap + 20/location/day, enforced in code and in the KB global rules"
metadata:
  node_type: memory
  type: feedback
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-29T21:14:51.106Z
---

Never publish to Google (GBP review replies, posts, offers, photos, profile edits) in bulk. One write at a time, at least **5 minutes apart across all projects**, at most **20 per location per 24 h**, and every reply written individually (no copy-paste text).

**Why:** Shoaib (2026-09-30) wants everything to look like a person doing it by hand, so the Google account and the Socially Snap app stay in good standing. He asked for it to be locked in so it's never forgotten.

**How to apply:** enforced in `lib/gbp.ts` → `paceGbpWrite()` / `reserveGbpWrite()` with the `gbp_write_log` table. Every new GBP write feature (posts, media, profile edits, Planner → GBP publishing) MUST go through `paceGbpWrite`, never call the API directly. The rule also lives in the KB global rule `google-friendly-pace` and in the MCP server instructions/tool descriptions. For "reply to all" requests: draft all, publish one, say when the next can go. Same no-burst spirit for Meta/LinkedIn/TikTok automation. Related: [[adsbyshoaib-gbp-integration]].
