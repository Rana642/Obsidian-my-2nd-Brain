---
name: feedback-plain-english
description: Plain (simple) English by default for ALL projects' copy — Pakistani audience; advanced/native only when Shoaib says so per project (global KB rule plain-english)
metadata:
  type: feedback
---

All English I write for any project uses **plain English** unless Shoaib says a project needs "advanced" or "native" English. This covers text on images, captions, ads, review replies and drafts.

He said on 2026-10-02:
- First, for the Pakistani brands: "jitna ho sakta hai assan english use kero jo pakistan audience samaj skyn".
- Then for everything: "Sab projects k liye hi assan english kr do mai khud bata dn ga kisi k liye advance kerni hui to ya native kerni hui to".

**Why:** the audience is mostly Pakistani and reads English as a second language. Words like "intestinal" or "facilitates" lose them.

**How to apply:**
- Short sentences and everyday words.
- Keep trade terms, product names, doses and claims exact. Change the wording only, never the meaning.
- No idioms.
- Older KB copy gets rewritten when it is next used, and Shoaib reviews the new wording. Example: "A Perfect Protection Against Intestinal Diseases" → "Strong Protection Against Gut Diseases".
- The rule lives in global KB rule `plain-english` and a server-instructions line (commit after 6abed17).
- A per-project exception would be `english_level: advanced|native` in that project's docs.

Related: [[feedback-social-post-locked-vs-free]].
