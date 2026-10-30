---
name: monthly-client-report
description: "Fixed monthly report pattern for Meezab + Tad (Shoaib 2026-10-30) — plan vs done, client extras \"plan mein nahi tha lekin hua\", next month plan saved in KB; Ads by Shoaib branding; deck builder location"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-30T11:58:52.590Z
---

**Rule (Shoaib, 2026-10-30), for Meezab Z. International and Tad Pharma:**
- He asked for a reporting presentation with a glimpse of the work so far plus next month's plan.
- From the next month on, the report compares last month with its plan.
- It also mentions extra things that came from the client: "plan nai tha lakin hua".
- He said: "yeah pattern meezab aur tad k fix ker do … Branding in 2no k liye Ads by Shoaib ki use kern".

**How to apply, every month:**
1. **Inputs:**
   - KB `marketing_doc/monthly-plan-YYYY-MM` (last month's plan);
   - KB `client-report-log-YYYY-MM` (the work log);
   - the Planner record (scheduled_posts);
   - `social_get_insights` (28 days, organic).
2. **The deck, in Roman Urdu with Ads by Shoaib branding** (Cloud/Ink/Citrus, Cobalt + Forest decorative; Instrument Serif + Geist + Geist Mono; `public/brand/logo-horizontal.svg`). Sections:
   - plan vs done (what slipped and why);
   - "Plan se hat kar": client extras that were not in the plan;
   - results;
   - next month's plan, week by week;
   - what we will measure;
   - what we need from the client.
3. **Save the new month's plan** as KB `marketing_doc/monthly-plan-<next>`. Keep appending to `client-report-log-<month>` as work happens.

**Builder:**
- Location: `Meezab Z/Reports/_source-2026-10/` (`data.mjs` → `node build.mjs` → Edge headless `--print-to-pdf`, Google Fonts online).
- For the next month or for Tad: copy the folder and rewrite `data.mjs`.
- Edge can silently write no PDF when another headless Edge holds the default profile. Always pass `--user-data-dir=<scratch>/edge-profile`.

**Status, 2026-10-30:**
- Meezab first edition built: `Meezab-Z-Monthly-Report-Oct-2026.pdf`, 13 slides. November plan saved in the KB.
- Tad Pharma's edition still to build in the same pattern. Note: Tad's calendar is empty from 31 Oct.

Related: [[meezab-knowledge-base]], [[tadpharma-knowledge-base]], [[shoaib-working-preferences]].

**Package terms for BOTH Meezab and Tad (Shoaib, 2026-10-30, second message):**
- **16 posts a month each.** We design, caption and post every one.
- **Video:** raw video → we edit, caption and post. An approved video REPLACES one planned image post, and that image post moves to next month, so the count stays at 16.
- **Website work for both:**
  - audit and major changes;
  - careers pages;
  - a WEBSITE dashboard: one admin panel for the site's leads and the careers page (job posts, applications). Shoaib corrected this on 30 Oct; it is NOT a LinkedIn dashboard.
- **Every plan says where Claude (AI) is used:** image editing, captions, content generation, website development, video editing. Add a line that nothing goes live without a check and the client's approval.
- In Meezab's November plan, the 16 include the 4 Jummah + 2 dated days. Confirm with Shoaib whether Jummah should count inside the 16.

**In the KB (Shoaib OK'd, 2026-10-30):** both October reports are uploaded as `document`, titled "Monthly Report — October 2026 (with November plan)".
- Meezab: asset c2d8db78.
- Tad: asset 38818c89.
- Next month: upload the new edition the same way, with `kb_create_direct_upload` and kind `document`.

**Updates, 2026-10-30:**
- **Language:** Meezab + Tad reports are in ENGLISH (Shoaib: "Reports English mai banao … Sirf inki filhal"). The other clients' decks stay as before.
- **Off-plan section:** Shoaib took the "Off-plan: not in the plan, but done" slide OUT of the October edition, but "discontinue nai kerna". The data (EXTRAS / NOT_DONE) stays in data.mjs, and `SHOW_OFF_PLAN = false` hides the slide. Set it back to true, or remove the flag, for future editions unless he says otherwise.
- **Shared builder:** `Meezab Z/Reports/_source-2026-10/build.mjs` (pass a data.mjs path).
  - Tad data: `tad-pharma-social/Reports/_source-2026-10/data.mjs`.
  - Edge print is flaky: use a fresh `--user-data-dir` per try and retry up to 5 times.
- **Client logos (Shoaib: "Dono k logos bhi laga do Reports mai"):** every report shows the client's logo on the cover ("Prepared for" + logo) and in every slide footer, next to the Ads by Shoaib branding.
  - Set it in data.mjs with `CLIENT_LOGO = "assets/client-logo.png"`, `CLIENT_LOGO_COVER` (px height) and `CLIENT_LOGO_FOOT`.
  - Meezab uses the CDN horizontal lockup. Tad uses the transparent logo with the PHARMA pill kept intact.
