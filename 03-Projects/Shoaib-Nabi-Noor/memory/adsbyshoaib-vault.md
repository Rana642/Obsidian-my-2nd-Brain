---
name: adsbyshoaib-vault
description: "Zero-knowledge password vault at /dashboard/vault — v2 (2026-09-26): client→project filing, per-platform fields, 2-step/recovery, generator, security badges, tell-client email; Phase 2 (client credentials link) planned"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-26T08:29:56.506Z
---

Built 2026-08-31 (2fae164). It is a NordPass-style vault inside the portal
for clients' account credentials. Shoaib chose it over Bitwarden.

**Zero-knowledge crypto (unchanged, the only responsible way):**
- All crypto runs in the browser via Web Crypto (`lib/vault-crypto.ts`).
- A random data key (DK) encrypts every entry with AES-256-GCM.
- DK is wrapped twice: once by a key from the master password (PBKDF2
  600k), once by a recovery key that is shown only once.
- The server stores only ciphertext and the wrapped keys.
- `rewrapRecovery` regenerates the recovery key (b84bbe5).
- Losing both the master password and the recovery key means the data is
  unrecoverable, by design.

**v2 (1d6b67c, 2026-09-26), his requests:** "balance nai", client first,
then project-wise details, platform-specific fields, a generator for
changing passwords after taking over, and emailing the client after a
change.

- **Filing:** an entry is filed as client → project (`vault_entries.project_id`
  → `client_projects`), or as "Ads by Shoaib — my accounts"
  (`secret.own = true`, no client).
- **What stays plaintext:** only the client/project links.
  Title, platform and all fields are inside the ciphertext. The legacy
  `title`/`service` columns are written as `''`/null.
- **Server trust:** the server takes the client from the project, never
  from the browser's pair.
- **`lib/vault-platforms.ts`:** 15 platform definitions, each with its own
  fields.
  - `signIn` platforms get the 2-step + recovery section: authenticator
    method, device, backup codes, recovery email, phone and WhatsApp, and
    under "More" the secret key and security questions.
  - Platforms reached through another account (FB page, Meta Business,
    Google Ads, GA4, GBP, YouTube) have no password field.
- **Change tracking:** `securityIssues()` produces the list badges and
  drives the "Needs attention" filter. `trackPasswordChanges()` keeps old
  secret-field values (last 10) and sets `passwordChangedAt`.
- **Telling the client:** when a password changed after the client was
  last told (`passwordChangedAt` later than `clientNotifiedAt`), the client
  group shows "Tell client". It opens a preview.
  `sendPasswordChangeNotice` (Resend, `passwordChangeNoticeEmail`) lists
  account names only, never passwords. "Mark as told" covers WhatsApp.
- **Password tools:** `lib/password-generator.ts` uses rejection-sampled
  crypto RNG and guarantees one character from each chosen class.
  `passwordStrength` caps passwords under 16 characters with 1–2
  character classes at "Fair".
- **UI files:** `components/dashboard/VaultApp.tsx` plus
  `components/dashboard/vault/*`.
  - Adding many accounts uses an in-page editor, not a route, so the
    in-memory key survives.
  - Field action buttons sit inside the inputs so every field in a row is
    the same width.
- **Fixes:** two-click delete; the clipboard clears 30 s after a copy; a
  "Master password" change button; recovery reset needs 12+ characters and
  a confirm; unlock fetches fresh meta and entries; entries that fail to
  decrypt are never opened for editing.
- **Verified:** tsx logic and crypto tests, a browser harness, and a check
  that the POST body held no plaintext.

**Master Gmails (739885d, 2026-09-26)** — his note: "Master Gmails bhi
hoti hain har client ki".
- **Marking and checks:** a Gmail entry can carry `secret.master`. It is
  pinned first with a crown and "Master" badge. It has a stricter check:
  2-step, backup codes, and BOTH a recovery email and a recovery phone.
- **Linking:** `FieldDef.linkable` sign-in fields can link to a saved
  Gmail of the same owner (`secret.links[fieldId] = entryId`).
  - A linked field shows the Gmail's current address, read-only.
  - The field keeps a copy of that address as of the last save.
  - On save, links pointing outside the owner are dropped.
- **Ids for new entries:** new entries get `crypto.randomUUID()` ids
  (`createVaultEntries` accepts the id), so a master Gmail and the
  accounts using it can be linked in the same form before saving.
- **Where it shows:** the master card lists the accounts that sign in
  with it, delete warns about those dependents, and list rows show "via
  master Gmail".

The 2026-09-26 SQL (letters + vault `project_id`) WAS run by Shoaib and
verified via the PostgREST spec.

The vault had 0 entries at the time of the v2 switch, so no legacy data
was converted.

**DB access:** this PC's `.env.local` has no `SUPABASE_DB_URL`. On
2026-09-26 Shoaib pasted the full pooler URI, password included, into
chat. It was NOT used, and he was asked to reset the DB password. To run
SQL himself later, he should add `SUPABASE_DB_URL` to `.env.local`
himself (never through chat); `scripts/run-sql.mjs` then works.

**Phase 2 (agreed, not built):** a client credentials-request link.
- Encryption: the client's browser encrypts to a vault public key
  (RSA/ECDH keypair, private key wrapped by DK), so even the server can't
  read submissions.
- Link rules: write-only, 7-day expiry, single submit, rate-limited, and
  an email notification with no secrets.
- Review: an "Inbox" in the unlocked vault to review, complete (2FA,
  backup codes) and import; the submission is deleted after import.

See [[adsbyshoaib-dashboard]] and [[shoaib-confirm-ux-before-building]].

**Login vs access, Phase 1 (2026-09-26):** pushed as da9b63f (build passed).

What it adds:
- Each entry holds a login (`loginHeld`), an access grant (`access`:
  via / grantedTo / role / status / verifiedAt), or both.
- `PlatformDef` now has:
  - `login`: required / recommended / optional / none
  - `access` routes, with roles and client steps
  - `starts`
  - `guidance`
  - `FieldDef.login` for credential-only fields
- Platform splits: GA4 / `google_tag_manager` / `search_console`, and
  LinkedIn profile (`linkedin`) / `linkedin_page`.
- Migration: fields a platform no longer has move to custom fields, and
  old login-less entries become access with status "client_added".

Where it shows:
- Settings → "My access accounts" (`lib/access-identities.ts`, saved in
  `settings.access_identities`).
- New issues: `login_missing` (e.g. Instagram with access only),
  `access_unverified`, `nothing_held`.
- Portal cards show "Share the login / Give access" and step-by-step
  instructions with his identity values; guidance is hidden in the portal.

Checks so far: 48 tsx logic checks pass, plus a harness check (removed).

**Left to do:**
1. Run the build, then push.
2. Shoaib runs the SQL `alter table settings add column if not exists access_identities jsonb not null default '{}'::jsonb;`
   and fills in Settings.
3. Phase 2: project setup board (A = new business / B = existing
   accounts, missing / improve).
4. Phase 3: share with client + handover pack + consent +
   remove-access checklist + archive (purge secrets, keep a portfolio
   record). The planned default is a button, not an automatic purge.

**API Vault inside the Password Vault (953f1dd, 2026-10-20)** — Shoaib: "api vault ko password vault mai rakh do filhal master password k ander".
- The unlocked vault has tabs **Accounts / API Vault** (`VaultTabs` in VaultApp, `components/dashboard/vault/ApiVaultPanel.tsx`). The API list loads only after unlock (client-side `listCredentials()`) and is dropped on lock.
- The keys are still server-encrypted (`API_VAULT_ENCRYPTION_KEY`), NOT by the master password, because crons/MCP/integrations decrypt them on the server. So the master password is a screen gate; the server actions are still guarded only by the admin login.
- `/dashboard/api-vault` redirects to `/dashboard/vault?tab=api`; Settings is now just Business profile. "filhal" = may move again later.
