---
name: fb-native-stuck-after-reconnect
description: "2026-10-30 — FB native-scheduled posts silently never published after the 24 Oct checkpoint/reconnect (stuck in Meta's scheduled list past their time) while the Planner showed \"ok\"; audit route + findings"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-30T12:15:54.786Z
---

**Found 2026-10-30.** Shoaib saw it first: Hotel Avalon, Danish Construction and Eventia had no FB feed posts after 23 Oct, including Jummah. Stories, IG, LinkedIn and GBP were fine.

**Cause.**
- Facebook posts handed to Meta's own scheduler (`result.native`, the two-step /feed schedule) sit in `/{page}/scheduled_posts` with a fire time already in the past. Meta never published them.
- The Planner marks native posts "posted ok" at their time without checking, so nothing showed it.
- Timing: it started with the Ijaz Danish login checkpoint on 24 Oct (~14:30 PKT; "You cannot access the app till you log in…"). Silver Sand (Shoaib's login) is stuck from 27 Oct too.
- Theory: natives created with a page token that was later invalidated never fire. Natives created after the reconnect fire fine (Avenza Land from 28 Oct, Choppers, Meezab, Tad).
- `created_time` on a scheduled post equals its fire time, so it can't show when the post was made.

**Stuck as of 30 Oct (planner since 20 Oct):**
- Avalon 7
- Eventia 7
- Danish 7
- Silver Sand 6
- Elegant 4, plus 3 reel drafts at 19:00 on 24, 26 and 28 Oct
- Avenza Land 2 (26 and 27 Oct)

Clean: Choppers, Choppers Studio, Avenza Avenue, Meezab, Tad, TONI&GUY.

**How the audit runs.**
- A temporary read-only route, `app/api/social/fb-audit` (commit f752ce1), uses cron-token auth.
- Call it with scratchpad `li-test/fb-audit-call.cjs`.
- Per page it compares Planner FB ids with Meta's published_posts and scheduled_posts.
- A STUCK post = still in scheduled_posts with its fire time in the past.
- Video (reel) ids return "nonexisting field is_published" on the probe. That does not mean missing. Their feed post shows as an "untracked" draft.
- Remove the route once the fix is done.

**Also seen:** some Elegant and Silver Sand rows dated 11-30 to 12-02 carry FB ids that were already published 6–8 Oct, so the cron will skip FB on those days.

**Done on 30 Oct (Shoaib: "aj hi ker do", Jummah first):**
- All 36 stuck Meta drafts were strictly deleted through the route's POST (scratchpad `fb-delete.cjs`).
- The 5 Jummah posts were re-sent to FB about 16:20 PKT and checked live: Avalon, Eventia, Danish, Elegant, Silver Sand.
- The other 28 rows were requeued as FB-only with `fb-requeue.cjs`, from 16:58 to 19:53 PKT, 35 min apart per page.
  - Eventia 98 and Avalon 97 also got Instagram added back (their IG had failed on 27 Oct).
- **First re-send failed** with "Missing or invalid image file": the upload had been deleted after posting.
  - Fix c544c8c: `restorePublishedMedia` now also uses the row's OWN published IG/FB copy.
- Elegant reel ROOM-201 (28 Oct) could not be re-sent: the video is gone from storage, and restore only handles photos. Needs the source file again.

**Permanent fix (Shoaib: "aj k baad jo issues aj thy dobara na hon"), all on 30 Oct:**
- **All 62 upcoming native drafts were re-submitted** with each Page's current token, at the same times. The route's POST `{resubmit:[rowIds]}` does this (scratchpad `fb-resubmit.cjs`, 3 photos or 1 reel per call). 62 ok, 0 failed.
- **ROOM-105** (Elegant reel, 1 Nov) was fixed by hand. Its "live" FB id did not exist and its targets were IG-only. FB is now native-scheduled.
- **Facebook check, `lib/social-verify.ts`** (c3dc725 + 2e2aa3d), runs at the end of every cron:
  - From 20 min after a post's time, for 3 days, each FB result is looked up on Graph.
  - Live → `verified_at` is set.
  - Still unpublished, or missing → the draft is deleted strictly, the post goes out live (2 per run), and ONE email per run is sent.
  - A lookup error → retried; after 4 tries it goes in the email.
  - "Missing" counts only if the token can still read the Page's `published_posts`. Graph's #100 message is the same for a deleted post and for no permission, so this stops double posts.
- **The cron keeps a posted row's upload** while a native FB copy is still unverified. The check deletes it once it is verified, so a stuck Reel can still be re-sent.
- **Why ROOM-103 and ROOM-201 were missed** (Shoaib asked):
  - Both were natively scheduled on Meta: ROOM-103 for 24 Oct 7 PM, ROOM-201 for 28 Oct. They got stuck like the other 36.
  - On 27 Oct the checkpoint backlog re-sent only Instagram. We wrongly believed the native FB posts had gone out, so the stale FB id stayed in `result.live`.
  - IG went live and the row counted as done, so the video was deleted from storage.
  - The local search of Desktop, Downloads and `E:\Rana Shoaib` found no ROOM-*.mp4. They were uploaded from the dashboard on 19 Oct.
- **First run** found 2 Elegant reels not on FB: ROOM-201 (28 Oct) and ROOM-103 (29 Oct; it was posted live, yet missing). Both videos are gone from storage, so both rows are now `failed`. They are on IG; FB needs the video files again.
- **Re-audit after the fix:** STUCK 0 on every Page. Reel probes still show "missing/untracked" because the id is a video id and its feed post is separate. That is expected.
- **Kept on purpose:** Silver Sand rows dated 11-11/11-12 and Elegant rows dated 11-30, 12-01, 12-02 carry FB ids published in Sep/Oct and target IG+LI only. That is a backfill and correct.
- **The temporary `/api/social/fb-audit` route is still deployed** (cron-auth). Remove it, or keep it for the morning report, once that is decided.

**Next (Shoaib said "shuru kero" to the plan):**
- A morning report, one email per day.
- An AI design check of new uploads. Give him the PKR cost first.

Related: [[fb-native-schedule-feed-fix]], [[aijaz-danish-brands]], [[waiting-on-shoaib]].
