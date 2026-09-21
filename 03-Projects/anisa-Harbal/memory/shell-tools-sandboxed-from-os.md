---
name: shell-tools-sandboxed-from-os
description: Bash/PowerShell tool calls run in an isolated context that does NOT share live state (like User-level env vars) with the real Windows session running the Claude desktop app.
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 709bfea5-0684-4e86-be30-895bb0f2135e
  modified: 2026-09-03T06:28:45.135Z
---

Setting a Windows **User-level environment variable** via the Bash or PowerShell tool (e.g. `[System.Environment]::SetEnvironmentVariable(..., "User")`) does **not** propagate to the actual Windows desktop session that launches the Claude desktop app and its MCP server child processes — even after the user fully quits (via system tray) and relaunches the app. Verified by reading the var back immediately afterward inside the same tool context (shows "present"), while the app-spawned `node` process still reported it as missing, across two separate full-quit-and-relaunch cycles.

**Why:** the shell these tools execute in is evidently a separate/sandboxed context from the user's real OS session, so registry/env changes made there aren't visible to processes the user launches through the actual desktop.

**How to apply:** Don't propose "set a persistent env var to avoid retyping secrets in MCP config" as a fix in this environment — it looks reasonable but doesn't work here. The only thing that reliably works for this user's MCP server credentials is putting them directly in `claude_desktop_config.json`'s per-server `env` block. See [[google-ads-mcp-setup]] for the backup-file workaround used instead (a plain local JSON file with the working config block, restored by copy-paste when the config gets wiped, rather than trying to avoid storing secrets in the config at all).
