---
name: planner-upload-from-local-folder
description: "How to put a local folder of posts/Reels into the Planner without the browser file picker (2026-10-16 Silver Sand batch) — shrink, presign via logged-in prod tab, curl PUT, service-role insert, MCP captions"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-16T13:29:08.399Z
---

On 2026-10-16 Shoaib asked: "E:\Rana Shoaib\Hotel\Silver Sand ki posts ka size kam ker k planner mai dal do, caption likh ker schedule ker do". This is the recipe that worked: 27 items, all scheduled.

1. **Shrink.**
   - Designer PNGs (3375×4219, 7–16 MB) go to 1440×1800 JPEG q90 (about 0.35 MB) with `scratchpad/pdftools/shrink.mjs` (@napi-rs/canvas).
   - Videos (2160×2700, about 36 MB) go to 1080×1350 H.264 CRF 20 + faststart (2–3 MB). ffmpeg is in the WinGet Gyan.FFmpeg folder, not on PATH.
   - Output goes to a `compressed/` folder next to the originals. The originals are never touched.
2. **Check every creative before anything else.** See [[feedback-analyze-posts-before-scheduling]].
   - For Reels, take frames at 1 fps of the footer strip. AI-animated Reels garble phone numbers mid-video: Reel 17 showed 051-… and 0914, Reel 24 showed 0328.
3. **Get presigned PUT URLs.**
   - Local `.env.local` has no S3_* vars, so presigning must happen on production.
   - In the built-in browser, logged in to adsbyshoaib.com, `fetch('/api/dashboard/social/upload-url', {name, type, size, projectId})` gives `{url, key}`.
   - The URLs last 10 minutes and sign content-length, so send the exact size.
4. **Upload:** `curl -X PUT -T file` with each URL, before the 10 minutes run out.
5. **Create the posts.**
   - Insert `scheduled_posts` rows with the service-role key from `.env.local`: status pending_caption, `scheduled_at` set to the exact time, `post_type: 'reel'` + `cover_key: null` for videos, and `target_platforms` null.
   - Script: `scratchpad/ss/insert.cjs`.
6. **Caption:** call `social_set_caption_and_schedule` for each post without scheduledAt, which keeps the inserted time. `social_get_post_image` returns the project's posting style guide.

**Stories** need no step: the `post-also-as-story` cron (4905df2) shares every published post and Reel as a Story.

**Idea, not built:** a `social_create_direct_upload` MCP tool, like `kb_create_direct_upload`, would skip the browser step.
