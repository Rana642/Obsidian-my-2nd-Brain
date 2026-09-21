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

## Devices configured
- This PC (Abdul Ahad / office-or-home): `E:\Rana Shoaib\My Projects Website\Obsidian my 2nd Brain`
