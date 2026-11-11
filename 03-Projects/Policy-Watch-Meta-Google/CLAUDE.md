# Policy Watch: Meta + Google Ads

Not a product — a shared state file for the global skills `meta-ads-policy-guard`
and `google-ads-policy-guard` (installed at `~/.claude/skills/` on Shoaib's
machine) and for the weekly "policy-watch" cloud routine that keeps them
current.

- `state.md` holds the latest checked summary + date for each of the 4
  policy areas (Meta Ads, Meta Developer Platform, Google Ads, Google Ads
  API). The cloud routine fetches the live pages weekly, updates this file
  and commits/pushes when something materially changed, and pings Shoaib
  on WhatsApp when it does.
- When working locally in a session with `~/.claude/skills/` access: if
  this file's dates are newer than the skill's local `references/*.md`
  files, copy the updated sections down into the skill folder so the local
  guard stays in sync with what the routine found.
- This repo is **public** — only policy summaries and source links go here,
  never any account IDs, tokens or client specifics.
