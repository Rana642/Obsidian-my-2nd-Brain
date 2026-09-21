---
name: google-ads-mcp-setup
description: "How the Google Ads MCP server is set up for the user's agency (Ads by Shoaib) — patches applied, IDs, backup location, known access-level status."
metadata: 
  node_type: memory
  type: project
  originSessionId: 709bfea5-0684-4e86-be30-895bb0f2135e
  modified: 2026-09-10T08:35:52.594Z
---

The user (Shoaib Nabi Noor) runs "Ads by Shoaib" (adsbyshoaib.com), an independent performance-marketing practice in Multan, Pakistan, managing Google Ads for hotel and local-business clients via manager account (MCC) **8859347478** (885-934-7478).

**Google Ads API Basic Access was approved** (application submitted and approved within ~24h in early September 2026), unblocking KeywordPlanIdeaService (previously blocked at Explorer-tier access).

**MCP server:** `@channel47/google-ads-mcp`, but heavily patched from its published state:
- Config launches it via `"command": "node"` pointing directly at `server/index.js` inside the npx cache (`C:\Users\Abdul Ahad\AppData\Local\npm-cache\_npx\eaea6e7e76c3da1e\node_modules\@channel47\google-ads-mcp\server\index.js`), **not** `npx -y ...@latest` — npx was re-resolving `google-ads-api` back to `^21.0.1` (the sunset Aug 2026 API version) on every launch, wiping any patch.
- Top-level `node_modules/google-ads-api` was upgraded from v21 to v24.1.0, and its `customer.js` `handleStreamError` was patched to gunzip/decompress the error-response stream before feeding it to `stream-json` (the library's own bug — un-decompressed error bodies crash the JSON parser with a confusing "Parser cannot parse input" error that masks the real underlying error).
- A 4th tool, `keyword_ideas`, was added (`server/tools/keyword-ideas.js`, registered in `server/index.js`) wrapping `customer.keywordPlanIdeas.generateKeywordIdeas`. Note: that RPC's response is a **plain array directly**, not `{results: [...]}` — code must do `Array.isArray(response) ? response : response?.results`.
- Full working `mcpServers` config block (with real credentials) is backed up at `C:\Users\Abdul Ahad\AppData\Roaming\Claude\google-ads-mcp-server-config-backup.json` — restore from there if `claude_desktop_config.json`'s `mcpServers` key ever gets wiped again (it has happened repeatedly), instead of asking the user to retype secrets.

**Known client account IDs under the MCC:** Hotel Silver Sand Ad Account = 4063371094 (406-337-1094), ABS | Hotel Elegant Multan = 6223250696.

**Refresh token expires every 7 days — `invalid_grant` error is expected, not a bug.** The OAuth consent screen is still in "Testing" publishing status, and Google caps refresh tokens issued in that mode at `refresh_token_expires_in: 604799` (~7 days), after which every API call fails with `MCP error -32603: invalid_grant`. First hit 2026-09-10 (last good use was 2026-09-05). Fix when it recurs: regenerate via Google OAuth 2.0 Playground (developers.google.com/oauthplayground) — gear icon → "Use your own OAuth credentials" → paste client_id/client_secret from the backup file → scope `https://www.googleapis.com/auth/adwords` → Authorize → Exchange authorization code for tokens → copy the new `refresh_token` into both `claude_desktop_config.json` and the backup file's `GOOGLE_ADS_REFRESH_TOKEN`. **Permanent fix (not yet done as of 2026-09-10):** switch the OAuth consent screen's Publishing status from Testing to "In production" in Google Cloud Console (APIs & Services → OAuth consent screen) — this removes the 7-day cap entirely and needs no Google verification wait, since only the developer's own account authorizes (the "unverified app" warning during consent is harmless to click through). Suggest this to the user next time this comes up if it still hasn't been done.

**Why:** Full audit + fix took an entire session; see [[mcp-config-file-instability]] and [[shell-tools-sandboxed-from-os]] for the specific gotchas hit along the way.
