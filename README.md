# NPT Intelligence Platform — Build README

**Last Updated: 15 May 2026 (Day 36)**

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

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live — Beta rollout started |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live — Revamp in progress |
| NPT Console | /console/ | Marketing/Sales (Kavitha) | ✅ Live — Fixed Day 35+36 |
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

## Plan Names
| Plan | Price |
|------|-------|
| Basic | ₹0 / forever |
| Starter | ₹14,950/month |
| Premium | ₹99,000/year |

---

## 🔴 TOP PRIORITY — NEXT SESSION (Day 37)

### Missing Contact Data Re-Import from Legacy DB

**Background:**
During the original data import from GoDaddy, four contact person fields were not imported:
- `comp_tel1` — Key Person 1 phone
- `comp_tel2` — Key Person 1 alternate phone
- `comp_email1` — Key Person 1 email
- `comp_email2` — Key Person 2 email

**Scale:**
- 22,827 companies have a key person name
- 13,351 (58%) are missing both phone and email
- This is blocking full subscriber rollout

**What's needed:** Full `npis_companies` SQL export from GoDaddy — upload at start of Day 37.

**Import plan:**
1. Upload SQL to server
2. Run Python UPDATE script — fills only blank fields, never overwrites
3. Join key: `comp_id` — identical in both DBs (safe)
4. Verify with spot checks (e.g. Fermenta Biotech comp_id 15999)

**Backup location (taken Day 34):**
`/home/newprojectstracker.in/npis_companies_backup_20260511.sql` (13MB)

**Restore command:**
```bash
mysql -u newp_npt_ai_user -pnpt_ai_user@123 newp_ai_engine < /home/newprojectstracker.in/npis_companies_backup_20260511.sql
```

**Status:** GoDaddy SQL export ready. Upload and run first thing Day 37.

---

## Day 36 — What Was Done (15 May 2026)

### Console Portal

#### add_user.php — Simplified for Legacy Rollout ✅
- Old complex form saved as `add_user_full.php` (parked for later)
- New simplified form for migrated legacy subscribers only
- Fields: Full Name, Email, Phone, Company, Designation, Password
- Plan Type: Premium (Annual) or Starter (Monthly)
- Start Date + End Date — end date auto-calculated (Premium = +1 year, Starter = +1 month)
- State Access: All States OR custom checkbox selection
- Industry Access: All Industries OR custom checkbox selection
- Source: hardcoded as `migrated`
- Credits: 99999 (unlimited) — daily_limit and weekly_limit also 99999
- Plan Status: always `active` on creation
- Generate Password button included
- Info banner explains this is for legacy paid subscribers only

### Subscriber Dashboard

#### dashboard/index.php — Welcome Banner Updated ✅
- Changed "your complimentary access is ready" → "welcome to NPT Intelligence"
- New body text: beta rollout messaging — "you are among the first to access it"
- Retains WhatsApp contact line

#### dashboard/_layout.php — Button Text Updated ✅
- "Upgrade Plan" button → "Pricing Plans"

#### dashboard/companies.php — Published Projects Filter ✅
- Companies listing now only shows companies with at least one published project
- Fixed states dropdown — only shows states from companies with published projects
- WHERE clause uses EXISTS subquery against npis_refer + npis_projects (proj_show='YES')

#### dashboard/company.php — Three Fixes ✅

**Fix 1 — Redirect if no published projects:**
- Added check after fetching company — if zero published projects, redirect to companies listing
- Prevents empty company pages from appearing

**Fix 2 — Address from npt_company_addresses:**
- Company detail page now pulls from `npt_company_addresses` first (not just npis_companies)
- Shows all address records for the company (each with address_type badge)
- Shows full address, tel, email, website, key persons per address record
- Falls back to npis_companies fields if no npt_company_addresses records exist
- Contact block now spans full width (grid-column: 1/-1)

**Fix 3 — CapEx News (attempted, reverted):**
- Attempted name-based match between blog.company and npis_companies.comp_company
- Name mismatch issue (e.g. "NTPC Ltd" vs "NTPC") — query not reliable
- Reverted cleanly — no news section on company page
- To be revisited post-launch with a proper company_id linkage in blog table

---

## Console Portal — All Files

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard — 4 source panels | ✅ Day 35 |
| users.php | All users — filterable by source | ✅ Day 35 |
| user.php | Individual user view/edit/delete | ✅ Day 35 |
| add_user.php | Add legacy subscriber (simplified) | ✅ Day 36 |
| add_user_full.php | Full add user form (parked) | 🔲 Parked |
| activity.php | Activity feed | ✅ |
| logins.php | Login history | ✅ |
| rfq.php | RFQ management | ✅ |
| _layout.php | Sidebar + topbar | ✅ |
| _layout_end.php | Footer | ✅ Day 35 |
| _auth.php | Auth guard | ✅ |

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
| A9 | company_addresses.php | ✅ Done |
| A10 | repository / daily / weekly | 🔲 Check |
| A11 | news_crud.php | 🔲 Check |
| A12 | news.php (admin) | 🔲 Check |

---

## Subscriber Dashboard — File Status

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard home | ✅ |
| projects.php | Project listing + search | ✅ |
| project.php | Project story (credit-gated) | ✅ |
| companies.php | Companies listing | ✅ Fixed Day 36 |
| company.php | Company detail page | ✅ Fixed Day 36 |
| news.php | CapEx News listing | ✅ |
| news_article.php | CapEx News article | ✅ |
| briefcase.php | Saved projects | ✅ |
| pricing.php | Pricing plans | ✅ |
| coming_soon.php | Placeholder pages | ✅ |
| legacy_welcome.php | Legacy user welcome | ✅ |
| _layout.php | Sidebar + topbar | ✅ |
| _layout_end.php | Footer | ✅ |
| _auth.php | Auth guard | ✅ |

---

## Full Pending Task List

### 🔴 IMMEDIATE — Day 37 First Task
1. **npis_companies contact data re-import** — comp_tel1, comp_tel2, comp_email1, comp_email2

### 🟠 AFTER DATA IMPORT
2. Testing results from Kavitha — fix any issues found
3. Admin revamp — companies.php, projects.php, check repository/daily/weekly/news
4. Project PDF download (dashboard/project.php) — elegant 2-page printable
5. Full rollout to all migrated + lapsed users

### 🟡 SUBSCRIBER DASHBOARD
6. Email SMTP fix (kavita@nptonline.com creds pending)
7. My Profile page
8. Downloads section
9. Book a Demo form
10. Fix &apos; entities in CapEx News sidebar

### 🟠 PRE-JULY 1
11. Registration redesign — OTP flow (Fast2SMS key pending)
12. Razorpay online payment (Key Secret pending)
13. Client logos for homepage
14. GSTIN for Kariyamangalam Technologies

### 🔵 ORDERS PORTAL (Post-Beta)
15. npt_quotations, npt_orders, npt_payments, npt_invoices
16. /orders/ portal build

### ⚫ POST-LAUNCH
17. AI Enrichment Pipeline (~$280)
18. Tag auto-population (~$8-10)
19. nptintelligence.ai domain
20. PitchOS + AI Tools
21. SEO public pages
22. CapEx News ↔ Company linkage (blog table needs company_id field)

---

## Dependencies Checklist

| Item | Needed For | Status |
|------|-----------|--------|
| npis_companies SQL from GoDaddy | Contact data re-import | ✅ Ready — upload Day 37 |
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
- `npis_companies` — 36,695 rows (comp_tel1/tel2/email1/email2 partially empty — fix Day 37)
- `npis_refer` — 50,845+ rows (ref_address_id, ref_comptype, ref_primary)
- `npt_company_addresses` ✅

### Platform
- `npt_users` — plan_type: basic/starter/premium; source: migrated/lapsed/admin/self_register
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`, `npt_rfq`

### Imported
- `blog` — 6,537 CapEx news articles (no company_id — name match only, unreliable)

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
| 36 | 15 May | Console add_user.php simplified for legacy rollout. Dashboard banner + button text updated. companies.php filtered to published-only. company.php address fix + redirect fix. CapEx news attempted + reverted. CPU warning rule added. |
