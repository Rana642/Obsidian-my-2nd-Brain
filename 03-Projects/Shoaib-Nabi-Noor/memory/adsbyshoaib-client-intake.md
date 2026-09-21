---
name: adsbyshoaib-client-intake
description: On-demand client intake forms + S3-compatible (Cloudflare R2) file uploads on the dashboard; foundation for a future client portal
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-08-31T11:07:37.668Z
---

Shipped 2026-08-28 (commit ea4a971). Shoaib wanted to collect client info
(business name, address, emails, competitors, brand assets) to set up their
social accounts — sent on demand via link/WhatsApp/email, saved to DB, and
processed from his dashboard. Framed as **phase 1** of a future full client
login portal (this data is the portal's foundation).

**Distinct from the agreement-triggered `onboarding_intakes`** — those
auto-create when an agreement is signed. `client_intakes` are created by
hand for ANY client, anytime.

**Editable until locked (2026-08-28):** submitting is NOT a dead end — the
public form pre-fills with the client's existing answers (`initial` prop,
`defaultValue` on plain inputs + seeded state for the widgets), the "thanks"
screen offers "Edit my answers", and re-submitting is allowed until Shoaib
**locks** it. New `client_intakes.locked boolean` + `setIntakeLocked` action
+ a Lock/Unlock toggle on the dashboard detail page; a locked intake shows a
read-only "closed" message on the public link. Also fixed an accidental
auto-submit two ways: (1) an `onKeyDown` guard blocks Enter (except in
textareas); (2) the real culprit — the "Next/Review" (type=button) and the
old "Submit" (type=submit) buttons rendered at the same tree position, so
React reused the DOM node and a real click flipping it button→submit
mid-event fired a native form submit (repro'd only with real pointer
clicks, not JS `.click()`). Fixed by giving the two buttons distinct
`key`s (separate nodes), making Submit `type="button"` that submits
programmatically via onClick, and setting the form's `onSubmit` to
`preventDefault` so native submit can never fire. **Lesson: a
conditionally-swapped button in a form where one branch is type=submit can
auto-submit on the click that triggers the swap — use distinct keys +
type=button + manual submit.**

**Form is a 5-step wizard** (`IntakeForm.tsx`, rebuilt 2026-08-28) — the
5th step is a **read-only Review** (snapshot via a form ref + state) with a
per-section "Edit" jump-back; Submit lives there since submitting locks the
form. Step 1 was also refined per Shoaib: "About you" = ONLY the person's
name + role; email/phone/WhatsApp/website/address all belong to "About your
business" (they're the business's, not personal). Original steps —
progress bar + Back/Next, all steps in one form (hidden via CSS so FormData
captures everything), so a client gives everything in one pass and intakes
aren't re-collected: (1) Contact & Company — **split into two clearly-headed
cards, "About you" (person: name, role, email, phone, WhatsApp) vs "About
your business" (name, website, address)**, because mixing them confused
clients; (2) Hours & Locations — day multi-select, open/close time,
service-area tag input, landmark; (3) Brand & Audience — drag-drop upload
zones for logo vs media (tagged `kind`), brand-colour picker, ideal
customer, asset links; (4) Preferences — competitor URLs, platform picker,
handles, master Gmail. **Account logins are deliberately NOT collected**
(Shoaib's call) — access arranged separately, no credentials stored.
Stateful bits (days, areas, colours, platforms, assets, whatsapp) are
appended to FormData on submit; plain inputs captured by name.

**Shape:** new `client_intakes` table (business_name required, client_id
nullable FK, access_token, status pending/submitted, + text fields:
contact_name/emails/phone, address, website, social_handles, competitors,
target_audience, brand_notes, account_access_notes, brand_asset_links,
additional_notes, and `assets jsonb` holding uploaded-file metadata
`[{key,name,size,type}]`). Dashboard: `/dashboard/intakes` (list) + `/new`
(create — pick a client or type a name; a selected client's saved
`client_projects` show as one-click chips that set the business name, so a
multi-project client like Ahmed → Tad Pharma / Meezab gets a per-project
intake in one click) + `/[id]` (share link + WhatsApp/email
+ view submission + presigned download of assets). Public form
`/intake/[token]` (`IntakeForm.tsx`). Actions in
`lib/dashboard/actions/intakes.ts` (create/delete authed; getByToken/submit
public). Sidebar gained an "Intakes" item (FolderInput icon).

**File uploads — provider-agnostic S3 (`lib/storage.ts`):** files go
straight from the browser to object storage via **presigned PUT URLs**
(API route `POST /api/intake/[token]/upload-url`, token-gated), so creds
never reach the client and uploads skip Vercel's serverless body limit.
Dashboard downloads via presigned GET generated server-side. Env-driven,
so R2 / Backblaze B2 / Storj all work by changing `S3_*` vars only:
`S3_ENDPOINT`, `S3_REGION` (auto for R2), `S3_ACCESS_KEY_ID`,
`S3_SECRET_ACCESS_KEY`, `S3_BUCKET`. **`isStorageConfigured` gates it —
until the env vars are set the form falls back to asset LINKS and the whole
thing still works.** Deps added: `@aws-sdk/client-s3` +
`@aws-sdk/s3-request-presigner`.

**Chosen storage: Cloudflare R2** (Shoaib's call, confirmed 2026-08-28) —
forever-free 10GB + **zero egress fees** (beats B2/Storj for serving assets
that get downloaded repeatedly). His R2 account id / S3 endpoint:
`https://38dcc5b41d188d82af5c77d62632eeb4.r2.cloudflarestorage.com`.

**Two manual steps Shoaib must do for it to go live** (I can't DDL or
handle secrets): (1) run the `client_intakes` migration in Supabase SQL
Editor (idempotent block in `supabase/dashboard-schema.sql`); (2) create
the R2 bucket + an Object-Read&Write API token, set a CORS policy allowing
PUT/GET from the site origins, and put the `S3_*` vars in `.env.local` +
Vercel. **R2 CORS is required** or browser presigned PUT fails. Until (1),
creating an intake errors (table missing); until (2), file upload is
hidden and only links work. See [[adsbyshoaib-dashboard]] and
[[adsbyshoaib-verify-via-local-dev-server]].
