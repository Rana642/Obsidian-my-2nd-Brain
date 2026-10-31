---
name: meta-app-review-video
description: "Where the Meta App Review for \"Socially Snap Planner\" stands (2026-10-31) — record the video on the home PC; clip plan, captions, Ads by Shoaib as the test Page, open decisions; do NOT submit without Shoaib"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-31T12:24:15.478Z
---

**Status 2026-10-31 17:30 PKT:** Shoaib will record the video on the **home PC about an hour later**, once Meta's hourly app limit has reset. Nothing is recorded or submitted yet. **Submitting is his decision; never submit for him.**

**App:** Socially Snap Planner, id 970252279417151.
- Live since 9 Oct, owned by the Socially Snap business.
- Every permission has Standard access only, so only role holders can grant them. The Testers list once lost Ijaz/Ajaz; see [[waiting-on-shoaib]].
- `docs/meta-app-review.md` has the use-case texts. Updated 31 Oct: **9 login permissions, `pages_read_user_content` included** (Insights reads comment counts), plus the 3 Instagram Login ones and public_profile.

**Test Page = Ads by Shoaib (Shoaib's choice).**
- FB Page id 1353208461202466.
- Instagram @adsbyshoaib (17841452557231397) was linked on 31 Oct and imported at 12:53 via Shoaib's login, after he reconnected with "all current and future Pages/Instagram".
- The test post goes on his own page, never a client's. It needs a real Ads by Shoaib design from him, with a caption Claude writes and he approves. Not "test" content.

**Clip plan:** record with Win+Alt+R, 1080p, no voice, English captions added in CapCut.
1. **Posting:** Planner → upload → caption → Ads by Shoaib → FB + IG → ~15 min ahead → calendar → pause → show the post on the FB Page and on Instagram.
2. **Insights:** Social → Insights → Ads by Shoaib: Page numbers, post list (reactions/comments), IG numbers, then Best posting times. Then move a scheduled post and delete it.
3. **Instagram Login** (instagram_business_*): needs an Instagram Tester account (avenzaland or danishconstructionco) and its password. **Claude recommended submitting this part later. Ask him.**
4. **Login:**
   - Integrations → Facebook Business login → Reconnect → "Edit previous settings" → scroll the permissions slowly → "all current and future" Pages/Instagram → Save → Import Pages.
   - Do this only after the app limit resets: the callback calls `/me/accounts` and debug_token.
   - Reconnect is safe now: `refreshFacebookPageTokens` keeps each Page on its login unless that login lacks permissions.

**Captions for the screen:**
- "Socially Snap: agency content planner for client Facebook Pages and Instagram accounts"
- "pages_manage_posts + instagram_content_publish: scheduling a client-approved post"
- "The post is published on the Facebook Page" / "…and on Instagram"
- "pages_read_engagement + pages_read_user_content + read_insights + instagram_manage_insights: post performance and insights"
- "Best times to post, from Page and Instagram insights"
- "Facebook Login for Business: the admin grants access to their Pages (pages_show_list, business_management)"

**Still open:**
- **The reviewer demo login** must never be the admin login. Note: a dashboard account sees every client's data, so decide how to limit what it can see.
- Instagram Login permissions now, or later?

Related: [[connections-import-pages]], [[fb-native-stuck-after-reconnect]], [[adsbyshoaib-dashboard-navigation]].
