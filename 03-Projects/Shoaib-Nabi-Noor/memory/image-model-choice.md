---
name: image-model-choice
description: "Which image model to use for post work — Nano Banana Pro until its Gemini credits run out, then GPT Image 2.5 Sunburst (Graphic Studio already switched, 6fffc65)"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-07T13:03:26.964Z
---

**Shoaib's decision on 2026-10-07:** keep using Nano Banana Pro (gemini-3-pro-image) for post images. When the Gemini credits run out, move to GPT Image 2.5 next. Do not run a side-by-side GPT test now.

**Why:**
- On the 2026-10-07 Recal Fos hero test, Nano Banana gave the best product integration.
- GPT Image 2.5 (released 2026-09-08) leads the public leaderboards and is live on his OpenAI key, but the test was skipped to save money.

**How to apply:**
- When Gemini returns a credits-depleted error (402), tell Shoaib and suggest GPT Image 2.5 Sunburst. Still ask before spending, with the PKR cost.
- **Graphic Studio** is already on 2.5 (commit 6fffc65, pushed):
  - premium = gpt-image-2.5-sunburst `max` (~$0.21 at 1024²);
  - standard = sunburst `high` (~$0.053);
  - draft = gpt-image-2.5-flare `low` (~$0.006).
  - The 2.5 quality ladder shifted down one rung: 2.5 high costs what gpt-image-2 medium did.
- **Scratch scripts:**
  - `tadstyle/openai-gen.mjs` (out, size, quality, model, prompt, refs) mirrors `gemini-gen.mjs` and prints token usage and cost.
  - A 1536² high Recal Fos hero test was estimated at ≈ PKR 39.
- **FLUX 3 Image** (Black Forest Labs, 2026-10-01) was added to Graphic Studio as a third provider "flux" on 2026-10-07 (commit 502cc36):
  - `lib/image-providers/flux.ts`, up to 10 base64 refs, tiers 1k/1.5k/2k (≈ $0.048/0.07/0.10), records the credits BFL actually charges;
  - no Batch API, so batch is disabled for it; its copy goes through Gemini; MCP tools accept "flux".
  - Needs BFL_API_KEY in graphics-studio .env.local. Shoaib makes the account himself at dashboard.bfl.ai. Not tested yet (no key).
  - Bounding-box placement is a JSON layout embedded in the prompt (0–1000 coords, [y0,x0,y1,x1]); not wired yet.
- Label text is garbled on every model, so the label swap is always needed: [[hero-pack-label-swap]].

Related: [[feedback-ask-before-spend]], [[graphics-studio-project]].

**2026-10-30: Gemini prepaid credits are DEPLETED** (402 RESOURCE_EXHAUSTED on gemini-3-pro-image). Nothing was charged.
- Shoaib approved GPT Image 2.5 Sunburst "high" for Tad's Jummah background (1024×1536, ~$0.053–0.08).
- **Free compose recipe that worked:**
  - Write HTML with Google Fonts (Amiri for the Arabic, Archivo, Inter) and render it with Edge `--headless=new --screenshot --window-size=1080,1350`.
  - Edge shapes Arabic correctly, which avoids the resvg RTL bug.
  - Files are in the scratchpad `tad-jummah/`.
- **Logo lesson (Shoaib circled it):**
  - A flood fill that removed the logo's white background also ate the white "P" of PHARMA.
  - Clear near-white only OUTSIDE the PHARMA pill box, then zoom in to check the logo before showing it.
