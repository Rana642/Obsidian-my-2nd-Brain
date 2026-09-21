---
name: project-instagram-import
description: "Instagram bulk import to WordPress — Meta App created, Step 1 (Add Account) pending"
metadata: 
  node_type: memory
  type: project
  originSessionId: 4c5e8067-e2f2-4c5c-9e78-5bbb6c8e8c43
  modified: 2026-08-07T11:46:14.744Z
---

## Instagram → WordPress Bulk Import (In Progress)

**Status as of 2026-08-07:** Meta Developer App created ("in taking my soul home", App ID: 1543111460627029, Business type). Instagram product added. **Step 1: "Add account"** button needs to be clicked to connect the @takingmysoulhome Instagram account and generate an access token. Steps 2-4 not needed.

**Why:** 399 Instagram posts — manual import impossible. API script needed.

**How to apply:**
- Instagram account is already Creator type (ready for API)
- Instagram app ID: 1036633585651127
- Hashtags map to WP series: `#MercifulShades` → Merciful Shades of the Beloved, `#SoulVsEgoSeries` → Two Wise Inside: Soul vs. Ego, `#Jummah`/`#JummahMubarak` → Jumu'ah Reminder, `#LetsTellAllah` → Let's Tell Allah, `#ABreathOfReturn` → A Breath of Compassion
- Du'a/prayer posts use `#DailyDua #IslamicDua` — need new category or skip
- Script will: fetch all reels via API, parse caption (title = first line, description = rest), auto-assign series by hashtag, create WP Episodes via REST API
- Highlights on Instagram: Jumu'ah Prayer, Breath of Ret..., SOUL vs. EGO, Let's Tell Allah, Moments of..., Du'as, Merciful...
