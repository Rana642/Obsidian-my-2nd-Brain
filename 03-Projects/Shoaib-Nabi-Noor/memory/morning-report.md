---
name: morning-report
description: "Daily 8 AM PKT Planner email (lib/morning-report.ts, 5fe341a, 2026-10-30) — free code report Shoaib chose over an AI team; what it covers, once-per-day key, preview script"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-30T12:15:51.202Z
---

**What it is.** Shoaib asked for a team of agents to watch the whole flow, from upload to post. I recommended code guards plus one report instead. He chose the free code report on 2026-10-30: "haan muft wali report bana do".
- `lib/morning-report.ts` (commit 5fe341a) runs at the end of every social cron.
- From 08:00 PKT it sends ONE email per PKT day, in Roman Urdu headings.

**Sections:**
- **KAL:** what went live per project, with per-platform post counts and stories.
- **MASLE:** failures, with stories grouped.
- **FACEBOOK CHECK NE KHUD THEEK KIYE:** the `resent` entries from [[fb-native-stuck-after-reconnect]].
- **RUKI HUI:** posts held by a login problem.
- **AAJ:** what is left for today, with times.
- **KHAALI DIN:** empty days in the next 7. It is listed ONLY for projects that have `pending_caption` posts, following Shoaib's rule "koi din khali nai jana chaiye agar posts available hain". Monthly-plan clients like Meezab therefore don't create noise.
- **CAPTION KE BAGHAIR:** posts waiting on us.
- **CLIENT KI CHANGES:** posts waiting on the client.

**Problems are written in Roman Urdu** (b2222c6, Shoaib: "haan Roman Urdu mai ker do").
- The `REASONS` regex list maps the usual errors to plain Roman Urdu: checkpoint, permission, token expired, media gone, Google pace, rate limit, timeout, video size, duplicate.
- An unknown error stays as "masla: <English>".
- Failed stories are grouped into one line per reason.

**Once per day.** `db.rpc("check_rate_limit", {p_key: "morning-report:<PKT date>", p_limit: 1})` claims the day atomically and fails closed. A failed send deletes the key so the next run retries. No new SQL was needed.

**Preview without sending:**
- Run from the project folder: `node --env-file=.env.local --conditions=react-server --import tsx <scratch>/li-test/morning-preview.mts [ISO]`.
- On Windows the import needs `pathToFileURL`.

**Status:** the first report went out on 30 Oct at about 17:00 PKT (key morning-report:2026-10-30). The next one is due 31 Oct, around 08:00.

**Still open (later):** an AI design check of new uploads (wrong numbers, spelling, logo). It costs money, so give a PKR estimate and get his yes first ([[feedback-ask-before-spend]]).
