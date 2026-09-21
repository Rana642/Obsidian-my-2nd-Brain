---
name: tadpharma-namecheap-deployment
description: "Tad Pharma static-export site, its Namecheap cPanel addon-domain layout, and the Windows zip backslash gotcha"
metadata: 
  node_type: memory
  type: project
  originSessionId: 56998598-b910-4d9b-a1f4-8135147af75c
---

Tad Pharma marketing site lives at `C:\Users\Abdul Ahad\Desktop\tad-pharma` — Next.js 16.2.9, App Router, Tailwind v4, Framer Motion. Deployed as a **static export** to Namecheap cPanel shared hosting.

- `next.config.mjs`: `output: "export"`, `trailingSlash: true`, `images.unoptimized: true`. Build → `out/`. Deploy artifact is `tadpharma-static.zip` (contents of `out/`, `index.html` at top level, `.htaccess` inside).
- `tadpharma.pk` is an **addon domain** whose document root is the `/tadpharma.pk` folder — NOT `public_html`. `public_html` is a separate live site (meezab.com / meezabz.com) and must never be touched.
- Site is all static routes (/, /about, /products, /distribution, /contact, 404). Contact form is client-side WhatsApp deep-link (`window.open` to wa.me/923004307810) — no server route.

**Gotcha — Windows PowerShell 5.1 `Compress-Archive` writes backslash path separators into zip entries**, which Linux `unzip` (cPanel Extract) turns into files with literal `\` in their names → `_next/` never becomes a folder → CSS/JS 404. Build the zip with `System.IO.Compression.ZipArchive` + `CreateEntryFromFile`, passing relative names with `.Replace('\\','/')`. Verify zero backslash entries before shipping.

Related: [[wacrm-hostinger-deployment]] (the user's other deployment project).
