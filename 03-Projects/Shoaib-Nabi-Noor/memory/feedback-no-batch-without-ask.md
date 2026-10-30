---
name: feedback-no-batch-without-ask
description: Never run batch image generation (Graphic Studio batch / OpenAI Batch API / many posts at once) unless Shoaib explicitly says so — make posts one by one on request
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-05T11:54:54.868Z
---

Never run a batch generation (the Graphic Studio batch feature, the OpenAI Batch API, or many posts in one go) unless Shoaib explicitly asks for a batch. Generate only the posts he asked for, one at a time, with draft → final.

**Why:** On 2026-10-05, while topping up OpenAI credits for the REEFCO product posts, Shoaib said: "Budget daal raha hn lakin kabhi batch nai banana jb tak kahon na". He pays for the credits and wants control over spending.

**How to apply:** For a request like "2 products ki post", make exactly those 2, sequentially. Say the approximate cost in PKR ([[feedback-prices-in-pkr]]). Never queue extra variants, other products or a batch job on my own. Related: [[graphics-studio-project]].
