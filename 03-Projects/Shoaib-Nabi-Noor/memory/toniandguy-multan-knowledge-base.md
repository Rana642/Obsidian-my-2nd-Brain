---
name: toniandguy-multan-knowledge-base
description: "2026-10-14 TONI&GUY Multan KB (Avenza client): all files uploaded; Multan rate lists = official prices; NAP from salon materials + GBP; IG insights reconnected — 33% Pakistan / 19% Multan followers, bridal deals carousel = best post; open: city on logo, dark exception, WhatsApp, Futura, GBP fixes"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-14T07:22:19.011Z
---

**What this is.** On 2026-10-14 Shoaib asked to start the KB for **TONI&GUY Multan**. It is the Multan branch of TONI&GUY South Pakistan, the only branch outside Karachi, opened 29 Jan 2022. In our client list it sits under Aijaz Danish / Avenza Group. KB project id 73a06df2.

**Client files** live in `E:\Rana Shoaib\My Projects Website\Toni and Guy\`:
- the `Toni And Guy Knowledge Base` folder: brand PDFs, two logo sheets, in-salon notice signs, and Futura.ai;
- two Multan rate lists: For Her (2 Jan 2025) and For Him (13 Jul 2025 update).

**Built in the KB (plain English):**
- `nap`: website first, plus the GBP.
- `brand_position`.
- `icp` and `pain_points`: both marked DRAFT.
- `graphic_rules`, `system_rules`.
- marketing docs: `price-list-multan` (full transcription), `social-media-playbook`, `nap-consistency-audit`.
- `memory`.
- 2 logo assets.

**Key brand rules (TONI&GUY London, strict):**
- Colours: black and white, plus a premium gold.
- Font: Futura PT. Headlines in UPPERCASE with wide letter spacing.
- Logo rules:
  - only white on imagery and black on light backgrounds;
  - never on a model's hair or face;
  - no added text, never a watermark.
- The EDITION ONE lock-up only goes on official campaign images, bottom-right. Model rights expire Jan 2027.
- Grammar: write "TONI&GUY" (never "Toni & Guy", "T&G" or an apostrophe).
- Social: at most 5 hashtags including #TONIGUY, at most 3 emojis, Instagram first.

**Shoaib's decisions, 2026-10-14 (these override my first draft):**
- "ye mera personal kb hai har cheez charha do". All 9 documents went into the KB: the 5 guideline PDFs (TONI&GUY marks them confidential, so never share their links outside the KB), the in-salon notices, both rate lists, and Futura.ai. The 2 logo sheets were uploaded too.
- "Price list karachi wali nai, hamari hogi". The Multan For Her and For Him lists are the **official** prices. The Karachi toniandguy.pk list must never be used, and it is not in the KB.
- "Website wali purani hain ignore kero". toniandguy.pk is outdated for Multan. The **NAP master is now the salon's own materials plus its GBP**:
  - address: C-5 A-Block, Model Town;
  - UAN 03-111-444-730;
  - phones 0315 7001633 / 7001644;
  - toniandguymultan@gmail.com;
  - handle toniandguymultan.

  This overrides the global nap-source rule for this project only.

**Mistake to avoid:** a `kb_upsert_doc` call with `mode: replace` and placeholder content wipes the doc. I did this once to price-list-multan and restored it straight away. Always send the full content.

**PDF tooling (no poppler on this PC).** The scratchpad `pdftools` folder has:
- pdf-lib to split files;
- pdfjs-dist + @napi-rs/canvas to render pages to PNG;
- `rows2.mjs` to pair price-list names with prices by line height.

Image-only pages have to be rendered and viewed. OCR wasn't needed in the end.

**Open questions, asked 2026-10-14:**
1. The Multan logos add "MULTAN", but the global logo rules forbid adding a city.
2. Should TONI&GUY get a dark-design exception?
3. Which number is on WhatsApp? (Email and address are settled by the decisions above.)
4. Prices: settled. The Multan lists are official.
5. Futura PT or Jost?
6. GBP fixes: add the website, update the "45 years" description, remove fake services (BOTOX, laser, dreadlocks…).

Related: [[aijaz-danish-brands]], [[feedback-design-glass-light]], [[reefco-social-posts]].

**Social media audit, 2026-10-14.** Full write-up in KB `marketing_doc/social-media-audit-2026-10`.
- **GBP:** 4.7★ from 1,636 reviews, but almost no replies (10 of 11 one-star reviews since April are unanswered).
  - Calls fell from 453 (May) to 241 (Sep) while views grew.
  - No website on the profile, chat not used.
  - 🔴 The 5 Aug "Chemical Peel" post is a pasted ChatGPT reply and is still live.
  - Old timing posts (10–10, 10–9) contradict the profile hours (10–8).
  - Two separate reviews say a staff member asks clients for loans and tips.
- **Facebook:** 26.3K followers, page id 108079275031734, about 1 post a week, low reach.
- **Instagram:** 33.7K followers, id 17841450799991832. The bio has male and female floor numbers but no link.
- **Meta Ads:** one click-to-WhatsApp highlights video has run since 30 Sep. We have no access to the ad account, and the vault token can't read the page or Instagram.
- **Ad Library** (public, through the browser pane) works for seeing a page's ads, follower counts and page history.

**Instagram audit, 2026-10-14.** Full write-up in KB `marketing_doc/instagram-audit-2026-10`.
- 33.7K followers. 13 posts in 90 days, 6–84 likes each, about 24 interactions per post (roughly 0.07% engagement).
- The same posts reach far more people on Facebook.
- **Why Instagram insights were blocked:** the FB Page connect scope lacked `instagram_manage_insights` (Meta #10).
- **Fix shipped in e3a9990:**
  - the scope was added;
  - when Meta refuses account insights, the Insights page still shows followers and each post's likes and comments, with a reconnect note.
- **Done 2026-10-14:** Shoaib reconnected (Workspace logins → Facebook Business login → Reconnect). Insights now load. Commit 0682062 makes the newest login's Page tokens win in `refreshFacebookPageTokens`.

**Instagram formats from the API (commit e92cb82, 2026-10-14).**
- New in the Insights page: format tags on top posts and a "By format" table.
- Last 90 days: 6 Reels (avg 31 likes), 2 carousels (33), 5 images (9).
- My first audit wrongly said "few Reels". It has been corrected in the KB.
- Lesson: check the media types through the API before making claims about format.
- Facebook video views: 29.6K in 90 days (+114.6%).
- The client answer to send is the **final Roman Urdu version** in `why-low-instagram-engagement`. The client is Pakistani, so write client answers in Roman Urdu.

**Audience and Reels data after the reconnect, 2026-10-14.** Full write-up in KB `marketing_doc/instagram-audience-2026-10`, which also holds the Roman Urdu client report.
- **Followers by country:** Pakistan 33%, India 20%, US 19%, Brazil 7%, Türkiye 5%. Multan has only 19% (6,394).
- **Gender unknown for 70%.** This is an internal red flag that many followers are inactive or low quality. Shoaib decides whether to tell the client.
- **Formats:** Reels average 2,092 views with 1–5 shares each, less than single images (2,398).
- **Best post:** the 2 Oct bridal deals carousel, with 20.4K views and 120 shares. This corrects my earlier "stop price posters" advice: bridal packages with prices get sent.
- **Best time:** 4–8 pm PKT.
- **Profile:** 0 link taps, because there is no link in the bio.
- **Open:** was the 2 Oct carousel boosted?

**Organic vs ads, 2026-10-14.** Full write-up in KB `instagram-audience-2026-10` → "ORGANIC vs ADS split". Background: [[meta-insights-organic-vs-ads]].
- **Instagram:** every per-post figure I sent is organic. The 2 Oct bridal carousel was **not** boosted (20,431 organic views). Ads added 14K views in 90 days (6%).
- **Facebook:** 81.5K of the views (77%) and 26.4K of the video views (88%) came from the click-to-WhatsApp ad (since about 30 Sep).
  - Organic video views were only 3,507 (−5%).
  - My earlier "+114% FB video views / video works on FB" line in the client message was **wrong**. A Roman Urdu correction line is in KB `why-low-instagram-engagement`.

**Client PDF report, 2026-10-14.** 22 slides, saved at `Toni and Guy/Reports/TONI-GUY-Multan-Social-Media-Report-Oct-2026.pdf` and as KB asset 4f7c0846.

**Shoaib's rules for this report:**
- Roman Urdu.
- No profile-optimisation advice and no ads advice; both are our job.
- Suggestions are about videos only, because the client shoots them.
- TONI&GUY look (white, black, gold, Jost); NOT Ads by Shoaib branding.
- Include the 70% unknown-gender note.
- Include post thumbnails.

**How it's built:** HTML slides, printed with Edge using `--headless=new --print-to-pdf`. Playwright's old headless mode fails with this Edge, so don't use it. The source (`data.mjs`, `build.mjs`, `style.css`, assets) is in `Reports/_source-oct-2026`. For next month's report, edit `data.mjs` and rebuild.

**Dashboard change:** the Insights posts table now lists every post ("Show N more", commit c6168e3), so per-post data and thumbnails can be read from the page.
