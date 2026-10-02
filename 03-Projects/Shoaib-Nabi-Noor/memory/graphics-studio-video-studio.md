---
name: graphics-studio-video-studio
description: "Video Studio in Graphic Studio (2026-10-02) — branded video editing on top of Shoaib's video-use fork; local-only; install state and the two Windows fixes"
metadata:
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-10-02T16:31:00.896Z
---

Shoaib forked **browser-use/video-use** → `github.com/Rana642/video-use`, a conversation-driven video editor skill (ElevenLabs Scribe word-level transcripts → EDL → ffmpeg render, plus subtitles, grade and overlays). He asked for it to become **part of Graphic Studio** ([[graphics-studio-project]]) and approved the plan.

**Install on the home PC (2026-10-02):**
- Clone: `C:\Users\PC\Developer\video-use`.
- `pip install -e .` done (Python 3.14 OK).
- ffmpeg 9.0.2 installed with winget (`Gyan.FFmpeg`). It's on the user PATH; old processes don't see it, so lib/videos.ts finds it under `%LOCALAPPDATA%\Microsoft\WinGet\Packages`.
- Skill registered as a junction `~/.claude/skills/video-use`.
- yt-dlp 2026.08.19 installed with pip (2026-10-03) for analysing/editing videos from links. Its exe dir (`%APPDATA%PythonPython314Scripts`) is NOT on PATH, so run it as `python -m yt_dlp`. Ask before every download. Public videos only; reposting other people's videos is a copyright risk.
- `.env` has a valid ElevenLabs key (sk_…, free tier, verified 2026-10-02). The first paste was the key ID, not the key; ElevenLabs answers "API key ID used as API key".

**Fork commits (pushed):**
- `bf650a8`: `VIDEO_USE_SUB_FORCE_STYLE` env var restyles captions.
- `8d07bf8`: Windows fix. The subtitles filter path is now `as_posix()`; before it, every captioned render failed on Windows with ffmpeg exit -2. Keep both if pulling upstream.

**Graphic Studio (commit 0efd416, rebased onto the office's Batch work):**
- `lib/videos.ts`: calls the helpers (`transcribe_batch`, `pack_transcripts`, `render`). It does not copy video-use.
  - Brand caption style: brand font, white text, outline in the primary hex as ASS BGR, MarginV 90.
  - `appendEndCard`: logo on the brand colour, 2.5 s.
  - Upload to a public Supabase bucket `videos` when under 45 MB; larger files stay local.
  - Scribe cost at `VIDEO_SCRIBE_USD_PER_HOUR` (default 0.40).
- MCP `mcp/tools/videos.ts`:
  - `studio_video_brand_style`
  - `studio_video_start`: confirm gate, since it costs money; preview shows minutes and cost.
  - `studio_video_render`: preview or final.
  - `studio_list_videos`
- `/videos` page, a Videos nav link, and a Videos column on Costs.
- Table **video_generations**: SQL is in supabase-schema.sql and **was run by Shoaib 2026-10-02 (with the Batch delivery block)**. There is no DB URL for this project.

**Why:** local-only on purpose. ffmpeg, Python and long renders can't run on Vercel. The editing conversation runs in Claude Code with the video-use skill, and Graphic Studio supplies the brand.

**How to apply:**
- Flow: `studio_video_start` → read `takes_packed.md` → plan → **user OK** → write `edit/edl.json` → render preview → self-check → render final.
- EDL `subtitles` paths are relative to the EDL's own folder. Prefer `--build-subtitles`, which render passes by default.
- Tested with a synthetic clip: brand captions and the end-card render correctly. **Not yet tested with real footage or Scribe**, because there's no key.
- The graphics-studio MCP server must be restarted to load the new tools.
