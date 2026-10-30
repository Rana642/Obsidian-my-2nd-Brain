---
name: planner-needs-changes
description: "Planner \"needs changes\" status (da83f6c, 2026-10-05) — flagged uploads alert + email the uploader, Replace image flow, MCP social_request_changes/social_clear_changes; needs SQL section"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-05T08:59:09.805Z
---

Shoaib asked on 2026-10-05 that the person who uploads a post should also find out what is wrong with it. They get a popup or alert in the app, and an email. Shipped in da83f6c.

**Data**
- `scheduled_posts.status` has a new value, `needs_changes`.
- New columns: `review_note` (plain English, shown to the client) and `review_flagged_at`.
- They come from the SQL section "Planner: needs changes" in `supabase/dashboard-schema.sql`. **Shoaib ran it on 2026-10-05.**

**Server code**
- `lib/post-review.ts`:
  - `flagPostsForChanges` sends one Resend email per uploader per batch, with bcc and reply-to Shoaib. Its thumbnails are presigned links that last 7 days.
  - Also has `clearPostFlag` and `replaceFlaggedPostMedia`.
- `lib/post-review-tools.ts` registers the MCP tools `social_request_changes` and `social_clear_changes`. Both the remote and the local MCP use it.
- `setCaptionAndSchedule` refuses posts that are needs_changes.

**Portal (client side)**
- The `/portal/planner` page shows `components/portal/NeedsChangesAlert.tsx`:
  - a banner;
  - a popup, shown once per set of flags per session through `useSyncExternalStore` (a setState inside an effect fails lint);
  - Replace image and Remove buttons.
- `replacePortalUpload` (in `lib/portal/planner.ts`) keeps the day and slot, deletes the old file, and emails Shoaib what was fixed.
- Portal home shows how many posts need changes.
- A post that Shoaib uploaded and flagged shows to the client as "Being prepared".

**Dashboard**
- An orange badge plus the note, and a Clear button (`clearPostChanges`).

**Tested 2026-10-05** with a temporary row: flag, scheduling blocked, clear, flag again, replace — all passed. The real Resend email has not been sent yet.

**social_delete_post (47d0b14)** removes a wrong upload.
- It is in the same shared module.
- First it cancels the Facebook native draft and confirms it is gone, using `deleteFacebookPostStrict` and `cancelNativeScheduleStrict`. If that fails, nothing is deleted.
- It refuses posts that are already posted.
- This PC has no SOCIAL_TOKENS_ENCRYPTION_KEY, so local scripts cannot cancel Meta drafts. Use the remote tool or the dashboard instead.
- New remote tools show up only in a NEW session; a connected connector cannot be refreshed mid-session.

**Gotcha:** the CSP lets R2 images load only on `/dashboard/social*` and `/portal/planner`. A temporary test route shows broken thumbnails, and that is expected.

Related: [[feedback-analyze-posts-before-scheduling]], [[adsbyshoaib-client-portal]].

**Portal upload report (commit e4034c7, 2026-10-30).** Shoaib: "1 aik post upload kerny ka alert nai ana chaiye … ikathi report ajaye k kis kis project ki post upload ki hai".
- **Old behaviour:** every portal upload and every fixed re-upload emailed him right away.
- **New behaviour:** they mark `scheduled_posts.client_activity`. `lib/upload-digest.ts` (social cron) then sends ONE email per uploader after 30 min of quiet, or after 3 h of non-stop uploading. It lists each project's new posts by day, the fixes and the notes.
- The `client_activity` column was added by Shoaib on 2026-10-30, so the report is live and the per-upload emails have stopped.
