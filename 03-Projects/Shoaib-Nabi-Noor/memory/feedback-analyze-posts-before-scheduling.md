---
name: feedback-analyze-posts-before-scheduling
description: "Whenever Shoaib asks to post or schedule planner posts, first view and analyze every creative; only then caption + schedule, holding any with errors"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-05T08:34:56.058Z
---

Whenever Shoaib says to post or schedule (planner posts, any project), the FIRST step is to analyze every post. Captions and scheduling come only after that.

For each post:
- Open the image with `social_get_post_image`.
- Check it is in the right project. The logo, phone and address must match that brand.
- Check it is on the right day. A Jummah or occasion post must sit on its own date, and a Friday must follow the Friday rule.
- Zoom into small text in photos. In the 2026-10-05 batch, a "DHA Multan" ad used a gate photo that said "DHA PESHAWAR".
- Check that icons and imagery suit the audience. A graveyard icon had a Christian cross, and one hotel room photo was stock.
- Check that the contact details match the brand.

A post with a problem is held back with `social_request_changes` (one call per batch). This marks it needs_changes and alerts and emails the uploader. Shoaib is also told exactly what is wrong. See [[planner-needs-changes]]. Everything that is correct gets its caption and is scheduled.

**Why:** Shoaib praised these catches on 2026-10-05: "Apka catch boht acha hai har baar jb bhi posting ka kahon sab se pehly yehi kerna". A wrong photo or icon on a client page damages the client's trust and is public.

**How to apply:** Do this every time, even if he only says "schedule kar do". Report the findings first, then what was scheduled. Brand-specific notes are in [[aijaz-danish-brands]]. Related rules: [[feedback-social-post-locked-vs-free]], [[occasion-posts]].

**Flag wording (rule from 2026-10-14):** every issue sent with `social_request_changes` is in English plus a "Roman Urdu:" explanation, because the uploaders (e.g. Fatima for the Aijaz brands) read Roman Urdu more easily. KB global rule `post-flag-roman-urdu`; the tool description in `lib/post-review-tools.ts` carries it too.
