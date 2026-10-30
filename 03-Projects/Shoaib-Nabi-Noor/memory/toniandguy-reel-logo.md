---
name: toniandguy-reel-logo
description: TONI&GUY Reels/Stories ALWAYS get the TG monogram logo on the right side (not the TONI&GUY wordmark); transparent PNGs saved in the TONI&GUY KB folder; ffmpeg recipe
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-30T10:55:36.431Z
---

Shoaib, 2026-10-23: "ye wala logo nai lagna TG wala lagana hai Reel per hamesha … aur right side per". When a TONI&GUY Multan Reel or Story video needs a logo, use the **TG monogram**, top-right. Do not use the TONI&GUY wordmark, and no "Multan".

**Why:** his brand choice for video. The wordmark (first try, top centre) was rejected.

**How to apply:**
- Files: `E:\Rana Shoaib\My Projects Website\Toni and Guy\Toni And Guy Knowledge Base\TG monogram - black (transparent).png` and `… - white (transparent).png` (716×907), cut from the client sheet `T&G logos (1).png` (rows 333–1219).
- On a 720×1280 video: 118 px wide, x = W−w−40, y = 150 (below the Story header). ffmpeg: `overlay=x=W-w-40:y=150`, then libx264 CRF 20, faststart, AAC.
- Pick black or white by the background. On light salon interiors black reads; white disappears on white walls. Check 3 frames first.
- Shoaib asked to see a preview before posting. Send preview frames and the mp4 for his OK the first times.

See [[toniandguy-multan-knowledge-base]], [[planner-upload-from-local-folder]].

**ON HOLD (2026-10-23):** the Reel from `C:\Users\Abdul Ahad\Desktop\New folder` (IMG_6257) has the black TG monogram added top-right. A preview was sent. Shoaib then said "ruk jao abhi", so it was never posted. In the middle section the logo sits over a wooden coat stand (still readable). The preview mp4 was in a session scratchpad, which may be gone. Remake it from the source file with the recipe above when he says go, and post it to the TONI&GUY Story on FB + IG. He has to be logged in to the dashboard for the upload.

**2026-10-30: 11 TONI&GUY Drive story videos must never run.** Shoaib: "in sb ko hold kero ye nai lagni chaiye".
- The videos: 132, 139, 141, 142, 152, 157, 163, 165, 166, 174, 176 (.mp4). He also deleted them from the Drive folder.
- What was done (script `tg-hold-stories.cjs`):
  - Their Drive ids were added to `story_drive_sources.hold_file_ids`; the TONI&GUY hold list is now 30.
  - Their 4 upcoming story rows were removed: 163, 139, 141, 165.
- Later on 2026-10-30 he added 2 more ("ye 2 Stories bhi del ker do"): 178.mp4 and 1130.mp4.
  - Both are on the hold list, which is now 32.
  - Neither was scheduled; each had only a failed row from 26 Oct.
- **Lesson:** deleting a file from Drive does NOT stop a story that is already imported. The R2 copy and the scheduled row stay, so remove the row too.

**2026-10-30 standing rule: TONI&GUY Stories must never stop.** Shoaib: "toni ki Stories nai rukni chaiye incase kabhi problem aye to apny inform kerna hai".
- **Watchdog** (commit 278ea77, `lib/story-watchdog.ts`): runs in the social cron. When the Drive Stories stop, it emails Shoaib; it emails again when they run again.
- The `alert` column was added by Shoaib on 2026-10-30, so the watchdog is live.
- **In any TONI&GUY session:** if stories look stuck, check this first and tell him in Roman Urdu.
