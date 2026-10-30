---
name: occasion-posts
description: "Jummah/event greeting posts for every KB project — global rule `occasion-posts` + MCP tool kb_get_occasion_post (ba1fc93, 2026-10-02); Arabic/Urdu only from verified texts; weekly style rotation"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-02T10:18:41.954Z
---

On 2026-10-02 Shoaib shared Meezab's ChatGPT-made Jummah and event posts. The first was plain. He wanted background and wording instructions so the post fits the occasion while the fixed items stay locked. Mid-turn he added: "ye Sab project k liye rule rakho", and Urdu and Arabic must be possible too.

**Global rule `occasion-posts`** (kb_global_docs; source scratchpad `occasion-posts.md`)
- Prose sections:
  - fixed brand items (logo, brand palette, typography, the project's footer strip);
  - what is not allowed: no product, claim, CTA or vet line; no faces or Prophets; no "Mubarak" for Ashura;
  - the creative parts;
  - Arabic/Urdu letter-perfect rules;
  - languages: english, urdu, arabic, mix;
  - caption rules: greeting, a dua, sign-off and 3–5 hashtags; no contact block, no CTA.
- JSON data:
  - 6 `style_families`;
  - 17 `occasions`, each with names, motifs, palette mood and texts;
  - 8 `verified_texts`: 2:152, 94:6, 76:30, 13:28, 97:3, durood, afw, taqabbal.
- Arabic and Urdu lines come ONLY from `verified_texts`. Never let the AI compose Arabic.

**Tool `kb_get_occasion_post`** (lib/mcp-knowledge-tools.ts)
- Args: projectName, occasion (free text, aliases such as "bakra eid" or "jumma mubarak post"; the longest alias wins), date, language.
- Style = `style_families[(ISO week + (non-jummah ? 2 : 0)) % 6]`, so each Friday looks different.
- Pulls the project's design lock items, brand_kit, graphic_rules and nap docs, the logo, and any "Occasion reference — …" assets. Meezab has 6 such assets.
- An unknown occasion falls back to the general rule and asks the user to confirm motifs.
- ChatGPT needs a new chat or a reconnect to see the tool.

Local test: scratchpad `occasiontest.ts` (tsx --conditions=react-server, dummy MCP_OAUTH_SIGNING_KEY).

Related: [[feedback-social-post-locked-vs-free]], [[meezab-knowledge-base]], [[tadpharma-knowledge-base]].

**International days and events (2026-10-02, commit `6abed17`)**

Shoaib: "Events aur International Days bhi plan mein add kero".

- **Kind per occasion:** every occasion now has a `kind` — islamic (9), national (6) or international (8).
- **Styles by kind:**
  - Non-Islamic days pick from the new `day_style_families`: Photo hero, Flat illustration, Bold typography, Paper-cut layers, Editorial split, 3D icon badge.
  - They never get masjid motifs or Arabic. Before this, World Egg Day could have drawn the "Line-art masjid" style.
- **New international days**, each with `fits` and message ideas, plus aliases in the tool:
  - World Animal Day — 4 Oct;
  - World Food Day — 16 Oct;
  - AMR Awareness Week — 18–24 Nov;
  - World Zoonoses Day — 6 Jul;
  - World Milk Day — 1 Jun;
  - World Food Safety Day — 7 Jun.
- **New global rule `event-posts`:**
  - real facts and photos only;
  - up to 4 posts per event: announcement 7–10 days before, reminder, live, recap;
  - no caution line unless the post is product-related.
- **Tad Pharma:**
  - New marketing doc `dated-posts-plan` covers Oct 2026 – Sep 2027: Jummah every Friday by 2 pm, plus 8 Islamic, 6 national and 8 international days with dates.
  - Islamic dates are astronomical, with Pakistan often +1 day — confirm after Ruet-e-Hilal.
  - The events table is empty until Tad Pharma shares its events.
  - The launch calendar now points to the plan.
- Scratchpad sources: `occ-update.cjs`, `event-posts.md`, `tad-dated-posts-plan.md`, `kb-upload-plan.cjs`.

**Jummah: new every week (Shoaib, 2026-10-03: "Har nai post mai new Islamic Layouts and Content… Qurani Ayat Ahadees aur Duyn")**
- `verified_texts` now holds 25 texts: 22 for Jummah, plus 17 new ones — 9 ayat, 3 Quranic duas, and 5 hadith (Muslim 854 and 233, Abu Dawud 1047, Bukhari 2363 and 2320).
- **How the Arabic was taken:** from the source text itself — Quran from Tanzil quran-simple via api.alquran.cloud, hadith from the fawazahmed0 hadith-api. Diacritics are stripped only to match words; the source's original characters are stored.
  - My typed Arabic differed in 16 of 17 cases (the order of combining marks), so NEVER type Arabic by hand.
  - References were confirmed on sunnah.com through the built-in browser; curl and WebFetch get a 403 there.
- **Gotchas:**
  - The hadith-api numbers Muslim differently (its #1977 = Muslim 854b).
  - Ibn Majah 925 was skipped because Zubair Ali Zai grades it Daif. Rule: skip any hadith whose grading is disputed.
- **English meanings** are our own plain English, checked against translations. Translations are copyrighted and were not copied.
- **Urdu** for the new texts is `null` until it is verified, so they are for English posts only.
- **Style families:** 6 → 16, with 10 new light Islamic layouts. No real named mosque, and no readable Quran pages.
- **Tad occasion-copy:** 16 dated Jummah drafts, from 2026-10-09 to 2027-01-22. A variant whose `date` matches wins (157a972). All are `approved:false`, pending Shoaib.
