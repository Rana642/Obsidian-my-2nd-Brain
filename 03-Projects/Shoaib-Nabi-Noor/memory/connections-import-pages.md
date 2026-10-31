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

**2026-10-31: two logins, which one a Page uses** (a33744e).
- **Problem:** both Facebook logins (slot 1 Shoaib, slot 2 Ajaz Hussain = the same FB user 1176365811541314 who was "Ijaz Danish") reach many of the same Pages.
  - `refreshFacebookPageTokens` used "the login just reconnected wins", and saving an import also refreshed every token.
  - When Ajaz reconnected at 16:21, 9 Pages moved onto his token. That login never granted `pages_read_user_content`: it was dropped from the login scope on 10-28, and the App Review doc wrongly had it on the "remove" list.
  - Their Facebook insights then failed with #10, because the Insights post list reads `comments.summary`.
- **Fix:**
  - The scope asks for `pages_read_user_content` again.
  - `bestSource` keeps an account on its current login unless that login lacks a needed permission (`PAGE_SCOPES_NEEDED` / `INSTAGRAM_SCOPES_NEEDED` in social-fb).
  - Saving an import no longer refreshes all tokens. It inserts with `fb_login_name` and moves only the Pages explicitly picked with a login.
  - Which login a stored token belongs to is worked out from `debug_token` `user_id` (`storedTokenOwners`). No new column, and it still works when a profile name changes.
- **Import dialog:** "via <login>" chips, a login select when two logins reach a Page, a "now via X ⚠" on imported Pages (re-selectable to move them), and a login filter. The browser sends `{project_id, page_id, login_slot}`; the zod schema was tested on real ids and the backslash is intact.
- **Integrations:** each FB/IG account shows "via X" plus an IssuePopover ⚠. A login that lacks permissions shows "Permission missing".
- **Check without the UI:** `/api/social/fb-audit?choices=1` (cron token) returns the dialog's data with no tokens.
- **State after deploy:**
  - Avenza Avenue, Avenza Land, Choppers, Choppers Studio, Elegant and TONI&GUY are reachable through both logins. Shoaib can move them in the dialog.
  - Danish, Eventia and Avalon are reachable only through Ajaz. He has to reconnect (Tester role, Standard access).

**Later on 2026-10-31:**
- **Eventia (FB + IG) and Danish (FB) were moved to Shoaib's login** at Shoaib's request ("eventia aur Danish construction ab mere porifile per ker do"), using the fb-audit POST `{move:[{page_id, project_id, login_slot:1}]}`, which calls `saveFacebookPageMappings`. 3 rows changed. **Only Hotel Avalon remains on slot 2.**
- **The rows got the "Facebook login 1" placeholder name**, because /me had failed under the rate limit. They were fixed in the DB to "Shoaib Nabi Noor".
- **Code fix eaf17a8:** /me is retried once, and only `realName` is ever saved as `fb_login_name`.
- **Meta hourly app limit (#4) was hit at ~17:00.** debug_token and /me use platform limits. Caches added in a362241: login health 5 min (cleared on reconnect), broken count 15 min, token owners 10 min.
