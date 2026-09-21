# Project Memory — Hotel Elegant Multan

- [Hotel Elegant Website Project](project_hotel_elegant.md) — Full Next.js 14 hotel booking website: stack, structure, key files, and deployment notes
- [Security & Admin Ops](security-and-admin-ops.md) — Pending security hardening, Supabase key-rotation plan (prod env caveat), admin_users RLS gotcha
- [Hotel Website Skill](hotel-website-skill.md) — Reusable ~/.claude/skills/hotel-website skill distilled from this build, for making future hotel sites
- [CI deploy broken, Hostinger auto-deploy works anyway](project_ci_deploy_broken.md) — GH Actions always fails at npm ci; verify deploys via live browser check, not Actions status.
- [Roman Urdu only](feedback_roman_urdu_only.md) — always chat in Roman Urdu with this user, never English/other languages.
- [Meta: direct API only, no MCP](feedback_meta_marketing_api_direct.md) — never use Meta Ads MCP connector; use Graph API directly with .env.local's META_ACCESS_TOKEN.
- [Cross-verify GA4+Meta+Google Ads together](feedback_cross_verify_tracking.md) — never call tracking broken/fixed from one source alone; check production env, not just local files.
- [Correct Google Ads account ID](reference_google_ads_account_id.md) — Hotel Elegant's real account is customer 6223250696; a same-named but unrelated/suspended account (6684011164) exists too, don't confuse them.
- [Google Ads must stay directly wired](feedback_gads_direct_connection_only.md) — never GTM or an indirect pipeline; direct gtag only, permanently.
- [GA4↔Google Ads cross-verify method](project_ga4_google_ads_cross_verify_method.md) — use gclid auto-tagging (session source/medium/campaign) to cross-check, not UTM params, not GA4-imported conversion actions.
