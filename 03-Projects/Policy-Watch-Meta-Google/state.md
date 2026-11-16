# Policy watch state — Meta + Google ads/developer policy

Each section: source URL, last-checked date, and a short plain-English
summary of the current rule. The weekly routine updates a section only when
the live page's actual rules changed (not for wording/formatting tweaks),
and always bumps the checked date even when nothing changed.

## Meta Ads (Advertising Standards)
**Source:** https://transparency.meta.com/policies/ad-standards/
**Checked:** 2026-11-16

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
**Checked:** 2026-11-16

All apps, any access level, must follow Platform Terms + Developer
Policies — calls run against real production data except in explicit
Sandbox mode. Platform data can only be used for the App-Review-approved
use case: not AI training, not resale, not kept longer than needed, not
shared outside the integration. Scope creep (new data/actions beyond what
was approved) can require a new App Review. Marketing API has its own rate
limiting, separate from general Graph API limits.

## Google Ads policies (advertiser-facing)
**Source:** https://support.google.com/adspolicy/answer/6008942
**Checked:** 2026-11-16

Four broad areas: prohibited content (counterfeit, dangerous
products/services, misrepresentation), prohibited practices (misleading
claims, data misuse, network abuse), restricted content (alcohol, gambling,
healthcare/pharma, financial services, adult, political — certification or
geo limits), editorial/technical quality (ad text format, destination must
match and work). A single ad can be disapproved and fixed/resubmitted;
repeated or egregious violations can suspend the whole account. English is
the authoritative enforcement language.
Recent changes (2026): Limited Ad Serving policy now covers all Google Ads
products (limits impressions for unqualified advertisers, not disapproval;
rollout through 2028). Government documents/services ads need advertiser
verification + certification (enforced from 5 Oct 2026). Appeals not
accepted for decisions older than 6 months (since Jul 2026).

## Google Ads API developer policy (access levels + RMF)
**Source:** https://developers.google.com/google-ads/api/docs/access-levels ; https://support.google.com/adspolicy/answer/6169371
**Checked:** 2026-11-16

Access levels: Test account → Basic (production accounts, daily limits) →
Standard (unlimited daily ops for most services). At Standard access,
Required Minimum Functionality (RMF) applies: a tool must offer a defined
minimum feature set across Creation/Management/Reporting categories;
Google's Compliance team decides if a tool counts as "full-service" and can
audit it; non-compliance can mean fees or a token downgrade. Developer tokens
were sunset on 9 Sep 2026: access levels now attach to the Google Cloud
project used to generate the OAuth credentials; a token header is optional
and ignored for now (may be rejected in a future major API version). Review
backlogs were noted in Feb 2026 and pending Basic applications reportedly
reset in the transition — plan access upgrades ahead of time. Verify the
~90-day-unused revocation rule against Google's page; it may no longer apply.
