---
name: shoaib-two-pc-workflow
description: Shoaib works on this repo from two PCs / Claude accounts — always fetch/rebase before starting and before pushing
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-11-02T07:56:36.675Z
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

**2026-11-02: the transcript mirror was switched off permanently** (Shoaib: "Copy process ko … stop ker dyn … Permanently").
- The scheduled task "ClaudeCodeToGoogleDrive" was robocopy /MIR of `~/.claude/projects` to `H:\My Drive\ClaudeCodeSync\projects` every 30 min, on the office PC.
- It popped up a cmd window with ERROR 3 because Google Drive for desktop wasn't running, so there was no H: drive.
- The task is now **Disabled**, not deleted. To turn it back on: `Enable-ScheduledTask ClaudeCodeToGoogleDrive`.
- The copy already on Drive stays.
- PC-to-PC continuity runs only through the Obsidian vault memory and the repo `docs/WORK-LOG.md`.
- The vault note `00-Meta/Multi-Device-Setup.md` still describes the mirror. Don't edit vault notes without asking.

**2026-11-02: C: cleanup on the office PC** (128 GB SSD, C: 1.6 → 19.5 GB free).
- Done: caches (npm, Chrome, Edge, CapCut); Filmora + NativePush + Copilot uninstalled. Filmora exports were backed up to `F:\Backups\Filmora-Output-2025`.
- Moved, not deleted, to `G:\Backups\C-cleanup-2026-11-02`: VS Code's 11 leftover `.<uuid>` extension-update folders (6.1 GB) and Codex's old `.cache\codex-runtimes` copies (1.7 GB). **Delete that folder after about 9 Nov** if VS Code and Codex still work.
- Dead ends:
  - Hibernation was already off (no hiberfil.sys).
  - Disk Cleanup "system files" freed 0.
  - Don't move `AppData\Local\Claude-Data` (its README forbids it). G: is a slow HDD anyway.
- Biggest remaining user on C: is Chrome, 15 GB across many profiles. That is his profiles' data; he removes unused profiles himself.
