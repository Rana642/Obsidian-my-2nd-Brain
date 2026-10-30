---
name: shoaib-two-pc-workflow
description: Shoaib works on this repo from two PCs / Claude accounts — always fetch/rebase before starting and before pushing
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-25T07:49:49.860Z
---

Since ~2026-09-10 Shoaib runs this project from a second PC on a Pro
account as well as this one. On 2026-09-25 a push from here was rejected:
origin/main had 69 commits from the other machine (social poster, API
vault, KB/MCP tools, the 2026-09-15 official-logo rebrand) that this clone
had never seen.

**Why:** working on a stale clone means building against the wrong design
system/logo and hitting conflicts at push time (the rebrand changed brand
colours and the logo rule in CLAUDE.md).

**How to apply:** at the start of substantial work run `git fetch` and
`git log HEAD..origin/main`; if behind, `git pull --rebase` (then
`npm install` — the other PC adds deps) before building. Never force-push.
Don't commit `package-lock.json` churn that only comes from this PC's npm
version (`"peer": true` flags) — restore it. Memory on this PC can lag the
other PC's; the repo's CLAUDE.md is the shared source of truth.
