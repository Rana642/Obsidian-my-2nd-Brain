# Policy watch state — Meta + Google ads/developer policy

Each section: source URL, last-checked date, and a short plain-English
summary of the current rule. The weekly routine updates a section only when
the live page's actual rules changed (not for wording/formatting tweaks),
and always bumps the checked date even when nothing changed.

## Meta Ads (Advertising Standards)
**Source:** https://transparency.meta.com/policies/ad-standards/
**Checked:** 2026-11-11

Meta reviews every ad against Advertising Standards (separate from, and in
addition to, Community Standards). Core principles: no discrimination, no
fraud/scams, no low-quality/disruptive content. Extra restrictions apply to
housing/employment/credit (Special Ad Category, narrower targeting),
social-issues/elections/politics (authorization needed in some regions),
health/pharma/supplements (claims must be substantiable), financial
products (often need permission/licensing proof), alcohol/gambling/adult
(age/geo limits). Repeated or serious violations can restrict the whole
account, not just one ad. Appeal through Account Quality.

## Meta developer platform (Platform Terms + Developer Policies)
**Source:** https://developers.facebook.com/devpolicy ; https://developers.facebook.com/docs/marketing-api/get-started/authorization
**Checked:** 2026-11-11

All apps, any access level, must follow Platform Terms + Developer
Policies — calls run against real production data except in explicit
Sandbox mode. Platform data can only be used for the App-Review-approved
use case: not AI training, not resale, not kept longer than needed, not
shared outside the integration. Scope creep (new data/actions beyond what
was approved) can require a new App Review. Marketing API has its own rate
limiting, separate from general Graph API limits.

## Google Ads policies (advertiser-facing)
**Source:** https://support.google.com/adspolicy/answer/6008942
**Checked:** 2026-11-11

Four broad areas: prohibited content (counterfeit, dangerous
products/services, misrepresentation), prohibited practices (misleading
claims, data misuse, network abuse), restricted content (alcohol, gambling,
healthcare/pharma, financial services, adult, political — certification or
geo limits), editorial/technical quality (ad text format, destination must
match and work). A single ad can be disapproved and fixed/resubmitted;
repeated or egregious violations can suspend the whole account. English is
the authoritative enforcement language.

## Google Ads API developer policy (access levels + RMF)
**Source:** https://developers.google.com/google-ads/api/docs/access-levels ; https://support.google.com/adspolicy/answer/6169371
**Checked:** 2026-11-11

Access levels: Test account → Basic (production accounts, daily limits) →
Standard (unlimited daily ops for most services). At Standard access,
Required Minimum Functionality (RMF) applies: a tool must offer a defined
minimum feature set across Creation/Management/Reporting categories;
Google's Compliance team decides if a tool counts as "full-service" and can
audit it; non-compliance can mean fees or a token downgrade. An unused
developer token (no calls for ~90 consecutive days) can be revoked.
Developer token applications have had review backlogs (noted Feb 2026) —
plan access upgrades ahead of time.
