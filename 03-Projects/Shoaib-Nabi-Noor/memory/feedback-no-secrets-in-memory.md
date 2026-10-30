---
name: feedback-no-secrets-in-memory
description: Never write API keys/tokens/passwords into memory or vault notes — the Obsidian vault is a PUBLIC GitHub repo; a Gemini key leaked that way and Google disabled it (2026-10-02)
metadata:
  type: feedback
---

Never put secret values in memory files, vault notes or commit messages. That covers API keys, tokens, passwords, service-role keys and webhook secrets. Write where the secret lives instead, e.g. "in `.env.local`" or "in API Vault".

**Why:**
- Memory is mirrored into the Obsidian vault (`03-Projects/<project>/memory/`). The vault is pushed to **github.com/Rana642/Obsidian-my-2nd-Brain, which is public**.
- The Graphic Studio Gemini key sat in plaintext in graphics-studio-project.md. On 2026-10-02 Google reported it as leaked and disabled it.
- I removed it in vault commit 67925e1. It stays in git history, but the key is dead.
- Shoaib's standing rule also applies: "Access Key aur Secret Key kabhi mujhe mat bhejna".

**How to apply:**
- Before writing memory, scan the text for key-like strings (AIza…, sk-…, sb_secret_…, EAA…, eyJ…).
- Recommended to Shoaib: make the vault repo private. That is his call; until it is done, treat the vault as public.

Related: [[graphics-studio-project]].
