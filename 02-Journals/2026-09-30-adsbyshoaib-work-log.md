---
type: journal
date: 2026-09-30
topic: adsbyshoaib.com, Tad Pharma, Meezab Z, Google Business Profile
source: Claude Code session (home PC)
---

# Work log — 2026-09-28 → 2026-09-30 (home PC)

Full details live in [[03-Projects/Shoaib-Nabi-Noor/memory/MEMORY]]. This is the short version, so work can continue on any machine.

## Tad Pharma (client: Ahmed Jahanzaib Shah)
- Knowledge base final. Facts come only from the PDF brochure and real pack photos; AI images are for presentation only. English is error-free, Urdu is copied exactly as verified, and FCR stays "increases" as printed.
- All 17 Urdu pages verified, with typed Jameel Noori page 3. Pack-label facts added. Reevit E-Se 150 moved to Meezab.
- Social footer strip and 30-day content calendar (English, locked JSON prompts).
- MCP tool `kb_get_social_post`: say "Day N ki post design karo" and the AI gets the prompt plus the original images.

## Meezab Z. International (same client)
- Knowledge base finished the same way: CDN, Urdu verified, typed page 3, 28 pages on a Design canvas.
- Rule: info comes from the PDF and the raw pack photos, and **the pack wins on a conflict**. Applied to Recal Fos composition, Fegatosim MgSO4 40,000, Laxi-Zyme 10⁹ counts, and AD₃EK 1 L + 5 L. 8 pages were rebuilt.
- Only products that have a PDF count, so Viro-Rid was dropped.
- Logo: clean mark plus horizontal (light/dark) and stacked lockups.

## adsbyshoaib.com dashboard
- **Meta Ads page** (/dashboard/ads) for the Meta App Review demo.
- **Client portal** now uses the dashboard's glass shell. Its Planner is the same calendar in client mode, for Owners and team members.
- **Sidebar** grouped into sections (Sales / Clients & billing / Marketing / Admin) and scrollable.
- **Google Business Profile**: API approved 2026-09-29 on the GCP project "Socially Snap" (875327223530, 300 QPM). Built:
  - Connect through the Socially Snap OAuth client (API Vault service `gmb`). One Gmail grant is reused across projects, and the location is auto-matched by name.
  - Reviews with replies, the Planner posting to GBP, and a Connections tile.
  - `gbp_*` MCP tools.
  - Live on Hotel Elegant: 20 locations, 624 reviews, 4.6 average.
- **Google-friendly pace rule:** never bulk. 5 minutes between Google writes and at most 20 per location per day, enforced in code (`paceGbpWrite`) and in the KB global rule `google-friendly-pace`.

## Pending / next
- GBP branding verification: after the 24 h wait, go to Branding → Verify branding → "I have fixed the issues". adsbyshoaib.com is now a Domain property in Search Console.
- Watch the first real Planner → GBP post.
- Next GBP phases: performance metrics, profile info editing.
- Meta App Review: record the demo and submit once Access verification is Verified. LinkedIn CM API approval is still pending.
- Meezab: meezabz.com product pages are wrong, and the site code isn't on the home PC.
