---
name: graphics-studio-project
description: "New internal tool 'Graphics Studio' at E:\Rana Shoaib\My Projects Website\graphics-studio — AI brand-graphics generator, separate repo/Vercel/Supabase from adsbyshoaib.com. Phase 1 (Foundation) built 2026-08-24."
metadata:
  type: project
---

**Renamed "Graphic Studio by Shoaib" (2026-08-25, commit `203d46e`)** — Shoaib
wants this tool to visibly carry the adsbyshoaib.com brand forward, not read
as a separate generic internal tool. `app/globals.css` now uses
adsbyshoaib.com's exact tokens (Cloud #fafafa, Ink #0f0f14, Citrus #eab308,
Cobalt #1e40af, Instrument Serif + Geist). Citrus is the primary button color
directly here (not the public site's 8%/2% budget rule) — matches how
adsbyshoaib.com's own dashboard already treats its primary buttons. Wordmark
pattern: lowercase serif italic "graphic studio by shoaib" with **only the
trailing period in Citrus** — never color the name text itself in Citrus,
it fails contrast on Cloud (this mistake was made and caught mid-edit before
committing). **If adsbyshoaib.com's own design tokens ever change, update
this repo's `app/globals.css` to match — they're meant to stay identical.**

Shoaib asked (2026-08-24) for an internal (not SaaS) tool that replaces hiring a
graphic designer + copywriter: pick a client brand, describe what's needed, get
back on-brand, multi-size, ready-to-post graphics via AI image APIs (Nano Banana
Pro / Gemini and GPT Image / OpenAI), without the brand drifting between
generations. He researched this himself extensively (pasted a long Google AI
Mode conversation) before asking me to plan and build it.

**Location: `E:\Rana Shoaib\My Projects Website\graphics-studio`** — a brand
new, separate Next.js repo, deliberately isolated from adsbyshoaib.com (its
own future GitHub repo, own Vercel project under a different Vercel account,
own Supabase project) so it never competes with the main site's usage/quotas.
Full plan lives in the repo at `docs/PLAN.md` (also the source of truth — read
that before continuing this project in a future session, not this memory).

**Key architecture decisions** (see docs/PLAN.md for full reasoning):
- Full Next.js web app was chosen over a Claude-Code-only Skill — Shoaib
  explicitly wants a real brand-switcher UI usable outside a Claude Code
  session, with each brand's identity strictly scoped per generation.
- Both Nano Banana Pro and GPT Image will be supported (model-agnostic
  provider interface), latest versions of each — his choice, not a
  recommendation to pick one.
- **Two-Track design** (my addition, improves on the pasted research):
  "Creative Track" = full AI generation for regular social/marketing content;
  "Asset-Locked Track" = for real estate/product photos where the real photo
  must never be AI-regenerated — composited in via `sharp` (deterministic
  crop, not AI) with all text rendered as crisp SVG overlay, not AI-drawn.
  This was in direct response to Shoaib's own concern about AI altering a
  real property photo.
- Auto-posting to Meta/LinkedIn is an explicitly deferred future feature —
  the `generations` table schema was designed now so that's a pure addition
  later, not a rework.

**Status as of 2026-08-24: Phase 1 (Foundation) complete AND fully verified
end-to-end with real infrastructure.** Committed locally (commits `85c5f18`
foundation + a follow-up RLS fix — not yet pushed anywhere, no GitHub remote
configured, since Shoaib hasn't provided one). Built: proxy.ts auth guard
(protects everything except /login, unlike adsbyshoaib.com's dashboard-only
middleware, since this whole app IS the internal tool), service-role Supabase
client (`lib/supabase/db.ts`), Brand Vault CRUD (create/edit/delete brand
kits: logo URL, hex colors, font, voice notes), `supabase-schema.sql` (brands
+ generations tables, RLS enabled with zero policies).

**Real Supabase project is live**: project ref `cdbrmhmkgjdarvouceey`
("Graphic Studtio" in the Supabase dashboard — Shoaib's own typo in the
project name, harmless, not worth renaming). Storage buckets `logos` and
`generations` created (public read, MIME-restricted to images, 5MB/20MB
limits respectively). `.env.local` populated with real keys — note Supabase's
newer key format (`sb_publishable_...` / `sb_secret_...`) is what this
project uses, not the legacy JWT anon/service_role keys, though both formats
work with `@supabase/ssr`.

**Caught and fixed a real bug during setup**: the original `supabase-schema.sql`
had RLS *disabled* with a comment claiming that was "default deny" — backwards.
RLS off is default-*allow* for the anon key (which is public, ships in the
browser bundle). Supabase's own SQL editor flagged it. Fixed by adding
`alter table ... enable row level security` (zero policies = default-deny for
anon/authenticated, service-role still bypasses). Verified via direct REST API
calls with both keys: anon key correctly sees `[]` on a table with a real row
in it; service-role key sees the row. **If this schema file is ever
regenerated or copied elsewhere, keep the RLS-enabled lines — don't
reintroduce the "RLS off" mistake.**

**End-to-end UI test passed**: created a temporary Supabase Auth test user via
the Admin API (created and deleted within this session — not a standing
account), signed in through the actual login form, created a real brand
through the actual Brand Vault form (**"Al Mannan Builders" — one of
Shoaib's real clients — deliberately left in the database as genuine starter
data rather than deleted as test junk**), confirmed it lists correctly and
the edit page reads back the exact saved values. Auth guard, service-role
writes, RLS bypass, and the create/read cycle are all confirmed working
against real infrastructure, not just a clean build.

**Known low-priority annoyance**: in the sandboxed preview browser tool used
this session, `computer.left_click` on a ref sometimes silently failed to
trigger a real button click (no server-side POST logged) — worked fine on
retry once, failed again on a fresh page. Root cause unconfirmed (possibly
Turbopack Fast Refresh remounting under the click, possibly a tool-side ref
resolution issue) — not a bug in the app itself (verified by driving the
exact same form via `element.requestSubmit()` in JS, which worked
immediately). If manual UI testing in a future session hits a click that
"does nothing," don't assume the app is broken — retry once, or drive it via
JS as a fallback before concluding there's a real bug.

**Next phase (2 — Creative Track, the core value)**: placement specs table,
`lib/image-providers/{nanoBanana,gptImage}.ts`, a copy-generation step, the
`/api/generate` parallel multi-size pipeline, and a results gallery. Don't
start Phase 3 (Asset-Locked) or Phase 4 (rare identity assets) before Phase 2
is solid, per the plan's stated build order.

**Explicitly reinforced by Shoaib (2026-08-24), don't let this slip**: he
wants image *enhancement* quality to match or beat Higgsfield's — Higgsfield's
own research (from the pasted conversation) showed they back onto Topaz Labs
for upscaling + a proprietary skin/face enhancer. The plan's docs/PLAN.md
already deferred an upscaler pipeline to "Phase 5, not built now" using
open-source equivalents via Fal.ai/Replicate (Real-ESRGAN/AuraSR/SUPIR for
upscaling, CodeFormer/GFPGAN for faces) reachable behind the same
`lib/image-providers/` abstraction. That reasoning still holds (v1 should
ship without it, add when actually needed) — but confirm with Shoaib before
considering Phase 2 "done enough to stop" if raw model output looks visibly
softer than Higgsfield's despite the plan saying enhancement is a later
phase. He's flagged this twice now (once in the original research paste,
once as an explicit reminder) — treat it as a real requirement to eventually
build, not a nice-to-have that can be quietly dropped.

**Blocked for Phase 2 start**: needs Google AI Studio API key (Nano Banana
Pro) and OpenAI API key (GPT Image) — both env var slots already exist in
`.env.local`, empty. Waiting on Shoaib to provide them; picked up again
"kal" (tomorrow, relative to 2026-08-24 — i.e. 2026-08-25 or whenever this
next resumes).

**Update 2026-08-25: Phase 2 (Creative Track) built and committed** (`70fb5f2`).
Both API keys obtained and wired into `.env.local` — Gemini key
`AIzaSyA18p7qxrYCgOAgKOI9j3nkEsI7i2HZu04` (project ref `cdbrmhmkgjdarvouceey`
matches the Supabase project's — coincidence, different services), OpenAI
key starting `sk-proj-JoRF...`. **Confirmed live model IDs** (don't
re-guess these, verify via `/v1beta/models` or `/v1/models` again if they
ever 404):
- Nano Banana family: `gemini-3-pro-image` (Pro/premium), `gemini-3.1-flash-image`
  (Nano Banana 2/standard), `gemini-2.5-flash-image` (original/draft).
- GPT Image family: `gpt-image-2` (premium/standard via `quality` param),
  `gpt-image-1-mini` (draft).
- Copywriting text model: `gemini-3.6-flash` for the Gemini path
  (**`gemini-2.5-flash` is deprecated for new accounts — 404s**, learned
  this the hard way mid-build); `gpt-5.6-luna` for the OpenAI path
  (confirmed to exist on his account, described there as "optimized for
  cost-sensitive work").

**Both platforms are on a prepaid-credits billing model, NOT the
postpaid/$300-free-trial system the original pasted research described**
— that research was wrong/outdated for his account. Confirmed by direct
testing: Google AI Studio's "Buy credits" dialog (Cloud Prepay) and
OpenAI's "Add credits" both showed $0.00 balance with no free trial
offered, real card required, $25 minimum on the Google side. **Billing
account existing is not enough on the Google side — the specific GCP
project owning the API key must be explicitly linked to a billing account
AND have credits purchased; these are two separate steps people (including
Shoaib) can easily complete only one of.**

**Full pipeline verified end-to-end except actual image generation**: auth
guard, brand lookup, prompt construction, and both providers' copywriting
calls all confirmed working against real keys — each fails cleanly with
the real provider error (insufficient credits) rather than crashing. A
real bug was caught and fixed here too: the copywriting call wasn't inside
the route handler's try/catch, so its failure was an uncaught exception →
Next's non-JSON default error page → the client's `res.json()` throwing →
a misleading "Network error" message that had nothing to do with the real
cause. The whole `POST` handler in `app/api/generate/route.ts` is now
wrapped in one try/catch specifically so this class of bug can't recur —
**if a future change to that route removes the outer try/catch, put it
back**.

**Reference images added (2026-08-25, commit `65112d8`)** — per-brand upload
of existing client posts/graphics (`brand_references` table + Storage under
the existing "logos" bucket's `{brandId}/references/` path, no new bucket
needed), shown as inlineData parts to Nano Banana ahead of the text prompt
for style-consistent generation. **GPT Image's basic endpoint can't use
these — it's text-only** — `gptImage.ts` explicitly ignores
`referenceImages` rather than silently dropping them; don't "fix" that
without switching to OpenAI's `/v1/images/edits` endpoint instead, a
different integration. Verified end-to-end with a real uploaded file
(upload → DB row + Storage object + public URL all confirmed → gallery
display → delete → both DB row and Storage object confirmed gone).

**Process lesson from this feature (important, will recur)**: updated
`supabase-schema.sql` in the repo but did NOT run the new table's DDL
against the live Supabase project — Shoaib hit a real 500
(`PGRST205`, "table not found in schema cache") testing it himself before
this was caught. **A schema file change is not done until Shoaib has
actually run it in the Supabase SQL Editor — committing the .sql file to
git does nothing to the live database.** Every future schema change to
this project needs the same explicit "here's the SQL, please run it, tell
me when done" step before the feature can be considered shipped, the same
way it's already done for adsbyshoaib.com's dashboard schema.

**GPT Image reference-image support added (2026-08-25, commit `1757c38`)** —
Shoaib wanted full parity, not Nano-Banana-only. `gptImage.ts` now routes to
OpenAI's `/v1/images/edits` (multipart/form-data, each reference as its own
`image[]` part) whenever `referenceImages` is non-empty, falling back to the
plain text-only `/v1/images/generations` otherwise. Verified the multipart
shape directly against OpenAI's real API via curl (got their billing error,
not a validation error) before wiring it in. **Both providers now use
reference images** when a brand has any uploaded.

**Full brand context added (2026-08-25, commit `d42e219`)** — `brands` table
gained `about`, `services`, `contact_phone`, `contact_email`, `website_url`,
`instagram_handle`, `facebook_handle`, `linkedin_handle`, `tiktok_handle`.
Feeds the copywriter (grounds hooks/CTAs in what the brand actually does,
not just voice_notes) and gets printed as a small footer band on generated
graphics via a new `buildFooterLine()` in `app/api/generate/route.ts` —
**only whichever fields are actually filled in, nothing invented for a
brand that leaves them blank.**

**Stated future direction, not built yet**: Shoaib explicitly wants this
data as groundwork for a **social media content calendar** — generating the
right post type (brand awareness, product, event, etc.) per brand's actual
needs. Don't start building that calendar unprompted; it needs its own
planning pass (content-type templates, scheduling, maybe recurring posts)
when he actually asks for it. This memory entry exists so a future session
knows *why* this brand-context data was added even before the calendar
exists.

**Caught my own mistake while verifying this feature**: used a plausible
but fabricated phone/email/website/Instagram handle for the real "Al Mannan
Builders" brand to test the save flow, then realized this violates the
established no-fabrication rule harder than usual here — these values feed
straight into the image-generation footer, so fake contact info could have
ended up printed on an actual client graphic. Cleared them immediately
after confirming the save mechanism worked. **Never leave placeholder/test
contact or social data on a real brand record in this app — always clear
it before ending the turn, not just before telling Shoaib it's done.**

**Next**: once Shoaib actually adds credits on both platforms, do one real
generation test end-to-end (image should land in Supabase Storage +
appear in the results gallery) before considering Phase 2 done. Then
Phase 3 (Asset-Locked track for real estate/product photos) per
`docs/PLAN.md`. Enhancer/upscaler quality (Higgsfield-parity, see the
reminder above) is still Phase 5/deferred but must not be dropped.

**Costing views added (2026-08-25, commit `bf0b206`)** — `/costs` (all-brands
overview) and `/brands/[id]/costs` (per-generation breakdown + a live
sale-price/margin calculator), reading `generations.est_cost_usd` which
Phase 2 already records. **Deliberately did not build actual invoice
generation here** — Shoaib already has a complete invoicing system (clients,
catalog, quotations, tax, currency, gap-free numbering) in the
adsbyshoaib.com Supabase dashboard; duplicating it in this separate codebase
would contradict the whole reason these two apps are kept apart. The
calculator is read-only/unsaved and explicitly tells him to take the decided
price to that other dashboard. **If a future request asks to persist a sale
price or generate an invoice PDF here, push back and point to the existing
dashboard first — don't silently start building a second invoicing system
unless he's explicit that he wants it duplicated.**
