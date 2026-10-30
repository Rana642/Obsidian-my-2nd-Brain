---
name: aijaz-danish-brands
description: "Aijaz Danish (Avenza Group) projects — contact details read off their own creatives (2026-10-02), caption style used, GBP location mapping, how a day's planner posts were spread"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-14T07:14:40.210Z
---

Client: "Aijaz Danish - Avenza Group of Companies".
- Projects: Avenza Avenue, Avenza Land, Choppers Salon, Choppers Studio, Danish Constructions, Eventia 360 Planner, Hotel Avalon Suites, Toni and Guy Multan.
- None of them has KB brand docs. The details below come from the brands' own post designs on 2026-10-02, not from their websites.

**Contact details**
- **Avenza Land** (marketing & sales agency, DHA Multan)
  - Phones: 0300-871-0204, 0300-8634-023, 0310-4000-689
  - Web: www.avenzaland.com
  - Head office: Avenza Avenue, adjacent to Beacon House, Main Bosan Road, Multan
  - Offer: 1 & 2 Kanal verified plots, sectors A/N/M/H/R/Q/U, "starting from PKR 60 Lac"
  - Instagram @avenzaland connects via Instagram Login.
- **Danish Construction** (Real Estate & Developers)
  - Phone: 0300 8634023
  - Address: 72-B Model Town, Multan
  - Line used on posts: "quality craftsmanship, smart design and reliable project management from concept to completion"
  - Instagram @danishconstructionco connects via Instagram Login. No GBP.
- **Eventia 360** (Events | Photography | Decor; weddings, birthdays, corporate)
  - Phone: 0300 8710307
  - Address: 72-B, First Floor, Apt 1, Commercial Area, Multan
  - Email: info.eventiaplanner@gmail.com
- **Hotel Avalon** (Hotel & Restaurant)
  - Phones: 0300-871-9858, 0300-871-9856
  - Address: 46-A Gulgasht Colony, Multan
  - Email: hotelavalonsuites@gmail.com (hotelavalon10@gmail.com also appears on posts)
  - Web: hotelavalon.com.pk (seen on a post, 2026-10-05)
  - Amenities: free Wi-Fi, 24/7 security, housekeeping, luggage storage
- **Choppers Salon**
  - Phones: Gulgasht (Main Branch) 0300-7715325; Rumanza by PC 0300-7715309; Model Town 0300-8719614
  - Web: www.choppers.com.pk
- **Avenza Avenue**
  - Logo line: "Elevating standards, enriching lives"
  - No contact details on its post.

**GBP mapping fix (2026-10-02).** Avenza Avenue's `gbp_connections.selected_location` pointed at the "Choppers Salon & Spa Multan" location (13757437040459439106), which is Choppers Salon's. It was changed to Avenza Avenue's own location, `locations/1215093275853877147`. When linking a Google grant, check that each project picks its own location.

**Caption style used (2026-10-02, 23 posts)**
- Structure, in English: hook line, body, CTA line on its own, contact block (📞 📍 🌐 📧 lines), 4–5 hashtags.
- `gbpCaption` drops the CTA line and the contact block for Google (rule 26de402: "CTA nai jana chaiye").
- Jummah posts: short dua, brand sign-off, no contact block.

**Posting times — CURRENT RULE (Shoaib, 2026-10-09), replaces the Friday 2 PM rule below:**
- No more "Jummah by 2 PM / spread before 2 PM". Friday posts go at **10:00 AM PKT** (Jummah included).
- Every other day: regular posting, each brand at **its own best time** (when its followers are most active / its posts reach most).
- Finding the times: local `.env.local` lacks `SOCIAL_TOKENS_ENCRYPTION_KEY`, so page tokens can't be decrypted on this PC. The meta_ads_get token can't read Page posts either (#210 page token required).
  - Ways in: Shoaib signs in to the dashboard in the browser pane → read /dashboard/social/insights?project=<id>&range=90 (PostingTimesPanel); or he adds the key to `.env.local` himself → run scratchpad `besttime.ts` (90-day reach by 2-h slot + IG online_followers probe).
- Once found, write each brand's times into `client_projects.posting_instructions` so social_list_pending_posts shows them every time.
- Project ids: Avenza Land 782c7070, Toni&Guy 73a06df2, Eventia f122c441, Avalon 2aa09e62, Choppers Salon 231e2107, Avenza Avenue b8cdd087, Danish 114c3fbb, Choppers Studio 375f1fc4.

**OLD Friday rule (Shoaib, 2026-10-02) — retired 2026-10-09.** On a Friday, each brand's Jummah post goes after all its other posts, and Jummah must be live by 14:00 PKT. So that day's posts all run before 2 PM, 19–23 minutes apart per brand, with Jummah last.
- Jummah posts were placed on separate cron ticks (12:44, 12:59, 13:14, 13:29, 13:44) so each gets the one Google slot per tick.
- Regular posts were limited to FB+IG with `target_platforms`, then queued into `gbp_post_backfill` after 2 PM so Google gets them later at backfill pace.
- Re-scheduling an already captioned post with `social_set_caption_and_schedule` used to leave the old FB native draft live (double post). Fixed in 29ca303: it now cancels and resubmits.

**Spreading a day's posts.** (These were the first plan, before the Friday rule.) When a day's posts all sit at 10:00:
- Captions were set after 10:00, so they would otherwise fire together.
- Jummah posts went first (~12:20–13:35).
- After that, each project got about 2h15 between posts, and projects were offset 12–15 minutes from each other, finishing by ~21:30 PKT.

**Week of 5–10 Oct 2026 (28 posts, 4 brands)**
- Uploader: fatimaejaz942@gmail.com, a portal member with planner and uploads access.
- Times on normal days (PKT): Eventia 10:00, Avalon 10:13, Danish 10:27, Avenza Land 10:41.
- The Sat 3 Oct posts were overdue, so they went out on 5 Oct around 14:00. That day's own posts went at around 18:00.
- No Jummah posts were uploaded for 9 Oct. Shoaib asked to keep Friday clear, so the 4 promo posts (Eventia 82, Avalon 83, Danish 70, Avenza 42) moved to Sun 11 Oct at the same times. Their Meta drafts were resubmitted.
- Avalon 80.png (10 Oct) was a wrong upload. It was deleted from the dashboard Planner on 5 Oct after Shoaib signed in to the browser pane. The dashboard delete also cancels the Meta draft.
- Toni & Guy "DEAL-4.png" (the design says Deal-5) is scheduled for 5 Oct, 18:45 PKT. Contacts: 0315 7001633, TONI&GUY Tower, C5 Model Town, Multan.
  - Since 2026-10-14 TONI&GUY has a full KB. Its `nap` comes from the salon materials + GBP: UAN 03-111-444-730, "C-5 A-Block, Model Town". See [[toniandguy-multan-knowledge-base]].
- Two Avenza Land posts were held back. Fatima re-uploaded both, fixed, by 13:40 PKT: (7) on 6 Oct and (6) on 12 Oct, both scheduled. The gate photo is still the Peshawar gate with the text edited out.
  - 39.png: the gate photo shows "DHA PESHAWAR" on the guard house.
  - 38.png: the "Graveyard" icon has a Christian cross.
- Avalon 80.png uses a stock room photo that isn't Avalon. It was scheduled anyway, and Shoaib was told.
- **Lesson:** zoom in on gate and building photos in DHA posts, and check that icons fit the audience's religion.

Related: [[instagram-login-connect]], [[gbp-reply-queue]].

**2026-10-22 — 61 posts scheduled (Fatima's uploads up to 9 Nov), times now saved per brand.**
- Regular-day times (PKT): Avalon 3:00 PM, Danish 3:30 PM, Avenza Land 5:00 PM, Eventia 6:00 PM. Choppers Salon 6 PM, Choppers Studio 1 PM, Avenza Avenue 5 PM (these three only had Jummah posts so far). Chosen from `social_best_posting_times` (90 days).
- Fridays: Jummah only, 10:00 AM, brands 15 min apart. Google waits a cron run if paced, so this keeps it near 10:00.
- All of the above is saved in `client_projects.posting_instructions`, which shows on every pending post.
- Overdue backlog (Eventia 14–21 Oct, Avalon 14–21 Oct, Danish 19–21 Oct):
  - 2 posts per brand went out the same evening (6–9 PM).
  - The rest went to the empty Sundays (25 Oct, 1 Nov), plus Eventia on 2–3 Nov.
- Avenza Land 94 "Start Your Week" moved to Monday 2 Nov, and 92 to 4 Nov.
- Avenza Land 29.png was uploaded twice (28 and 31 Oct, pixel-identical). The 31 Oct copy is held (needs_changes, notify=false, so no email to Fatima).
- Danish has no Google Business.
- Same file names (99–102.png) in Avalon's Oct and Nov batches are DIFFERENT designs. Compare pixels before calling something a duplicate.
- Contact-sheet trick for captioning many posts: sharp-tile the saved tool-results blobs 3×2 at 600 px with labels.
- Flagged to Shoaib (2026-10-22): three Eventia posts share the headline "From Vision to Execution" (26 Oct, 28 Oct, 1 Nov). The designs differ and each has its own caption, so they were scheduled. No answer on asking the designer to change them.
- Open (2026-10-22): for the held 31 Oct duplicate of Avenza Land 29.png, either email Fatima or ask for a new post. Shoaib hasn't said which.
- 2026-10-23: the Avalon and Eventia Stories failed on a Facebook checkpoint. They went out after Shoaib reconnected the second login.
