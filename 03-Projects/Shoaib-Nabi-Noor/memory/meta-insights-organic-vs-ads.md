---
name: meta-insights-organic-vs-ads
description: "Dashboard Insights splits organic vs ads (566a03c, 2026-10-14) — which Meta figures include ads, how each is split, and the raw-Graph paging token leak gotcha"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-14T11:00:26.313Z
---

On 2026-10-14 Shoaib asked to take ad numbers out of the Insights metrics: "sirf organic ka data batao ya dono alag". The dashboard now shows **organic as the main figures** and a separate **"From ads" card** (commits 566a03c and 3e89430). Facts below were checked against the live API through a temporary probe, which has since been removed.

**Instagram**
- Per-post views, reach, likes, comments, shares and saves are **organic only at the source**. `total_views`, `total_likes` and `total_comments` add ads **and** the views of the cross-posted Facebook post, so `total_views − views` is NOT the ad part.
- Account totals include ads:
  - The paid part comes from `breakdown=media_product_type`, value "AD", which works for views, reach, likes, comments, shares, saves and total_interactions.
  - `accounts_engaged`, `profile_views`, link taps and website clicks can't be split, so the dashboard labels them "Includes ads".
  - Reach is unique, so "total − AD" gives a floor ("at least").
  - Saves and interactions are net counts, so the AD part can come back negative; it is floored at 0.

**Facebook**
- `page_media_view` and `post_media_view` take `breakdown=is_from_ads`. Meta returns one value per day per side, `{value, end_time, is_from_ads:"0"|"1"}`, and leaves out days where that side is zero.
- Video views come from `page_video_views_organic` and `page_video_views_paid`.
- Viewers, post engagements and page visits can't be split, so they are labelled "Includes ads".

**Gotcha:** raw Graph responses carry `paging.previous/next` URLs that **contain the access_token**. Never pass raw Graph JSON on to a browser or a log. Strip `paging` first.

**Why it matters:** for TONI&GUY Multan, the "+114% Facebook video views" I reported earlier was 88% ads. Check the organic/ads split before you claim any growth. See [[toniandguy-multan-knowledge-base]].

**MCP tool `social_get_insights` (dbf0a11, 2026-10-29).** Read-only. It returns the dashboard Insights data for a project's FB, IG and LinkedIn Page, for audits from chat: followers, current vs previous period, organic vs ads, formats, IG audience, best days/hours, top posts. Arguments: `project` (an exact name wins; it errors if the name matches several projects), `days` 7/28/90, `postLimit`. Code: `lib/social-insights-tools.ts`, registered on both MCP servers. First used for the Choppers Salon audit.
