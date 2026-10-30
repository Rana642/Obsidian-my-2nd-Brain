---
name: feedback-pkt-time-gotcha
description: "On this Windows PC, Git Bash `TZ=Asia/Karachi date` silently prints UTC — get PKT as `date -u` + 5h before scheduling anything"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-21T08:28:39.802Z
---

On 2026-10-21, `TZ=Asia/Karachi date` printed 08:15, which was UTC (Git Bash has no zoneinfo). The real time was 13:15 PKT. I planned a day of posts from "09:30 PKT", and 4 of them were already past due. I caught it and parked them before the 15-minute cron fired, so nothing burst out.

**Why:** the planner cron publishes everything whose time has passed, all in one run. A wrong clock means a burst of posts on a client Page, and FB native scheduling silently skips times under 10 minutes ahead.

**How to apply:** before scheduling, always run `date -u` and add 5 hours for PKT, or use `node -e "console.log(new Date().toLocaleString('en-GB',{timeZone:'Asia/Karachi'}))"`. Never trust `TZ=…` in Git Bash. The first slot should be at least 20 minutes ahead.

Related: rescheduling an already natively-scheduled post with `social_set_caption_and_schedule` cancels the old Meta draft only best-effort (`deleteFacebookPost` swallows errors). Nothing verifies it yet, so check Meta Business Suite → Planner for stale drafts. See [[fb-native-schedule-feed-fix]].
