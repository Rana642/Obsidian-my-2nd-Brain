---
name: adsbyshoaib-security-hardening
description: "Security hardening pass — CSP/security headers, rate limiting, secret hygiene; what's covered and what's still on Shoaib"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-09-01T09:23:16.603Z
---

Done 2026-08-31 (commit da5ef76) after the password vault shipped, so an
XSS bug can't read decrypted vault secrets. Prompted by a web-research
threat review (CVE-2025-29927 Next middleware bypass, Supabase RLS/service-
role, OWASP, password-manager XSS).

**Security headers + CSP (`proxy.ts` middleware, all HTML routes):**
⚠️ CSP is now **nonce-free** — `script-src 'self' 'unsafe-inline' https:`
(commit 194da7a, 2026-09-01). **Do NOT reintroduce nonce + `strict-dynamic`**
— it took the whole live site blank (see the outage note below). The real
anti-exfiltration teeth are the tight **connect-src** and **img-src**
allowlists: even if an injected script runs, it can't fetch/beacon a
decrypted vault secret to an attacker host. connect-src allowlists
`*.supabase.co`, `*.r2.cloudflarestorage.com`, sanity, analytics
(GTM/GA/FB/Vercel); img-src is `'self' data: blob: https://cdn.sanity.io`
(no blanket https:). `'unsafe-eval'` is added ONLY in dev (Next HMR).
`/studio` is exempted (Sanity SPA needs a looser policy). Also HSTS,
X-Frame-Options DENY + frame-ancestors 'none', X-Content-Type-Options
nosniff, Referrer-Policy, Permissions-Policy. Matcher covers all routes; the
dashboard Supabase auth flow is preserved exactly and still only runs for
/dashboard. Verified against a **local production build** + live: home
renders, scripts execute, hydration + framer-motion reveals fire, dashboard
auth 307→login intact, /studio has no CSP, 0 real CSP violations.

**⚠️ CSP outage post-mortem (2026-09-01, fixed in 194da7a):** the original
nonce + `strict-dynamic` policy (da5ef76/e5d572d) took the live site BLANK.
Root cause: **most pages are statically prerendered**, so Next bakes their
`<script>` tags at build time with NO per-request nonce. `strict-dynamic`
then blocked every script (25+ violations) → no hydration → the
scroll-reveal (framer-motion `whileInView`) content stayed at opacity:0 →
blank. A per-request middleware nonce **cannot** be stamped onto static
HTML; making it work would need forcing the whole site dynamic (kills static
perf + inflates cost). **Lesson:** nonce/`strict-dynamic` CSP is
incompatible with statically-rendered Next pages — use a nonce-free policy
here. **Verification lesson:** dev-only CSP testing was a false positive;
ALWAYS verify a CSP change with a real `next build` + `next start` and check
served HTML for the actual policy before pushing. (Also: the in-app browser's
console buffer replays STALE CSP errors across navigations — a repeated
identical nonce means stale, not live; confirm via a fresh `fetch()` of the
header + DOM hydration check, not `read_console_messages`.)

**Rate limiting (`lib/rate-limit.ts` + `check_rate_limit()` Postgres fn):**
public endpoints — /api/contact (5/10min), /api/newsletter (5/10min),
/api/intake/[token]/upload-url (40/10min per ip+token) — throttled per IP
via an atomic DB counter shared across serverless instances. **Fails OPEN**
(limiter error never blocks real users — it's an abuse/cost brake, not an
auth gate). New `rate_limits` table + function in dashboard-schema.sql.

**Already solid (confirmed):** Next 16.3.2 is past the CVE-2025-29927 patch;
service-role key is server-only; RLS on every table; `.env.local` gitignored
and NO env file tracked in git; UUID tokens; vault zero-knowledge + 10-min
auto-lock; Vercel L3/L4 DDoS mitigation.

**Still on Shoaib (can't be done in code):** (1) enable 2FA + a strong
password — he logs into Supabase **via GitHub OAuth** ("Continue with
GitHub"), so the real gate is his **GitHub** account: 2FA belongs there; (2)
run `npm audit` periodically; (3) never paste secrets into client/logs; (4)
run `npm run backup:db` periodically (Free plan has NO auto-backups) and keep
a copy off-PC — see the backup note below.

**DB backups (commit 69f4cb4, 2026-09-01):** Supabase Free plan does NO
automatic backups, so a lost DB = permanent loss of clients/leads/quotations/
invoices/intakes + vault ciphertext. Schema is tracked in
supabase/dashboard-schema.sql; the DATA is not. `scripts/backup-db.mjs`
(`npm run backup:db`) is a zero-dep, READ-ONLY dump: reads the service-role
key from .env.local, pulls every table's rows via the REST API, writes one
timestamped JSON to `./backups/` (gitignored — holds real client data +
vault ciphertext, though vault rows are only encrypted blobs). Auto-discovers
tables via the PostgREST spec (found `contacts`/`subscribers` too, which
aren't in dashboard-schema.sql). Restore = re-apply the schema SQL, then
re-insert rows from the JSON. Shoaib should run it on a cadence and keep an
off-machine copy. Pro plan ($25/mo) would add daily backups + PITR instead.

**Pentest pass (commit e5d572d, 2026-08-31):** acting as pentester, found +
fixed 6 real loopholes — (1) open redirect via login `?next=` (now only
same-origin relative paths accepted in LoginForm); (2) rate-limit bypass via
spoofable leftmost X-Forwarded-For → `clientIp` now uses Vercel's trusted
`x-real-ip`; (3) CSP `img-src` dropped blanket `https:` (kept
self/data/blob/cdn.sanity.io) to close the image-beacon exfil channel an XSS
could use to leak a decrypted vault secret; (4) intake presigned uploads
bake the declared size into the signature (`ContentLength`) so a client
can't PUT more than the 50 MB cap — **verify with a real upload when
convenient, wasn't live-tested**; (5) vault master-password min 8→12; (6)
`generateNumber` moved to a plain internal module `lib/dashboard/numbering.ts`
so it's no longer an unauthenticated server action that could burn the
document-number sequence. Auth coverage audit: every mutating dashboard
action already calls assertAuthed (good). Everything committed + pushed;
rate_limits migration run live. **Still on Shoaib:** Supabase 2FA + strong
password; periodic `npm audit`. See [[adsbyshoaib-vault]] and
[[adsbyshoaib-dashboard]].
