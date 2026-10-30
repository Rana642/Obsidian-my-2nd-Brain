---
name: mcp-quran-iconify-tools
description: "2026-10-09 ABS MCP tools from Shoaib's public-apis fork — quran_get_ayah / quran_search / kb_add_verified_ayah (Quran Cloud, exact Tanzil Arabic into occasion-posts verified_texts) and icon_search / icon_get_svg (Iconify); 2a8b409"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-09T12:26:05.694Z
---

Shoaib forked public-apis/public-apis to `Rana642/public-apis--By-shoaib`. It is identical to upstream: just the API list, with no code. From it he picked "Quran Cloud aur Iconify laga do" (2026-10-09). The other candidates still waiting for his go:
- Pexels/Pixabay/Unsplash stock photos instead of paid AI scenes;
- currency-api for a live PKR rate;
- Calendarific/caldays for PK/JO holidays;
- PageSpeed;
- QR codes.

**Built in `lib/mcp-content-tools.ts`** (adsbyshoaib 2a8b409). It is registered inside `registerKnowledgeTools`, so it runs on both the local stdio and the remote OAuth servers. No keys needed, and it only calls the two fixed hosts.
- `quran_get_ayah(reference "65:3" | "94:5-6", translations?, script?)` returns the exact Tanzil quran-simple Arabic, numbered words and reference translations (en.sahih, ur.jalandhry). Translations are copyrighted, so use them for meaning only.
- `quran_search(keyword, edition?, surah?, limit?)`.
- `kb_add_verified_ayah(id, reference, words? "6-11", type, english, urdu?, occasions?, overwrite?, confirm?)`:
  - Copies the API's characters into the `occasion-posts` JSON → `verified_texts`, using the same fields as the 2026-10-03 entries (type, arabic, english, reference, reference_ur, urdu, source, verified).
  - It is a preview until called with `confirm: true`.
  - The word-range excerpt keeps any pause marks inside the range.
  - Tested: words 6-11 of 65:3 match the stored `tawakkul` text exactly.
- `icon_search(query, prefixes?, limit?)` and `icon_get_svg(icon, color?, height?)` return SVGs with the licence of their set.
  - The default sets are lucide, tabler, mdi, ph, material-symbols, healthicons (has animal-chicken), fluent and simple-icons.
  - simple-icons:whatsapp is the same glyph now used in the footer.ts fix.
- **Hadith is not covered.** It still goes through the hadith-api + sunnah.com check in [[occasion-posts]].
- The new tools appear only after the Vercel deploy and an ABS connector refresh (or a new session).
