---
name: adsbyshoaib-mcp-remote-oauth
description: "Remote MCP server (OAuth 2.1 + DCR + PKCE) at /api/mcp letting Claude web/mobile/desktop connect, separate from the local stdio server"
metadata: 
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-15T21:38:47.520Z
---

Shoaib asked (2026-09-16) to make the existing local-only MCP server ([[graphics-studio-mcp-server]] is the sibling project's version) reachable from Claude.ai web/mobile, not just Claude Code. Built as a hand-rolled Next.js OAuth 2.1 authorization server (NOT the MCP SDK's Express-bound `server/auth` framework, which doesn't fit Next's App Router) — single-tenant: the only human who can reach the consent screen is whoever is logged into `/dashboard`.

**Two parallel MCP surfaces now exist**: `mcp/` (local stdio, Claude Code only, run via `npm run mcp:social`) and `app/api/mcp` (remote HTTP + OAuth, Claude web/mobile/desktop). Tool registration logic lives in `lib/mcp-*.ts` files with no Next-specific imports so both surfaces can call the same `register*Tools(server)` functions — see `mcp/tools/social.ts` + `lib/mcp-remote-tools.ts` (deliberately different, smaller subset) vs `mcp/tools/marketing.ts` + `lib/mcp-marketing-tools.ts` (identical set on both, see [[adsbyshoaib-mcp-marketing-tools]]).

**Key files**: `lib/mcp-oauth.ts` (HMAC-signed access tokens via `MCP_OAUTH_SIGNING_KEY`, DB-backed auth codes/refresh tokens in `mcp_oauth_*` tables), `app/.well-known/oauth-authorization-server` + `oauth-protected-resource` (metadata), `app/api/mcp/oauth/register` (DCR), `app/api/mcp/oauth/authorize` (consent screen), `app/api/mcp/oauth/token`, `app/api/mcp/route.ts` (the actual MCP endpoint, stateless — fresh `McpServer` per request since Vercel serverless has no persistent memory).

**Non-obvious gotchas already hit and fixed** — don't re-debug these:
1. The consent-form POST redirect MUST use status 303, not the default 307 (307 preserves POST on the OAuth callback, which expects GET).
2. `proxy.ts`'s CSP `form-action 'self'` blocks that same redirect going cross-origin to Claude.ai — relaxed specifically for `/api/mcp/oauth/authorize` only.
3. `MCP_OAUTH_SIGNING_KEY` must be added to **Vercel's** env vars separately from `.env.local` — a bare 500 with no body on the token endpoint means this is missing (temporarily wrap the route in try/catch to surface the real error if this happens again, then revert the wrapper).

**How to apply**: any new MCP tool category should decide up front whether it needs the social-tools' "different/smaller remote subset" pattern or the marketing-tools' "identical on both surfaces" pattern, and follow the corresponding wiring (both are already established, don't invent a third way).
