---
type: journal
date: 2026-10-30
topic: adsbyshoaib.com Planner, Facebook check, morning report, monthly client reports, Choppers, TONI&GUY, Meezab, Tad
source: Claude Code session (office PC)
---

# Work log — 2026-10-30 (office PC)

Full details are in [[03-Projects/Shoaib-Nabi-Noor/memory/MEMORY]], synced today. The code log is `docs/WORK-LOG.md` in the website repo. Everything from 1–29 Oct is in those memory files.

## Facebook posts that never went live: fixed for good
- **Problem:** Avalon, Danish Construction and Eventia had no FB feed posts after 23 Oct.
  - An audit of every Page found **36 posts** (Avalon 7, Eventia 7, Danish 7, Silver Sand 6, Elegant 4 + 3 reels, Avenza Land 2). They sat in Meta's own scheduler past their time and never published.
  - They were scheduled before the 24 Oct login checkpoint/reconnect. The Planner still showed "posted".
- **Same day:**
  - The stuck drafts were deleted.
  - All 5 Jummah posts were re-sent first, then the other 28 across the evening.
  - All 62 upcoming natively scheduled posts were re-submitted with fresh tokens.
- **For good:**
  - `lib/social-verify.ts` checks every FB post on Facebook 20 min after its time. If the post isn't live, it re-posts and emails Shoaib.
  - Uploads are kept until FB is confirmed.
  - A permission error never causes a double post.
- **Still needs files:** Elegant reels ROOM-201 and ROOM-103 never reached FB, and their videos are gone (they are on IG). See [[03-Projects/Shoaib-Nabi-Noor/memory/fb-native-stuck-after-reconnect]].

## Morning report email (free, no AI)
- One email every day from 8 AM PKT: what went live yesterday, problems, auto-fixes, held posts, today's queue, empty days, and posts waiting on us or the client.
- Shoaib picked this over "a team of AI agents". An AI design check of uploads is for later, only after a PKR cost estimate. See [[03-Projects/Shoaib-Nabi-Noor/memory/morning-report]].

## Monthly client reports: Meezab Z + Tad Pharma
- Pattern fixed for both: plan vs done, client extras, results, next month's plan, what we'll measure, what we need.
  - English, Ads by Shoaib branding, client logo on the cover and in every footer.
  - The "Off-plan" slide is hidden this month but kept in the pattern.
- November plan: 16 posts each. Video replaces an image post when approved. Website work: audit, major changes, careers page and a **website dashboard** (leads + careers), not a LinkedIn one.
- PDFs are in the KB, and the November plans are in KB `marketing_doc/monthly-plan-2026-11`. See [[03-Projects/Shoaib-Nabi-Noor/memory/monthly-client-report]].

## Social Planner, day's work
- **TONI&GUY:**
  - Stories re-fetched from Drive. 13 videos are on hold for good (132, 139, 141, 142, 152, 157, 163, 165, 166, 174, 176, 178, 1130) and replaced.
  - Watchdog emails if Stories ever stop.
- **Portal uploads:** one combined report per uploader session, not one email per upload.
- **Jummah:**
  - Done for every project. Avenza Group caption approved and saved in the KB.
  - A Tad Jummah post was designed with GPT Image 2.5 (Gemini credits are used up) and posted at 12:45.
- **Overdue posts:** all scheduled today. Meezab Friday-1 and Egg Day were deleted.
- **Meezab:** 10 posts from the client's second Drive folder plus the Director's birthday post, all today.

## Choppers Salon
- New Facebook page draft.
- Audit + proposal deck (TONI&GUY style), uploaded to the KB.
- Full KB update so a new chat starts with everything via MCP.
- `Choppers Salon/CLAUDE.md` + folder memory created.

## Open / next
- Elegant reels ROOM-201 and ROOM-103 need their video files again for Facebook.
- The temporary `/api/social/fb-audit` route is still deployed. Remove it, or keep it for audits.
- Optional AI design check of uploads: cost first.
- Older open items: Danish IG "Unexpected end of JSON input", Meta App Review, LinkedIn upgrade form (~18 Nov), UptimeRobot, Choppers client questions. See [[03-Projects/Shoaib-Nabi-Noor/memory/waiting-on-shoaib]].
