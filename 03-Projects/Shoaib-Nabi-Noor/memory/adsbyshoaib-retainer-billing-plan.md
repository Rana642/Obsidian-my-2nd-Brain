---
name: adsbyshoaib-retainer-billing-plan
description: Approved plan (2026-10-06) for automatic monthly retainer invoices + monthly client reports in the dashboard/portal; not built yet
metadata:
  type: project
---

Shoaib asked for this on 2026-10-06: every client whose proposal is accepted gets a monthly retainer invoice created automatically, with a monthly report attached, and it is sent to the client. Full plan: `docs/RETAINER-BILLING-PLAN.md`.

Decisions:
- Billing on the **1st of every month** for all clients (advance for that month). The first invoice (one-time items + first month) is created at agreement signing or proposal acceptance.
- **Draft first, Shoaib sends with one click.**
- **Ad spend is NOT invoiced:** clients pay Google/Meta directly; spend appears only in the report.

Build order:
1. Retainers table + UI, created automatically from the proposal's monthly lines.
2. Daily GitHub Actions cron → `/api/billing/cron`. Never a sub-daily Vercel cron on Hobby.
3. `client_reports`, built from the KB `client-report-log-YYYY-MM` + Ads / GA4 / GBP / planner numbers. Public `/report/[token]` page and PDF.
4. Review + send by email (Resend). New public `/invoice/[token]` page.
5. Portal tab "Invoices & Reports".
6. Payments / overdue reminders.
7. MCP tools, shipped on local and remote.

Status: plan approved. Build not started (as of 2026-10-06).

Related: [[adsbyshoaib-proposal-agreement-funnel]], [[adsbyshoaib-dashboard]], [[adsbyshoaib-client-portal]], [[feedback-client-work-log]]
