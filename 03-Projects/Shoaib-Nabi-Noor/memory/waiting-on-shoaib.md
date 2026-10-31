---
name: waiting-on-shoaib
description: "One list of open questions and on-hold work waiting on Shoaib's answer, with dates and links (as of 2026-10-27); prune items as he answers"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-31T12:24:02.654Z
---

Each item has its details in the linked file. Remove an item here as soon as it is settled. Don't nag: bring an item up when the topic comes back, or when he asks what's pending.

**Social posting**
- **LinkedIn October backfill (28 Oct).** The image-recovery fix 3831183 works (Tad test 16:45, Tad 17:10 OK). 51 copies are re-spaced to ONE per cron run, 28 Oct 17:52 → 29 Oct 06:22 PKT, because each LinkedIn copy takes ~13 s of the cron's 20 s budget, and two per run pushed client posts back. Avalon's 15 copies stay paused (`held`, next_try 30 Nov) until its FB/IG token works; their image recovery needs it. On 29 Oct: check for failures, then release Avalon after the Ijaz Danish connect.
- **NEXT, on the home PC: the Meta App Review video for "Socially Snap Planner"** (Shoaib, 2026-10-31 17:30: "Ghar waly pc per keron ghanty baad"). The full plan, status and open decisions are in [[meta-app-review-video]]. **Lesson: for anything about the Meta/Google apps, check the vault's Socially Snap notes first.**
- **Facebook logins (2026-10-31):** 15 of 16 Pages now run on Shoaib's login.
  - **Only Hotel Avalon (FB + IG) is left on Ajaz Hussain's login (slot 2)**, because only that login reaches it. "Ajaz Hussain" is the same FB user who used to be "Ijaz Danish".
  - Ask Ajaz to give Shoaib access to Avalon, then move it: Import Pages → "via Shoaib Nabi Noor", or the fb-audit POST `move`. Slot 2 can then be removed.
  - Until then, Ajaz should reconnect once. His 16:21 login lacks `pages_read_user_content`, so Avalon's Facebook insights fail.
  - Details: [[connections-import-pages]].
- **Meta app hourly limit hit on 2026-10-31 ~17:00 PKT** ("(#4) Application request limit reached"). It came from many debug_token and /me calls (audits, and the new Integrations page and sidebar badge). Those are now cached (a362241). Page-token posting wasn't affected. Avoid bursts of audit calls.
- **Later, after App Review:** a System User token for posting, so no personal profile sits behind the Page tokens. Then a checkpoint on one profile can't stop posting.
- **Meta checkpoint 24–26 Oct** (asked 2026-10-27): from 24 Oct 10:00 UTC to 26 Oct 17:32 UTC every live Meta call failed with "You cannot access the app till you log in to www.facebook.com and follow the instructions given." That is a security checkpoint on the Facebook account that owns the token. **Correction (30 Oct):** the natively scheduled FB feed posts did NOT go out either. 36 sat in Meta's scheduler. They were found and fixed on 30 Oct; see [[fb-native-stuck-after-reconnect]]. 48 rows failed: 37 Stories (21 of them TONI&GUY) and Instagram for 9 feed posts and 2 Reels (Avalon 95/96/56, Eventia 96/57/95, Elegant 3.jpg + ROOM-103/105, Silver Sand 12/13). It works again from 27 Oct 05:00 UTC. **The 11 were re-queued Instagram-only on 2026-10-27, as Shoaib approved**, 28 Oct – 1 Nov PKT. The FB result was moved to `result.live` (not `native`), so a reschedule can't delete the live FB post. Auto mode blocked my direct DB PATCH twice, so Shoaib ran the script himself in his PowerShell terminal (PowerShell 5.1: no `&&`, and write the script file in a separate call first). Still open: failed Stories count as "used" in `lib/story-drive.ts` recycling, so they go to the back of the line; should they go first instead? Related fix 836381a: posted and failed stories now count toward `per_day`. Before it, TONI&GUY got 11 that day instead of 8 after the slot hours changed to 16–23 PKT. TONI&GUY `109.mp4` (61 s) is skipped until it is trimmed to 60 s.
- **TONI&GUY IMG_6257 Reel → Story** with the TG monogram: he said "ruk jao abhi" (2026-10-23). Not posted. [[toniandguy-reel-logo]]
- **Avenza Land 29.png** 31 Oct duplicate is held: email Fatima, or ask for a new post? (2026-10-22) [[aijaz-danish-brands]]
- **Tad**: he should check Meta Business Suite for stale old native drafts after 22 Oct. Should I build a check tool? (2026-10-21) [[tadpharma-knowledge-base]]
- **REEFCO**: Khaled wants dark backgrounds, which clashes with the global light rule; also whether to lock the layout. (since 2026-10-10) [[reefco-social-posts]]
- **Tad**: ask for an official white/mono logo. (2026-10-03) [[social-design-system]]

**Platforms (waiting on others)**
- **LinkedIn Community Management API**: vetting passed (2026-10-28), Development Tier added. **The upgrade form is due by about 2026-11-18** and needs a working demo. Waiting on his OK to build the OAuth connect + Posts API. [[linkedin-community-api]]
- **YouTube**: he needs to enable the APIs and scopes and apply for the audit. Until then, uploads stay private. [[planner-reels]]

**Website and dashboard**
- **GHL backlog**: which items to build first (planned for 2026-10-17, still not chosen). [[adsbyshoaib-ghl-improvements-backlog]]
- **Second-brain vault memory sync**: the vault copy is from 2026-09-30 (41 files, against 77 here). The vault repo is PUBLIC and these notes name clients, so ask whether to sync everything or only notes without client details. (asked 2026-10-26) [[feedback-no-secrets-in-memory]]
- **Socially Snap revert**: remove "formerly Socially Snap" once he confirms the GBP rename is done (since 2026-08-24). [[adsbyshoaib-socially-snap-temporary]]
- **About page "8 industries"** list doesn't match the real clients (since 2026-08-24). [[adsbyshoaib-aesthetic-audit-2026-08-24]]
- **Resume work experience** (4 jobs + 6 remote projects) is still missing. [[adsbyshoaib-resume-data]]
- **Indexing** stays off until he says the site is final. [[adsbyshoaib-no-indexing-until-final]]

**Open from 2026-10-30:**
- **Elegant reels ROOM-201 (28 Oct) and ROOM-103 (29 Oct)** never reached Facebook. They are on IG. The video files are gone from storage, so they need uploading again for FB. [[fb-native-stuck-after-reconnect]]
- **The temporary `/api/social/fb-audit` route** (cron-auth; audit, delete, resubmit) is still deployed. Decide: remove it, or keep it for audits.
- **AI design check of new uploads** (numbers, spelling, logo) was proposed. Give a PKR cost estimate first. [[morning-report]]

**Resolved 2026-10-30 (second-login outage + checkpoint backlog):**
- **Second Facebook login** was reconnected on 29 Oct by Ijaz Danish (Tester). Its tokens cover Avalon, Eventia and Danish FB+IG; no remapping was needed. The 3 held posts went out.
- **12 partly-failed rows from 28–29 Oct** were re-queued for their missing platforms only (script `requeue-missing.cjs`), 30 Oct 10:58 AM – 12:38 PM PKT:
  - Avalon/Eventia: Instagram parts + Stories
  - Avalon: 2 LinkedIn copies
  - Danish: Stories
- **TONI&GUY's 20 checkpoint-failed Drive stories (24–26 Oct).** Shoaib: "google drive se fetch ker schedule ker do pichli ab rehny do".
  - Nothing was deleted. 21 already-shown stories (31 Oct, 1 Nov, first 5 of 2 Nov) moved +6 days to 6–8 Nov (`tg-story-swap.cjs`).
  - The hourly Drive sync refills the freed slots, 2 per run: 132.mp4, then the 20 failed files, fetched fresh from Drive.
  - The old failed story rows stay as they are, and other projects' old failed stories are left too.
- **Still open:** Danish IG (Instagram Login) gave "Unexpected end of JSON input" on 29 Oct. Check whether its 30 Oct 11:28 AM retry also fails.
