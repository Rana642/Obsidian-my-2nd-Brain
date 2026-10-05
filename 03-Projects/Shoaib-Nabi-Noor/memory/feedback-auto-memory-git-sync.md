---
name: feedback-auto-memory-git-sync
description: Always update memory, vault and the git work log automatically after each piece of work; never wait for Shoaib to ask
metadata:
  type: feedback
---

Shoaib (2026-10-06): "Ab memory aur git per update kar do — ye kaam pocha na kero, sath sath ker diya kero."

Don't wait to be asked. After every meaningful piece of work (a fix shipped, a decision made, a campaign launched, a rule given), in the same turn:
1. Update the relevant memory file(s) and the `MEMORY.md` index.
2. Copy changed memory files to the vault (`Obsidian-my-2nd-Brain/03-Projects/Shoaib-Nabi-Noor/memory/`, plus the project's own vault folder if one exists), then commit and push the vault.
3. Add or extend the dated entry in `docs/WORK-LOG.md` (adsbyshoaib repo) and commit and push. Pull/rebase first; other sessions push too.
4. Log client-facing work in that client's KB `client-report-log-YYYY-MM` ([[feedback-client-work-log]]).
5. Commit and push code repos as usual after each change.

**Why:** Shoaib had to keep repeating "memory aur git per update kar do". The sync should be automatic, so nothing is lost between sessions or machines.

**How to apply:** treat the sync as part of finishing the task, not a separate step. Mention it in one line at the end of the reply (e.g. "memory/vault/work log updated") — no need to ask.
