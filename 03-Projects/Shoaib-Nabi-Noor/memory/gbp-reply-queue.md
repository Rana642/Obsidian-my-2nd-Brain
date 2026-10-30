---
name: gbp-reply-queue
description: GBP review replies are written by Claude and sent by a human-paced cron (5e0bcf2/7f0724d); Hotel Elegant + Silver Sand state; open question (2026-10-23) on putting recent reviews first; new reviews via scheduled Claude tasks
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-27T11:36:31.780Z
---

Shoaib, 2026-09-30: every review gets a reply, old and new, sent automatically "jaisy koi insaan replies ker raha hn manually". He asked why the API was needed ("api se q yahan se nai likhy ja sakty?"). So Claude writes every reply, and there is no Anthropic API key or SDK on the server.

**Built in 5e0bcf2:**
- `lib/gbp-replies.ts` with `queueReplies`, `sendNextReply`, `replyQueueStats` and `listQueuedReplies`.
- Table `gbp_reply_queue`, with SQL in dashboard-schema.sql. Its status is approved, draft, sent, skipped or failed.
- `/api/gbp/reply-cron` (CRON_SECRET) is called by a second step in `.github/workflows/social-cron.yml` every 15 minutes, with `if: always()`.
- **Human pace (7f0724d, 2026-10-09; constants at the top of `lib/gbp-replies.ts`):**
  - 09:00–23:00 PKT only;
  - 15–35 minutes after the previous reply (varied per reply). The gap is **global**: every location shares one clock;
  - at most 18 replies per location in any 24 hours;
  - a 20% chance of skipping a turn;
  - each review is re-read first and skipped if it already has a reply.
  - The location write cap is 25 a day (`GBP_DAILY_CAP_PER_LOCATION`), with 5 reserved for posts. The 5-minute gap across all projects (`paceGbpWrite`) still applies.
  - **Order (`nextReply`):** complaints (≤3★) first, oldest first, across all locations; then the rest, newest first.
  - **How it got there:** b755b80 first slowed it to 20–45 min and 12 a day ("legal delay lazmi, burst nai hona chaiye k suspension ajaye"). Research then found no official daily reply limit (the API ceiling is 10 edits per minute per profile), and every reply sent shows `reviewReplyState: APPROVED`. Shoaib then asked for more per day.
  - **Real risk is content, not count.** Google's 2026 moderation rejects copy-paste templates, keyword stuffing, promotion, links and contact details. Check `reviewReplyState` through gbp_request GET `.../reviews` after big batches.
  - **Late edits:** reviewers sometimes edit a review after our reply. The queue skips reviews that already have a reply, so a correction goes through gbp_reply_review directly. Examples: Haroon Bloch's review now names Ms. Zahra, not Hassan Ali; Baharay Gull changed 5★ to 1★ with no text.

**Status on 2026-10-23, and an OPEN QUESTION:**
- Hotel Elegant had 659 reviews (4.6★) and 185 replies sent, but nothing had gone out since 17 Oct. Its 4 approved replies (Yasir Khan, Malik Danish, Muhammad Haseeb, Daniyal Akram) were stuck behind Silver Sand.
- Silver Sand had 850 reviews (3.8★), 196 replies sent and 341 approved in line. 332 of those are 2015–2022 reviews, including 58 complaints. All of the last day's 14 replies went to Silver Sand's 2019 complaints.
- **I proposed a new order:**
  1. reviews from the last 30 days first (any hotel, any stars);
  2. then the old complaints;
  3. then the rest.
- I asked twice (2026-10-23). Shoaib never answered, and by 2026-10-27 the question had settled itself, so it is closed:
  - Silver Sand's old complaints were all sent, and the queue moved on to positives, newest first.
  - Hotel Elegant caught up: 661 reviews, 4.6★, 195 sent, 0 waiting, 0 unreplied.
  - Silver Sand: 853 reviews, 3.8★, 258 sent, 283 approved waiting (4 from Oct 2026, 5 from Jul 2025, the rest from 2019–2021), about 11–18 a day, so done around mid-to-late November.
  - The new-review scheduled tasks are working: replies created on 26–27 Oct were sent the same day.
- **MCP:** `gbp_queue_replies` (confirm gate, status approved/draft/skipped) and `gbp_reply_queue`.
- **Dashboard:** a "Reply queue" card on `/dashboard/gbp` with edit, save, and "Don't send".

**Hotel Elegant:** project f82159f5…, location `locations/422250134438438877`.
- On 2026-09-30 it had 627 reviews, 158 of them unreplied (1★ 11, 2★ 1, 3★ 3, 4★ 11, 5★ 132), all from 6 July onwards.
- The hotel's old replies were just "Thank you!". Its one real complaint reply was signed "Warm regards, Management, Hotel Elegant Executive Suites Multan".
- The new replies are personal:
  - Positives are signed "— Team Hotel Elegant".
  - Complaints get the Management signature and no promises (no free stays, no staff-firing claims).
  - 1★ reviews with praise text are thanked as praise.
- Replies were in scratchpad `gbpreplies/{a,b,c,d}.json`. On 2026-09-30 Shoaib ran the SQL and all 158 were queued as `approved`.

**Hotel Silver Sand Multan:** project 5ada7f8d…, location `locations/3905396707891391632`.
- Audit on 2026-10-09: 846 reviews (3.8★), **536 unreplied**.
  - 54 are recent (Jul 2025 – Oct 2026). 24 were sitting as `draft`. 4 of those drafts had problems: 3 complaint drafts had the phone number and 1 promoted the Deluxe Triple Room.
  - 482 are old: 1 from 2022 and 481 from 2015–2021, mostly stars only.
- **Same day, Shoaib said "pehle 1 aur 2 karein, phir 4":**
  - All 54 recent reviews are now queued as `approved`. The 24 drafts were fixed, and 30 new replies were written.
  - **Silver Sand voice:** positive replies have no signature. Complaints end with "Warm regards, Management, Hotel Silver Sand Multan". Replies never name staff (the client's brand rule) and never include a phone number or promotion.
  - The scheduled task `hotel-silver-sand-review-replies` runs at 11:00 and 18:00. It only handles reviews created on or after 2026-10-01, so the 482-review backlog is never picked up as "new".
  - The 482 old reviews (option 3) were later approved: by 2026-10-23 there were 332 old (2015–2022) replies approved in the queue.

**Sender clock:**
- GitHub's `*/15` schedule really fires only every 4–5 hours (seen 2026-09-30), which is far too slow for the queue.
- The fix: Supabase pg_cron + pg_net calls `/api/gbp/reply-cron` every 10 minutes. The job is named `gbp-reply-cron`.
- **Auth (236a841):** the job does not use CRON_SECRET. Shoaib ran the Vault attempt with the literal placeholder, and the secret isn't viewable in Vercel or on the office PC.
  - Supabase generates its own random token in the `cron_tokens` table, row `gbp_reply` (RLS on, no policies).
  - The endpoint reads it back with the service role and accepts either that token or CRON_SECRET (used by the GitHub backup).
  - The SQL is in dashboard-schema.sql.
  - To check the job: `select status_code, content from net._http_response order by created desc limit 3;`
- **Verified on 2026-09-30:** the first reply (18:06 PKT) was a manual call using the token. The second (Hasan Ahmad, 3★, at 13:20:09 UTC) was sent by pg_cron on its own at the 13:20 tick, so the pipeline works end to end.
- The GitHub step stays as a backup. Two callers at once are safe because `reserveGbpWrite` inserts the slot first.
- The social poster (`/api/social/cron`) has the same GitHub slowness and hasn't been moved yet.

**Planner → Google, commit 073d3dc (2026-09-30).** Shoaib asked for Hotel Elegant and Silver Sand to be on all three platforms (Facebook, Instagram, Google), plus a catch-up of past posts "with legal delay".
- **Backfill:**
  - The `gbp_post_backfill` table holds 32 rows (16 per hotel, 15–30 Sep). `lib/gbp-backfill.ts` posts them one at a time, oldest first:
    - only 10:00–22:00 PKT;
    - at least 3 hours apart per location (the planner post counts);
    - never while a planner post is due;
    - with a 30% chance of skipping a turn.
  - The original uploads were deleted from R2 after posting. The image is fetched back from the Instagram `media_url` (falling back to Facebook), parked in R2 and served via `mintFacebookMediaUrl`.
- **Google captions:** `gbpCaption` now drops the whole contact block (phone, email, links, 📞📧📍🌐 lines). Shoaib: "Text + Hashtags kafi hain".
- **Daily Google write cap:** replies stop at 20 of the location's 25 daily writes (`REPLY_RESERVED_FOR_POSTS` = 5; the cap was 20 until 2026-10-09). `/api/gbp/reply-cron` yields while a planner post is due, then sends a backfill post first and otherwise a reply.
- **Social cron:** it is now on pg_cron job `social-cron` every 15 minutes (token `social`), verified returning 200. `lib/cron-auth.ts` accepts CRON_SECRET or any `cron_tokens` token.
- **TODO:** make the GitHub social step manual-only (`workflow_dispatch`) so Instagram can't be double-posted by overlapping runs.

**New reviews:** these go through a local scheduled task, `hotel-elegant-review-replies`, on the office PC.
- It runs at 10:30 and 17:30 every day, and only while the PC and Claude app are running.
- It reads reviews with `gbp_list_reviews` (onlyUnreplied) and keeps only those where `queued` is null.
- It writes the replies itself and queues them with `gbp_queue_replies` (onlyNew=true, confirm=true).
- Reviews with serious allegations, or a burst of 3+ one-star reviews, go in as `draft` for Shoaib to check.
- **Fix 2026-10-09:** the runs showed "succeeded" but stopped at the first `gbp_list_reviews` call (waiting on a permission prompt), so 18 reviews from 1–9 Oct were never queued. After checking the 100 sent replies (none wrong), Shoaib said "always allow". `~/.claude/settings.json` → `permissions.allow` now has `gbp_list_reviews`, `gbp_queue_replies` and `gbp_reply_queue` (ABS connector id e41c058f…). The 18 were written and queued by hand the same day. To verify, check the task's runs with list_task_runs / list_events: a working run lasts more than a few seconds.
- The `queued` field and the `onlyNew` option were added in e26ac7d and a10ed85. `stars` and `limit` also accept numeric strings, because this client sends numbers as strings.

Related: [[feedback-google-friendly-pace]] (vault) and the GBP integration memory in the vault.
