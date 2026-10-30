---
name: feedback-prices-in-pkr
description: "Always tell Shoaib prices/costs in PKR (convert USD at a live rate, show the rate used)"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-02T11:39:41.129Z
---

When I report a price, cost, credit balance, savings or budget to Shoaib, I give it in **PKR**. Examples: API costs, image generation, ad spend estimates, subscriptions.

Shoaib said on 2026-10-02: "pricing k liye hamesha pkr use kero pkr mai bataya kero mujy".

**Why:** he thinks and pays in PKR. A USD figure alone means nothing to him without conversion.

**How to apply:**
- Fetch a live USD→PKR rate before converting. I used `https://open.er-api.com/v6/latest/USD`. Never guess a rate.
- Say which rate I used, e.g. "1 $ = Rs 2xx".
- Lead with PKR. Add USD in brackets only when it helps, for example when a provider's dashboard shows dollars.
- Mention that the bank's actual card charge can differ a bit because of the bank's own rate, FX fees and taxes.
- Amounts below 1 rupee: say "1 rupee se kam" or give 2 decimals.

Related: [[graphics-studio-project]].
