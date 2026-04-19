# NPT Intelligence Platform — Build README

**Last Updated: 19 April 2026 (Day 14)**

---

## Three Internal Tools — One Database

All three share `newp_ai_engine`. Each has its own login, session, and team.

| Tool | URL | Team | Purpose |
|------|-----|------|---------|
| NPT Admin Portal | /admin/ | Researchers | Daily project entry, publish workflow |
| NPT Console | /console/ | Marketing | User management, analytics, CIN tracking |
| NPT Orders Portal | /orders/ | Sales & Finance | Payments, invoices, quotations *(TO BUILD)* |

Plus the subscriber-facing product:

| Product | URL | Users | Purpose |
|---------|-----|-------|---------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | Browse projects |

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine

---

## CIN — Customer Information Number

Auto-generated unique ID per subscriber. Format: `NPT-YYYY-NNNNN`
Example: `NPT-2026-00001`

- Generated on registration (self or manual via Console)
- Links: npt_users ↔ npt_orders ↔ npt_payments ↔ npt_invoices ↔ npt_activity_log
- Column `cin` in `npt_users` table ✅

---

## NPT Admin Portal (`/admin/`)

### Files
| File | Purpose |
|------|---------|
| login.php | Dark theme login |
| _auth.php | Auth guard (session: npt_admin_*) |
| _layout.php | Sidebar (dark navy + red accent) |
| index.php | Dashboard — draft queue, stats |
| projects.php | All projects list |
| project.php | Project view + Quick Edit |
| crud.php | Full project entry/edit form |
| crud_save.php | Save handler |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add company |
| qe_save.php | Quick Edit AJAX save |
| qe_milestone.php | Add milestone AJAX |
| repository.php | Researcher's bucket |
| daily.php | Today's 15 + download + flush |
| weekly.php | Week's 75 + download + flush |
| download.php | CSV download handler |
| publish.php | Quick publish |
| unpublish.php | Unpublish |
| companies.php | Companies list |
| company.php | Company view |
| logout.php | Logout |

### Admin Users (npt_admin_users)
| Email | Role |
|-------|------|
| icarusprakash@gmail.com | superadmin |
| indscan.projects@gmail.com | superadmin |
| sbirumca85@gmail.com | editor |

### Daily Workflow
```
Mon–Fri: Researcher adds projects → REPOSITORY
3:30 PM: Select 15 → Move to Daily & Publish → visible to subscribers
Email marketer: Download Excel → create report → Flush Daily
Friday: Download Weekly Excel → send Monday → Flush Weekly
```

### Project Flags
| Field | Value | Meaning |
|-------|-------|---------|
| proj_repository | YES | In bucket, not published |
| proj_show | YES | Published |
| proj_today | YES | In daily section |
| proj_weekly | YES | In weekly section |
| proj_publish_date | date | Go-live date |

---

## NPT Console (`/console/`)

### Files
| File | Purpose |
|------|---------|
| login.php | Dark theme login (green accent) |
| _auth.php | Auth guard (session: npt_console_*) |
| _layout.php | Sidebar (dark + green CONSOLE badge) |
| index.php | Dashboard — stats, signups, expiring, activity |
| users.php | All subscribers list |
| user.php | Individual user — manage plan, credits, access |
| add_user.php | Manually add new subscriber (CIN auto-generated) |
| activity.php | Platform-wide activity feed |
| logout.php | Logout |

### Console Users (npt_console_users)
| Email | Role |
|-------|------|
| icarusprakash@gmail.com | superadmin |
| indscan.projects@gmail.com | superadmin |
| sbirumca85@gmail.com | marketing |

Password: `NPTConsole2026!` (change immediately)

### Activity Tracking
- Table: `npt_activity_log` (id, user_id, action, page, detail, ip_address, created_at)
- **TO DO:** Add logging calls to subscriber portal — login, page views, project clicks, searches

---

## NPT Orders Portal (`/orders/`) — TO BUILD

### What it will manage
- **Quotations** — proposals sent to prospects
- **Orders** — confirmed subscriptions
- **Payments** — every payment received (Razorpay + manual)
- **Invoices** — auto-generated, downloadable PDF

### New tables needed
- `npt_quotations` — (id, cin, plan_type, amount, valid_until, status, created_at)
- `npt_orders` — (id, cin, plan_type, amount, start_date, end_date, payment_mode, status)
- `npt_payments` — (id, order_id, amount, paid_date, mode, reference, status)
- `npt_invoices` — (id, payment_id, invoice_number, amount, issued_date, pdf_path)

### Invoice numbering
Format: `NPT-INV-YYYY-NNNNN` e.g. `NPT-INV-2026-00001`

---

## NPT Subscriber Portal (`/dashboard/`)

### Pages Status
| File | Status |
|------|--------|
| _auth.php | ✅ |
| _layout.php | ✅ |
| index.php | ✅ Stats + recent projects |
| projects.php | ✅ Search + filters + access controls |
| project.php | ✅ 4 tabs, credit gate |
| companies.php | ✅ |
| company.php | ✅ |
| briefcase.php | ✅ |
| watchlist.php | ✅ |
| usage.php | ✅ |
| profile.php | ✅ |
| pricing.php | ✅ Needs layout refactor |
| admin/index.php | ✅ Subscriber user mgmt (to be retired → Console) |
| admin/user.php | ✅ User control (to be retired → Console) |

---

## Database Tables

### Core Data
- `npis_projects` — 40,948 rows (all published)
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845 rows

### Platform
- `npt_users` — subscribers (has cin column ✅)
- `npt_admin_users` — admin portal logins
- `npt_console_users` — console logins
- `npt_activity_log` — user activity tracking
- `npt_source_tags` — source tag autocomplete
- `npt_waitlist` — waitlist captures
- `npt_briefcase` — saved projects
- `npt_watchlist_phrases` — watch phrases
- `npt_watchlist_projects` — matched projects
- `npt_password_resets` — reset tokens

### Orders (TO CREATE)
- `npt_quotations`
- `npt_orders`
- `npt_payments`
- `npt_invoices`

---

## npis_projects New Fields
| Field | Purpose |
|-------|---------|
| proj_tags | Product tags |
| proj_milestones | JSON milestone history |
| proj_source_tags | Source classification |
| proj_publish_date | Go-live date |

---

## Auto-generation Rules
- **CIN:** NPT-YYYY-NNNNN (sequential per year)
- **proj_pkey:** P + comp_pkey[1] + comp_pkey[1] + proj_id
- **proj_slug:** sanitized name + district + pkey
- **comp_pkey:** C + company[0] + city[0] + comp_id
- **Invoice no:** NPT-INV-YYYY-NNNNN

---

## Design Systems
| Tool | Sidebar | Accent | Badge |
|------|---------|--------|-------|
| Admin | #0f2444 | #e94560 red | ADMIN |
| Console | #0d1117 | #2d6a4f green | CONSOLE |
| Orders | TBD | TBD | ORDERS |
| Subscriber | #1a3c6e | #e87722 orange | — |

---

## Razorpay — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending (Jp to get from Shopify vendor)

---

## Tomorrow's Agenda (20 Apr)
1. **Activity tracking** — add logging to subscriber portal (_auth.php, project.php, projects.php)
2. **Orders Portal** — build /orders/ with quotations, orders, payments, invoices
3. **Razorpay** — wire up if key pair available
4. **Test end-to-end** — login → Console logs it → payment → invoice

---

## Next Tasks (Full List)
| # | Task |
|---|------|
| 1 | Activity tracking in subscriber portal |
| 2 | Orders Portal build |
| 3 | Razorpay integration |
| 4 | Watchlist trigger on new project save |
| 5 | login.php — credits_reset_date on registration |
| 6 | Email verification — AWS SES |
| 7 | pricing.php — refactor to _layout.php |
| 8 | Retire dashboard/admin/ → Console |
| 9 | Public pages Phase 3 |

---

## Session Log
| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–2 | Early Apr | Placeholder + waitlist |
| 3–8 | Apr | Full subscriber dashboard |
| 9 | 14 Apr AM | Admin panel, access controls |
| 10 | 14 Apr PM | Watchlist, Razorpay domain |
| 11 | 15 Apr AM | Import pipeline, PRIMARY KEY fix |
| 12 | 16 Apr | Full dataset imported — 41,003 projects |
| 13 | 17–18 Apr | NPT Admin portal. Repository → Daily → Weekly. |
| 14 | 19 Apr | NPT Console built. CIN system. npt_activity_log. Companies page in Admin. Orders Portal planned. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- Three portals: Admin (/admin/), Console (/console/), Orders (/orders/)
- Subscriber portal: /dashboard/
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
