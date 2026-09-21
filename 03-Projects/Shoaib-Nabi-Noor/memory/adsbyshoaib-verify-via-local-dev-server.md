---
name: adsbyshoaib-verify-via-local-dev-server
description: "When browser-verifying uncommitted code changes on this project, use the local dev server, not the production URL"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-27T12:04:31.248Z
---

Browser-verifying an uncommitted change against `adsbyshoaib.com` directly
tests nothing — that domain serves the last deployed (pushed + built)
version, not the local working tree.

**Why:** During the Agreement-clauses feature (2026-08-27), `preview_start`
with `{url: "https://adsbyshoaib.com/agreement/..."}` silently rendered
the OLD code path (no error, just wrong behavior — old rendering, not a
crash) because that day's changes weren't deployed yet. Caught it before
drawing conclusions, but wasted a round trip.

**How to apply:** For any live-smoke-test of code not yet pushed, use
`.claude/launch.json`'s `adsbyshoaib-dev` config (`npm run dev`, port
3000) via `preview_start({name: "adsbyshoaib-dev"})`, then navigate
`localhost:3000/...` — never the production domain — until the change is
committed and deployed. Production URLs are fine for checking
already-shipped behavior or for the user's own review, just not for
verifying same-session edits. See [[adsbyshoaib-proposal-agreement-funnel]]
for the feature this came up on.
