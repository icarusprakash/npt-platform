# NPT Intelligence Platform — Build README

**Last Updated: 11 May 2026 (Day 34)**

---

## ⚠️ CRITICAL RULES — READ FIRST

1. **newprojectstracker.com** — NEVER touch. Legacy site. Untouchable.
2. **newprojectstracker.in** — Active build. All work happens here.
3. **proj_history** — INTERNAL ONLY. Never display to subscribers ever.
4. **proj_equipments** — Legacy field, wrong data. Ignore. Use proj_equip_tags instead.
5. **Standing instruction** — ONE page/feature at a time. No moving forward until Jp says "yes".
6. **Admin revamp is TOP PRIORITY** — Data entry staff paused. Nothing else until admin is complete.
7. **Source field** — never changes once set. Values: migrated / lapsed / admin / self_register.
8. **No public registrations** until July 1, 2026.
9. **Email** — PHP mail() blocked. PHPMailer installed. SMTP pending (need kavita@nptonline.com creds).
10. **AI features** — all shelved until NPT 2.0 ships. Phase 2 only.
11. **localhost in PHP** — Always use `$host = 'localhost'` not IP.
12. **nptintelligence.ai** — deferred to post-launch.
13. **Web root** — `/home/newprojectstracker.in/public_html/` (NOT /var/www/)
14. **Git** — Server is NOT linked to GitHub. Files edited directly on server. GitHub is for reference only.
15. **npis_companies data model** — One company can have MULTIPLE rows, each row = a different office/plant address. All linked to a project via npis_refer with different ref_comptype values (Register Office, Plant Address, Manufacturing Unit etc.)

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live — Revamp in progress |
| NPT Console | /console/ | Marketing/Sales | ✅ Live |
| NPT Orders Portal | /orders/ | Sales & Finance | 🔲 To Build |
| Public Website | / | Public | ✅ Live |

---

## Server Details
- **Domain:** newprojectstracker.in
- **Web Root:** /home/newprojectstracker.in/public_html/
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **GitHub:** https://github.com/icarusprakash/npt-platform (public — reference only, not linked to server)

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

## ⚠️ CRITICAL PENDING OPERATION — DO FIRST ON DAY 35

### Missing Contact Data Re-Import from Legacy DB

**Background:**
During the original data import from GoDaddy legacy DB, four contact person fields were not imported:
- `comp_tel1` — Key Person 1 phone
- `comp_tel2` — Key Person 1 alternate phone
- `comp_email1` — Key Person 1 email
- `comp_email2` — Key Person 2 email

**Scale of the problem:**
- 22,827 companies have a key person name (comp_person1 not empty)
- 13,351 of those (58%) are missing both phone and email
- This is blocking subscriber rollout — contact data is NPT's most critical value

**What we need from GoDaddy developer:**
Export the full `npis_companies` table from the legacy GoDaddy DB as a SQL file and upload it here at the start of Day 35.

**What we will do with it (Day 35 plan):**
1. Upload the SQL file to the server
2. Run a Python script that parses the legacy SQL
3. For each comp_id, extract comp_tel1, comp_tel2, comp_email1, comp_email2
4. UPDATE our npis_companies ONLY where our fields are currently blank
5. Never overwrite any existing data
6. Log every row updated
7. Verify with spot checks on known companies (e.g. Fermenta Biotech comp_id 15999)

**Why the comp_id join is safe:**
The full npis_companies table was imported from GoDaddy originally — comp_id values are identical in both DBs. The join is 100% reliable.

**Backup location (taken Day 34):**
`/home/newprojectstracker.in/npis_companies_backup_20260511.sql` (13MB)

**Restore command if anything goes wrong:**
```bash
mysql -u newp_npt_ai_user -pnpt_ai_user@123 newp_ai_engine < /home/newprojectstracker.in/npis_companies_backup_20260511.sql
```

**Status:** Waiting for GoDaddy SQL export. Once uploaded, this is the FIRST task of Day 35.

---

## Day 34 — What Was Done (11 May 2026)

### dashboard/project.php — Three Changes Made

#### Change 1: Primary Company Name Below Title ✅
- Primary company name (ref_primary = YES) now displays in bold orange directly below the project title
- Falls back to first company in list if no primary found
- Style: font-size 17px, font-weight 700, color var(--orange)

#### Change 2: Elaborate Contacts Block in Left Column ✅
- Full "Promoter & Key Contacts" block moved from sidebar into left column (main content area)
- Appears before AI Intelligence section
- Each company entry (there can be multiple — e.g. Register Office, Plant, Manufacturing Unit) shows:
  - Company name (18px bold) + role badge
  - ★ Primary badge for the promoter
  - Address: full address line (address1, address2, city, state, pincode) as label-value rows
  - Tel, Fax, Email, Website — each on separate labeled rows
  - Key Person 1: name, designation, tel1, tel2, email1
  - Key Person 2: name, designation, email2
- Label column: 60px min-width, muted color — clean legacy-style layout
- Primary company: white background. Others: #fafafa background
- Dashed separator between company info and key personnel section

#### Change 3: Compact Sidebar Reference ✅
- Right sidebar now shows compact "Companies Involved" widget only
- Shows: role label, company name (orange for primary), city + state
- All detail is in the left column — sidebar is reference only

#### SQL Query Fix ✅
- Added comp_fax, comp_email1, comp_email2 to the SELECT in the companies JOIN query
- These were missing before — now all contact fields are pulled correctly

### Data Gap Discovered
- Tested with Fermenta Biotech (comp_id 15999, proj with 3 address rows)
- comp_tel1, comp_tel2, comp_email1 all blank in our DB despite having data in legacy
- Confirmed: 13,351 / 22,827 companies with key persons have no contact details
- Root cause: fields not imported during original GoDaddy migration
- Fix: re-import from legacy SQL — planned for Day 35

### npis_companies Data Model Confirmed
- Fermenta Biotech has 3 rows: comp_id 15997 (Thane), 15998 (Mandi), 15999 (Bharuch/Dahej)
- All 3 linked to same project via npis_refer with different ref_comptype
- Our display code now handles this correctly — all 3 show as separate contact cards

### Task 3 (PDF Download) — NOT YET BUILT
- Deferred to after data import is complete
- Design spec: elegant premium research report, max 2 pages, printable
- User can read the webpage OR download PDF and print
- The "Download Project PDF" button currently links to coming_soon.php — leave as is for now

---

## Complete Project Entry Workflow (Final)

```
Step 1: /admin/crud.php
  - Search/connect company OR add new (name only)
  - Fill all project fields in legacy order
  - Save New Project → auto-redirects to project_address.php

Step 2: /admin/project_address.php
  - Legacy address shown if available → "Yes — Use This Address"
  - OR add new address → auto-connects
  - Add another company if needed (EPC, Consultant etc.)
  - "Done — Move to Repository" → project goes to Repository

Step 3: /admin/projects.php
  - Draft: View + Delete
  - Repository: View + Publish + Delete
  - Published: View only

Step 4: /admin/project.php?id=X
  - Full view of all fields
  - Quick Edit on every section
  - Connected Address block
  - Full Edit button
```

---

## NPT Admin Portal — All Files

| File | Purpose | Status |
|------|---------|--------|
| login.php | Login | ✅ |
| _auth.php | Auth guard | ✅ |
| _layout.php | Sidebar | ✅ |
| _layout_end.php | Footer | ✅ |
| index.php | Dashboard | ✅ Done |
| projects.php | Projects list | 🔲 Revamp pending |
| project.php | Project view + Quick Edit | 🔲 Check/Polish |
| crud.php | Full entry/edit form | ✅ Done |
| crud_save.php | Save handler | ✅ |
| crud_search.php | AJAX project search | ✅ |
| crud_company_search.php | AJAX company search | ✅ |
| add_company.php | Standalone Add Company form | ✅ Done Day 32 |
| project_address.php | Address connection screen | ✅ Good |
| company_address.php | Company address form | ✅ |
| company_addresses.php | Standalone address search | ✅ Built Day 31 |
| ajax_save_address.php | AJAX address save | ✅ |
| ajax_update_comptype.php | AJAX company role update | ✅ |
| delete_project.php | Delete draft | ✅ |
| qe_save.php | Quick Edit AJAX | ✅ |
| repository.php | Repository bucket | 🔲 Check |
| daily.php | Daily workflow | 🔲 Check |
| weekly.php | Weekly workflow | 🔲 Check |
| companies.php | Companies list | 🔲 Revamp pending |
| company.php | Company view + edit + delete | ✅ Done Day 33 |
| news.php | CapEx News list | 🔲 Check |
| news_crud.php | Add/edit CapEx News | 🔲 Check |
| hattips.php | Hat Tips list | ✅ |

---

## Admin Revamp Status

| # | Page | Status |
|---|------|--------|
| A1 | index.php — Dashboard | ✅ Done |
| A2 | crud.php — Add/Edit Project | ✅ Done |
| A3 | add_company.php — Add Company | ✅ Done |
| A4 | company.php — View/Edit/Delete | ✅ Done |
| A5 | companies.php — All Companies | 🔲 After data import |
| A6 | projects.php — All Projects | 🔲 Pending |
| A7 | project.php — Project View | 🔲 Check/Polish |
| A8 | project_address.php | ✅ Good |
| A9 | company_addresses.php | ✅ Done Day 31 |
| A10 | repository / daily / weekly | 🔲 Check |
| A11 | news_crud.php | 🔲 Check |
| A12 | news.php (admin) | 🔲 Check |

---

## Full Pending Task List

### 🔴 IMMEDIATE — Day 35 First Task
- Legacy contact data re-import (comp_tel1, comp_tel2, comp_email1, comp_email2)

### 🟠 AFTER DATA IMPORT — Day 35 Onwards
- Project PDF download (dashboard/project.php) — elegant 2-page printable
- Admin revamp: companies.php, projects.php, check repository/daily/weekly/news
- Roll out to migrated + lapsed users

### 🟡 SUBSCRIBER DASHBOARD
- Email SMTP fix (kavita@nptonline.com creds)
- My Profile page
- Downloads section
- Book a Demo form
- Fix &apos; entities in CapEx News sidebar

### 🟠 PRE-JULY 1
- Registration redesign — OTP flow
- Razorpay online payment
- Client logos for homepage
- GSTIN for Kariyamangalam Technologies

### 🔵 ORDERS PORTAL (Post-Beta)
- npt_quotations, npt_orders, npt_payments, npt_invoices
- /orders/ portal build

### ⚫ POST-LAUNCH
- AI Enrichment Pipeline (~$280)
- Tag auto-population (~$8-10)
- nptintelligence.ai domain
- PitchOS + AI Tools
- SEO public pages

---

## Dependencies Checklist

| Item | Needed For | Status |
|------|-----------|--------|
| npis_companies SQL from GoDaddy | Contact data re-import | ⏳ **URGENT** |
| kavita@nptonline.com SMTP creds | Email | ⏳ Pending |
| Fast2SMS API key | OTP registration | ⏳ Pending |
| npis_users SQL dump (GoDaddy) | Legacy user migration | ⏳ Pending |
| Razorpay Key Secret | Online payment | ⏳ Pending |
| Client logo image files | Homepage | ⏳ Pending |
| GSTIN of Kariyamangalam Technologies | Tax invoice | ⏳ Pending |
| ~$280 Anthropic API credits | AI Enrichment | 🔲 Post-launch |

---

## Database Tables

### Core
- `npis_projects` — 40,948+ rows
- `npis_companies` — 36,695 rows (comp_tel1/tel2/email1/email2 partially empty — fix pending Day 35)
- `npis_refer` — 50,845+ rows (ref_address_id, ref_comptype, ref_primary)
- `npt_company_addresses` ✅

### Platform
- `npt_users` — plan_type: basic/starter/premium; source: migrated/lapsed/admin/self_register
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`, `npt_rfq`

### To Create
- `npt_otp_log`, `npis_legacy_users`
- `npt_quotations`, `npt_orders`, `npt_payments`, `npt_invoices`
- `npt_downloads`, `npt_demo_requests`

### Imported
- `blog` — 6,537 CapEx news articles

---

## Key Rules (Always Apply)
- newprojectstracker.com: NEVER TOUCH
- proj_history — INTERNAL ONLY
- proj_equipments — legacy field, wrong data, ignore. Use proj_equip_tags
- proj_industry NOT proj_sector (proj_sector NULL for ~95% records)
- Company names never on listing pages — only on project story (credit gate)
- Address saves → npt_company_addresses, NOT npis_companies
- Only Repository projects can be published
- Company cannot be changed after connecting to a project
- Heavy COUNT DISTINCT JOIN queries avoided — 100% CPU risk
- npis_companies: one company = multiple rows, each = different address/location

---

## Bug Fixes Log

| Day | File | Bug | Fix |
|-----|------|-----|-----|
| 16 | crud_save.php | Email whitelist, extra column, date, ref_ID | Fixed |
| 21 | _auth.php, news pages | localhost corruption, missing session_start | Fixed |
| 22 | crud/project_address | Date hardcoding, updateddate, redirect | Fixed |
| 19 | crud.php | saveAndConnectCompany JS reading deleted fields | Fixed |
| 33 | admin/company.php | Mixed quotes → HTTP 500 | Fixed |
| 34 | dashboard/project.php | comp_fax/email1/email2 missing from SQL SELECT | Fixed |

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–8 | Early Apr | Subscriber dashboard, waitlist |
| 9–10 | 14 Apr | Admin panel, Watchlist |
| 11–12 | 15–16 Apr | Import pipeline, full dataset |
| 13 | 17–18 Apr | Admin portal. Repository → Daily → Weekly |
| 14 | 19 Apr | Console. CIN. Companies |
| 15 | 20 Apr | Activity tracking. Public website |
| 16 | 21 Apr | Branding. Plan rename. 4 crud_save bugs fixed |
| 17 | 22 Apr | Address system. npt_company_addresses table |
| 18 | 23 Apr | Full workflow. project_address.php. Delete drafts |
| 19 | 25 Apr | crud.php legacy layout. Multiple bug fixes |
| 20 | 25–26 Apr | Planning. Specs: Key Persons, CapEx News, Registration, Payment |
| 21 | 27 Apr | CapEx News module. blog.sql imported |
| 23 | 28 Apr | Sidebar redesign. Dashboard home. Pricing page |
| 24 | 29 Apr | Launch strategy. Closed Beta. Public Launch July 1 |
| 25 | 30 Apr | Closed beta page. Migrated user experience |
| 26 | 2 May | Lapsed user group. Public CapEx News magazine |
| 27 | 4 May | Lapsed system. RFQ form. Payment Methods. PHPMailer |
| 28 | 4 May | AI features shelved. Console RFQ management |
| 29 | 5 May | proj_teaser box. proj_tags chips. proj_equip_tags chips |
| 30 | 6 May | Sidebar restructured. AI Tools page. project.php two-column |
| 31 | 7 May | Admin project.php all companies+addresses. company_addresses.php |
| 32 | 8 May | Admin revamp. crud.php. add_company.php. project_address.php |
| 33 | 9 May | company.php 500 fixed. Web root confirmed |
| 34 | 11 May | project.php: company name below title, elaborate contacts block, compact sidebar, SQL fix. Data gap: 13,351 companies missing contact fields. Backup taken at /home/newprojectstracker.in/npis_companies_backup_20260511.sql. Re-import planned Day 35. PDF download deferred. |
