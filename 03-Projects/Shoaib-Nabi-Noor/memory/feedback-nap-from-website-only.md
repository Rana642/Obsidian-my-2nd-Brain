---
name: feedback-nap-from-website-only
description: "For any brand, NAP (name/address/phone/email) comes only from the brand's official website — never from product PDFs/labels, never inside branding docs"
metadata:
  node_type: memory
  type: feedback
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-24T06:35:40.419Z
---

Shoaib's standing rule (2026-09-24): contact details (NAP) must always come from the brand's **official website**, never be extracted from product PDFs, pack/label photos or old creatives, and never be made part of a branding document (positioning/ICP/graphic rules). Keep them in a separate `nap` doc updated from the website.

**Why:** I had pulled Tad Pharma's address/phones/email out of the product PDFs (Jahanian/Khanewal address, 0337/0321 numbers, a gmail address) into the knowledge base and an Instagram post — but the real, live site (tadpharma.pk) says Bukhari Colony, M.A. Jinnah Road, Multan · 0300 4307810 · info@tadpharma.pk. Literature/labels carry older or different details. Also the PDFs made me wrongly say Tad Pharma is based in Jahanian; the website says Multan HQ (founded Dec 2022, 3 principals: REEFCO, Lexington, Biorise).

**How to apply:** before writing any creative/caption/doc with contact info, read the brand's website (live, or its repo like `tad-pharma/lib/data.js` → `COMPANY`), and use the project's `nap` KB doc (`kb_get_brief`). Stored as global KB rule `nap-source`. Related: [[adsbyshoaib-tad-pharma-knowledge-base]].
