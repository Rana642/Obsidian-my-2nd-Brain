---
name: fb-native-schedule-feed-fix
description: 2026-10-05 bug — FB native scheduling via /photos published to the Photos album only (not the Posts feed) since 2026-09-16; fixed a95bbf5 with /feed + attached_media; 33 upcoming posts resubmitted
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-05T11:34:28.238Z
---

**The bug (Shoaib noticed it 2026-10-05):** Hotel Avalon's posts showed in the Page's **Photos**, but not in its **Posts feed**.

**Cause.** `scheduleFacebookPhoto` used `/{page}/photos` with `published=false` and `scheduled_publish_time`. Meta did publish these, with a page_story_id, Public, and a permalink that works. But they appear only in the Photos album, never in the Posts list.

**Who was affected.** Every native Facebook post since 282839f (2026-09-13):
- Hotel Elegant: 20 posts from 16 Sep
- Silver Sand: 20 posts from 16 Sep
- Aijaz brands: 27 posts from 2 Oct
These past posts were left as they are. Re-posting would duplicate them, so Shoaib was asked what he wants.

**Fix (a95bbf5).**
- Upload the photo with `published=false` and no schedule.
- Then `POST /{page}/feed` with `message`, `attached_media[0]={"media_fbid":…}`, `published=false` and `scheduled_publish_time`.
- The result's `post_id` is now `{page}_{post}`.
- Cancel and delete work with this id. `publishedImageUrl` falls back to `full_picture`.

**Moving the queue over.**
- A temporary route resubmitted the 33 upcoming posts through `rescheduleNativePosts`.
- Batches of 8 timed out on Vercel's 60 s limit, so keep batches to about 3.
- An audit with `/{page}/scheduled_posts` found 4 old drafts whose delete had failed silently; they were deleted strictly.
- Final check: 0 untracked items.
- Both routes were then removed (0879ca4).

**How to apply.**
- `/{page}/scheduled_posts` lists Meta-side scheduled items as `{page}_{id}`, including old photo-style ones. Use it to audit for double posts.
- The non-strict `deleteFacebookPost` can fail silently. Use `deleteFacebookPostStrict` where it matters.
- The ABS `meta_ads_get` token can read Photo nodes, but not page feeds or posts: it lacks pages_read_engagement.

Related: [[planner-needs-changes]], [[aijaz-danish-brands]].
