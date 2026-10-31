---
type: journal
date: 2026-10-31
topic: adsbyshoaib.com Planner, Facebook logins, Integrations page, Meta App Review, Choppers Studio ad
source: Claude Code session (office PC)
---

# Work log — 2026-10-31 (office PC)

Details are in [[03-Projects/Shoaib-Nabi-Noor/memory/MEMORY]], synced today. The code log is `docs/WORK-LOG.md` in the website repo.

## ▶ Pick up here (home PC)
**Record the Meta App Review video.** Plan, captions, test Page (Ads by Shoaib + @adsbyshoaib) and open decisions: [[03-Projects/Shoaib-Nabi-Noor/memory/meta-app-review-video]]. Wait about an hour after 17:30: Meta's hourly app limit was hit. **Never submit without Shoaib.**

## Facebook logins
- **Morning:** Ijaz Danish's login had lost every permission. The app's Testers list was empty, after Meta changed roles for business-owned apps.
  - Facebook Login showed "Feature unavailable" for a while.
  - He came back as **Ajaz Hussain** (the same FB user) with a Tester role.
- **Afternoon:** two problems broke Facebook insights for 9 Pages with #10.
  - The login scope had dropped `pages_read_user_content` (on 28 Oct).
  - "Newest login wins" moved those 9 Pages onto Ajaz's token, which didn't have that permission.
- **Fixed:**
  - The scope is back.
  - Pages keep their login unless it lacks permissions.
  - Import Pages shows "via <login>" and lets you choose the login.
  - Integrations shows which login each account uses, with a ⚠ when something is wrong.
  - Moved: 6 Pages by Shoaib in the dialog, then Eventia + Danish by Claude. **Now 15 of 16 Pages are on Shoaib's login. Only Hotel Avalon is on Ajaz's.**
- **Meta hourly app limit (#4)** was hit from too many token checks. Those are cached now, and posting was unaffected.
- Instagram **@adsbyshoaib** was linked to the Ads by Shoaib Page and imported.

## Dashboard
- **Socially Snap → Settings → Integrations:** Connections moved out of Marketing.
  - Each login shows its health: Meta's `debug_token`, the Pages it serves, "Reconnect needed" / "Posts retrying" / "Permission missing".
  - Details sit behind a ⚠ pop-over, not inline text.
  - A red badge on Settings shows while anything is broken.
  - The big page-wide pop-up was built, then removed at Shoaib's request.
- **The cron now tracks a blocked login per account,** so one bad login no longer holds back other Pages' posts (TONI&GUY Stories waited all night because of it).

## Choppers Studio Pink October ad
- Built via the Marketing API, left paused for Shoaib to switch on.
  - Leads goal (qualified WhatsApp leads); Multan 12-mile pin; Women 22–50; FB + IG + WhatsApp.
  - Runs until 3 Nov.
  - Primary text, headline and greeting set; Meta's auto-enhancements off.
- To get there:
  - ABS had to be assigned to the ad account and the Page.
  - Lead Ads terms were accepted for the Page.
  - A new campaign was needed, because a CBO campaign needs the same goal on every ad set.
- See [[03-Projects/Shoaib-Nabi-Noor/memory/meta-ads-api-setup-lessons]].

## Also today
- TONI&GUY: Stories 178 and 1130 were put on hold for good.
- The morning report now writes problems in Roman Urdu.
- `meta_ads_get` no longer returns the token inside paging URLs.

## Open
- Hotel Avalon: Ajaz gives Shoaib access, then it moves. Until then, Ajaz reconnects once so Avalon's insights work.
- Elegant reels ROOM-201 and ROOM-103 need their video files again for Facebook.
- App Review: the reviewer demo login, and whether to include the Instagram Login permissions now.
- Remove the temporary `/api/social/fb-audit` route eventually.
