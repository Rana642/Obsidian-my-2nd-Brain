---
name: adsbyshoaib-client-portal
description: "Client portal /portal (v1 2026-09-26, ea05c6f): invites, client role, write-only sealed credential submissions, vault inbox, requests; planner/reports are the next phases"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-30T08:00:24.005Z
---

**Why it exists:** Shoaib's idea (2026-09-26) was to give clients an
account instead of one-off links, so the portal grows into a client
portal with the planner and reports later. The agreed order:
1. Foundation: roles (done in cf1be56, see [[adsbyshoaib-security-hardening]]).
2. Portal v1 with credentials. **Built in ea05c6f.**
3. Planner view (read-only posts calendar + approve/comment). Not started.
4. Reports (ads/social performance, invoices/agreements). Not started.

Language: English. Status shown to clients: Requested / Received / Secured.

**Access model:**
- A portal user is a Supabase login with `app_metadata { role: "client",
  client_id }`. The role is written ONLY by the dashboard invite
  (`lib/dashboard/actions/portal.ts`, service role).
- `lib/dashboard/roles.ts` has `isAdmin` and `portalClientId`.
- `proxy.ts` `AUTH_AREAS` guards `/dashboard` (admin) and `/portal`
  (client). Public paths: `/portal/login`, `/portal/forgot`,
  `/portal/welcome`.
- `app/portal/(app)/layout.tsx` re-checks via `requirePortalUser`
  (`lib/portal/auth.ts`). Every portal query uses the session's
  `clientId`, never a browser-sent id.
- Sign-ups are OFF. Invites use `db.auth.admin.generateLink`: type
  `invite` for a new email, `recovery` for an existing portal login.

**Emails:**
- Invite and reset emails go through Resend (`portalInviteEmail`,
  `portalResetEmail`), not Supabase SMTP.
- Links are `siteUrl/portal/welcome?token=<hashed_token>&type=invite|recovery`.
- The token is redeemed ONLY on form submit (`completePortalWelcome` →
  `verifyOtp` → `updateUser({password})`), so link-prefetching scanners
  can't burn it. No Supabase redirect-URL config is needed.
- Supabase's email OTP expiry is probably 1 h by default. If clients
  report expired links, suggest raising "Email OTP Expiration" in Auth
  settings (max 24 h), or use Resend.
- `requestPortalPasswordReset` is rate-limited (per IP, and per SHA-256
  of the email) and gives the same answer whether or not the email has
  access.

**Dashboard pieces:**
- `components/dashboard/PortalAccess.tsx` on a client's page: invite,
  resend, remove (remove deletes the auth user). Data comes from
  `listPortalMembers` in `lib/dashboard/portal-users.ts`.

**Credentials, write-only and end-to-end encrypted:**
- The vault gets an RSA-OAEP 3072 keypair: `vault_meta.public_key`
  (plaintext), and `wrapped_private_key` + iv, wrapped by the data key.
- The keypair is minted on the first unlock after the portal shipped
  (`VaultApp` `preparePortal`). `setVaultKeypair` only writes if
  `public_key` is null, so a key can never be replaced.
- The portal's "Send accounts" form (`/portal/send?project=<id|general>`,
  `components/portal/SendAccountsForm.tsx`) reuses `AccountCard`
  (`hideTitle`). It seals `{ v: 1, accounts }` with `sealForVault` in the
  browser. `submitAccounts` (`lib/portal/accounts.ts`) stores
  `vault_submissions` with plaintext `platforms` (request keys) only,
  marks matching `vault_requests` fulfilled, and emails Shoaib the names
  only.
- Vault inbox: opened with `openVaultSeal`, then `sanitizeSubmitted`
  strips links, history and flags. "Review and save" opens `EntryEditor`
  with `initialCards`. After save, `markSubmissionImported` wipes the
  ciphertext (status becomes "imported", shown as "Secured").
- Requests: `RequestModal` (toolbar "Request accounts", or a client
  group's "Request" chip) works with request keys, `master_gmail` or a
  platform id (`lib/vault-platforms.ts` `requestKeyOf` /
  `requestKeyLabel`).
- `lib/portal/status.ts` builds the client's status sections.

**Verified:**
- tsx crypto tests of the seal: opens after a master-password change,
  a different key can't open it, and tampering is detected.
- Signed-out routing (307 to the right login, noindex), and the forgot
  and welcome forms with a fake token.
- Harness: send form → POST body contains no plaintext → the vault opens
  it → the import editor is prefilled correctly.

**NOT verified (Claude can't create logins):** a real invite, a real
portal sign-in, and the real inbox flow. Shoaib should test with a
second email of his own.

**Login problem, 2026-09-30 (fixed in 50fc1f5).** Shoaib's report: "logout k baad login nai hota, account does not exist".
- The DB was fine and no SQL was missing: one portal login (Owner of Ahmed Jahanzaib Shah) with the correct app_metadata and row.
- **Cause:** the welcome form had no username field. The browser saved the password without the portal email and then autofilled the ADMIN login on /portal/login. The admin sign-in succeeded (last_sign_in 2 minutes after the portal one), was signed back out, and showed "doesn't have access to the client portal".
- **Fix:**
  - `portalWelcomeUrl(token, type, email)` now adds `&email=`, shown read-only with `autoComplete="username"`. Old links still work, just without the field.
  - The portal login names the admin case, and the dashboard login points portal logins to /portal/login.
- Auth admin list API returns `identities` empty for every user. That is normal and not a bug.

**SQL:** the "Client portal" section was run by Shoaib on 2026-09-26 and verified (tables + vault_meta key columns present). The vault keypair is minted on his next unlock. A real invite/login/inbox flow has NOT been tested yet.

See [[adsbyshoaib-vault]] and [[adsbyshoaib-dashboard]].

**Roles (6f58dfd, 2026-09-26).** Shoaib's rule: "pehly mai access don ga
owner ko phr owner aage unhi mai se access dy ga".
- **Top level:** `clients.portal_features` (text[]) is set by Shoaib and
  is exactly what Owners get. Features: intakes, planner, uploads,
  approvals, credentials, reports, team.
- **Owners delegate:** an Owner with "team" invites members from
  `/portal/team`, giving each a subset of the Owner's own features
  (never "team") and a project scope (`client_portal_users.project_ids`,
  null = all).
- **Recomputed on every request:** effective access is always
  `member.permissions ∩ client features` (`lib/portal/features.ts`
  `effectiveFeatures`). Role, permissions and projects come from
  `client_portal_users` each time, so no re-login is needed.
- **Code map:**
  - `lib/portal/auth.ts`: `getPortalUser` returns
    `{ role, features, projectIds }`, plus `can`, `canSeeProject` and
    `requirePortalFeature`.
  - `lib/portal/team.ts`: Owner actions. An Owner can't touch an Owner;
    invites are rate-limited.
  - `createPortalInvite` / `deletePortalLogin` in `portal-users.ts`: one
    invite path for the dashboard and the portal.
  - `components/portal/AccessPicker.tsx`: shared by both.
- **New portal pages:** `/portal/intakes` (the client's intake forms) and
  a nav.
- **Dashboard client page:** feature ticks, and a role/ticks/projects
  editor per person.
- **Before the roles SQL:** everyone reads as Owner with the default
  features (intakes, credentials).
- **SQL:** the "Client portal roles" section was run by Shoaib and verified on 2026-09-28.
- **Next, per his order:** portal planner / uploads / approvals screens
  (these features exist as ticks but show "Coming soon"), then vault
  Phase 2/3.

**Portal planner and uploads (2026-09-26).** Shoaib: "approval ka chakkar
na ho, wo approved hi upload keryn gy". So there is NO approval workflow,
and the "approvals" feature was removed.
- **Page:** `/portal/planner` (feature "planner" and/or "uploads").
  - Project chips, limited to the member's projects.
  - "Coming up" / "Posted" lists with R2 thumbnails. Failed posts are
    hidden from clients.
  - Someone with uploads but no planner sees only their team's uploads.
- **Upload flow:** `components/portal/PlannerUploader.tsx` gets a URL from
  `/api/portal/upload-url` (images only, JPG/PNG/WebP, 25 MB, rate
  limit), does a presigned PUT into `social/<project>/`, then calls
  `createPortalUploads` in `lib/portal/planner.ts`.
  - Result: normal `pending_caption` scheduled_posts on the chosen date
    (date required, default tomorrow PKT), with `uploaded_by_email` and
    `client_note`.
  - The key prefix is checked, and Shoaib gets an email.
- **Why this is safe:** nothing publishes until a caption is set, through
  the existing Planner/MCP flow.
- **Removing uploads:** clients can remove their own uploads only while
  still `pending_caption`.
- **Why the date is required:** the dashboard PlannerCalendar skips
  undated posts entirely.
- **Dashboard side:** `PlannerCalendar` shows a "From client" badge plus
  the note.
- **CSP:** proxy.ts img-src adds the R2 origin for `/portal/planner` only.
- **Local testing:** this PC's `.env.local` has no `S3_*` vars, so real
  uploads can't be tested here. The auth, validation and UI were tested.
- **SQL:** `scheduled_posts.uploaded_by_email` and `client_note` were run by Shoaib and verified on 2026-09-28.
