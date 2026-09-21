---
name: hotel-elegant-website
description: "Full Next.js 14 hotel booking website for Hotel Elegant Executive Suites, Multan — what it contains and how it was built"
metadata: 
  node_type: memory
  type: project
  originSessionId: 5ee77ef0-5f7b-4fe0-a9a4-5273b61bf92f
---

This project is a complete hotel booking website + admin dashboard for Hotel Elegant Executive Suites, Multan, Pakistan.

**Location:** `c:\Users\Abdul Ahad\Desktop\Hotel Elegant Multan`

**Stack:** Next.js 14.2.13 (App Router) · TypeScript · Tailwind CSS · Supabase (PostgreSQL, Auth, Storage) · Resend emails · date-fns

**Status:** Built and production-ready. `npm run build` succeeds (25 pages, standalone output).

**Key facts:**
- `output: 'standalone'` in next.config.mjs — ready for Hostinger Node.js hosting
- Supabase schema + seed in `/supabase/schema.sql` and `/supabase/seed.sql`
- 5 rooms seeded: Executive King, Family Suite, Presidential Suite, Junior Suite, Triple Sharing
- No online payment — "pay at hotel" model, booking requests confirmed via WhatsApp
- Admin at `/admin` (Supabase Auth protected via middleware)
- Booking server action at `app/actions/booking.ts` — atomic availability check + block, no double-booking
- Resend emails degrade gracefully if `RESEND_API_KEY` is absent
- `getRooms()` = server client (needs request context); `getRoomsStatic()` = service client (for generateStaticParams/sitemap)
- Brand colors: deep-purple `#1A0B2E`, brand-red `#E30613`, whatsapp `#25D366`
- Fonts: Playfair Display (headings) + Montserrat (body)
- WhatsApp: 923173330998 | Phone: 0317-333-0998 | Email: info@elegant-suite.com
- Address: 77-A Gulgasht Colony, Multan, Punjab 60750

**Why:** Setup `.env.local` with Supabase credentials, run `supabase/schema.sql` then `supabase/seed.sql`, then `npm run dev`.
**How to apply:** When resuming work on this project, use this context to avoid re-deriving the architecture.
