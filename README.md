# NPT Intelligence Platform — Build README

**Last Updated: 17 May 2026 (Day 37)**

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
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live — Beta rollout ready |
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

## Plan Names
| Plan | Price |
|------|-------|
| Basic | ₹0 / forever |
| Starter | ₹14,950/month |
| Premium | ₹99,000/year |

---

## 🔴 TOP PRIORITY — DAY 38 (Tomorrow Morning)

### Project Freshness Indicator System

**Background:**
NPT database has 40,000+ projects going back to 2012. Old projects appear in search results alongside fresh ones, causing subscriber disappointment and sales issues. Prospects think coverage is weak when old projects don't appear, but subscribers are frustrated when they click on a 4-year-old project.

**Solution — Three-layer approach:**

**Layer 1: Age badge on every project listing card**
Based on `proj_updateddate`:
- 🟢 Fresh — updated within 12 months
- 🟡 Archived — 1 to 3 years old  
- 🔴 Legacy — 3+ years old

**Layer 2: Warning banner on old project story pages**
For projects older than 12 months — show a visible banner:
*"This project was last tracked in [year]. Our team can check for latest developments on request."*
Plus a "Request Update" button.

**Layer 3: Request Update logging**
- New table: `npt_update_requests` (proj_id, user_id, requested_at, status)
- Simple insert on button click — no email needed yet
- Internal team sees requests and prioritizes research

**Files to touch:**
- `dashboard/projects.php` — add age badge to listing cards
- `dashboard/project.php` — add warning banner + Request Update button
- DB: create `npt_update_requests` table

**Rollback guarantee:**
- Both files backed up before any changes
- Age badges are CSS/display only — zero DB changes to existing tables
- Warning banner is read-only — just reads proj_updateddate
- New table can be dropped instantly if needed
- Revert = one file restore command per file

**Age calculation field:** `proj_updateddate` (reflects last knowledge, not just entry date)

---

## Day 37 — What Was Done (17 May 2026)

### npis_companies Contact Data Re-Import ✅ COMPLETE

**What was done:**
- GoDaddy legacy `npis_companies` SQL (14MB, 36,882 rows) uploaded to server
- Python script written and executed — parsed all rows, updated 4 fields:
  - `comp_tel1` — Key Person 1 phone
  - `comp_tel2` — Key Person 1 alternate phone
  - `comp_email1` — Key Person 1 email
  - `comp_email2` — Key Person 2 email
- UPDATE logic: IF(field = '' OR field IS NULL, new_value, keep_existing) — never overwrites
- 8,229 rows processed with data, 28,652 skipped (already had data or legacy was also blank)
- 1 collation error (minor, one row) — not significant

**Verification:**
- Fermenta Biotech (comp_id 15999) confirmed: comp_tel1=022-67910888, comp_tel2=022-67910800, comp_email1=prashant.puranik@fermentabiotech.com ✅
- Subscriber dashboard company page showing correctly with all contact details ✅

**Key finding:**
- Only 3,584 companies (out of 36,695) have `comp_tel1` populated
- The legacy DB itself only had phone/email for ~10% of key persons
- Data was simply never collected for most records — not a migration issue
- Staff will fill gaps over time via admin/company.php edit form

**Cleanup:**
- SQL file deleted from public_html after import ✅
- Backup remains at: `/home/newprojectstracker.in/npis_companies_backup_20260511.sql`

**Status: COMPLETE. Site ready for migrated user rollout.**

---

## Complete Project Entry Workflow (Final)

```
Step 1: /admin/crud.php — Add project, connect company
Step 2: /admin/project_address.php — Connect address
Step 3: /admin/projects.php — Repository → Publish
Step 4: /admin/project.php?id=X — View published project
```

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

## Subscriber Dashboard — File Status

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard home | ✅ |
| projects.php | Project listing + search | ✅ — freshness badges tomorrow |
| project.php | Project story (credit-gated) | ✅ — warning banner tomorrow |
| companies.php | Companies listing | ✅ |
| company.php | Company detail page | ✅ |
| news.php | CapEx News listing | ✅ |
| news_article.php | CapEx News article | ✅ |
| briefcase.php | Saved projects | ✅ |
| pricing.php | Pricing plans | ✅ |
| _layout.php | Sidebar + topbar | ✅ |
| _layout_end.php | Footer | ✅ |
| _auth.php | Auth guard | ✅ |

---

## Full Pending Task List

### 🔴 DAY 38 — FIRST TASK
1. **Project freshness indicator system** — age badges on listing, warning banner on story, Request Update button

### 🟠 AFTER FRESHNESS SYSTEM
2. Roll out to all migrated + lapsed users
3. Admin revamp — companies.php, projects.php, check repository/daily/weekly/news
4. Project PDF download — elegant 2-page printable

### 🟡 SUBSCRIBER DASHBOARD
5. Email SMTP fix (kavita@nptonline.com creds pending)
6. My Profile page
7. Downloads section
8. Book a Demo form
9. Fix &apos; entities in CapEx News sidebar

### 🟠 PRE-JULY 1
10. Registration redesign — OTP flow (Fast2SMS key pending)
11. Razorpay online payment (Key Secret pending)
12. Client logos for homepage
13. GSTIN for Kariyamangalam Technologies

### 🔵 ORDERS PORTAL (Post-Beta)
14. npt_quotations, npt_orders, npt_payments, npt_invoices
15. /orders/ portal build

### ⚫ POST-LAUNCH
16. AI Enrichment Pipeline (~$280)
17. Tag auto-population (~$8-10)
18. nptintelligence.ai domain
19. PitchOS + AI Tools
20. SEO public pages
21. CapEx News ↔ Company linkage (blog table needs company_id field)

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

### To Create (Day 38)
- `npt_update_requests` — (proj_id, user_id, requested_at, status) for Request Update feature

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
| 36 | 15 May | Console add_user.php simplified. Banner updated. companies.php + company.php fixed |
| 37 | 17 May | npis_companies contact re-import complete (8,229 rows updated). Site ready for migrated user rollout. Project freshness system planned for Day 38. |
