---
name: adsbyshoaib-letterhead
description: "A4 letterhead + saved letters at /dashboard/letterhead — vector rebuild of Shoaib's Canva PDF, official logo, brand colours, autosaving letter drafts (letters table); how print was verified"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-26T08:29:59.188Z
---

**Phase 1 (2026-09-25, b879e63 + c1c33ff):** `components/dashboard/Letterhead.tsx`
redraws Shoaib's A4 letterhead as one vector SVG. His Canva PDF
("Untitled design (3).pdf") was a single ~127 DPI raster that printed
soft, so every element was measured off it and rebuilt in 0.1 mm units
(viewBox 2100×2970). Text uses `textLength` and is set in Inter, which
lives in `letterhead-font.ts`.

**Phase 2, drafts (2026-09-25, 1b0f43e; he asked for it with "Brand Color aur Draft wala bhi ker k final kero"):**
- `/dashboard/letterhead` is the list of saved letters. `/new` and `/[id]`
  both render `LetterEditor.tsx`.
- **Autosave:** saves 1.2 s after typing stops, plus Ctrl+S. It calls
  `saveLetter(id|null, input)` in `lib/dashboard/actions/letters.ts`,
  which deliberately never revalidates or redirects, because a re-render
  would reset the editor.
- **New letters:** a letter only becomes a record on its first change,
  so opening one to print a blank pad leaves nothing behind. After that
  first save, `history.replaceState` moves it to `/[id]` without a
  remount.
- **Ref numbers:** `LTR-YYYY-###` via `generateNumber("letter")`. It
  probes that the table exists before reserving a number.
- **Body HTML:** whitelisted by `lib/dashboard/letter-html.ts` (b/i/u,
  ul/ol/li, div/p/br, text-align only) both on load and on save.
  Paste and drop come in as plain text.
- **Fields:** title (for the list only, never printed); date (default
  is today in Asia/Karachi); `show_meta` prints "Ref" and "Date" above
  the body.
- **Other actions:** Duplicate (makes a new ref, date set to today) and
  Delete.
- **Unmount gotcha:** the flush on unmount reads `bodyHtmlRef`, not the
  DOM, because refs are already null during passive-effect cleanup.

**`letters` table:** it is in `supabase/dashboard-schema.sql`, "Letters"
section. Shoaib ran it in the SQL Editor on 2026-09-26, and it was
verified live. If the table is ever missing, the list page shows a setup
card and saving shows a setup message (PGRST205).

**Design decisions (his calls):**
- Header is the official lockup, `public/brand/logo-horizontal.svg`,
  with a typeset "PERFORMANCE MARKETING" line under it.
- Watermark is `mark.svg` at 3% opacity.
- All colours are brand tokens: the stripe, and the tagline rule, use
  Cobalt #2196F3, Citrus #FEC107 and Forest #3FA343; text is Ink
  #0F0F14. These replaced the PDF's #007EFF, #FEC40A and #1B9650.

**Print:** the `SHEET_CSS` inside `LetterheadSheet` sets
`@page{size:A4;margin:0}` and uses a `:has()` rule to hide all dashboard
chrome. It also restores list bullets and numbers, which Tailwind
preflight removes. Verified: 1 page, 209.9×297 mm, zero raster images.

**How to verify (reusable):**
1. The in-app browser can't log into /dashboard, so render the editor
   on a temporary public route (`app/lhtmp/page.tsx`) and DELETE it
   before committing. Any edit there autosaves, the action's auth check
   fires, and the page gets redirected to /dashboard/login. That is
   expected.
2. Print with headless Edge:
   `msedge --headless=new --no-pdf-header-footer --print-to-pdf=<ABSOLUTE path> <url>`
3. Render the PDF with the `pdfjs-dist` + `@napi-rs/canvas` setup in the
   scratchpad (`pdfrender/render-letter.mjs`). There is no poppler or
   python on this PC.

See [[adsbyshoaib-dashboard]] and [[shoaib-two-pc-workflow]].

**2026-10-05 — address, NTN and one grid (5a403e7)**

Shoaib: "Address ye wala pora aur Business NTN bhi Add kero", then "Design change ker sakty ho Aesthically allignment k liye".

- **Footer row:** web · phone · email · `NTN 8200652-5`, with an ID-card glyph.
  - The NTN is the FBR "Reference No". The 13-digit "Registration No" is his CNIC and is deliberately not printed.
  - The four groups spread between the 20 and 190 mm margins with equal gaps. Positions are computed from Inter-measured text widths in `FOOT_GROUPS`; re-measure if the wording changes.
- **Second line:** the pin + the full FBR address, centred. On 2026-10-05 "Cantt." was removed at Shoaib's request (the address is not in the cantonment), so it now reads "...Street 6, Multan, Pakistan" (15b35b5).
- **Header:** the logo and the tagline block moved onto the body's 20/190 mm margins. The watermark is centred.
- **Hairline:** at 271.3 mm, closing the writing area.
- **FooterText `width` is now optional:** original lines keep their textLength, while new lines use Inter's natural width.
- **Verification recipe re-used:**
  - temp `app/lhtmp` → headless Edge PDF → `pdfrender/render-letter.mjs`;
  - result: 1 page, 209.9×297 mm, 0 raster images;
  - the temp route was deleted before the commit.

**2026-10-05 — blank date, no slogan rule (e4a0e8e)**

Shoaib: "Date manual per ker do _________ laga do likho tab add kero by defalut add na ho Slogan k side line bhi hata do".

- **Blank date:** new and duplicated letters start with no date, and the sheet prints "Date: ____" (a 38 mm bottom-border span).
- **Today / Clear buttons** sit beside the date picker.
- **Optional `letter_date`:** the schema allows `""` → null, and `LetterInput` is now `z.input`.
- **Waiting on Shoaib:** the column needs SQL that he must run — the "Letters: optional date" section of `dashboard-schema.sql`: `drop not null` plus `drop default`.
  - Until then, saving a blank date returns DATE_SETUP_MESSAGE (code 23502) instead of the raw error.
  - Old letters keep their dates.
- **Header:** the cobalt rule beside the tagline is removed.

**2026-10-05 — Ref typed by hand (34e212c)**

Shoaib: "Ref: ka bhi same ker do Line show kery jb tak likhon na khud".

- **New `print_ref` column:** the Ref field sits beside Date. Blank prints "Ref: ____"; typed text prints exactly as typed.
- **`ref_no` (LTR-YYYY-###)** stays the internal record number, shown in the list only.
- **The SQL section is renamed** "Letters: optional date and own ref". It covers:
  - `letter_date` drop not null and drop default;
  - add `print_ref text not null default ''`;
  - backfill `print_ref = ref_no` for letters created before 2026-10-06.
- **SQL run by Shoaib on 2026-10-05, verified** (the backfill set LTR-2026-001 as its print_ref; a null-date test insert worked and was deleted). Before it ran:
  - Before it runs, a letter with an empty ref still saves (the action retries without `print_ref` on PGRST204).
  - A typed ref, or a blank date, shows a setup message.
