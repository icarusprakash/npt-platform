# NPT Intelligence Platform — Build README

**Last Updated: 18 May 2026 (Day 38)**

---

## ⚠️ CRITICAL RULES — READ FIRST

1. **newprojectstracker.com** — NEVER touch. Legacy site. Untouchable.
2. **newprojectstracker.in** — Active build. All work happens here.
3. **proj_history** — INTERNAL ONLY. Never display to subscribers ever.
4. **proj_equipments** — Legacy field, wrong data. Ignore. Use proj_equip_tags instead.
5. **Standing instruction** — ONE page/feature at a time. No moving forward until Jp says "yes".
6. **Source field** — never changes once set. Values: migrated / lapsed / admin / self_register.
7. **No public registrations** until July 1, 2026.
8. **Email** — PHP mail() blocked. PHPMailer installed. SMTP pending (need kavita@nptonline.com creds).
9. **AI features** — all shelved until NPT 2.0 ships. Phase 2 only.
10. **localhost in PHP** — Always use `$host = 'localhost'` not IP.
11. **Web root** — `/home/newprojectstracker.in/public_html/` (NOT /var/www/)
12. **Git** — Server is NOT linked to GitHub. Files edited directly on server.
13. **npis_companies data model** — One company = multiple rows, each = different address. All linked via npis_refer with different ref_comptype values.
14. **⚠️ CPU WARNING** — npis_projects has 40,000+ rows. Any feature involving full-table search/scan MUST be flagged before building. Always use indexed fields, LIMIT clauses, avoid COUNT DISTINCT JOINs.
15. **Age badges use proj_updateddate** — not proj_takendate.
16. **Correct login URL** — `/login.php` (NOT `/dashboard/login.php`)

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live — Beta rollout active |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live — Revamp in progress |
| NPT Console | /console/ | Marketing/Sales (Kavitha) | ✅ Live |
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
- **GitHub:** https://github.com/icarusprakash/npt-platform (public — reference only)

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

## Plan Names & Access Rules

| Plan | Price | Archive Access | Project Views |
|------|-------|---------------|---------------|
| Basic | ₹0 | None | 5/month |
| Starter | ₹14,950/month | plan_start minus 3 months | Unlimited within window |
| Premium | ₹99,000/year | Full — all 40,000+ projects | Unlimited |

---

## Day 38 — What Was Done (18 May 2026)

### 1. Project Freshness Indicator System ✅

#### dashboard/projects.php
- Added `freshness()` function — calculates age from `proj_updateddate`
- Three tiers: 🟢 Fresh (≤12 months) / 🟡 Archived (1–3 years) / 🔴 Legacy (3+ years)
- Age badge shown on every project in both table and card view
- Backed up as `projects_backup.php` before changes

#### dashboard/project.php
- Warning banner on all projects older than 12 months
- 🟡 Archived banner — amber, "last tracked X months ago"
- 🔴 Legacy banner — red, "last tracked X years ago"
- "Request Latest Update →" button on all old projects
- Button logs to `npt_update_requests` table (auto-created if not exists)
- After clicking — confirmation message shown, no duplicate requests within 7 days
- Backed up as `project_backup.php` before changes

### 2. Date Filter on Projects Listing ✅

#### dashboard/projects.php
- Added second filter row below main filters
- Quick select: Last 7 / 15 / 30 days, Last 3 / 6 months, Last 1 year — auto-submits on change
- Date range picker: From Date + To Date + Apply button
- "Clear Date" button resets date filters while keeping other filters
- Both date filters based on `proj_updateddate`
- Pagination preserves all date filter parameters correctly
- Stage filter removed from main filter row (kept in DB query for URL-based filtering)

### 3. Starter Plan Archive Gate ✅

#### dashboard/_auth.php
- Added `plan_start` to session data fetch
- Session condition updated to re-fetch if `plan_start` missing
- `$auth_plan_start` now available across all dashboard pages

#### dashboard/projects.php
- `$starter_cutoff` = `plan_start - 3 months` for starter users
- Projects older than cutoff shown greyed out (opacity 0.55) with 🔒 icon
- Locked projects not clickable (pointer-events:none on cards)
- Inline message: "Outside your plan's archive window · Upgrade to Premium"
- Premium users — no restriction, full access

#### dashboard/project.php
- Starter users hitting locked project → "Archive Access Required" gate page
- Shows their archive window start date
- Shows project's last updated date
- "Upgrade to Premium →" button + "← Back to Projects" button
- No credit consumed for gated projects

### 4. Public Pages Fixes ✅

#### login.php
- Removed "Forgot your password?" link (email not configured yet)

#### register.php
- Fixed "Already have access? Sign in now" link → `/login.php` (was pointing to `/dashboard/login.php`)

---

## New DB Table Created

### npt_update_requests
```sql
CREATE TABLE IF NOT EXISTS npt_update_requests (
    id INT AUTO_INCREMENT PRIMARY KEY,
    proj_id INT NOT NULL,
    user_id INT NOT NULL,
    proj_name VARCHAR(255),
    requested_at DATETIME DEFAULT NOW(),
    status VARCHAR(20) DEFAULT 'pending'
);
```
- Auto-created on first "Request Update" button click
- Duplicate prevention: same user + same project within 7 days = ignored

---

## Subscriber Dashboard — File Status

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard home | ✅ |
| projects.php | Project listing + search + date filter + starter gate | ✅ Day 38 |
| project.php | Project story + freshness banner + request update + starter gate | ✅ Day 38 |
| companies.php | Companies listing (published only) | ✅ |
| company.php | Company detail page | ✅ |
| news.php | CapEx News listing | ✅ |
| news_article.php | CapEx News article | ✅ |
| briefcase.php | Saved projects | ✅ |
| pricing.php | Pricing plans | ✅ |
| _auth.php | Auth guard + plan_start session | ✅ Day 38 |
| _layout.php | Sidebar + topbar | ✅ |
| _layout_end.php | Footer | ✅ |

---

## Admin Revamp Status

| # | Page | Status |
|---|------|--------|
| A1 | index.php — Dashboard | ✅ Done |
| A2 | crud.php — Add/Edit Project | ✅ Done |
| A3 | add_company.php — Add Company | ✅ Done |
| A4 | company.php — View/Edit/Delete | ✅ Done |
| A5 | companies.php — All Companies | 🔲 Pending |
| A6 | projects.php — All Projects | 🔲 Pending |
| A7 | project.php — Project View | 🔲 Check/Polish |
| A8 | project_address.php | ✅ Good |
| A9 | company_addresses.php | ✅ Done |
| A10 | repository / daily / weekly | 🔲 Check |
| A11 | news_crud.php | 🔲 Check |
| A12 | news.php (admin) | 🔲 Check |

---

## Console Portal — All Files

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard — 4 source panels | ✅ |
| users.php | All users — filterable by source | ✅ |
| user.php | Individual user view/edit/delete | ✅ |
| add_user.php | Add legacy subscriber (simplified) | ✅ |
| add_user_full.php | Full add user form (parked) | 🔲 Parked |
| activity.php | Activity feed | ✅ |
| logins.php | Login history | ✅ |
| rfq.php | RFQ management | ✅ |
| _layout.php | Sidebar + topbar | ✅ |
| _layout_end.php | Footer | ✅ |
| _auth.php | Auth guard | ✅ |

---

## Full Pending Task List

### 🔴 NEXT SESSION — TOP PRIORITY
1. Testing results from migrated users — fix any issues found
2. Admin revamp — companies.php, projects.php, check repository/daily/weekly/news

### 🟠 SUBSCRIBER DASHBOARD
3. Project PDF download — elegant 2-page printable
4. Email SMTP fix (kavita@nptonline.com creds pending)
5. My Profile page
6. Downloads section
7. Book a Demo form
8. Fix &apos; entities in CapEx News sidebar

### 🟠 PRE-JULY 1
9. Registration redesign — OTP flow (Fast2SMS key pending)
10. Razorpay online payment (Key Secret pending)
11. Client logos for homepage
12. GSTIN for Kariyamangalam Technologies

### 🔵 ORDERS PORTAL (Post-Beta)
13. npt_quotations, npt_orders, npt_payments, npt_invoices
14. /orders/ portal build

### ⚫ POST-LAUNCH
15. AI Enrichment Pipeline (~$280)
16. Tag auto-population (~$8-10)
17. nptintelligence.ai domain
18. PitchOS + AI Tools
19. SEO public pages
20. CapEx News ↔ Company linkage (blog table needs company_id field)

---

## Dependencies Checklist

| Item | Needed For | Status |
|------|-----------|--------|
| kavita@nptonline.com SMTP creds | Email notifications | ⏳ Pending |
| Fast2SMS API key | Registration OTP | ⏳ Pending |
| npis_users SQL dump (GoDaddy) | Legacy user migration | ⏳ Pending |
| Razorpay Key Secret | Online payment | ⏳ Pending |
| Client logo image files | Homepage | ⏳ Pending |
| GSTIN of Kariyamangalam Technologies | Tax invoice | ⏳ Pending |
| ~$280 Anthropic API credits | AI Enrichment | 🔲 Post-launch |

---

## Database Tables

### Core
- `npis_projects` — 40,000+ rows ⚠️ CPU risk on full scans
- `npis_companies` — 36,695 rows ✅ contact data re-imported Day 37
- `npis_refer` — 50,845+ rows (ref_address_id, ref_comptype, ref_primary)
- `npt_company_addresses` ✅

### Platform
- `npt_users` — plan_type: basic/starter/premium; source: migrated/lapsed/admin/self_register
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`, `npt_rfq`
- `npt_update_requests` ✅ created Day 38

### Imported
- `blog` — 6,537 CapEx news articles

---

## Key Rules (Always Apply)
- newprojectstracker.com: NEVER TOUCH
- proj_history — INTERNAL ONLY
- proj_equipments — ignore. Use proj_equip_tags
- proj_industry NOT proj_sector
- Company names never on listing pages — only on project story (credit gate)
- Address saves → npt_company_addresses, NOT npis_companies
- Only Repository projects can be published
- npis_companies: one company = multiple rows, each = different address
- ⚠️ Full table scans on npis_projects (40k rows) = CPU risk. Always flag before building.
- Age badges use proj_updateddate (not proj_takendate)
- Correct login URL = /login.php (not /dashboard/login.php)
- Starter plan cutoff = plan_start minus 3 months (rolling from user's plan start date)

---

## Bug Fixes Log

| Day | File | Bug | Fix |
|-----|------|-----|-----|
| 16 | crud_save.php | Email whitelist, extra column, date, ref_ID | Fixed |
| 21 | _auth.php, news pages | localhost corruption, missing session_start | Fixed |
| 22 | crud/project_address | Date hardcoding, updateddate, redirect | Fixed |
| 19 | crud.php | saveAndConnectCompany JS bug | Fixed |
| 33 | admin/company.php | Mixed quotes → HTTP 500 | Fixed |
| 34 | dashboard/project.php | comp_fax/email1/email2 missing from SQL | Fixed |
| 35 | console/_layout_end.php | Footer floating outside content area | Fixed |
| 35 | console/user.php | 2-col grid too narrow, credits_used editable | Fixed |
| 36 | dashboard/companies.php | Companies with no published projects showing | Fixed |
| 36 | dashboard/company.php | Address from npt_company_addresses not showing | Fixed |
| 36 | dashboard/company.php | Empty company pages accessible | Fixed |
| 38 | dashboard/_auth.php | plan_start not in session — starter gate not working | Fixed |
| 38 | dashboard/projects.php | Pagination dropping date filter params | Fixed |
| 38 | register.php | Sign in link pointing to /dashboard/login.php | Fixed |
| 38 | login.php | Forgot password link shown (email not configured) | Removed |

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
| 34 | 11 May | project.php: company name, contacts block, SQL fix. Data gap found. Backup taken |
| 35 | 15 May | Console: index.php 4 panels, user.php fixes, users.php source filter, footer fixed |
| 36 | 15 May | Console add_user.php simplified. Banner updated. companies.php + company.php fixed |
| 37 | 17 May | npis_companies contact re-import complete. Site ready for migrated user rollout |
| 38 | 18 May | Freshness badges (Fresh/Archived/Legacy). Date filter on projects listing. Starter plan archive gate (locked projects + upgrade prompt). Request Update button. login.php forgot password removed. register.php sign-in link fixed. |
