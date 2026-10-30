---
name: meezab-knowledge-base
description: "Meezab Z International KB (2026-09-29): 9 products, ALL approved, Urdu VERIFIED, 67 images on CDN, pack-wins-over-brochure rule; the KB memory doc is the live status; meezabz.com product pages are wrong"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-30T07:08:58.029Z
---

On 2026-09-29 Shoaib asked for "meezabz.com ka bhi same knowledge set … same rules same instruction" as Tad Pharma. See [[tadpharma-knowledge-base]].

**Live status:** read the Meezab KB `memory` doc (kb_get_doc) before doing anything. More than one session worked on Meezab the same day. The office-PC scratchpad script `meezab/approve.cjs` is OBSOLETE: running it with `--apply` would overwrite newer datasheet-template and system_rules content.

**State at the end of 2026-09-29:**
- All docs, the 28 presentation pages, the design lock and the 30-day calendar are APPROVED ("Approved hai sab").
- Urdu for all 9 products is VERIFIED. Page 3 is typed in Jameel Noori.
- CDN: 67 images are on jsDelivr, and the calendar attach links are widget-safe.

**Sources:**
- Brochures and packshots are in `Meezab Knowledge Base/` in the site repo. It's untracked and about 37 MB; never commit it.
- Raw phone photos of 8 real packs are stored as reference_image.
- Shoaib's rule: facts come from the PDF and the raw pack photos, and where the two differ, the pack wins. AI packshots are for display only.

**Social posts (2026-09-30):** the lock and the 30-day calendar were rebuilt on the locked-vs-free rule. See [[feedback-social-post-locked-vs-free]].
- Day 18 and Day 29 keep the E-Se 150 brochure pack photo with its blue background, framed on white.

**Key finding:** every meezabz.com product page contradicts its brochure, which is a farmer-safety risk. Correct the site later; its code isn't on the office PC.

**Uploading without R2 creds:**
1. Create an upload link with kb_create_upload_link.
2. POST to `/api/kb/upload`: presign, then PUT the file, then complete.
3. Change the asset kind with a service-role REST PATCH.

The script for this is scratchpad `meezab/upload.cjs`.

**2026-10-03 — same social pattern as Tad (same client, Ahmed Jahanzaib Shah)**

Shoaib: "Sab Approved hai meezab k liye bhi yehi pattern ker do q k inka client owner 1 hai".

- **Lock:**
  - logo A/B (top-left for info posts, top-centre for greetings and brand);
  - the stale dark-panel lines removed;
  - ALIGNMENT line, crop rule and the Arabic-on-Jummah exception added;
  - a `design_system` key added. Brand device = the logo's teal square tile, plus an optional white sweep echoing the script "M".
- **KB `design_system/main`:** the Meezab DESIGN.md (teal tokens, Inter only, Meezab Teal Strip).
- **occasion-copy:** 16 Jummah entries, APPROVED.
  - Same verified texts as Tad, offset 8 weeks so the two pages never post the same verse on the same Friday (9 Oct: Tad gets Muslim 854, Meezab gets Muslim 233).
  - World Animal Day, Egg Day and Food Day have Meezab-specific wording as drafts (approved:false), waiting for Shoaib.
- **dated-posts-plan:**
  - Tad's plan adapted: poultry only, so no World Milk Day; principals are REEFCO and Lexington.
  - Added an event: the 12th anniversary on 18 Oct 2026 (the website says founded 18 Oct 2014). Confirm with the client first.
- **Brand kit:** "Meezab Deep" (dark) marked as not for social posts.

**2026-10-06: Nano Banana Pro samples (Aminoreef Sol)**
- **Brand post** `meezab-social/product-posts/aminoreef-sol-brand.png`:
  - The wireframe concept from `tadstyle/mz-concept.cjs` (no placeholder labels, full KB text) goes in as image 1, with CONCEPT_NOTE env, plus the real pack.
  - The result is a real broiler-shed background with all text and the pack correct.
- **UGC** `aminoreef-sol-ugc-final.png`: a farm worker's hand holds the bottle in a shed, phone-photo look, with only the name and headline as white caption pills.
  - Finished in `tadstyle/ugc-compose.ts`, free: a full-bleed photo under the header (no stretched band), plus the label repair. The AI garbled the tiny "NOT FOR HUMAN…" print; the fix is median(29) to erase it, then overlay the real label text taken from the pack photo by an alpha threshold.
- Cost: 2 images, about PKR 75. The fixes were code only.
- Waiting for Shoaib to choose brand, UGC or both for the other 8 products.

**2026-10-06: Meezab product posts v2 via Gemini BATCH** (Shoaib approved the batch, 8 images, about PKR 150)
- Diureef Plus was made instantly (staged on the shed floor) and approved as the style test.
- `tadstyle/mz-batch-prep.cjs` builds the 8 concepts (wireframes, full KB text) and writes the staging and scene into overrides.json.
- `meezab-gen.ts` has DRYRUN=1 (saves `<id>-v2-prompt.txt`), `compose <raw> <suffix>` mode and CONCEPT_NOTE env.
- `tadstyle/mz-batch.mjs` submit/poll. It uploads refs through the Gemini Files API, creates batchGenerateContent (gemini-3-pro-image, 1:1 2K) and saves `<id>-v2-raw.png`. State is in `meezab-social/product-posts/batch-v2.json`.
- After it is done: compose each, check the packs against the originals, and put the finals in `final/`.
- **2026-10-06 result:** 8 of the 9 finals are in `meezab-social/product-posts/final/`. Shoaib wants finals always kept in their own `final/` folder, and to be given the path.
  - The batch was cancelled after 25 minutes still RUNNING; there was no charge. The posts were then made instantly instead. Two posts hit Gemini 503s; retries worked.
  - Nano Banana invented the packs for Recal Fos, Reeftox Sol and Reevit AD3EK, and garbled the tiny "Vet" text on Aminoreef.
  - Free fix: `tadstyle/mz-realpack.cjs` pastes the real pack photo over the AI pack. It supports a cut with y/y0/l/r/ell for the base, and a patch option (avoid: it leaves a lighter rectangle). Leftover base bits were filled with nearby desk colour.
  - Mentofresh missed its product name; it was added by code with Inter-600 plus stroke (Inter.ttf renders thin).
  - Reevit AD3EK was redone 2026-10-06 (option A, about PKR 37): a "NO riser/stand" prompt, then the real bottle pasted by code. **All 9 finals are DONE** in `final/`.

**Posting, 2026-10-21 (Tad + Meezab same-day push):**
- Six Meezab product/brand posts went out 2:15–9:45 PM, 90 minutes apart, with KB-approved captions. Meezab has no Google Business connected, so these went to FB + IG only.
- Jummah posts go only on their own Friday (Shoaib: "Jumy ki posts aj nai honi chaiye wo apny friday ko hi honi chaiye"). They are scheduled at 10 AM on the KB plan dates, not the designer's numbering, with no contact block:
  - Friday-2 on 23 Oct (62:9);
  - Friday-3 on 30 Oct (2:186);
  - Friday-4 on 6 Nov (20:114).
- **HELD: Friday-1** (Bukhari 2320, the tree/crop hadith). It has two wrong harakat: فَيَأْكُلُ written as فَيَأُكُلُ, and مُسْلِمٍ as مُسّلِمِ. Its date (16 Oct) has passed. It sits in the planner with no caption, so it won't post. Waiting for the designer's fix, then any later Friday. This is also logged in the client KB.
- **HELD: World Egg Day.** The design says "9 October", so it can't go out late.

**2026-10-30 (Shoaib):**
- **World Egg Day post** (WhatsApp Image 5 Oct) deleted from the Planner.
- **Friday-1** (Bukhari 2320) moved to Fri 13 Nov, 10 AM. It is still pending_caption, so it can't post.
- **Still needed:** the designer's fix for the 2 wrong harakat.
- **Clash to settle with Shoaib:** the KB occasion-copy for 13 Nov is the "wusaha" verse (2:286), not Bukhari 2320. Either use Friday-1 on 13 Nov and save its copy for that date, or move it to a later free Friday.
- **Update, same day:** Shoaib had **Friday-1 deleted** from the Planner ("del kero isko"). There is no Bukhari 2320 post any more. The 13 Nov Jummah follows the KB occasion-copy ("wusaha", 2:286), so there is no clash now.

**2026-10-30: Drive folder "APPROVED OF MEEZAB"** (drive.google.com/drive/folders/13he1-PlQ6E-ndjd6iPsu_QyGPnganiIg). Shoaib: use only this folder, not the older 1C12I3…. It holds 10 images from 25 Oct; none had been posted.
- **The 9 launch-calendar posts** (Day 4, 5, 6, 7, 9, 11, 15, 16, 17) were uploaded to the Planner. Each got its KB `content-calendar-launch-30` caption_for_posting verbatim.
- **Schedule:** 5:00 PM PKT, one a day, 31 Oct – 9 Nov. Friday 6 Nov is skipped (Jummah, Friday-4).
- **Held:** the "Happy Birthday — Dildar Hussain Shah (Director Marketing)" post, joint with Tad Pharma. It is date-bound, so ask Shoaib for the date and caption.
