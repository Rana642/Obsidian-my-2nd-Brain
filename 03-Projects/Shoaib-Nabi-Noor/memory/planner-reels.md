---
name: planner-reels
description: "Planner Reels (0131523) + Stories (024e21c, full-frame c07c10e) — FB Reels API + IG two-phase via result.pending; YouTube + TikTok video added 2026-10-15 (see work log); cron run budget"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-27T07:55:20.155Z
---

Shoaib on 2026-10-02: build video Shorts and Reels upload first, then work out YouTube. Stories were also discussed. The UX was agreed as a Post / Reel / Story switch in the upload dialog.

**Built in 0131523: Reels for Facebook and Instagram**
- **Schema:** `scheduled_posts.post_type` ('post' | 'reel' | 'story', default 'post') and `cover_key`.
  - Inserts only send these two columns for Reels, so photo posts keep working even before the SQL is run.
- **Upload:**
  - The Post/Reel tabs live in the Planner's UploadModal.
  - Video is MP4/MOV, max 300 MB via `upload-url`, with an optional cover image.
  - The dialog reads the duration client-side; Facebook takes 3–90 s, Instagram 3 s–15 min.
  - A Reel always sends `platforms` filtered to facebook/instagram.
- **Facebook:** `publishFacebookReel` in `social-fb.ts` does start → rupload with a `file_url` header → finish.
  - When the post is ≥10 min ahead, `submitNativeSchedule` sends `video_state=SCHEDULED`, so Meta's scheduler handles it.
  - Cancel / re-schedule deletes the video id.
- **Instagram:** `postReelToAccount` in `social-post.ts`:
  - creates the REELS container (share_to_feed, cover via the instagram-media JPEG proxy) and polls 4×3 s;
  - if not done, returns `pending {container_id, since}`;
  - the cron stores `result.pending` and keeps the post 'scheduled', then publishes on a later run;
  - gives up after 45 min.
  - Works for both Page-linked and Instagram-login accounts.
- **Elsewhere:**
  - Reels skip GBP, LinkedIn and TikTok.
  - Calendar cards use `PostMedia` (cover, else the video's first frame, plus a Reel badge).
  - The planner page passes `coverUrl`.
  - `social_get_post_image` returns the cover, not the video.
  - The CSP now has `media-src 'self' blob:` (plus R2 on the social/portal planner pages).

**Same-day fixes**
- **29ca303:** `social_set_caption_and_schedule` on an already natively-scheduled post now cancels the old FB draft. Before this it double-posted.
- **da9cc46:** the social cron starts no new post after 20 s (`RUN_BUDGET_MS`). Six posts due in one run hit the 60 s Vercel limit mid-post, which risks a published-but-unrecorded post.
- **e5a6ee6:** the reply-cron yields when a planner post is due within 15 min.

**Stories, built in 024e21c (2026-10-02)**
- The upload tab "Story" takes a 9:16 photo, or an MP4/MOV video of 3–60 s, with no caption. `createScheduledPost` schedules a story straight away (it has no pending_caption step).
- **Facebook:** `publishFacebookPhotoStory` uploads the photo with published=false, then posts it to `photo_stories`. `publishFacebookVideoStory` uses `video_stories` start → rupload `file_url` → finish. Meta can't schedule stories, so the cron publishes them at the post's time.
- **Instagram:** `createInstagramStoryContainer` creates a STORIES container (image_url via the JPEG proxy, or video_url), then follows the same pending/publish flow as Reels.
- `ReelInput` now has `kind`, `videoUrl`, `fbImageUrl` and `igImageUrl`. Whether the file is a video is judged by the `media_key` extension (.mp4/.mov).

**Update 2026-10-15:** YouTube (Shorts + videos) and TikTok video (sent as an inbox draft) were built; details are in `docs/WORK-LOG.md` (2026-10-15). Still waiting on Shoaib to enable the YouTube APIs, add the scopes and apply for the YouTube API audit. The original notes:
- YouTube upload needs a per-project channel OAuth with youtube.upload, chunked resumable upload across runs, and `status.publishAt` scheduling.
- Until Google's YouTube API audit passes, uploads stay private.
- Quota is about 6 uploads per day.

Related: [[instagram-login-connect]], [[aijaz-danish-brands]].

**2026-10-21, c07c10e (Story crop fix):** auto-shared Stories of photo posts were side-cropped. IG and FB zoom a 4:5 image to fill 9:16, so headlines and footer contacts got cut; the Meezab accounts manager reported it.
- `queueStoryCopy` now redraws photos with `lib/story-frame.ts`: 1080×1920, the whole post centred.
- Above and below get the post's edge colour if that edge is flat (stdev ≤ 14), otherwise white.
- Videos are still copied as-is. If the redraw fails, the plain copy goes out.
- 12 Stories went out cropped before the fix. The IG API can't delete Stories; they expire in 24h.
