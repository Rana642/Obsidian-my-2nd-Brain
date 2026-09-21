---
name: reference-google-ads-account-id
description: The correct/only Google Ads account for Hotel Elegant Multan is customer ID 6223250696 (ABS | Hotel Elegant Multan) — never a different account
metadata: 
  node_type: memory
  type: reference
  originSessionId: fb776024-2ed0-44c1-a91d-40c7ca3ba0dd
  modified: 2026-09-18T06:22:16.691Z
---

Hotel Elegant Multan's real, currently-used Google Ads account is **customer ID 6223250696** ("ABS | Hotel Elegant Multan", under the "Ad By Shoaib" manager account). This is the account with the 2 active campaigns ("Planning to Travel", "Already in Multan", both launched 11/14-Sep-2026) and the one GA4 (property 546101152) is officially linked to (Google Ads link created 2026-08-04 by shoaib.nabi.noor@gmail.com).

**Why this needed saving:** A second, unrelated Google Ads account also happens to be named "Hotel Elegant Multan" — **customer ID 6684011164**, accessible via a different MCP connector under the "Ad By Shoaib" (8859347478) manager account. That account has been **suspended since ~26 June 2026** (billing/serving_status=SUSPENDED) and is NOT connected to this site's tracking or GA4 in any way. Confusing the two once already caused a real bug: the site's `NEXT_PUBLIC_GADS_TAG_ID` was wrongly set to `AW-18202393540` (6684011164's conversion tracking ID) instead of `AW-18370206861` (6223250696's real ID) — fixed 17 Sep 2026.

**How to apply — strict rule, explicitly reinforced by the user on 2026-09-18 after this exact confusion happened live:** Whenever checking, querying, or auditing "Hotel Elegant" Google Ads data, first confirm the connector being used actually has verified access to **customer_id 6223250696** specifically (e.g. a successful `customer.id` query against exactly that ID). Only proceed with that connector if so.

**If no available connector has access to 6223250696: stop and say so plainly. Do NOT check, query, or report on any other account instead — not 6684011164, not any other "Hotel Elegant"-named or similar-looking account turned up by `list_accounts` on a different connector, even temporarily or "just to see."** Substituting a different account, even to compare or investigate, is exactly the mistake that caused this — the user does not want it repeated in any form. If asked to check Google Ads and no connector reaches 6223250696, the correct response is "can't reach the right account right now, here's why" — never a report built from a different account's data.

Cross-check against GA4's `googleAdsLinks` (property 546101152) if ever unsure which account is real — that link is the authoritative source, currently pointing to 6223250696.

**Working access path (found 2026-09-18):** the original `mcp__google-ads__*` tool ("claude dev" connector) lost its grant (`invalid_grant`) and needs interactive `/mcp` re-auth to fix. In the meantime, the other connector (tool names `mcp__e41c058f...__google_ads_*`, the "ABS" one) CAN reach 6223250696 — but only when called with an explicit `loginCustomerId: "8859347478"` param (the "Ad By Shoaib" manager/MCC account) alongside `customerId: "6223250696"`. Without that login-customer-id, the same connector 403s on 6223250696 (it only sees 6223250696 as an MCC client, not directly). Always pass both params together on this connector.
