---
name: feedback-direct-execution
description: User prefers Claude to directly execute infra setup via API/SSH rather than just handing over instructions or files
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 9f9895a1-018b-4c9f-b94c-4dbe8936d616
  modified: 2026-08-28T14:06:51.961Z
---

When the user has API keys, SSH access, or other direct control available, they want Claude to actually perform the setup (create credentials, push configs, run commands via the user relaying terminal output) rather than just producing documentation or import-ready files for them to apply manually.

**Why:** After Claude generated importable n8n workflow JSON files and a manual setup checklist, the user pushed back with "yahan se ker do na connected hai n8n vs code mai hi" (do it from here, it's already connected) once they saw a live API/tool connection was possible — they wanted hands-on execution, not a handoff.

**How to apply:** Default to checking whether a live channel (API key, CLI, MCP, SSH-relayed terminal) is available before defaulting to "here are the files/instructions, please apply them yourself." Static deliverables are still worth producing alongside as a portable backup/reference, but the live system should be actually configured when possible. See [[project-clif-whatsapp-agent]] for the project this pattern emerged from.
