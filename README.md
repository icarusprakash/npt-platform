# NPT Intelligence Platform — Build README

**Last Updated: 15 May 2026 (Day 35)**

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

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live — Revamp in progress |
| NPT Console | /console/ | Marketing/Sales (Kavitha) | ✅ Live — Day 35 fixes done |
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

## 🔴 TOP PRIORITY — NEXT SESSION (Day 36)

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
- This is blocking subscriber rollout

**What's needed from GoDaddy developer:**
Full `npis_companies` table SQL export — upload at start of Day 36.

**Import plan (Day 36):**
1. Upload SQL to server
2. Run Python UPDATE script — fills only blank fields, never overwrites
3. Join key: `comp_id` — identical in both DBs (safe)
4. Verify with spot checks (e.g. Fermenta Biotech comp_id 15999)

**Backup location (taken Day 34):**
`/home/newprojectstracker.in/npis_companies_backup_20260511.sql` (13MB)

**Restore if anything goes wrong:**
```bash
mysql -u newp_npt_ai_user -pnpt_ai_user@123 newp_ai_engine < /home/newprojectstracker.in/npis_companies_backup_20260511.sql
```

**Status:** GoDaddy SQL export ready. Upload and run first thing Day 36.

---

## Day 35 — What Was Done (15 May 2026)

### Console Portal — Full Session

#### 1. console/index.php — Full Redesign ✅
- Replaced old generic dashboard with 4 source-based panels
- Top stat strip: Migrated / Lapsed / Manually Added / Self Registered counts
- Each panel shows: latest 5 users in reverse chronological order (name, company, plan, date added)
- "More →" button links to users.php?source=X
- "View all N users →" footer link on each panel
- Color coding: Migrated=blue, Lapsed=amber, Admin=green, Self=purple

#### 2. console/user.php — Multiple Fixes ✅

**Width fix:**
- Changed 2-column grid to single full-width column — panels now stretch across full content area
- Input boxes now full width

**Footer fix:**
- Footer was floating to the right of content in black area
- Fixed by restructuring `_layout_end.php` — footer now inside `.c-main`, renders below content

**Edit/Delete buttons:**
- Added "✏ Edit User" button — page loads in view mode (all fields disabled/greyed)
- Click Edit to enable all fields for editing
- Click again to cancel (reloads page)
- Save Changes button hidden until Edit mode activated
- Added "🗑 Delete User" button — red, asks for confirmation before deleting
- Delete removes user from `npt_users` AND `npt_activity_log`
- Redirects to users.php?deleted=1 after delete

**Credits Used field:**
- "Used This Month" field permanently non-editable even in Edit mode
- Marked with `data-readonly` — JS skips it when enabling fields
- Stays greyed out always

#### 3. console/users.php — Source Filter Added ✅
- Added "All Sources" dropdown filter alongside existing Plan and Status filters
- Options: All Sources / Migrated / Lapsed / Manually Added / Self Registered
- Wired to `source` column in `npt_users`
- Works in combination with existing search, plan, and status filters
- Pagination preserves source filter across pages
- "More →" buttons on console index link directly to users.php?source=X

#### 4. console/_layout_end.php — Footer Restructured ✅
- Footer moved inside `.c-main` div
- Now renders below all page content correctly
- Styled with white background and light border (not dark)

---

## Console Portal — All Files

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard — 4 source panels | ✅ Redesigned Day 35 |
| users.php | All users — filterable table | ✅ Source filter added Day 35 |
| user.php | Individual user view/edit/delete | ✅ Fixed Day 35 |
| add_user.php | Add new user (4 source types) | ✅ |
| activity.php | Activity feed | ✅ |
| logins.php | Login history | ✅ |
| rfq.php | RFQ management | ✅ |
| _layout.php | Sidebar + topbar | ✅ |
| _layout_end.php | Footer | ✅ Fixed Day 35 |
| _auth.php | Auth guard | ✅ |
| logout.php | Logout | ✅ |

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

## Subscriber Dashboard — Remaining Tasks

| # | Task | Status |
|---|------|--------|
| 1 | Project PDF download — elegant 2-page printable | 🔲 After data import |
| 2 | Email SMTP fix (kavita@nptonline.com creds) | 🔲 Pending creds |
| 3 | My Profile page | 🔲 Ready to build |
| 4 | Downloads section | 🔲 Ready to build |
| 5 | Book a Demo form | 🔲 Ready to build |
| 6 | Fix &apos; entities in CapEx News sidebar | 🔲 Quick fix |

---

## Full Pending Task List

### 🔴 IMMEDIATE — Day 36 First Task
1. **npis_companies contact data re-import** — comp_tel1, comp_tel2, comp_email1, comp_email2

### 🟠 AFTER DATA IMPORT
2. Project PDF download (dashboard/project.php)
3. Admin revamp — companies.php, projects.php, check repository/daily/weekly/news
4. Roll out to migrated + lapsed users

### 🟡 SUBSCRIBER DASHBOARD
5. Email SMTP fix
6. My Profile page
7. Downloads section
8. Book a Demo form
9. &apos; fix in CapEx News sidebar

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

---

## Dependencies Checklist

| Item | Needed For | Status |
|------|-----------|--------|
| npis_companies SQL from GoDaddy | Contact data re-import | ✅ Ready — upload Day 36 |
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
- `npis_projects` — 40,948+ rows
- `npis_companies` — 36,695 rows (comp_tel1/tel2/email1/email2 partially empty — fix Day 36)
- `npis_refer` — 50,845+ rows (ref_address_id, ref_comptype, ref_primary)
- `npt_company_addresses` ✅

### Platform
- `npt_users` — plan_type: basic/starter/premium; source: migrated/lapsed/admin/self_register
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`, `npt_rfq`

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
| 35 | 15 May | Console full session: index.php redesigned (4 source panels), user.php width/footer/edit/delete fixed, users.php source filter added, _layout_end.php footer fixed. GoDaddy SQL ready for Day 36. |
