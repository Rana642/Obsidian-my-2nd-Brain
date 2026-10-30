---
name: feedback-content-ours
description: Every word on a post + its caption is OURS — written in the KB and approved by Shoaib before design; design tools (image AI, ChatGPT while designing) never write words. Global rule content-ours; occasion-copy doc
metadata:
  type: feedback
---
Shoaib, 2026-10-03: "Content bhi apny khud create kerna hai wo ai per nai chorna". He said this right after deciding that the AI designs the layout and scene.

**The rule** (KB global rule `content-ours`, all projects): the words on the image and the caption come only from approved KB entries.
- Calendar posts: `text_on_image` and `caption_for_posting`.
- Product posts: `product-brief-posts`.
- Occasions: `marketing_doc/occasion-copy`, entries `{key: [{approved, language, text_on_image, caption}]}`; Jummah variants rotate by week.

**The tool:** `kb_get_occasion_post` (ac29e54) uses approved entries only. With none approved it returns STOP plus the drafts, so the designer never writes a message or caption.

**Why:** brand voice, accuracy and compliance stay in our hands. AI-written greetings and claims drift.

**How to apply:**
- Before designing any post, check that its words are approved in the KB.
- If they are not, draft them, show Shoaib, save them with `approved: true` after he OKs, and only then design.
- Tad drafts saved 2026-10-03 (approved:false): World Animal Day, Jummah ×4, World Egg Day, World Food Day.

Related: [[feedback-plain-english]], [[occasion-posts]], [[social-design-system]].

**2026-10-03 (later)**
- Shoaib approved all the Tad drafts (16 Jummah + 3 days) and Meezab's 3 days. Meezab's Jummah = the same approved texts, offset 8 weeks.
- MCP tools (fe2a034), shared by the remote and local servers:
  - `kb_list_occasion_copy`: approved / pending per occasion;
  - `kb_save_occasion_copy`: draft or approved entry; same occasion + date replaces it;
  - `kb_approve_occasion_copy`: ONLY after the user OKs the exact words.
- `kb_get_brief` now shows `design_system` docs in full.
- Testing tip: the local social MCP needs SOCIAL_TOKENS_ENCRYPTION_KEY and API_VAULT_ENCRYPTION_KEY (not on this PC). Test KB tools with a tsx harness: `new McpServer`, `registerKnowledgeTools`, then call `_registeredTools[name].handler(args, {})` with `--conditions=react-server`.
