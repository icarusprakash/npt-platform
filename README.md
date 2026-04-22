# NPT Intelligence Platform — Build README

**Last Updated: 22 April 2026 (Day 17 — Session End)**

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

## Day 17 — What Was Built

### 1. crud.php — Two Changes
- **Removed** "Search Existing Projects First" block — searching done in projects.php
- **Added** green "📍 Add / Edit Address" button next to company section — only visible in edit mode

### 2. Company Address System — NEW

#### New DB Table: `npt_company_addresses`
```sql
id, comp_id, address_type, address1, address2, city, pincode, state, region,
telephone, email, website,
person1_title, person1_name, person1_designation, person1_email,
person2_title, person2_name, person2_designation, person2_email,
remarks, taken_by, taken_date, created_at
```

#### New DB Column: `npis_refer.ref_address_id`
- Links a specific address to a project-company connection
- One project → multiple companies → each with their own address

#### New File: `company_address.php`
- **URL:** `/admin/company_address.php?comp_id=X&proj_id=Y&return=URL`
- Top: Lists all existing addresses for the company
  - Green = currently connected to this project
  - Connect / Edit / Delete buttons per address
- Bottom: Add new address form (or edit selected)
- On new save: auto-connects to project if proj_id present
- Address types: Promoter, Corporate Office, Plant/Factory, Regional Office, Consultant, EPC Contractor, Other

### 3. Project Entry Workflow (Final)
```
1. /admin/crud.php → Connect company + fill project details → Save
2. Draft Queue → project.php?id=X
3. Full Edit → crud.php?edit=X
4. Click "📍 Add / Edit Address"
5. Check existing addresses → Connect if match
   OR add new address → auto-connects
6. Return to project → Publish
```

---

## Tomorrow's Agenda (Day 18)

### 1. Test Address System (First Thing)
- Open existing project → Full Edit → click "📍 Add / Edit Address"
- Add a new address → verify it saves and appears in list
- Click Connect → verify green highlight
- Edit an address → verify changes saved
- Delete a test address

### 2. Add Addresses Section to project.php
- project.php currently does NOT show connected addresses
- Need new block: "CONNECTED ADDRESSES" showing each company's address
- Allows researcher to see all address info without going to Full Edit

### 3. Remaining crud.php Field Changes
- Jp to specify field-level changes at start of session

### 4. Test Full Daily Workflow
- Add projects → Repository → select 15 → Move to Daily & Publish
- Download Excel → Flush Daily → verify Weekly accumulates

### 5. Backlog Data Entry (if time)
- Enter backlog projects Apr 14–21 with correct taken dates

---

## NPT Admin Portal — All Files

| File | Purpose |
|------|---------|
| login.php | Login |
| _auth.php | Auth guard |
| _layout.php | Sidebar |
| _layout_end.php | Footer |
| index.php | Dashboard |
| projects.php | Projects list |
| project.php | Project view + Quick Edit |
| crud.php | Full entry/edit form |
| crud_save.php | Save handler |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add company |
| company_address.php | ✅ NEW — Address management |
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
Dependencies: Fast2SMS key + npis_users SQL dump + WhatsApp number

### Pricing & Payment Flow (Day 20+)
Starter: Razorpay + Offline + Tax invoice PDF
Premium: above + Formal quotation PDF
Basic credits: Coming Soon

---

## Database Tables

### Core
- `npis_projects` — 40,948+ rows
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845+ rows + ref_address_id column ✅
- `npt_company_addresses` — NEW ✅

### Platform
- `npt_users`, `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`

### Orders (TO CREATE)
- `npt_quotations`, `npt_orders`, `npt_payments`, `npt_invoices`

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
| 17 | 22 Apr | Search block removed. Address button added. npt_company_addresses table created. company_address.php built. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
- company_address.php saves to npt_company_addresses — NOT npis_companies
