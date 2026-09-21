---
name: adsbyshoaib-resume-data
description: "Shoaib's real CV data is stored in lib/resume.ts; work experience (4 jobs + 6 remote projects) is still missing"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-22T13:27:06.468Z
---

Shoaib sent his complete real CV data on 2026-08-22 (summary, all technical/soft skills, languages, education — B.Com from Bahauddin Zakariya University 2012, HSSC/SSC — certifications — OEC & ICMPD Soft Skills Training cert #26cc3a76 — and social profiles). It's wired into `lib/resume.ts` and rendered on `/shoaib-nabi-noor`.

**Work experience received and wired in (2026-08-22).** Data lives in `lib/experience.ts`, rendered by `components/sections/ExperienceAccordion.tsx` as an interactive timeline/accordion (Shoaib asked for "zyada dynamic way" instead of a flat list). 3 primary onsite roles (Avenza Group, Al Mannan Builders, Choice Shoes) + 7 remote/client projects (Meezab Z, TAD Pharma, Hotel Elegant Executive Suite, Hotel Silver Sand, Multan Law Firm, Come Live In France, Trönninge Pizza — the last two were split out of a combined "International Clients" entry per his feedback below). Akhuwat Foundation (his real pre-2019 accounting job) and Moro Creatives are excluded per his explicit rule — see the corrected [[accountant-story-resume-only]] memory.

**Per-brand cards, not shared lists (2026-08-22).** Shoaib pushed back: Avenza Group's 8 managed brands were dumped into one flat pill list under a shared "Key Contributions" — no individual weight, "har project ki apni importance hai" (each project deserves its own importance). Fixed: `managed` is now `{name, note?, url?}[]` per role, rendered as individual cards. **Rule: only add a `url` when Shoaib has stated the real domain as fact — never guess a business's website/social handle.** Confirmed real links so far: AvenzaLand.com, choiceshoes.pk, meezabz.com, tadpharma.pk, comeliveinfrance.com, tronningepizza.se. Toni&Guy Multan, Choppers Salon, Hotel Avalon Suites, Eventia 360, Pines Institute, Avenza Avenue have no confirmed link yet — the `url` field is Studio-editable now, so ask Shoaib to add them there directly rather than touching code.

**Downloadable full CV PDF built (2026-08-22).** `public/documents/shoaib-nabi-noor-resume.pdf`, generated via `scripts/generate-resume-pdf.tsx` (`npm run generate:resume-pdf`, uses `@react-pdf/renderer`). This is the one place Akhuwat Foundation appears on the whole project — it's a "CV/Resume context" per Shoaib's own rule, not the website itself. Still excludes CNIC/father's-name/marital-status even here (ask him explicitly before ever adding those). The Resume page's sticky button now downloads this file directly (`Button.tsx` gained a `download` prop for this). If his work history or skills change later, edit the script's inline data and rerun the npm script to regenerate.

**Why some of his data was excluded:** his CV data also included Personal Information — father's name (Rana Noor Nabi), CNIC number, marital status. These were deliberately left out of the public website (a government ID number shouldn't be indexable/scrapable). If Shoaib wants a downloadable PDF resume that includes this, that's a separate private artifact, not page content — flag this distinction again if he asks why it's missing from the site. See [[accountant-story-resume-only]] for the parallel rule about the accountant background (same page, opposite exclusion logic — that's the one piece of "risky" personal narrative that's ALLOWED here, since it's professional history, not government ID data).
