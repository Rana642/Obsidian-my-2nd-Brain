# Memory Index

## Start here
- [Waiting on Shoaib](waiting-on-shoaib.md) — every open question / on-hold item with date + link (2026-10-27); prune as he answers
- [Shoaib's preferences](shoaib-working-preferences.md) — Roman Urdu replies, short; screenshot+circle = the ask; "ruk jao" = stop; push fast
- [Two-PC workflow](shoaib-two-pc-workflow.md) — repo also worked from another PC: fetch/rebase + npm install first, never force-push
- [Confirm UX before building](shoaib-confirm-ux-before-building.md) — layout/UX restructuring: describe the design, wait for OK; small fixes don't need it
- [Ask before ANY spend](feedback-ask-before-spend.md) — every paid API call needs his yes first: what, how many, model, PKR cost
- [No batch without ask](feedback-no-batch-without-ask.md) — never batch-generate images unless he says; make requested posts one by one
- [Prices in PKR](feedback-prices-in-pkr.md) — costs in PKR at a live USD→PKR rate (state it); USD only in brackets
- [No secrets in memory](feedback-no-secrets-in-memory.md) — Obsidian vault repo is PUBLIC (a key leaked 2026-10-02); write "in .env.local", never values
- [PKT time gotcha](feedback-pkt-time-gotcha.md) — Git Bash `TZ=Asia/Karachi date` prints UTC here; PKT = UTC+5; check before scheduling

## Website (adsbyshoaib.com)
- [Site infra](adsbyshoaib-site-infra.md) — DNS in Vercel (not Hostinger), Sanity build gotchas, env-gated analytics; repo + dev server
- [Brand colours](adsbyshoaib-color-swap.md) — 2026-09-15 rebrand: Citrus #FEC107, Cobalt #2196F3 decorative, Forest #3FA343; logo = public/brand SVGs
- [No indexing until final](adsbyshoaib-no-indexing-until-final.md) — SITE_IS_LIVE=false blocks indexing; never flip without his explicit go-ahead
- [CMS architecture](adsbyshoaib-cms-architecture.md) — Sanity Studio /studio (m3je8htk) runs content, Sanity-first-with-fallback; leads in Supabase
- [Resume data](adsbyshoaib-resume-data.md) — real CV in lib/resume.ts; work experience still missing; CNIC/father/marital status left out on purpose
- [Accountant story](accountant-story-resume-only.md) — Akhuwat + Moro Creatives kept out of all personal branding (site, LinkedIn); CV PDF only
- [Aesthetic audit 2026-08-24](adsbyshoaib-aesthetic-audit-2026-08-24.md) — box-decoration-clone highlight lesson; About "8 industries" list still unreviewed
- [Perf/cost cleanup](adsbyshoaib-perf-cost-cleanup-2026-08-24.md) — ISR 1h is the main Vercel/Sanity lever; faster edits = webhook revalidation, not shorter
- [Privacy policy & API compliance](adsbyshoaib-privacy-policy-api-compliance.md) — GBP API disclosure; verify any AI-drafted legal copy against real code
- [Socially Snap — TEMPORARY](adsbyshoaib-socially-snap-temporary.md) — "formerly Socially Snap" in 3 spots; revert only when he confirms GBP rename
- [Hostinger Partner](adsbyshoaib-hostinger-partner.md) — partner badge everywhere, floating widget, /hostinger-coupon page; code + details in lib/hostinger.ts
- [Verify via local dev server](adsbyshoaib-verify-via-local-dev-server.md) — browser-test uncommitted changes on localhost:3000, never production
- [Print: extension overlays](print-extension-overlay.md) — bf3e34e: extension icons printed over amounts; print CSS fix + headless Chrome PDF test

## Dashboard
- [Business dashboard](adsbyshoaib-dashboard.md) — /dashboard on Supabase: clients, catalog, quotations, invoices; what stays in Sanity vs here
- [Dashboard navigation + Finance](adsbyshoaib-dashboard-navigation.md) — GHL-style sections, Finance phases 2–3, + New / Ctrl+K; API Vault inside Password Vault
- [Dashboard glass UI](adsbyshoaib-dashboard-glass-ui.md) — glass is dashboard-only; collapsible sidebar; Lightning-CSS backdrop-filter gotchas
- [GHL improvements backlog](adsbyshoaib-ghl-improvements-backlog.md) — 9 GoHighLevel ideas (pipeline, invoice cards, bell…); none picked as of 10-27
- [Proposal/Agreement funnel](adsbyshoaib-proposal-agreement-funnel.md) — accept/sign cascades, offline controls, discount-on-total; "use server" tsx gotcha
- [Setups + proposal lines](adsbyshoaib-setups-and-proposal-lines.md) — 13 single setup services (/setups), post-count stepper, complimentary lines
- [Invoice add-ons + discount](invoice-addons-discount.md) — discount on invoice, add-ons, complimentary lines; groups + per-group discount + Grand total
- [Payments + receipts](adsbyshoaib-payments-receipts.md) — no gateway: bank details + /pay, client uploads receipt, manual confirm at /dashboard/receipts
- [Letterhead](adsbyshoaib-letterhead.md) — /dashboard/letterhead vector A4 + autosaving letter drafts; print-verify recipe
- [Password vault](adsbyshoaib-vault.md) — zero-knowledge /dashboard/vault (client→project, 2-step, generator); API Vault is a tab inside
- [Client intake](adsbyshoaib-client-intake.md) — /intake/[token] forms → Supabase; brand assets to Cloudflare R2 via lib/storage.ts
- [Client portal](adsbyshoaib-client-portal.md) — /portal: client role by invite, sealed credential submissions → vault inbox, requests
- [Security hardening](adsbyshoaib-security-hardening.md) — CSP, rate limits, backups; sign-ups off, new users need app_metadata.role

## Social planner + platforms
- [Analyze posts before scheduling](feedback-analyze-posts-before-scheduling.md) — view + check every creative first; hold flawed ones; flags in English + Roman Urdu
- [Planner needs-changes](planner-needs-changes.md) — flag flawed uploads → portal alert + one email per uploader; Replace image; social_request_changes
- [Planner Reels + Stories](planner-reels.md) — Reels, Stories (full 9:16 frame c07c10e), YouTube + TikTok video (10-15); IG two-phase publish
- [Planner upload from a local folder](planner-upload-from-local-folder.md) — shrink → presign in logged-in prod tab → curl PUT → insert; check Reel phone numbers
- [FB natives stuck after reconnect](fb-native-stuck-after-reconnect.md) — 10-30: 36 FB posts never published yet Planner "ok"; fixed + lib/social-verify.ts checks every FB post now
- [Morning report](morning-report.md) — daily 8 AM PKT Planner email (free code, not AI): kal/masle/aaj/khaali din; once-per-day key; preview script
- [FB native schedule → feed fix](fb-native-schedule-feed-fix.md) — /photos scheduling missed the feed; a95bbf5 uses /feed; audit /{page}/scheduled_posts
- [Instagram Login connect](instagram-login-connect.md) — non-Page IG via Instagram Login: Tester role, API Vault "instagram_login", graph.instagram.com
- [Import Pages + two logins](connections-import-pages.md) — which login each Page uses (via chips, move, bestSource); only Avalon left on Ajaz; app hourly limit #4; regex lesson
- [LinkedIn Community API](linkedin-community-api.md) — vetting passed 10-28, Dev Tier; upgrade form due ~11-18 with demo; don't re-request
- [Meta App Review video](meta-app-review-video.md) — NEXT (home PC, 31 Oct eve): clip plan, captions, Ads by Shoaib test Page; never submit without him
- [Meta Ads API setup lessons](meta-ads-api-setup-lessons.md) — ABS needs ad-account + Page assignment; Lead ToS; CBO same-goal; Choppers Studio Pink Oct campaign ids
- [Meta insights: organic vs ads](meta-insights-organic-vs-ads.md) — IG per-post = organic; subtract "AD" breakdown; FB is_from_ads; paging URLs leak token; MCP tool social_get_insights for FB/IG audits
- [GBP reply queue](gbp-reply-queue.md) — Claude writes replies, human-paced cron (15–35 min, 18/day/location); Elegant caught up 10-27, Silver Sand 283 left
- [Occasion posts](occasion-posts.md) — Jummah/event posts for every project via kb_get_occasion_post; Arabic/Urdu only from verified texts
- [Quran Cloud + Iconify MCP tools](mcp-quran-iconify-tools.md) — quran_get_ayah/search, kb_add_verified_ayah (never type Arabic verses), icon search
- [KB upload tools](kb-mcp-upload-tools.md) — kb_create_direct_upload (curl PUT + finish), kb_update_asset, kb_upsert_products

## Post content + design rules (ALL projects)
- [Content is ours](feedback-content-ours.md) — every post word + caption approved in KB before design; design AI never writes words
- [Plain English default](feedback-plain-english.md) — simple English for all client content (Pakistani audience) unless he says otherwise
- [No "dealer" word](feedback-no-dealer-word.md) — never "dealer", any project; always "distributor" / "Become a Distributor"
- [Arabic/Urdu typing](feedback-arabic-urdu-typing.md) — may type authentic native Arabic/Urdu copy; no machine translation; Quran/hadith from verified_texts
- [Social posts: locked vs free](feedback-social-post-locked-vs-free.md) — lock only content, logo, real product, footer, colours; vet line only on product posts
- [Design: light, no glass](feedback-design-glass-light.md) — general default: never dark, no glass; BUT a brand with its own established look keeps it (Choppers black+gold)
- [Text readability gate](feedback-text-readability.md) — size floor 22–24 px, deep colours, WCAG 4.5/3:1 check, 360 px phone preview
- [Cool white, not warm](feedback-cool-white-not-warm.md) — scenes cool + white (no beige/cream/warm wood); cool-grade.cjs fixes one free
- [Layout alignment](feedback-layout-alignment.md) — think first, rule of thirds, shared left edge, minimal, one light source, crop policy
- [Presentation ideas](feedback-presentation-ideas.md) — 9 patterns from his inspiration posts, one per prompt, rotated
- [Social design system](social-design-system.md) — DESIGN.md per client in KB; logo A top-left (info) / B top-centre (greetings); AI does layout
- [Product staging variety](feedback-product-presentation-variety.md) — new staging every post; podium ≤1 in 6; UGC on hold
- [Real poultry backgrounds](feedback-poultry-real-backgrounds.md) — poultry clients: photo-real scene per product topic; show free wireframes first

## Image production
- [Image model choice](image-model-choice.md) — Nano Banana Pro until credits end, then GPT Image 2.5 Sunburst; FLUX 3 provider needs BFL key
- [Hero pack + label swap](hero-pack-label-swap.md) — AI makes the pack, code warps the REAL label on; resvg needs fontkit outlines for Inter
- [Hero packshot: AI + real label + Topaz](hero-packshot-ai-topaz.md) — REEFCO heroes: GPT pack → Topaz 2x → real label → Topaz cut-out
- [Pack edges + props check](feedback-pack-edges-and-props.md) — zoom 100% on pack edges; never paste real pack over AI pack; props match form
- [resvg RTL bidi bug](resvg-rtl-bidi.md) — Urdu/Arabic punctuation/digits flip on LTR base; RLE…PDF wrap; REEFCO Arabic needs re-check
- [Graphics Studio project](graphics-studio-project.md) — local app "Graphic Studio by Shoaib": batch, prompt contract, quality gate, photoshoot
- [Graphics Studio MCP server](graphics-studio-mcp-server.md) — local stdio server in graphics-studio/mcp; `--conditions=react-server`; he runs `claude mcp add`
- [MCP sync standing rule](graphics-studio-mcp-sync-feedback.md) — every new graphics-studio feature ships with a matching MCP tool

## Clients
- [Tad Pharma KB](tadpharma-knowledge-base.md) — 17 products approved; "increases FCR" rule; 2026-10-21 push, "dealer" post went out; check stale Meta drafts
- [Meezab KB](meezab-knowledge-base.md) — approved KB + Tad-style social; Jummah on its own Friday; Friday-1 + Egg Day held
- [Monthly client report](monthly-client-report.md) — Meezab + Tad: plan vs done, client extras, next plan saved in KB; Ads by Shoaib branding; builder in Meezab Z/Reports
- [REEFCO social posts](reefco-social-posts.md) — AR+EN series; Khaled wants DARK bg (clashes with rule), awaiting Shoaib; no brand kit
- [Aijaz Danish brands](aijaz-danish-brands.md) — 7 brands, per-brand times saved, Friday 10 AM Jummah; Fatima uploads; 29.png duplicate held
- [TONI&GUY Multan KB](toniandguy-multan-knowledge-base.md) — full KB; Multan rate lists official; IG audience mostly not Multan; open GBP fixes
- [Choppers KB](choppers-knowledge-base.md) — started 10-29 in E:...Choppers Salon; build ONLY from his material, planner data excluded
- [TONI&GUY reel logo](toniandguy-reel-logo.md) — Reels/Stories always TG monogram top-right; IMG_6257 Story on hold ("ruk jao")
- [Own KBs: Ads by Shoaib + Personal](feedback-own-knowledge-base.md) — keep building his own KB projects as work happens
