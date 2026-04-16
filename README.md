# NPT Intelligence Platform — Build README

**Last Updated: 16 April 2026 (Day 12 — Session End)**

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine
- **All dashboard files:** /home/newprojectstracker.in/public_html/dashboard/

---

## Source Tables — Current State ✅
- `npis_projects` — **41,003 rows** (complete dataset 2017–Apr 13, 2026)
- `npis_companies` — **36,695 rows**
- `npis_refer` — **50,845 rows**
- PRIMARY KEY on `npis_projects(proj_id)` ✅
- PRIMARY KEY on `npis_companies(comp_id)` ✅
- Dashboard stat card shows: **40,948 projects, 43 states, 21 industries, last updated 13 Apr 2026**
- **3 days of data (Apr 14–16) pending manual entry via CRUD screen (TO BUILD)**

**KEY RULES:**
- `proj_industry` = primary sector (NOT proj_sector)
- `proj_project` = project name (NOT proj_name)
- `proj_id` = primary key (NOT id)

---

## NEXT PRIORITY — Admin CRUD Screen

**File to build:** `/dashboard/admin/crud.php`

This is the daily data entry screen for researchers. Replaces all manual SQL workflows. Every day after the platform launches, researchers use this screen to enter new projects, update existing ones, and connect companies.

---

### Researcher Workflow — Three Cases

When a researcher has a new project lead, they first search existing projects by company name and industry:

**Case 1 — Same project, old entry, updates now available**
Edit the existing entry. Record all changes. Save.
- `proj_takendate` stays unchanged
- `proj_updateddate` = today

**Case 2 — Same project, taken recently**
Skip. No action.

**Case 3 — New project**
Create new entry. All three tables written: npis_projects + npis_companies (if new) + npis_refer.
- `proj_takendate` = today
- `proj_updateddate` = today

---

### Full New Entry Flow

```
1. Search existing projects (company name + industry)
       ↓
   No match found
       ↓
2. Fill project details form (manual + AI-assisted)
       ↓
3. Click "Connect Company"
       ↓
4. Search companies DB by name
       ├── Found → Select → Auto-connects
       └── Not found → "Add New Company" mini-form
                            ↓
                       Save new company
                            ↓
                       Auto-connects to current project
       ↓
5. Save project
       ↓
   Writes: npis_projects + npis_refer (+ npis_companies if new)
       ↓
6. Entry complete ✅
```

---

### Auto-generation Rules

**proj_id:** AUTO_INCREMENT (PRIMARY KEY already set)

**proj_slug:** `{project-name-sanitized}-in-{district}-{proj_pkey_lowercase}`
Example: `new-steel-plant-in-vizag-pvv12345`

**proj_pkey:** `P` + first 2 letters of industry code + first letter of state + sequential number
Example: `PVV12345`

**proj_costrange:** Auto-calculated from proj_cost:
- < 100M → `Less than 100 Million`
- 100–999M → `From 100 Million to 999 Million`
- 1000–4999M → `From 1000 Million to 4999 Million`
- 5000–9999M → `From 5000 Million to 9999 Million`
- 10000–49999M → `From 10000 Million to 49999 Million`
- ≥ 50000M → `More than 49999 Million`

**comp_id:** AUTO_INCREMENT

**comp_slug:** Company name sanitized, lowercase, hyphenated

**comp_pkey:** `C` + first letter of company + first letter of city + sequential number

**ref_ID:** AUTO_INCREMENT

**ref_comptype:** `Promoter` (for primary company connection)

**ref_primary:** `YES` (for primary company connection)

---

### Project Fields

| Field | Source | Notes |
|-------|--------|-------|
| proj_id | Auto | PRIMARY KEY |
| proj_slug | Auto | name + district + pkey |
| proj_pkey | Auto | P + codes |
| proj_company | From refer | Pulled from connected company name |
| proj_cin | Manual | Optional |
| proj_project | Manual | Full project name |
| proj_synopsis | Manual/AI | One-line description |
| proj_teaser | Manual/AI | Short paragraph |
| proj_endproduct | Manual | Product/output |
| proj_equipments | Manual | Key equipment |
| proj_type | Dropdown | New / Expansion / Modernisation |
| proj_ownership | Dropdown | Private / Central PSU / State PSU / Government |
| proj_industry | Dropdown | Primary industry sector |
| proj_cost | Manual | Investment in millions (numeric) |
| proj_costrange | Auto | Calculated from proj_cost |
| proj_prodcap | Manual | Production capacity |
| proj_cod | Manual | Expected completion |
| proj_location | Manual | Village/area |
| proj_district | Manual | District |
| proj_state | Dropdown | State |
| proj_stage | Dropdown | Project stage |
| proj_details | Manual/AI | Full description |
| proj_source | Manual | Source of information |
| proj_reftags | Manual | Company name (for search indexing) |
| proj_takendate | Auto | First entry date — never changes on edit |
| proj_updateddate | Auto | Today on every save |
| proj_recently_viewed | Auto | Today on save |
| proj_show | Default | YES |
| proj_taken_by | Auto | Logged-in researcher name |

---

### Company Fields (for new company entry)

| Field | Source | Notes |
|-------|--------|-------|
| comp_id | Auto | PRIMARY KEY |
| comp_slug | Auto | From name |
| comp_pkey | Auto | C + letters + number |
| comp_company | Manual | Full company name |
| comp_address1 | Manual | Address line 1 |
| comp_address2 | Manual | Address line 2 |
| comp_city | Manual | City |
| comp_pincode | Manual | PIN |
| comp_state | Dropdown | State |
| comp_reftags | Auto | Company name |
| comp_takendate | Auto | Today |
| comp_updateddate | Auto | Today |
| comp_takenby | Auto | Logged-in user |

---

### Refer Entry (auto-created on connect)

| Field | Value |
|-------|-------|
| ref_ID | AUTO_INCREMENT |
| ref_compid | comp_id of connected company |
| ref_projid | proj_id of current project |
| ref_projfk | proj_pkey of current project |
| ref_compfk | comp_pkey of connected company |
| ref_comptype | Promoter |
| ref_primary | YES |

---

### CRUD Screen UI Plan

**Page:** `/dashboard/admin/crud.php`

**Section 1 — Search Panel (top)**
- Search bar: keyword / company name
- Industry dropdown filter
- Results table: proj_id, name, company, state, stage, last updated
- Per-row buttons: Edit | Skip

**Section 2 — Project Entry Form**
- All project fields
- "AI Populate" button — sends synopsis to Anthropic API → fills proj_details
- "Connect Company" button — triggers company search modal

**Section 3 — Company Search Modal (overlay)**
- Text search input
- Results list from npis_companies (live search)
- "Select" button per result → closes modal, connects company
- "Add New Company" button → expands mini-form inline
  - Fields: name, address1, address2, city, state, pincode
  - "Save & Connect" → saves company, connects, closes modal

**Section 4 — Save Bar (sticky bottom)**
- "Save as New" button (Case 3)
- "Save Update" button (Case 1)
- Validation before save
- On success: redirect to view of saved project

---

## Platform Tables (npt_ prefix)
- `npt_users` — registered users
- `npt_waitlist` — waitlist captures
- `npt_password_resets` — reset tokens
- `npt_briefcase` — saved projects
- `npt_watchlist_phrases` — user watch phrases (max 5)
- `npt_watchlist_projects` — matched projects (max 50)

---

## npt_users Schema
id, email, username, password_hash, full_name, company, designation, phone,
plan_type (free/basic/premium), plan_status (active/expired/suspended),
plan_start, plan_end, credits_monthly, credits_used, credits_reset_date,
daily_limit, daily_used, daily_reset, weekly_limit, weekly_used, weekly_reset,
state_access (VARCHAR 500, default 'all'),
industry_access (VARCHAR 500, default 'all'),
source, imported_from_npt, email_verified, created_at, last_login

---

## Test Users
| Email | Role | Password | Plan |
|-------|------|----------|------|
| icarusprakash@gmail.com | Super Admin | (existing) | Basic, 1000 credits |
| sbirumca85@gmail.com | Admin Manager | NPTAdmin@2026 | Basic, 1000 credits |
| revathid883@gmail.com | Test Subscriber | (existing) | Basic |
| kavi.santhi@gmail.com | Test Free User | (existing) | Free |

---

## Admin Panel
- **URL:** /dashboard/admin/
- **Allowed:** icarusprakash@gmail.com, sbirumca85@gmail.com
- **Built:** admin/index.php, admin/user.php
- **To build:** admin/crud.php

---

## Credit System
| Plan | Monthly | Daily | Weekly |
|------|---------|-------|--------|
| Free | 5 | — | — |
| Basic | 400 | 30 | 100 |
| Premium | 10,000 | 50 | 600 |

---

## Dashboard Pages — Status

| File | Status | Notes |
|------|--------|-------|
| _auth.php | ✅ | Auth guard |
| _layout.php | ✅ | Sidebar + topbar + watchlist badge |
| _layout_end.php | ✅ | Closes body |
| index.php | ✅ | Stat cards + recent projects. 2017–2026 label. |
| projects.php | ✅ | Search + filters + pagination + access controls |
| project.php | ✅ | 4 tabs, credit gate, restriction modal |
| companies.php | ✅ | Card grid, search, state filter |
| company.php | ✅ | Company profile + projects |
| briefcase.php | ✅ | Saved projects |
| briefcase_toggle.php | ✅ | AJAX handler |
| watchlist.php | ✅ | Phrase manager + matched projects |
| usage.php | ✅ | Usage stats |
| profile.php | ✅ | Edit profile + password |
| pricing.php | ✅ | Plans (needs layout refactor) |
| admin/index.php | ✅ | User list + stat cards |
| admin/user.php | ✅ | Full user control panel |
| **admin/crud.php** | 🔲 | **NEXT — Daily project + company entry** |

---

## Next Tasks (In Priority Order)

| # | Task | Notes |
|---|------|-------|
| 1 | **admin/crud.php** | Full spec above. Build search + entry + company connect + save. |
| 2 | **Enter Apr 14–16 backlog** | 3 days of projects via CRUD once built |
| 3 | **Watchlist trigger in CRUD** | On new project save, fire watchlist phrase matching |
| 4 | **Staff testing** | Revathi + Sbiru test access controls |
| 5 | **Razorpay keys** | Jp to sort with Shopify vendor |
| 6 | **Razorpay integration** | create_order → verify → webhook |
| 7 | **login.php** | Set credits_reset_date on registration |
| 8 | **Email verification** | AWS SES |
| 9 | **pricing.php** | Refactor to _layout.php |
| 10 | **Public pages Phase 3** | Company + product tag pages |

---

## Design System (Locked)
- Navy: #1a3c6e, Orange: #e87722, Gray BG: #f5f7fa
- Fonts: DM Serif Display (headings) + DM Sans (body)

---

## Razorpay — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending (Jp to get from Shopify vendor)

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1 | Early Apr | Placeholder deployed |
| 2 | Early Apr | Waitlist landing page live |
| 3–8 | Apr | Full dashboard built |
| 9 | 14 Apr AM | Admin panel, access controls, test users |
| 10 | 14 Apr PM | Access controls enforced, watchlist built, Razorpay domain verified |
| 11 | 15 Apr AM | Import pipeline built, PRIMARY KEY issue found, tables wiped, keys added |
| 12 | 16 Apr | Full dataset 2017–Apr 2026 imported. 41,003 projects, 36,695 companies, 50,845 refer. Dashboard live. CRUD spec locked. |

---

## Key Rules (Non-Negotiable)
- **newprojectstracker.com: NEVER TOUCH**
- Company names: ONLY on project.php, never in listings
- `proj_industry` = primary sector (NOT proj_sector)
- `proj_project` = project name (NOT proj_name)
- `proj_id` = primary key (NOT id)
- No public project detail pages
- Delete SQL dumps immediately after import
- Always verify PRIMARY KEY before importing
- Never split exports by year
