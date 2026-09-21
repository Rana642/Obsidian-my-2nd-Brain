---
type: journal
date: 2026-09-21
---

# Second Brain — Build Day

- Chose Obsidian + GitHub over Baldur (too new/unproven) and Obsidian's own paid Sync (avoided via free Git-based sync instead)
- Two-layer structure: root = strategy/context (Layer 1), `03-Projects/` = execution (Layer 2), matches the KJ reference design
- Private repo: github.com/Rana642/Obsidian-my-2nd-Brain
- **Security incident:** an auto-installed community plugin ("Git Sync" / Obsidian MultiSync by Livan Kumar) requested broad GitHub OAuth `repo` scope from an unreviewed solo-dev plugin, and silently repointed the local git remote to its own auto-created repo. Caught before damage — revoked OAuth access, deleted the two stray auto-created repos (`obsidian-obsidian-my-2nd-brain`, `obsidian-jarvis-vault`), removed the plugin. Switched to the official **Git** plugin (by Vinzent, 3.1M downloads) — uses local git + Windows Credential Manager, no third-party OAuth app.
- Migrated 68 memory files from 9 local coding projects (`~/.claude/projects/*/memory/`) into `03-Projects/<name>/memory/` — copies, not moves; originals untouched. Two projects (Hotel Elegant, Hotel Silver Sand) had duplicate memory from being worked on at different paths/machines — merged, no overlap in content.
- Added `00-Meta/Projects-Index.md` so every project links back to one place — one vault, one graph, not fragmented islands.
- Set up automatic cross-device context: `~/.claude/CLAUDE.md` (global, per-machine) tells Claude to check this vault's `03-Projects/<name>/` before starting work on any matching project. See `00-Meta/Multi-Device-Setup.md` for the per-device setup steps.
- Jarvis vault (pre-existing, separate) — decision on merge vs. keep separate still pending.
