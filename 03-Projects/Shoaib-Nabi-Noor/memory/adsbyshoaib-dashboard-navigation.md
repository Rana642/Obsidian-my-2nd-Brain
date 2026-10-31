---
name: adsbyshoaib-dashboard-navigation
description: "2026-10-13 (76dc704) dashboard IA redone GoHighLevel-style — one sidebar, section pages as top tabs, Setup tab per section, Settings at bottom; Finance phase 2 (expenses/P&L) next"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-13T10:58:27.983Z
---

**Shoaib's request (2026-10-13):** the dashboard should feel like GoHighLevel. He specifically asked for three things:
- Settings sits next to Sign out.
- Each section keeps its own setup inside itself (the bank "accounts" part belongs with billing).
- The empty right-hand side of wide pages gets used.

**He chose:**
- Top tabs (instead of a second sidebar).
- The name **Finance** for the money section.
- Phase 1 first.
- Hotels: he wants salon bookings and career pages for management companies later. So it became **Websites**, grouped by kind of data (bookings / inquiries / contacts), not by industry. New verticals become more tabs (in the portal too).

**Phase 1 is live in 76dc704.** All page URLs are unchanged.
- `lib/dashboard/sections.ts` is the single source of sections. A group titled "Setup" is pinned to the right of the tabs. `placement: "bottom"` puts a section next to Sign out (that is how Settings is placed).
- **Sales:** Leads, Proposals, Agreements, Onboarding, Intakes, plus the catalog (Single / Bundle services).
- **Clients:** Clients, Monthly reports, Password Vault, Letterhead, and Setup at `/dashboard/clients/setup` (My access accounts).
- **Finance:**
  - Invoices, Quotations, Payment receipts, Retainers.
  - Setup at `/dashboard/finance/setup`: Payment accounts, Tax, Documents numbering and terms.
  - `/dashboard/finance` redirects to Invoices for now.
- **Settings:** Business profile (`/dashboard/settings`) and API Vault.
- **Layout:**
  - `SettingsCard` in `ui.tsx` puts the title on the left and the fields on the right.
  - Pages go up to `max-w-[96rem]`.
  - The tab bar is exactly 3.25rem. `--section-nav-h` and `.app-height` keep the full-height WhatsApp inbox inside the screen.
- **Settings saving:** `updateSettings` takes `_part` = `business` | `billing`.

**Phase 2, Finance: live and tested on 2026-10-13** (commits 2828a3c and 8f171e8; SQL run).
- Tables:
  - `finance_entries` (expense / other income, `pkr_amount` + `fx_rate`, `pass_through`, receipt in R2 under `finance/`)
  - `finance_bills` (Mark paid → expense, `next_due` moves on)
  - `payments.pkr_amount` / `fx_rate`
- **Maths** in `lib/dashboard/finance.ts` `summarize`:
  - revenue = invoice payments + other income + min(0, client money in − client ad spend)
  - client money held is never profit
- **Reports:** P&L, monthly, by category, per client, tax year Jul–Jun (local vs foreign), CSV.
- **Not tax advice:** the tax card says so. Never compute tax owed.

**Testing gotcha:** to run server-only lib code in a script, use `node --require <stub that maps "server-only" to an empty module> --import tsx file.ts`. The tsx CLI spawns a child, so `--require` on it does nothing.

**Phase 3 shipped in 876b48b:**
- **+ New:** the menu lives in `SidebarTools`; its actions come from `quick-actions.ts`.
- **Ctrl+K:** `CommandPalette` plus `/api/dashboard/search`.
- **Sidebar counts:** `/api/dashboard/counts`.
  - Leads have no status column. Their "new" count is leads newer than the localStorage timestamp `dashboard-leads-seen`.
  - Gotcha: the count fetch is throttled, so a "cancelled" guard in the effect dropped the result under React strict mode (the badge never appeared). The guard was removed.

**Client hub (6 tabs, prefilled "+ Proposal / Invoice / Expense") is ON HOLD.** On 2026-10-13 Shoaib picked "Abhi ruk jao". Don't build it unless he asks.

Possible later: finance MCP tools so Claude can log expenses from receipts.

Related: [[adsbyshoaib-dashboard]], [[adsbyshoaib-payments-receipts]], [[adsbyshoaib-dashboard-glass-ui]], [[shoaib-confirm-ux-before-building]].

**2026-10-16, commit 270e5b3: Planner is its own sidebar section.** Shoaib asked for this ("planner ko marketing se alag").
- **Layout:**
  - Planner sits above Marketing in the sidebar.
  - A vertical **Posts / Stories** toggle on the left of the calendar.
  - Tabs on top: All · Image · Reels & Shorts · Videos. "Posts" was renamed "Image".
  - The client portal has the same layout.
- **Counts:** the badge key changed from `marketing` to `planner`.
- **Code:**
  - `SectionItem.hidden` = reachable inside the section but not shown as a tab.
  - Sections now resolve to the most specific prefix (`matchStrength`), because `/social/planner` sits under Marketing's `/social`.

**2026-10-19, commit 92d1228: Business Management and Password Vault.**
- **Business Management:** "Websites" became **Business Management**. Each industry system is a labelled tab group (`showTitle`). It has Hotel Management now (Bookings, Inquiries, Contacts); Salon Management and others get added as new groups once they're built. Shoaib does not want empty placeholders.
- **Password Vault:** now its own sidebar item, removed from Clients. Shoaib: "bahr hi bas".

**2026-10-31 — Socially Snap → Settings → Integrations** (0ecd372, Shoaib OK'd the design).
- Connections moved out of Marketing into a new **Settings** entry. It is the last item in the "Socially Snap Apps" dropdown (section key `socially-settings`).
- The tab is called **Integrations**. The URL is still `/dashboard/social`, because every OAuth callback returns there.
- Marketing now matches only `/social/insights`, `/ads` and `/youtube`.
- Each Facebook login card shows its Page count. When Meta blocks the login it shows a red "Reconnect needed": since when, how many held posts, and the fix. The data comes from `heldPostsSummary`.
- Small link cards go to Google Business and WhatsApp numbers.
- The banner link now reads "Open Integrations".

**Broken-connection pop-up** (349bde9 and 4eafe2c, 2026-10-31; Shoaib: "alert pop mai ana chaiye jahan jahan bhi connection kharab hai").
- `lib/connection-health.ts` (`brokenConnections`, 60 s cache) lists what is broken:
  - a Facebook login Meta stopped. Checked live with `facebookLoginWorks` (debug_token: valid AND has scopes);
  - an Instagram Login account that expired or has held posts;
  - the LinkedIn login when expired or expiring within 7 days.
- Posts that are only retrying behind a working login are excluded. Integrations shows those as yellow "Posts retrying".
- `ConnectionAlert`: a pop-up on every dashboard page, once per browser session (sessionStorage), with "Later" and "Open Integrations".
- Socially Snap → Settings gets a red count badge (`ALERT_BADGES` in SidebarTools, `integrations` in /api/dashboard/counts).
- The HeldPostsBanner was deleted.
- `loginNames()`: a broken login can't read its own name, so the name falls back to `fb_login_name` on its Pages.
- **Update, later on 2026-10-31 (81326bb, 6aa6fb5):** Shoaib had meant a pop-over, not a modal ("bara popup hata do, sirf ⚠ wala rakho").
  - `ConnectionAlert` (the modal) and `/api/dashboard/connection-health` were deleted.
  - Login problems on Integrations now sit behind a ⚠ sign beside the status pill (`IssuePopover` in ConnectionsHub). Hover or focus opens the details above the card; a click pins them; Esc or a click outside closes them.
  - **Lesson:** "pop" from him = a pop-over on the thing itself, not a page-wide modal.
  - Kept: the red badge on Settings (`brokenConnections`).
