---
name: feedback-meta-marketing-api-direct
description: "User wants Meta ad checks done via direct Marketing API (project's .env.local credentials), never the Meta Ads MCP connector"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fb776024-2ed0-44c1-a91d-40c7ca3ba0dd
  modified: 2026-09-16T07:59:10.776Z
---

Never use the "Meta Ads MCP" connector (id `13ce12d1-9611-42f4-ba7f-9d690d9fbb8d`, tool prefix `mcp__13ce12d1-9611-42f4-ba7f-9d690d9fbb8d__ads_*`) for this project. Use Meta's Marketing API directly (raw Graph API calls) with the credentials already in the Hotel Elegant Multan project's `.env.local`: `META_ACCESS_TOKEN`, `META_AD_ACCOUNT_ID` (currently `act_239008850511120`), `META_PIXEL_ID` (currently `27407654508906433`).

**Why:** The user has deliberately turned off the Meta Ads MCP connector on their account. They said this explicitly and asked it be remembered so they don't have to repeat it: "Meta ads mcp use nai kerna wo off kr rakha hai maine, Marketing api use kerna hamesha jo project ki env local mai hai."

**How to apply:** For any Meta/Facebook Ads, Pixel, CAPI, or dataset-quality check on this project, call `graph.facebook.com` endpoints directly via curl/fetch using the `.env.local` token instead of loading or enabling the Meta Ads MCP connector. If the connector was enabled earlier in a session by mistake, disable it again (`set_session_connector_enabled`, `enabled: false`). This preference is specific to this project's env-local-based setup — see [[project_hotel_elegant_ad_accounts]] for the actual account IDs if that memory exists, otherwise re-check `.env.local` each time since tokens can rotate.
