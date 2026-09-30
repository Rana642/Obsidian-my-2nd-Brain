---
name: adsbyshoaib-home-redesign-2026-09-28
description: "Home page rebuilt problem-first with keyword H1 (commit f1b8f2b); pending inputs (case-study numbers, WhatsApp, new FAQs) and planned Solutions pages"
metadata:
  node_type: memory
  type: project
  originSessionId: b56a07d2-4e15-4c3c-a2a6-3889366b3699
  modified: 2026-09-27T19:39:37.660Z
---

2026-09-28, at Shoaib's explicit request, the home page was restructured and its copy rewritten (Hook → Leap → Hold → CTA), commit f1b8f2b on main:
order Hero → TrustSignals → PainPoints (each links "How I fix this") → CaseStudiesPreview (citrus bg + audit CTA) → Services → Testimonials → new Process (3 steps + CTA) → AboutMini (philosophy quote moved here) → FAQ → FinalCTA. Turn / WhyChooseUs / Philosophy components still exist but are off the home page.
H1 = "Performance marketing that brings customers, not just clicks." (problem-led + keyword, Shoaib's call); title "Performance Marketing Consultant for Meta & Google Ads | Shoaib Nabi Noor".
Portrait is a background-removed cutout: public/images/shoaib-cutout.png (hero circle + about box).
Hostinger floating widget on "/" only appears after 50% scroll.

**Why:** competitor research showed ranking individuals put role+keyword in H1, proof early, repeated single CTA; Shoaib sells solutions, not services.

Commit 6d1a589 (same day): keyword title/eyebrow/problem-led H1 per service page via lib/service-seo.ts (code-side on purpose — Sanity edits go live instantly); new /hotel-marketing page (hotel digital marketing) pulling the 3 hotel case studies + hospitality testimonial from Sanity; footer links to each service; Hostinger widget waits for half-page scroll on all long pages.
Commit a0b0cd5: the 3 testimonials were placeholders → removed from home and hotel page, fallbackTestimonials emptied (only real quotes ever); the placeholder docs still sit in Sanity Studio for Shoaib to delete. Hostinger badge removed from the home trust strip (kept in footer/about/resume/coupon + floating widget).

Commit 3912c43: problem-first /solutions index + 4 pages from lib/solutions.ts (facebook-ads-not-working, google-ads-no-conversions, low-quality-leads, conversion-tracking-not-working; keywords from Keyword Planner, all LOW competition). Proof only where a real case study fits (Google/tracking pages have none yet). Solutions first in nav/footer; home pain points link there. Hostinger widget starts as the small badge with an outline button (c23a41a).

Commit cd194b5: UX audit in Shoaib's own Chrome (viewport ~1272x549 — design for that short laptop fold): home hero text no longer starts hidden (initial={false}), H1 2 lines/CTA above fold; contact form beside headline, message optional (client+API); hero CTA on services/service/case-studies/about; section padding cut site-wide (py-16 md:py-24), --text-hero max 4rem; Reveal faster + reduced-motion; final CTA secondary = WhatsApp (+92 301 7461642, Shoaib's own business number from the contact page).

Commit 4c1f70f: keyword pass — 24 of ~40 researched keywords now on-site (was 11). Service `lead` paragraph in lib/service-seo.ts; exact search phrases in solution intros; About targets "digital marketing expert in Pakistan" (title/eyebrow) + cutout portrait. Deliberately NOT used: agency keywords (brand voice = independent practice), social media marketing (not a listed service), AdWords synonyms (stuffing).

Commit 96d68c5: Shoaib confirmed he sells GBP management → /google-business-profile-management landing page (no ranking guarantees, no proof section until a GBP case study exists), linked from footer + /services "Also" block + sitemap.

Commit a5fcf7b: typography — --text-tag 12px, mono labels text-ink-muted (not ink-subtle, which fails AA at 3.55:1), dark-bg labels cloud/60; card titles = Geist semibold ("font-sans font-semibold text-xl md:text-2xl"), while H1/H2, quotes, blog titles stay Instrument Serif italic. Commit 6fcc0ee: portraits have no fade (they looked empty).

**How to apply:** still waiting on Shoaib for real numbers on results cards, booking link (WhatsApp number found: +92 301 7461642), answers for 2 new FAQs, whether "no long contracts" is true. Service keyword pages, hotel page and Solutions pages are done; Shoaib should confirm the causes/fix-steps copy matches his real audit method; redo competitor keyword research once DataForSEO is bought. Indexing stays off ([[adsbyshoaib-no-indexing-until-final]]).
