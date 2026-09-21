---
name: focusgeniuslab-affiliate-site
description: "Focus Genius Lab affiliate site — v2 multi-product build location, affiliate link rules, and pre-launch placeholders to fill"
metadata: 
  node_type: memory
  type: project
  originSessionId: a9dbaf0b-a81e-45eb-92b8-589cb3ef1f5d
---

Multi-product affiliate authority site (v2, built 2026-07-13) reviewing brainwave-audio programs. Supersedes the single-product v1.

- **v2 location:** `E:\Rana Shoaib\Affiliate\focusgeniuslab-website\` — 8 pages (index, about, thankyou, genius-switch-review, genius-wave-review, privacy, terms, contact) + assets/styles.css + sitemap.xml + robots.txt + .htaccess (extensionless clean URLs matching canonicals).
- **v1 (old, single-product):** `C:\Users\Abdul Ahad\Downloads\Affiiliate\focusgeniuslab-website\` (double-i "Affiiliate" typo) — left untouched; do not deploy.
- **Domain:** https://focusgeniuslab.com — deploys to Hostinger (see [[wacrm-hostinger-deployment]]).
- **Genius Switch (ClickBank, US):** `https://hop.clickbank.net/?affiliate=shoaib642&vendor=thegeniusx&pid=vsl` + `&tid=review` / `&tid=home` / `&tid=optin` per page. Never link an order form directly.
- **Genius Wave (Digistore24, EU/global):** `https://ingeniuswave.com/DSvsl/#aff=shoaib642` — use exactly as-is, no extra params.
- **Compliance:** Genius Wave page follows a stricter banned-word list (no IQ/medical/"makes you smarter" claims; no ADHD/anxiety/depression mentions); Dr. James Rivers credentials framed as "according to the vendor". Both review pages: Review + FAQPage JSON-LD, soft "results vary" language, FDA disclaimer + affiliate disclosure in every footer. nasa.png deliberately not referenced (avoids implying NASA endorsement).
- **Pending before launch:** drop vendor images into `assets/images/` (exact-filename list in assets/images/README.txt — some names contain spaces), paste MailerLite embed in index.html (redirect to thankyou.html), replace GA4 `G-XXXXXXXXXX` + Meta `PIXEL_ID` commented blocks on all pages, confirm Genius Wave price with vendor (page uses soft "promotional price" framing), replace contact email + governing-law placeholders in privacy/terms/contact.
