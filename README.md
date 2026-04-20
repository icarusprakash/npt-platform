# NPT Intelligence Platform — Build README

**Last Updated: 20 April 2026 (Day 15 — Mid Session)**

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live |
| NPT Console | /console/ | Marketing | ✅ Live |
| NPT Orders Portal | /orders/ | Sales & Finance | 🔲 To Build |
| Public Website | / | Public | 🔲 In Progress |

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine

---

## Login Credentials — All Portals

### Jayaprakash (icarusprakash@gmail.com / Npt@2026)
- NPT Admin: https://newprojectstracker.in/admin/
- NPT Console: https://newprojectstracker.in/console/
- NPT Subscriber: https://newprojectstracker.in/dashboard/

### Indscan Admin (indscan.projects@gmail.com)
- NPT Admin: NPTAdmin2026!
- NPT Console: NPTConsole2026!
- NPT Subscriber: Npt@2026

### Sbiru (sbirumca85@gmail.com)
- NPT Admin: NPTEditor2026! (editor role)
- NPT Console: NPTConsole2026! (marketing role)

---

## CIN — Customer Information Number
- Format: NPT-YYYY-NNNNN (e.g. NPT-2026-00001)
- Auto-generated on registration (self or manual via Console)
- Column `cin` added to `npt_users` ✅
- Links: npt_users ↔ npt_orders ↔ npt_payments ↔ npt_invoices ↔ npt_activity_log

---

## Activity Tracking — COMPLETED TODAY

Added logging to subscriber portal:

**Files modified:**
- `dashboard/_auth.php` — logs `page_view` on every page (throttled per session)
- `dashboard/login.php` — logs `login` on successful login
- `dashboard/project.php` — logs `project_view` with project name as detail

**Table:** `npt_activity_log` (id, user_id, action, page, detail, ip_address, created_at)

**Console Activity Feed** at `/console/activity.php` — confirmed working, showing real events.

---

## NPT Admin Portal (`/admin/`)

### All Files
| File | Purpose |
|------|---------|
| login.php | Dark theme login |
| _auth.php | Auth guard (npt_admin_*) |
| _layout.php | Sidebar (dark navy + red) |
| _layout_end.php | Closes layout |
| index.php | Dashboard |
| projects.php | All projects list |
| project.php | Project view + Quick Edit |
| crud.php | Full entry/edit form |
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

### Bug Fixed Today
- `proj_updateddate` now ONLY changes when `proj_details` OR `proj_stage` is edited
- Fixed in both `crud_save.php` and `qe_save.php`
- All other field edits (source, investment, location, publish status) do NOT change updated date

### Daily Workflow
```
Mon–Fri: Researcher adds projects → REPOSITORY (proj_repository = 'YES')
3:30 PM: Select 15 → Move to Daily & Publish
Email marketer: Download Excel → create PDF report → Flush Daily
Friday: Download Weekly Excel → send Monday → Flush Weekly
```

### Project Flags
| Field | YES means |
|-------|-----------|
| proj_repository | In bucket, not published |
| proj_show | Published, visible to subscribers |
| proj_today | In today's daily section |
| proj_weekly | In weekly section |

---

## NPT Console (`/console/`)

### All Files
| File | Purpose |
|------|---------|
| login.php | Dark login (green accent) |
| _auth.php | Auth guard (npt_console_*) |
| _layout.php | Sidebar (dark + green CONSOLE) |
| index.php | Dashboard |
| users.php | All subscribers |
| user.php | Manage individual user |
| add_user.php | Add user manually (CIN auto-generated) |
| activity.php | Platform activity feed |
| logout.php | Logout |

---

## NPT Subscriber Portal (`/dashboard/`)

### Pages Status
| File | Status |
|------|--------|
| _auth.php | ✅ + activity logging added |
| login.php | ✅ + login event logging added |
| project.php | ✅ + project_view logging added |
| projects.php | ✅ |
| companies.php | ✅ |
| company.php | ✅ |
| briefcase.php | ✅ |
| watchlist.php | ✅ |
| usage.php | ✅ |
| profile.php | ✅ |
| pricing.php | ✅ needs layout refactor |
| register.php | 🔲 NEEDS TO BE BUILT |
| admin/index.php | ✅ (to retire → Console) |
| admin/user.php | ✅ (to retire → Console) |

---

## Database Tables

### Core Data
- `npis_projects` — 40,948 rows (all published)
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845 rows

### Platform Tables
- `npt_users` — subscribers (cin column ✅)
- `npt_admin_users` — admin logins
- `npt_console_users` — console logins
- `npt_activity_log` — user activity ✅
- `npt_source_tags` — source tag autocomplete
- `npt_waitlist` — waitlist captures
- `npt_briefcase` — saved projects
- `npt_watchlist_phrases` — watch phrases
- `npt_watchlist_projects` — matched projects
- `npt_password_resets` — reset tokens

### Orders Tables (TO CREATE)
- `npt_quotations` — (id, cin, plan_type, amount, valid_until, status)
- `npt_orders` — (id, cin, plan_type, amount, start_date, end_date, payment_mode, status)
- `npt_payments` — (id, order_id, amount, paid_date, mode, reference, status)
- `npt_invoices` — (id, payment_id, invoice_number, amount, issued_date, pdf_path)

---

## Public Website — IN PROGRESS

### Current State
Existing `index.php` is a well-built waitlist landing page. Design is excellent and will be repurposed.

### Plan — Keep & Modify (not rebuild)
**What changes:**
- Header CTA: "Join Waitlist" → "Create Free Account" → `/dashboard/register.php`
- Hero CTA: Replace waitlist button with "Create Free Account" + "Sign In"
- Stats: Update 37,000 → 40,948
- Waitlist section → Replace with "Get Started Free" section (two CTAs only)
- Add client logos section (Jp to provide logos)
- Add thin beta banner at top: "Now in Beta — Currently accepting free registrations"
- Nav: Add "About" and "Sign In" links
- Footer: Update copyright to 2026

**What stays:**
- Problem section (excellent, keep as-is)
- Features/What We Do section
- Industries grid (16 industries)
- Who It's For section
- Overall design, fonts, colors

### Pages Planned
1. Home (`/`) — modify existing index.php
2. About (`/about.php`) — 30yr history, founder story
3. Features (`/features.php`) — platform capabilities, no pricing
4. Contact (`/contact.php`) — inquiry form

### Key Decisions
- No pricing on public site — pricing only inside dashboard
- Only CTA: "Create Free Account" — no "Buy Now" anywhere
- Legacy NPT users callout: "Already an NPT subscriber? Your new dashboard is here"
- register.php needs to be built first

---

## Next Steps — When We Resume (2 Hours)

### Immediate (this session):
1. **Build `register.php`** — new subscriber registration with CIN auto-generation
2. **Redesign `index.php`** — modify existing landing page per plan above
3. **Build `about.php`**, `features.php`, `contact.php`

### After public site:
4. **Orders Portal** — build `/orders/`
5. **Razorpay integration** — Jp to get key secret
6. **Watchlist trigger** — fire on new project save
7. **Test case list** — prepare for afternoon testing session

---

## Test Cases (Prepare for Afternoon)

### NPT Admin Portal
- [ ] Add new project — search first, not found, create new
- [ ] Edit existing project — change proj_details → updateddate changes
- [ ] Edit existing project — change source URL only → updateddate does NOT change
- [ ] Edit existing project — change proj_stage → updateddate changes
- [ ] Add milestone inline on project view
- [ ] Quick Edit — stage section, save, verify change
- [ ] Move 3 projects from Repository to Daily
- [ ] Verify Daily shows those 3 projects
- [ ] Download Daily Excel
- [ ] Flush Daily — verify section empties
- [ ] Verify Weekly accumulates
- [ ] Publish a draft project
- [ ] Unpublish from project view page only
- [ ] Search companies, view company page

### NPT Console
- [ ] Login as indscan.projects@gmail.com
- [ ] View dashboard stats
- [ ] View all users list
- [ ] Open user profile — verify CIN shown
- [ ] Change user plan type and save
- [ ] View activity feed — verify login + page_view events show

### NPT Subscriber Portal
- [ ] Login — verify activity log records login event
- [ ] Browse projects page — verify page_view logged
- [ ] Click a project — verify project_view logged with project name
- [ ] Add project to briefcase
- [ ] Set a watchlist phrase
- [ ] View usage page
- [ ] View pricing page

---

## Design Systems
| Portal | Sidebar | Accent | Badge |
|--------|---------|--------|-------|
| Admin | #0f2444 | #e94560 red | ADMIN |
| Console | #0d1117 | #2d6a4f green | CONSOLE |
| Orders | TBD | TBD | ORDERS |
| Subscriber | #1a3c6e navy | #e87722 orange | — |
| Public Site | — | navy + orange | — |

---

## Razorpay — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending (Jp to get from Shopify vendor)

---

## Session Log
| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–2 | Early Apr | Placeholder + waitlist landing page |
| 3–8 | Apr | Full subscriber dashboard built |
| 9 | 14 Apr AM | Admin panel, access controls |
| 10 | 14 Apr PM | Watchlist, Razorpay domain verified |
| 11 | 15 Apr AM | Import pipeline, PRIMARY KEY fix |
| 12 | 16 Apr | Full dataset imported — 41,003 projects |
| 13 | 17–18 Apr | NPT Admin portal. Repository → Daily → Weekly workflow. |
| 14 | 19 Apr | NPT Console. CIN system. Companies page in Admin. Orders Portal planned. |
| 15 | 20 Apr | Activity tracking wired. Login/page/project events logging. proj_updateddate bug fixed. User credentials set across all portals. Public website redesign plan locked. Register.php identified as dependency. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- Three internal portals: Admin, Console, Orders
- Subscriber portal: /dashboard/
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
