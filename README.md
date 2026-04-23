# NPT Intelligence Platform — Build README

**Last Updated: 23 April 2026 (Day 18 — Session End)**

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

## Login Credentials

### Jayaprakash (icarusprakash@gmail.com / Npt@2026)
- Admin: https://newprojectstracker.in/admin/
- Console: https://newprojectstracker.in/console/
- Subscriber: https://newprojectstracker.in/dashboard/

### Indscan (indscan.projects@gmail.com)
- Admin: NPTAdmin2026! · Console: NPTConsole2026! · Subscriber: Npt@2026

### Sbiru (sbirumca85@gmail.com)
- Admin: NPTEditor2026! · Console: NPTConsole2026!

---

## Plan Names
| Plan | Price |
|------|-------|
| Basic | ₹0 / forever |
| Starter | ₹14,950/month |
| Premium | ₹99,000/year |

---

## Day 18 — What Was Built

### 1. Complete Project Entry Workflow (Final)

```
Step 1: /admin/crud.php
  - Search company in modal → connect existing OR add new (name only)
  - Fill project details → Save New Project

Step 2: /admin/project_address.php (auto-redirect after save)
  - Shows connected company tab with ✓ tick when address connected
  - Shows legacy address from npis_companies if no new address exists
  - "Yes — Use This Address" → saves legacy as new record → auto-connects
  - OR click "+ Add New Address" → fill form → save → auto-connects
  - "+ Add Another Company" → search company → set role → connect
  - Each company gets its own address tab
  - "✓ Done — Move to Repository" → moves project to repository

Step 3: /admin/projects.php?status=draft OR repository
  - View button → /admin/project.php?id=X
  - Delete button (draft only)
  - Publish button (repository only)

Step 4: /admin/project.php?id=X
  - Full project view with all fields
  - Quick Edit on every section
  - Connected Address block
  - Full Edit button → crud.php?edit=X
```

### 2. New Files Built

| File | Purpose |
|------|---------|
| project_address.php | Step-by-step address connection screen |
| ajax_save_address.php | AJAX handler — save new address + auto-connect |
| ajax_update_comptype.php | AJAX handler — update company role in project |
| delete_project.php | Delete draft projects (drafts only, never published) |

### 3. crud.php Changes
- "Add New Company" modal: removed address fields — company name only
- "Change Company" button removed — company cannot be changed after connecting
- "+ Connect Company" button hidden once company is connected
- After new project save: redirects to `project_address.php` instead of `project.php`

### 4. crud_save.php Changes
- New project redirect → `/admin/project_address.php?proj_id=X`

### 5. project.php Changes
- **Teaser** added to Description view section
- **End Product** and **Key Equipment** added to Investment & Capacity view section
- **Connected Address** block added — shows all addresses from `npt_company_addresses`
- Legacy address fallback — shows `npis_companies` address for old projects
- All Quick Edit sections work for Draft, Repository, and Published projects

### 6. projects.php Changes
- **Edit** button renamed to **View**
- **Publish** button shown only for Repository projects
- **Delete** button shown only for Draft projects
- Draft projects: View + Delete only
- Repository projects: View + Publish + Delete
- Published projects: View only

### 7. project_address.php Features
- Company tabs — click to switch between connected companies
- Role dropdown (Promoter, EPC Contractor, Consultant etc.) — saves via AJAX
- Legacy address display with "Yes — Use This Address" button
- Existing address list with Connect / Edit buttons
- Add New Address inline form
- "+ Add Another Company" section with company search
- Auto-connects new address to project on save

---

## Company Address System

### Tables
- `npt_company_addresses` — stores all address records per company
- `npis_refer.ref_address_id` — links specific address to project-company connection

### Key Rules
- One company can have multiple addresses
- One project can have multiple companies (each with different role)
- Same company can be Promoter in one project, EPC Contractor in another
- Address type is per connection, not per address record
- Legacy addresses in `npis_companies` are shown as fallback — never deleted
- When "Yes — Use This Address" is clicked, legacy address is copied to `npt_company_addresses`

### Address Types
Promoter, Corporate Office, Plant/Factory, Site Office, Regional Office, Consultant Office, EPC Contractor Office, Other

### Company Roles per Project
Promoter, Corporate Office, Plant/Factory, Regional Office, Consultant, EPC Contractor, Sub-Contractor, Financial Institution, Government Body, Other

---

## NPT Admin Portal — All Files

| File | Purpose |
|------|---------|
| login.php | Login |
| _auth.php | Auth guard |
| _layout.php | Sidebar |
| _layout_end.php | Footer |
| index.php | Dashboard |
| projects.php | Projects list (View/Publish/Delete logic) |
| project.php | Project view + Quick Edit (all fields) |
| crud.php | Full entry/edit form |
| crud_save.php | Save handler → redirects to project_address.php |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add company (name only) |
| project_address.php | ✅ NEW — Address connection screen |
| company_address.php | Company address add/edit form |
| ajax_save_address.php | ✅ NEW — AJAX address save |
| ajax_update_comptype.php | ✅ NEW — AJAX company role update |
| delete_project.php | ✅ NEW — Delete draft project |
| qe_save.php | Quick Edit AJAX |
| qe_milestone.php | Milestone AJAX |
| repository.php | Repository bucket |
| daily.php | Daily workflow |
| weekly.php | Weekly workflow |
| download.php | CSV download |
| publish.php | Quick publish |
| unpublish.php | Unpublish |
| companies.php | Companies list |
| company.php | Company view |
| logout.php | Logout |

---

## Backlog Entry Plan
- ~75 projects piled up Apr 14–23 in legacy system
- Recommendation: Manual entry (NOT SQL import)
- Reason: New schema changes (address system, workflow logic) make direct import risky
- Strategy: Enter today's 15 first to smooth workflow, then tackle backlog in batches
- Legacy addresses will auto-show in project_address.php — staff clicks "Yes — Use This Address"

---

## Critical Bug Fixes (Day 16) — crud_save.php
1. Email whitelist blocking indscan saves — removed
2. Extra '$today' (41 vs 40 columns) — removed
3. proj_recently_viewed DATE getting string — NULL
4. npis_refer ref_ID missing — MAX+1 logic added

---

## Branding
- Logo: `/assets/img/logo-orange.jpg` (public/subscriber), `/assets/img/logo-blue.jpg` (console)
- Footer all pages: "A product of Kariyamangalam Technologies Pvt Ltd, Chennai · Built with Claude"
- Sales: Ms. Kavitha Prakash — +91 91710 15659

---

## PLANNED FEATURE SPECS

### Registration Redesign (Day 19+)
Dependencies: Fast2SMS API key + npis_users SQL dump + WhatsApp number for paid activation

### Pricing & Payment Flow (Day 20+)
- Starter: Razorpay (instant) + Offline + Tax invoice PDF
- Premium: above + Formal quotation PDF route
- Basic credits: Coming Soon teaser

---

## Database Tables

### Core
- `npis_projects` — 40,948+ rows
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845+ rows + ref_address_id ✅
- `npt_company_addresses` — NEW ✅

### Platform
- `npt_users` — basic/starter/premium
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`

### Orders (TO CREATE)
- `npt_quotations`, `npt_orders`, `npt_payments`, `npt_invoices`

---

## Next Tasks (Priority Order)

| # | Task |
|---|------|
| 1 | Enter today's projects through new workflow — test end to end |
| 2 | Enter backlog Apr 14–23 in batches |
| 3 | Test Repository → Daily → Download → Flush workflow |
| 4 | Registration redesign (needs dependencies) |
| 5 | Pricing & payment flow |
| 6 | Orders Portal |
| 7 | Watchlist trigger on new project save |
| 8 | Client logos for homepage |

---

## Session Log
| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–8 | Early Apr | Subscriber dashboard, waitlist |
| 9–10 | 14 Apr | Admin panel, Watchlist |
| 11–12 | 15–16 Apr | Import pipeline, full dataset |
| 13 | 17–18 Apr | Admin portal. Repository → Daily → Weekly. |
| 14 | 19 Apr | Console. CIN. Companies. |
| 15 | 20 Apr | Activity tracking. Public website. |
| 16 | 21 Apr | Branding. Plan rename. 4 crud_save bugs fixed. |
| 17 | 22 Apr | Search block removed. Address button. npt_company_addresses table. |
| 18 | 23 Apr | Full project entry workflow. project_address.php. Address connection. Company roles. Delete drafts. View/Publish/Delete logic. project.php full view. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
- company_address.php and ajax_save_address.php save to npt_company_addresses — NOT npis_companies
- Only Repository projects can be published — not Drafts
- Company cannot be changed after connecting to a project
