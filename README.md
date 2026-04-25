# NPT Intelligence Platform — Build README

**Last Updated: 25 April 2026 (Day 20 — Planning Session)**

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
- **GitHub:** https://github.com/icarusprakash/npt-platform

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

## Day 19 — What Was Done

### 1. crud.php — Form Restructured to Match Legacy Layout
Field order now matches legacy NPT system:
1. Project Name
2. Synopsis
3. Teaser
4. Ownership + Project Type (side by side)
5. Industry
6. Production Capacity
7. End Product
8. Investment + Cost Range (side by side, cost range is auto/readonly)
9. Expected Completion + Key Equipment (side by side)
10. Project Stage
11. Current Status
12. Product Tags
13. Full Project Details (with AI Populate button)
14. CIN Number
15. Location section (separate block): Location, District, Pincode, State, Region, Plant Address

### 2. Bug Fix — saveAndConnectCompany
JS function was trying to read deleted address fields (addr1, addr2, city etc.)
Fixed to send empty strings for those fields — company name only saves correctly now.

### 3. Cancel Button Fixed
Was pointing to `/dashboard/admin/` — now correctly points to `/admin/projects.php`

### 4. Old Description Section Removed
Duplicate Synopsis/Teaser/Details block (lines 415-440) removed — was causing double field submission risk.

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

| File | Purpose |
|------|---------|
| login.php | Login |
| _auth.php | Auth guard |
| _layout.php | Sidebar |
| _layout_end.php | Footer |
| index.php | Dashboard |
| projects.php | Projects list |
| project.php | Project view + Quick Edit |
| crud.php | Full entry/edit form (restructured Day 19) |
| crud_save.php | Save → redirects to project_address.php |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add company (name only) |
| project_address.php | Address connection screen |
| company_address.php | Company address form |
| ajax_save_address.php | AJAX address save |
| ajax_update_comptype.php | AJAX company role update |
| delete_project.php | Delete draft project |
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

## Company Address System
- `npt_company_addresses` — address records per company
- `npis_refer.ref_address_id` — links address to project-company connection
- Legacy fallback: shows `npis_companies` address if no new record exists
- "Yes — Use This Address" copies legacy to new table and connects

---

## Critical Bug Fixes Log
### Day 16 — crud_save.php
1. Email whitelist blocking indscan — removed
2. Extra '$today' (41 vs 40 columns) — removed
3. proj_recently_viewed DATE getting string — NULL
4. npis_refer ref_ID missing — MAX+1 logic added

### Day 19 — crud.php
5. saveAndConnectCompany JS reading deleted fields — fixed to send empty strings

---

## Branding
- Logo: `/assets/img/logo-orange.jpg` (public/subscriber), `/assets/img/logo-blue.jpg` (console)
- Footer: "A product of Kariyamangalam Technologies Pvt Ltd, Chennai · Built with Claude"
- Sales: Ms. Kavitha Prakash — +91 91710 15659

---

## PLANNED FEATURE SPECS

---

### SPEC 1 — Registration Redesign (Revised)

**Dependencies:**
- Fast2SMS API key (Jp to obtain)
- `npis_users` SQL dump from GoDaddy VPS (DBA to export)
- WhatsApp number for manual paid account activation message

**Registration Flow:**
```
1. User fills registration form (name, email, mobile, company, designation, password)
2. OTP sent to mobile via Fast2SMS
3. OTP verified → account created (plan_type = 'basic', unactivated)
4. Welcome splash screen shown
5. Redirected to Pricing Plans page
   - Basic (Free) → "Start Free — No credit card needed" → Activate Basic Plan
   - Starter (₹14,950) → Razorpay checkout
   - Premium (₹99,000) → Razorpay checkout
6. User MUST activate Basic plan to access free projects (5/month)
7. User can skip Basic and go directly to Starter or Premium
```

**Key UX note:** Basic plan button must say "Start Free — No credit card needed"
to avoid confusion that it costs money.

**Legacy Migration Lookup:**
- On registration, check mobile against `npis_legacy_users`
- If found + paid legacy user → show manual activation message with WhatsApp contact
- If found + free legacy user → auto-migrate, show "Welcome back" message
- If not found → fresh registration, proceed normally

**DB Changes Needed:**
- Add `phone_verified` TINYINT to `npt_users`
- Add `plan_activated` TINYINT to `npt_users` (0 = registered but not activated)
- Create `npt_otp_log` table: (id, mobile, otp, expires_at, verified, created_at)
- Import legacy users as `npis_legacy_users` (read-only reference table)

**Files to Build:**
- `register.php` — 2-step form: mobile OTP → profile completion
- `otp_send.php` — Fast2SMS API call
- `otp_verify.php` — AJAX OTP verification
- `dashboard/welcome.php` — splash screen / modal
- `dashboard/pricing.php` — plan selection page (also used post-registration)

**Console Flags:**
- `source` field in `npt_users`: `self_register` / `migrated` / `admin`
- Console shows badge: "New" or "Migrated" on user profile and list

---

### SPEC 2 — Pricing & Payment Flow

- Starter: Razorpay + Offline + Tax invoice PDF
- Premium: Razorpay + Formal quotation PDF + Tax invoice PDF
- Basic: Activate button only (no payment)
- Razorpay Key ID: rzp_live_D1cUKmkOav2Xx9
- Razorpay Key Secret: **PENDING** (Jp to retrieve from Shopify vendor)

---

### SPEC 3 — Key Persons DB (Subscriber Dashboard)

**What it is:**
A searchable directory of key personnel (decision makers) associated with
companies in the NPT database. Available to Starter and Premium subscribers only.

**Legacy reference:** Key Persons Dashboard on newprojectstracker.com
- Listing: Name, Designation, Location, State, Date
- Detail page: Company Details + Contact Coordinates

**New design:** NPT Intelligence style — upgraded from legacy
- Listing page with filters: State, City, Designation type
- Search by name or company
- Detail page: person card + company block + contact block + linked projects
- "Connected projects" section showing projects associated with their company

**Data source:** PENDING — Jp to confirm table name with DBA
(Likely a separate `npis_people` or `npis_persons` table)

**Access:** Starter + Premium only (not Basic/free)

**Files to Build (once table confirmed):**
- `dashboard/keypersons.php` — listing + filters
- `dashboard/keyperson.php?id=X` — detail page
- `admin/keypersons.php` — admin list view (future)

---

### SPEC 4 — CapEx News Module

**Three deliverables:**

**A. Public News Magazine** (`newprojectstracker.in/capex-news/`)
- Modern news/magazine UI, publicly accessible, SEO-optimised
- Article listing: headline, sector tag, date, excerpt
- Article detail: full content, social share (Facebook, Twitter, WhatsApp)
- Right sidebar: Monthly Archive, Industry filter with counts, Company search
- Clean URL structure: `/capex-news/{slug}`

**B. Searchable News Archive (Subscriber Dashboard)**
- Available to ALL users including Basic (free) — no credit consumption
- Filters: keyword search, company name search, date range, sector/industry
- Results list with headline, date, sector tag, excerpt → click to full article
- Full article view within dashboard

**C. News CRUD (NPT Admin Portal)**
- Add / Edit / Delete news stories
- Fields: Headline, Slug (auto-generated from headline), Sector, Date,
  Excerpt, Full Content, Company link (from npis_companies)
- Slug auto-generated on headline entry, editable manually
- Company linkage: connects story to npis_companies record (new — not in legacy)

**URL Strategy:**
- New site URL: `newprojectstracker.in/capex-news/{slug}`
- Legacy .com URLs redirect via 301 in .htaccess:
  `RewriteRule ^capex-news/(.*)$ https://www.newprojectstracker.in/capex-news/$1 [R=301,L]`
- Slugs must match legacy slugs exactly for seamless redirect
- Check on `news.sql` import: if slugs already stored as column → import directly;
  if generated from title on the fly → regenerate on import

**Data source:** PENDING — Jp to share `news.sql` dump from legacy system

**DB additions needed (once news.sql reviewed):**
- Add `slug` column if not present
- Add `company_id` column to link to `npis_companies`
- Confirm sector/industry field name

**Files to Build:**
- `/capex-news/index.php` — public listing page
- `/capex-news/article.php` — public article detail (slug-based routing via .htaccess)
- `dashboard/news.php` — subscriber news archive with search/filters
- `dashboard/news_article.php` — full article view inside dashboard
- `admin/news.php` — news list in admin
- `admin/news_crud.php` — add/edit news story
- `admin/news_delete.php` — delete news story

---

## Next Tasks — Priority Order

| # | Task | Dependencies | Status |
|---|------|-------------|--------|
| 1 | Test full project entry workflow with staff | Data entry operator | ⏳ Tomorrow |
| 2 | Test Repository → Daily → Download → Flush | Staff test above | ⏳ Tomorrow |
| 3 | **Key Persons DB** — dashboard listing + detail | DBA confirms table name | 🔲 Pending |
| 4 | **CapEx News Module** — all three deliverables | news.sql dump from Jp | 🔲 Pending |
| 5 | **Registration Redesign** — OTP + pricing page + activation flow | Fast2SMS key + npis_users dump | 🔲 Pending |
| 6 | **Pricing & Payment Flow** — Razorpay + invoices | Razorpay key secret | 🔲 Pending |
| 7 | **Orders Portal** — /orders/ | After payment flow | 🔲 Pending |
| 8 | Client logos for homepage | Jp to provide | 🔲 Pending |

---

## Dependencies Checklist (Jp to Arrange)

| Item | Needed For | Status |
|------|-----------|--------|
| Fast2SMS API key | Registration OTP | ⏳ Pending |
| `npis_users` SQL dump (GoDaddy) | Legacy migration | ⏳ Pending |
| WhatsApp number for paid migration message | Registration | ⏳ Pending |
| Razorpay Key Secret | Payment flow | ⏳ Pending |
| `news.sql` dump from legacy system | CapEx News module | ⏳ Pending |
| Key Persons table name from DBA | Key Persons DB | ⏳ Pending |

---

## Database Tables

### Core
- `npis_projects` — 40,948+ rows
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845+ rows + ref_address_id
- `npt_company_addresses` ✅

### Platform
- `npt_users` — plan_type: basic/starter/premium
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`

### To Create
- `npt_otp_log` — registration OTP verification
- `npis_legacy_users` — imported from GoDaddy for migration lookup
- `npt_news` — CapEx news stories (structure pending news.sql review)
- `npt_quotations`, `npt_orders`, `npt_payments`, `npt_invoices` — Orders Portal

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
| 17 | 22 Apr | Address system foundation. npt_company_addresses table. |
| 18 | 23 Apr | Full workflow. project_address.php. Delete drafts. |
| 19 | 25 Apr | crud.php restructured to legacy layout. Cancel fix. Duplicate section removed. saveAndConnectCompany bug fixed. |
| 20 | 25 Apr | Planning session. Specs drafted: Key Persons DB, CapEx News Module, Registration redesign revised (OTP + Basic activation flow). |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
- Only Repository projects can be published — not Drafts
- Company cannot be changed after connecting to a project
- address saves go to npt_company_addresses — NOT npis_companies
