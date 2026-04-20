# NPT Intelligence Platform — Build README

**Last Updated: 20 April 2026 (Day 15 — Session End)**

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live |
| NPT Console | /console/ | Marketing | ✅ Live |
| NPT Orders Portal | /orders/ | Sales & Finance | 🔲 To Build |
| Public Website | / | Public | ✅ Live |

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
- NPT Admin: NPTEditor2026! (editor)
- NPT Console: NPTConsole2026! (marketing)

---

## Public Website — ✅ COMPLETE

### Pages
| URL | File | Status |
|-----|------|--------|
| / | index.php | ✅ Full landing page |
| /register.php | register.php | ✅ Split-screen registration |
| /about.php | about.php | ✅ Story, timeline, values |
| /features.php | features.php | ✅ Feature blocks + plan compare |
| /contact.php | contact.php | ✅ Inquiry form → npt_contact_forms |
| /login.php | login.php | ✅ Existing |
| /forgot-password.php | forgot-password.php | ✅ Existing |

### Key Decisions
- No pricing on public site — pricing only inside /dashboard/pricing.php
- Only CTA: "Create Free Account" — no "Buy Now"
- Beta banner at top: "Now in Beta — Currently accepting free registrations"
- Client logos section: placeholder "coming soon" — Jp to provide logos
- Legacy NPT callout: "Already an NPT subscriber? Your new dashboard is here"
- Contact form saves to `npt_contact_forms` table (auto-created)

### register.php
- Split layout: left = marketing pitch + stats + free plan benefits
- Right = registration form
- Auto-generates CIN on signup (NPT-YYYY-NNNNN)
- Sets plan_type = 'free', credits_monthly = 5
- Logs 'register' event to npt_activity_log
- Redirects to /dashboard/?welcome=1

---

## CIN — Customer Information Number
- Format: NPT-YYYY-NNNNN (e.g. NPT-2026-00001)
- Auto-generated on registration (self or Console)
- Column `cin` in `npt_users` ✅
- Links all databases

---

## Activity Tracking — ✅ COMPLETE

**Files modified in subscriber portal:**
- `dashboard/_auth.php` — page_view on every page (session-throttled)
- `dashboard/login.php` — login event on successful login
- `dashboard/project.php` — project_view with project name
- `register.php` — register event on new signup

**Table:** `npt_activity_log` (id, user_id, action, page, detail, ip_address, created_at)
**Console Activity Feed:** `/console/activity.php` — confirmed working ✅

---

## NPT Admin Portal (`/admin/`)

### Bug Fixed (Day 15)
`proj_updateddate` now ONLY changes when `proj_details` OR `proj_stage` is edited.
Fixed in both `crud_save.php` and `qe_save.php`.

### All Files
| File | Purpose |
|------|---------|
| login.php | Dark theme login |
| _auth.php | Auth guard (npt_admin_*) |
| _layout.php | Sidebar (dark navy + red) |
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
| download.php | CSV download |
| publish.php | Quick publish |
| unpublish.php | Unpublish |
| companies.php | Companies list |
| company.php | Company view |
| logout.php | Logout |

### Daily Workflow
```
Mon–Fri: Add projects → REPOSITORY
3:30 PM: Select 15 → Move to Daily & Publish
Email marketer: Download Excel → report → Flush Daily
Friday: Download Weekly → send Monday → Flush Weekly
```

---

## NPT Console (`/console/`)

### All Files
| File | Purpose |
|------|---------|
| login.php | Dark login (green) |
| _auth.php | Auth guard (npt_console_*) |
| _layout.php | Sidebar (dark + green CONSOLE) |
| index.php | Dashboard |
| users.php | All subscribers |
| user.php | Manage individual user |
| add_user.php | Add user (CIN auto-generated) |
| activity.php | Activity feed |
| logout.php | Logout |

---

## NPT Subscriber Portal (`/dashboard/`)

### Pages Status
| File | Status |
|------|--------|
| _auth.php | ✅ + page_view logging |
| login.php | ✅ + login logging |
| project.php | ✅ + project_view logging |
| projects.php | ✅ |
| companies.php | ✅ |
| company.php | ✅ |
| briefcase.php | ✅ |
| watchlist.php | ✅ |
| usage.php | ✅ |
| profile.php | ✅ |
| pricing.php | ✅ needs layout refactor |
| admin/index.php | ✅ to retire → Console |
| admin/user.php | ✅ to retire → Console |

---

## Database Tables

### Core Data
- `npis_projects` — 40,948 rows (all published)
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845 rows

### Platform
- `npt_users` — subscribers (cin ✅)
- `npt_admin_users` — admin logins
- `npt_console_users` — console logins
- `npt_activity_log` — user activity ✅
- `npt_contact_forms` — contact page submissions ✅
- `npt_source_tags` — source tag autocomplete
- `npt_waitlist` — old waitlist captures
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

## Next Tasks (Priority Order)

| # | Task |
|---|------|
| 1 | **Client logos** — Jp to provide, add to index.php logos section |
| 2 | **Orders Portal** — build /orders/ |
| 3 | **Razorpay integration** — Jp to get key secret |
| 4 | **Watchlist trigger** — fire on new project save in crud_save.php |
| 5 | **login.php** — set credits_reset_date on registration |
| 6 | **Email verification** — AWS SES |
| 7 | **pricing.php** — refactor to _layout.php |
| 8 | **Retire dashboard/admin/** → Console |
| 9 | **Test case session** — run full test list (see below) |

---

## Test Cases (For Next Testing Session)

### NPT Admin Portal
- [ ] Add new project — search first, not found, create new entry
- [ ] Edit project details → proj_updateddate changes
- [ ] Edit source URL only → proj_updateddate does NOT change
- [ ] Edit proj_stage → proj_updateddate changes
- [ ] Add milestone inline on project view page
- [ ] Quick Edit — stage section, save, verify
- [ ] Move 3 projects from Repository → Daily
- [ ] Verify Daily shows those projects
- [ ] Download Daily Excel
- [ ] Flush Daily — verify empties
- [ ] Verify Weekly accumulates
- [ ] Publish a draft project
- [ ] Unpublish from project view page only
- [ ] Search companies, view company page

### NPT Console
- [ ] Login as indscan.projects@gmail.com
- [ ] View dashboard stats
- [ ] Open user profile — verify CIN shown
- [ ] Change user plan type and save
- [ ] View activity feed — login + page_view events showing

### NPT Subscriber Portal
- [ ] Login — activity log records login
- [ ] Browse projects — page_view logged
- [ ] Click project — project_view logged with project name
- [ ] Add project to briefcase
- [ ] Set watchlist phrase
- [ ] View usage page
- [ ] View pricing page

### Public Website
- [ ] Register new user via /register.php
- [ ] Verify CIN auto-generated in Console
- [ ] Verify redirect to /dashboard/?welcome=1
- [ ] Contact form submission → saved to npt_contact_forms
- [ ] All nav links working across all pages
- [ ] "Already an NPT subscriber" link goes to /login.php

---

## Design Systems
| Portal | Sidebar | Accent | Badge |
|--------|---------|--------|-------|
| Admin | #0f2444 | #e94560 red | ADMIN |
| Console | #0d1117 | #2d6a4f green | CONSOLE |
| Orders | TBD | TBD | ORDERS |
| Subscriber | #1a3c6e | #e87722 orange | — |
| Public | — | navy + orange | — |

---

## Razorpay — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending (Jp to get from Shopify vendor)

---

## Session Log
| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–2 | Early Apr | Placeholder + waitlist |
| 3–8 | Apr | Full subscriber dashboard |
| 9 | 14 Apr AM | Admin panel, access controls |
| 10 | 14 Apr PM | Watchlist, Razorpay domain |
| 11 | 15 Apr AM | Import pipeline, PRIMARY KEY fix |
| 12 | 16 Apr | Full dataset — 41,003 projects |
| 13 | 17–18 Apr | NPT Admin portal. Repository → Daily → Weekly. |
| 14 | 19 Apr | NPT Console. CIN system. Companies in Admin. |
| 15 | 20 Apr | Activity tracking. proj_updateddate bug fix. Public website complete — Home, Register, About, Features, Contact. Beta banner. Client logos placeholder. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- Three internal portals: Admin (/admin/), Console (/console/), Orders (/orders/)
- Subscriber portal: /dashboard/
- Public site: / (index.php, register.php, about.php, features.php, contact.php)
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
