---
name: security-and-admin-ops
description: "Pending security hardening, Supabase key-rotation plan, and the admin_users RLS gotcha for Hotel Elegant"
metadata: 
  node_type: memory
  type: project
  originSessionId: 9511844e-13c0-42ff-86af-800dc65bec3e
---

Security hardening for the Hotel Elegant site is **deferred until dev work completes** (user's call, decided 2026-07-17). Then the user will rotate/revoke the exposed `service_role` key + Supabase Personal Access Token (both were pasted in chat, so treat as compromised — rotate, don't just stop using).

**Pending code hardening (does NOT need the keys — can be done anytime):**
- Booking/contact forms have no CAPTCHA or rate-limiting → a bot can spam fake bookings that fill `availability_blocks` and make all rooms show as booked (availability DoS) + email spam. Highest-value fix.
- `next@14.2.13` has a known security vulnerability + `npm audit` shows 5 vulns (1 critical) → update Next.js + `npm audit fix`.
- No CSP / security headers configured in next.config.
- Booking notification email (app/actions/booking.ts) interpolates guest input into HTML unsanitized.

**Key-rotation caveat (flag before rotating so prod doesn't break):**
- PAT (`sbp_...`) is only used for direct API/migration ops → safe to revoke anytime, no live-site impact.
- `service_role` / anon keys are used by the deployed site too → rotating them requires updating BOTH `.env.local` AND the Hostinger production env vars, or booking/admin breaks.

**admin_users RLS gotcha:** Dashboard writes are gated by RLS `is_admin()`, which requires the logged-in admin's auth user id to exist in the `admin_users` table. Admin account: `shoaib@elegant-suite.com` (auth id 610f7d94-40bf-4fa9-b8f0-057e3c4af7d1). This table was empty, which silently blocked all dashboard edits from saving (the fix was inserting the account). Any NEW admin must also be added to `admin_users`. See [[project_hotel_elegant]].
