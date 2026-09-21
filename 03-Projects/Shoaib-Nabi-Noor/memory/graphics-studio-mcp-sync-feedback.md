---
name: graphics-studio-mcp-sync-feedback
description: Standing instruction — every new graphics-studio app feature must get a matching MCP tool in the same pass
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-25T08:37:18.660Z
---

Whenever a new feature is added to the Graphics Studio app (`graphics-studio/`), also add the matching tool(s) to the [[graphics-studio-mcp-server]] MCP server in the same pass — don't treat it as a follow-up task.

**Why:** Shoaib said explicitly (2026-08-25): "jo bhi addition keron MCP mai ad kerty rehna aur controls deyty rehna" — keep adding every addition into the MCP too, with proper controls (clear schemas/annotations, not just a thin passthrough).

**How to apply:** When building any new graphics-studio feature (new generation mode, new data type, new workflow), plan the web feature and its MCP tool together — same data-layer functions (`lib/`), shared helpers where the logic overlaps (e.g. `lib/generate-batch.ts`'s `runGenerationBatch`, reused by both API routes and both `studio_generate_*` MCP tools instead of duplicating the batch-generation logic four times). Update `README.md`'s MCP tools list in the same commit/pass.
