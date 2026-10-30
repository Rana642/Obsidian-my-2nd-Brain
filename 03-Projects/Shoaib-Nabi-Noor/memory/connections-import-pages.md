---
name: connections-import-pages
description: Import Pages can add an Instagram linked to a Page after it was imported (4c5c405 + a684e4b); Page tokens stay server-side; Graph checks on Page links need production (no SOCIAL_TOKENS_ENCRYPTION_KEY locally)
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-21T08:12:52.070Z
---

**2026-10-21, Tad Pharma Instagram via Business portfolio.** The Page was imported (FB only) before its Instagram was linked. The import dialog locked imported Pages, so the Instagram had no way in.
- Fix 4c5c405: an imported Page whose linked Instagram isn't on its project shows "Imported · X · its Instagram isn't connected yet — select to add it". It is pinned to that project, and only the missing rows are inserted.
- Same commit: the dialog no longer receives Page tokens. The browser sends `{project_id, page_id}`; `saveFacebookPageMappings` re-discovers Pages and tokens on the server.
- Tad Pharma IG now connected (external_id 17841460987340445). Its 23 scheduled posts have `target_platforms` null, so they go to Instagram too from 2026-10-22.

**Gotcha:** this PC's `.env.local` has no `SOCIAL_TOKENS_ENCRYPTION_KEY` (Vercel only), so stored FB tokens can't be decrypted locally. Check Page→Instagram links through the production dashboard (Import Pages dialog shows "Facebook Page + Instagram").

**Lesson (a684e4b):** a node/heredoc scripted edit turned `/^\d{1,30}$/` into `/^d{1,30}$/`, so every import failed with "Invalid mapping data", and tsc/eslint can't catch it. **Why:** escaping layers (bash heredoc → JS string → file) ate the backslash. **How to apply:** use the Edit tool for code with backslashes/regex, and run a tiny node test of any new zod schema on real ids before pushing.

In the browser pane the dashboard is phone-width and refs go stale after the dialog re-opens. Filter the import dialog with its search box first, so the click can't hit the wrong row. One click once ticked The Panache by mistake; it was unticked, nothing was imported.

See [[instagram-login-connect]], [[adsbyshoaib-dashboard-navigation]].
