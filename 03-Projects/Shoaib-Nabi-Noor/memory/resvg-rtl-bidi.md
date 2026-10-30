---
name: resvg-rtl-bidi
description: "resvg lays Urdu/Arabic on an LTR base — trailing \":\" lands on the right, a line-initial digit on the left, and English words reverse the Urdu order; fix = RLE…PDF wrap + manual runs for Latin words"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-12T08:24:18.498Z
---

Found on 2026-10-12 while building the Tad Urdu product posts. Shoaib had already saved Biofurose v8 to the KB with its dose-heading colon on the wrong side, so it was replaced by v9.

**Bugs in resvg (used by every code-rendered text in tad-urdu.ts, rf-series.ts and the other render scripts):**
- `direction="rtl"` only sets the text anchor. Bidi still runs on an LTR base.
  - A trailing neutral (`:` `،`) at the end of a line prints on the RIGHT. Example: `خوراک و طریقہ استعمال:`.
  - A line that starts with a digit (`1 گرام فی …`) puts the digit on the LEFT end.
- English words inside an Urdu line (`K3`, `FCR`, `(Bile)`, `Feed Intake`) make the Urdu segments come out in reversed word order.
- `unicode-bidi="embed"` does nothing. RLM fixes the colon but not the English-word case.

**Fix, in `scratchpad/tadstyle/tad-urdu.ts`:**
- Wrap every Urdu string in U+202B … U+202C (RLE…PDF), both when measuring and when drawing. This fixes the colon and leading digits; the probe proved it.
- Lines that contain Latin LETTERS are split into script runs and laid out right-to-left by code:
  - Urdu runs are RLE-wrapped.
  - English runs use Inter at 0.78× size.
- Digits stay inside the Urdu run.
- Measure ordering objectively with `tadurdu/bidi-probe.cjs`, which reports ink segments: a short first token must be the RIGHTMOST segment. Reading the render by eye is unreliable.

**How to apply:**
- Any new RTL render script (Urdu or Arabic, any project) must use the same RLE wrap.
- REEFCO's Arabic posts (Dubai font, rf-series.ts / rf-distributor.ts / rf-mothersday.ts) were rendered before this fix. Re-check them for trailing colons, leading digits and Latin words such as product names.
- Width measuring is a full resvg layout per call, so cache it (`w1Memo`). Without the cache, wrap DP on mixed lines took more than 10 minutes.

Related: [[tadpharma-knowledge-base]], [[reefco-social-posts]], [[feedback-arabic-urdu-typing]].
