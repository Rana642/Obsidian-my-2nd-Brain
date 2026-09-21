---
name: project-ci-deploy-broken
description: "GitHub Actions \"Deploy to Hostinger\" workflow fails on every run (npm ci step), but Hostinger has its own independent working auto-deploy — verify via the live site, not Actions status"
metadata: 
  node_type: memory
  type: project
  originSessionId: fb776024-2ed0-44c1-a91d-40c7ca3ba0dd
  modified: 2026-09-11T11:10:38.621Z
---

The `.github/workflows/deploy.yml` "Deploy to Hostinger" workflow has failed on **every run since at least 2026-09-10 08:53 UTC** (commit `b365ff8` onward, 8+ consecutive failures through `e3790ca` on 2026-09-11) — always at the "Install dependencies" (`npm ci`) step, on the GitHub-hosted runner itself, before the SSH-deploy-to-Hostinger step ever executes. The only annotation available (public repo, no `gh` CLI/log auth in this environment) is the generic "Process completed with exit code 1" — the real npm error text was not retrievable via the GitHub REST API without an auth token. Reproducing `npm ci` locally (Windows, Node 24) succeeds cleanly in ~3 min, and `package-lock.json` does contain the Linux-platform optional deps for `sharp` (`@img/sharp-linux-x64` etc.), so it isn't an obvious lockfile/platform mismatch — root cause still unconfirmed.

**Despite this, the live site (https://elegant-suite.com) DOES receive every commit** — verified directly by browser-checking production after several pushes (new room prices, new tax-inclusive wording, direct gtag/fbq scripts from an earlier session's tracking migration) and finding it fully up to date within minutes of pushing to `main`. This means Hostinger almost certainly has its **own independent Git-based auto-deploy** (hPanel's native "Auto Deploy" feature, or similar) running in parallel with — and succeeding where — the GitHub Actions SSH workflow fails. The Actions workflow appears to be redundant/vestigial at this point.

**Why:** Confirmed by cross-referencing the GitHub Actions API (`/actions/runs`, `/check-runs/.../annotations`) showing consistent failures, against live production checks via the Browser pane (`window.fbq`, `window.gtag`, room page text) showing the same commits' changes already deployed and correct.

**How to apply:**
- **Never trust the GitHub Actions run status alone as a deploy-success signal for this repo** — it will show red even when the deploy actually succeeded. A past session mistakenly treated an Actions "success" read (via WebFetch, which has its own 15-min cache) at face value; the run was actually a failure per the API, yet the site was fine.
- After pushing to `main`, verify by **loading the actual production page in the Browser pane** (not WebFetch — it caches for 15 min and can serve stale content within that window) and checking the specific thing that changed.
- The broken Actions workflow is worth fixing eventually (get real error logs via `gh run view --log-failed` or the hPanel/GitHub UI with the user's own login) so there's a working fallback deploy path and accurate CI signal — but it is not currently blocking anything, so don't treat a failed Actions run as a reason to halt or panic.
- If asked to investigate this further, the user needs to supply either `gh` CLI auth in this environment or paste the actual failed-step log text from the GitHub UI, since the public REST API's log/log-archive endpoints require auth even for public repos.
