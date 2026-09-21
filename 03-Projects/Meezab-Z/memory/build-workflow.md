---
name: build-workflow
description: Meezab Z site serves minified CSS/JS; edits must go to source then be re-minified
metadata: 
  node_type: memory
  type: project
  originSessionId: 42b65a4f-e886-403c-8fef-05ec5237a37e
---

The Meezab Z. International static site (C:\Users\Abdul Ahad\Desktop\Meezab Z) has its 21 HTML pages referencing **minified** assets: `assets/css/style.min.css` and `assets/js/main.min.js`. The readable **source** files are `assets/css/style.css` and `assets/js/main.js`.

**Why:** Minified for production/hosting performance.

**How to apply:** Always edit the SOURCE files, then regenerate the minified versions, or the change won't appear on the site:
- CSS: `npx clean-css-cli -o assets/css/style.min.css assets/css/style.css`
- JS: `npx terser assets/js/main.js -c -m -o assets/js/main.min.js`

Brand palette (in `:root`): `--bright:#01B1AE` `--teal:#047887` (Deep Teal, primary buttons/links) `--deep`/`--ink:#03333B` (Ink, text/headings/footer) `--soft:#D3F0F0` `--wash:#F1FBFB`. See [[meezab-brand-palette]].

**Security headers / CSP:** A Content-Security-Policy is enforced via (a) `<meta http-equiv>` in every page's `<head>` AND (b) host configs `.htaccess`, `_headers`, `vercel.json`. Allowlist currently permits only: self, `fonts.googleapis.com` (CSS), `fonts.gstatic.com` (fonts), `openstreetmap.org` (contact map iframe), and `img-src 'self' data:`. **If you add any new external resource (a script, an external image, a different map/embed, an analytics tag), you MUST add its origin to the CSP in all 4 places or the browser will block it.** Product/og images are local. The site is static (no backend/DB/login) so SQLi/CSRF/RCE/file-upload/session threats are N/A; DDoS/DNS/defacement are host/registrar-level.

**SEO / domain:** The production domain is **assumed** to be `https://www.meezabz.com` (from the info@meezabz.com email — user did not confirm www-vs-non-www). This base URL is hard-coded across `sitemap.xml`, `robots.txt`, every page's canonical/og:url/og:image/twitter:image, and the LocalBusiness/BreadcrumbList JSON-LD. **If the real domain differs, find-and-replace `https://www.meezabz.com` across all .html + sitemap.xml + robots.txt.** Office geo-coords: 30.228898, 71.517139. og:image is currently the logo (a proper 1200×630 share card was offered but not yet made).
