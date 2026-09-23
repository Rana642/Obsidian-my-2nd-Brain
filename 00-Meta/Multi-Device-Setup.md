---
type: meta
---

# Multi-Device Setup

## Per new device
1. `git clone https://github.com/Rana642/Obsidian-my-2nd-Brain.git` (any location)
2. Open that folder as an Obsidian vault
3. Install the **Git** plugin (by Vinzent, official) — auto-detects this repo
4. Set Auto commit-and-sync interval → 5, Auto pull interval → 5
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

## Mobile (Android)
Tried MGit (git client) + Obsidian mobile for the full vault experience — MGit crashed when changing its root storage location (first clone landed in MGit's private app storage, invisible to Obsidian). Dropped it.

**Final setup: GitHub app only.** Sign in as Rana642, open the `Obsidian-my-2nd-Brain` repo, browse/edit/create files directly, commits go straight to GitHub — desktop picks it up on the next auto-pull. No local vault on phone, but 100% reliable, no crashes, no extra app needed.

## Devices configured
- PC "Abdul Ahad" (office-or-home): `E:\Rana Shoaib\My Projects Website\Obsidian my 2nd Brain`
- PC "PC" (connected 2026-09-23): `D:\Rana Shoaib\My Projects Website\Obsidian-my-2nd-Brain` — global CLAUDE.md created pointing here

## Important: this vault only syncs Claude Code memory, not the Claude.ai app
- **Claude Code** (terminal / VS Code / this CLI) stores memory as local files per machine (`~/.claude/projects/<hash>/memory/`). It has NO built-in cross-device sync. This vault + the per-device global `~/.claude/CLAUDE.md` pointer is the *only* thing making Claude Code aware of prior context on a new machine — and only if a session actually reads/writes `03-Projects/<project>/memory/` in the vault, not just its own local memory folder.
- **Claude.ai (Desktop app, mobile app, web)** is a completely separate product. If Shoaib has the account-level "Memory" feature turned on in Claude.ai Settings, that already syncs automatically across desktop/mobile/web for the same login — this vault has nothing to do with it and can't turn it on or off.
- So "one memory hub across everything" needs both pieces working: (1) Claude.ai's own Memory setting on for the app/mobile side, and (2) this vault kept current + every machine's global CLAUDE.md pointing to it, for the Claude Code side.
