---
type: meta
---

# Multi-Device Setup

## Per new device
1. `git clone https://github.com/Rana642/Obsidian-my-2nd-Brain.git` (any location)
2. Open that folder as an Obsidian vault
3. Install the **Git** plugin (by Vinzent, official) — auto-detects this repo
4. Set Auto commit-and-sync interval → 10, Auto pull interval → 10
5. Create/edit `~/.claude/CLAUDE.md` on that machine (global, not synced) pointing to this vault's actual local path on that device — see template below

## Global CLAUDE.md template (per-device, edit the path)
```
# Global Instructions

## Second Brain
Shoaib has a synced second brain vault at:
<path to this vault on THIS device>

Before starting substantial work on any project, check if a matching folder exists at
<vault>\03-Projects\<project-name>\. If it exists, read its CLAUDE.md and memory\MEMORY.md
first for accumulated context before proceeding.

If working inside the vault itself, follow its own root CLAUDE.md instead of this rule.
```

## Raw Claude Code chat history (separate from this vault)
This vault only holds distilled `memory/*.md` notes — not full chat transcripts (too large for git). Raw session `.jsonl` files are mirrored separately:
- Source: `C:\Users\Abdul Ahad\.claude\projects\`
- Destination: Google Drive (5TB account) → `H:\My Drive\ClaudeCodeSync\projects\`
- Scheduled Task "ClaudeCodeToGoogleDrive" mirrors every 30 min (Task Scheduler, this PC only)
- Google Drive Desktop was set up with folder-backup (Desktop/Documents/Downloads) explicitly **skipped** — only this one synced folder, nothing else from the PC
- **Caveat:** for a session to be resumable "as-is" on another PC, that project's actual code folder must exist at the *exact same absolute path* there too (Claude Code identifies a project by its working-directory path). Otherwise the mirrored `.jsonl` files just sit there as reference, not directly resumable.
- To use on a new device: install Google Drive (skip folder backup), let it sync down `ClaudeCodeSync`, then copy the relevant project's folder from there into that device's own `~/.claude/projects/` if you want to resume it.

## Devices configured
- This PC (Abdul Ahad / office-or-home): `E:\Rana Shoaib\My Projects Website\Obsidian my 2nd Brain`
