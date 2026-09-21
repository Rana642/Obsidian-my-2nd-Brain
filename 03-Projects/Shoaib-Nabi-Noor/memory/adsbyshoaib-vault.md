---
name: adsbyshoaib-vault
description: "Zero-knowledge password vault in the dashboard for client credentials — client-side AES-256, master password + recovery key"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-02T08:12:18.593Z
---

Built 2026-08-31 (commit 2fae164). Shoaib needed a secure place for many
clients' account passwords (Google Ads, Meta, Gmail, etc.) + backup codes,
recovery emails/phones — a NordPass-style vault **inside his portal**, tied
to client records. He explicitly chose the zero-knowledge in-portal option
over Bitwarden.

**Zero-knowledge design (the ONLY responsible way — never store client
secrets plaintext/server-decryptable):** all crypto is browser-side (Web
Crypto, no deps — `lib/vault-crypto.ts`). A random **data key (DK)**
encrypts every entry with AES-256-GCM. DK is wrapped twice: by a PBKDF2-
SHA256 (600k iters) key derived from the **master password**, and by a
one-time **recovery key** shown once at setup. Supabase stores ONLY
ciphertext + wrapped keys + salt — the server (and Claude) can never
decrypt. Changing the master password just re-wraps DK. Verified
end-to-end: round-trip works, wrong password/recovery rejected, ciphertext
never contains the plaintext.

**Tables:** `vault_meta` (single row id=1: salt, iterations, wrapped_dk +
iv, wrapped_dk_recovery + iv) and `vault_entries` (client_id nullable FK,
title, service, ciphertext, iv — title/service/client are the ONLY
plaintext, for listing). RLS on. Migration in
`supabase/dashboard-schema.sql` (already run live 2026-08-31).

**UX (`components/dashboard/VaultApp.tsx`, page `/dashboard/vault`, sidebar
"Vault" KeyRound):** first visit → set up master password → save recovery
key (once). Then unlock with master password (or recovery key → set new
master). Entry modal captures username, password (reveal + copy + generator),
2FA/TOTP, backup codes, recovery email/phone, security Q&A, url, notes.
Search, add/edit/delete, each linked to a client. **Auto-locks after 10 min
idle** (clears the in-memory data key). Actions in
`lib/dashboard/actions/vault.ts` only ever store/return ciphertext.

**Regenerate recovery key (commit b84bbe5, 2026-09-01):** an unlocked vault
now has a **"Recovery key"** button (header, next to Add/Lock) that mints a
fresh recovery key — `rewrapRecovery(dkRaw)` in vault-crypto re-wraps the
existing DK under a new random recovery key (client-side), `updateVaultRecovery`
persists only the new wrapped_dk_recovery, and the key is shown once in a
modal. The old recovery key stops working; master password + all secrets are
unaffected (DK never changes). Needs an active unlock (dkRaw in memory) — not
available right after first setup. Added because Shoaib set up the vault but
never saved the setup-time recovery key. Verified in Node: new key unlocks +
reads pre-rotation secrets, old key rejected, master password still works.

**Critical for Shoaib:** master password AND recovery key both matter —
lose both and the data is unrecoverable by design (server can't reset it).
Recovery key is shown ONCE at setup (or when regenerated) — save it then.
See [[adsbyshoaib-dashboard]].
