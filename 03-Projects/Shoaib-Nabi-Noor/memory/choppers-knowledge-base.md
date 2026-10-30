---
name: choppers-knowledge-base
description: "Choppers Salon/Studio KB build started 2026-10-29 — local folder E:\\...\\Choppers Salon, built ONLY from material Shoaib gives (planner data excluded on his instruction)"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-29T08:24:31.487Z
---

Shoaib started the Choppers KB on 2026-10-29.
- **Local folder:** `E:\Rana Shoaib\My Projects Website\Choppers Salon`. It holds 2 logo PNGs (glossy white on black; white with shadow on white). Working notes are in `Choppers Knowledge Base/audit-2026-10-29.md`.
- **Dashboard projects:** Choppers Salon `231e2107`, Choppers Studio `375f1fc4`. Both are among the [[aijaz-danish-brands]].

**Rule (2026-10-29):** "Planner wala data abhi hissa nai banana… jo mai dy raha wo filhal dekho". Build the Choppers KB only from what he provides (folder files, his messages) plus connection/API facts. Don't mine past planner posts or captions for it unless he asks.
**Why:** he wants the KB to come from source material, not from content that was already posted.
**How to apply:** for each new file in the folder, read it and add the facts to the KB draft. The API audit (followers, reach, GBP rating) needs the ABS connector.

Facts found so far:
- choppers.com.pk is an empty WordPress site ("Hello world!").
- Google listings: Gulgasht (77-A Gulgasht Colony), linked to the Salon project; Model Town (7-B Main Northern Bypass), linked to the Studio project.
- Names differ across platforms; official naming not settled yet.

**Shoaib's material, 2026-10-29:** `Logo/` and `Final Edited Posts/` (8 published designs = the design reference). Full write-up: `Choppers Knowledge Base/brand-and-design.md`.
- **Brand look:** black/charcoal + golden-yellow gradient accent + white, with the white logo on a dark area. Footer bar: yellow gradient, Gulgasht (Main) 0300-7715325 · Rumanza by PC 0300-7715309 · Model Town 0300-8719614. Mid-2025 posts also show Cantt 0316-7715325.
- **Women's services exist.**
- A mid-2025 deals price list (7 deals, PKR 2,800–12,000) is in that file.
- **Settled 2026-10-29:** Choppers KEEPS its own black + gold look; an established brand look overrides the general light rule ([[feedback-design-glass-light]]).
- **Open:** is Cantt still open; are the deal prices current.

**Update, 2026-10-29 (later):** Shoaib said "kuch final nai hai pehly sirf data dekho" and "kb mai save kerty jao". Analyse data only, make NO guidelines yet (he will make the colour/brand guideline later), and save each finding to the dashboard KB marked draft/data.
- Old posts are a colour/style reference only; their deals/prices are stale.
- The rate list PDF is official and current: 14 pages, men's services, groom packages 14.5k–38.5k, 3 branches (Gulgasht, Model Town C-7, DHA Rumanza by Pearl Continental). No Cantt.
- KB docs created (project "Choppers Salon"): `nap/main`, `marketing_doc/price-list`, `marketing_doc/social-media-audit-2026-10`, `marketing_doc/design-reference-analysis`, `memory/main`.
- Raw Photos: the interior is cream + sage-olive green + oak + black, matching the rate list's olive + gold. Kids' toy-car chairs.
- Tooling: PDF pages were rendered with mupdf (npm, scratchpad), and HEIC was converted with heic-convert + the project's sharp. Neither poppler nor pypdf is installed.
- Raw Photos = the Gulgasht (Main) branch (Shoaib confirmed 2026-10-29).
- **Choppers Salon = MEN's salon; Choppers Studio = the separate WOMEN's salon** (Shoaib, 2026-10-29; Studio to be discussed later). Women's services in old reviews/posts predate the split. The dashboard links the Model Town GBP to the Studio project; may need changing later (nothing changed).
- **FINAL 2026-10-29 (KB `graphic_rules/main`):**
  - Colours 60-30-10: black #121212 / white #FFFFFF / gold #F9C224 (charcoal #1F1F1F for panels). Never white text on gold. The rate list's olive is excluded.
  - Fonts (option A): Montserrat ExtraBold caps headlines + Montserrat body + Great Vibes script for 1–2 words. Size floors 64/28/72 px at 1080.
  - Also in the KB: GBP Gulgasht audit and FB+IG audit (built with the new `social_get_insights` tool). HypeAuditor says IG followers are authentic, but 69% are non-local. The IG bio still reads as a women's salon (open).

**Decision, 2026-10-29 (client allowed new pages):**
- **NEW Facebook page:** the old FB is about 91% foreign or bought.
- **KEEP Instagram @choppersalon:** its followers are authentic; fix the bio and add Reels.
- Old FB page: don't merge (the fake followers would come across). Post "moved", then unpublish after 2–3 weeks; don't delete.
- Ads for followers: Multan, "people living in", men 18–45. NEVER worldwide or cheap-country likes.
- Setup draft (name, bio options, services, cover, pinned and "moved" copy, ads, 7 open questions) is in KB `marketing_doc/new-facebook-page-draft`. It is NOT approved yet.
- Creating the page is Shoaib's or the client's job (in the client's Business Portfolio). After that, move the IG link to the new page and run "Import Facebook Pages" to connect it to the Planner.

**Client deck, 2026-10-29:** "Audit aur Proposal" (Roman Urdu, 22 slides). It follows the TONI&GUY report format, adapted to Choppers branding (dark #121212, gold #F9C224, Montserrat + Great Vibes).
- **What's in it:** findings for GBP, FB and IG, plus 5 solution steps: new FB page, IG cleanup, GBP fixes, Multan-only ads, Reels. Also the client decisions needed and the 90-day targets.
- **File:** `Choppers Salon/Reports/Choppers-Salon-Audit-aur-Proposal-Oct-2026.pdf`.
- **Source:** `Reports/_source-oct-2026` (`data.mjs`, `build.mjs`, `style.css`, assets).
- **Rebuild:**
  1. Run `node build.mjs`.
  2. Print with Edge: `--headless=new --print-to-pdf --virtual-time-budget=20000`. Fonts load from Google Fonts at print time, so the PC needs to be online.
- **Approved by Shoaib and uploaded 2026-10-29 as KB asset 73dc8ca1** (kind document, "Client deck: Audit aur Proposal (Oct 2026)").

**KB completed for standalone Choppers chats, 2026-10-29.** Shoaib: "jb isky apny chat session mai start keron mcp se already sb mojoud ho".
- **KB docs:**
  - `memory/main` rewritten as the full current state.
  - New `system_rules/main`.
  - `nap`: social links + the unknown WhatsApp number.
  - Superseded notes on `design-reference-analysis` and `social-media-audit-2026-10`.
  - `client-report-log` 29 Oct entries.
- **KB assets:** 2 logos, the rate list PDF (literature_pdf), 8 old posts (reference_image, stale prices), 9 full-size Gulgasht photos, and the deck. The rate-list page images were skipped; the price-list doc has every price.
- **Choppers folder setup:**
  - `Choppers Salon/CLAUDE.md` created.
  - Its own auto-memory, `~/.claude/projects/E--Rana-Shoaib-My-Projects-Website-Choppers-Salon/memory/`, seeded with: project state, working rules, Shoaib's preferences, deck build.
