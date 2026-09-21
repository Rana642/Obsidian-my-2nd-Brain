---
name: hss-non-negotiables
description: "Locked constraints for the Hotel Silver Sand website (colors, tracking, prices, content rules)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 48f648cb-9c79-4134-90c6-bf52b9121542
---

Non-negotiable constraints from the client's build brief (violating any of these is a defect):

- **Colors:** ONLY navy #061B4D, royal #0B2E73, gold #D9A441, gold-rich #C88A18, white, offwhite #F7F7F5, gray-soft #D9D9D9, gray-dark #2C2C2C. Enforced via Tailwind v4 palette reset.
- **Prices (PKR/night):** Deluxe King 6,500 (8 rooms) / Deluxe Triple 7,500 (5) / Executive Twin 9,000 (4) / Executive Family 11,000 (5, "Most Popular", 4 adults + 2 children). 22 rooms total.
- **Tracking:** GTM-KNTXGW9J only injected (GA4 G-TFTT1WXL5L + Pixel 1336120438462344 fire inside GTM, never hardcoded). Exact snake_case events: whatsapp_click, call_click, bookingcom_click, email_click, directions_click, begin_checkout (NOT a conversion), booking_confirmed (conversion), contact_form_submit, view_item, view_item_list. Names must match the live container.
- **No individual staff names anywhere** on the site — review quotes only (brand trust). Reviews with staff names must be trimmed.
- **NAP everywhere identical:** Hotel Silver Sand Multan, 514 Akbar Road, Railway Colony, near Aziz Hotel Chowk, Cantt, Multan 60000, Pakistan; 0300-872-0939 / +923008720939; founded 1986; 3.8/5, 822+ reviews; coords 30.184679295816633, 71.44285529818006. Single source: src/lib/constants.ts.
- **Email CONFIRMED (2026-07-03):** info@hotelsilversandmultan.com is the only hotel email — use everywhere (footer, contact, schema).
- **8 schema types:** LodgingBusiness, FAQPage, Speakable, HotelRoom, Organization, BreadcrumbList, LocalBusiness, AggregateRating.
- **AI crawlers allowed** in robots.txt (GPTBot, Google-Extended, PerplexityBot, ClaudeBot, anthropic-ai, Bingbot) + /llms.txt file.
- **Direct booking primary** (WhatsApp wa.me/923008720939, free early check-in 12PM / late check-out 2PM messaging), Booking.com fallback subtle/secondary.
- Check-in 2PM / check-out 12PM (direct booking: 12PM / 2PM free).

Related: [[hss-build-status]]
