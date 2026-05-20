# NPT Intelligence Platform — Build README

**Last Updated: 20 May 2026 (Day 39)**

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
14. **⚠️ CPU WARNING** — npis_projects has 40,000+ rows. Any feature involving full-table search/scan MUST be flagged before building.
15. **Age badges use proj_updateddate** — not proj_takendate.
16. **Correct login URL** — `/login.php` (NOT `/dashboard/login.php`)
17. **Address deduplication** — project_address.php now checks for existing npt_company_addresses before INSERT. Never creates duplicates.
18. **Dashboard project.php JOIN** — uses `a.id = r.ref_address_id` (connected address only), NOT `a.comp_id = c.comp_id` (all addresses).

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live — Beta rollout active |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live |
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

## Day 39 — What Was Done (20 May 2026)

### 1. Admin project.php — Connect / Unconnect Address ✅

- Added **Unconnect** button on connected address card (red)
- Added **Connect** button on unconnected address cards (green)
- Both buttons POST to `/admin/project.php?id=X` with `ref_ID` and `addr_id`
- **Unconnect** sets `ref_address_id = 0` in npis_refer
- **Unconnect also auto-unpublishes** — sets `proj_show = 'NO'` and `proj_repository = 'YES'`
- **Connect** sets `ref_address_id = addr_id` in npis_refer
- Handler uses `ref_ID` (capital ID — correct column name in npis_refer)

### 2. project_address.php — Duplicate Address Fix ✅

- Root cause: "Yes — Use This Address" was blindly INSERTing a new npt_company_addresses record every time
- Fix: Check if record already exists for comp_id before INSERT
- If exists → reuse existing record ID
- If not → create new record
- Eliminates duplicate address records going forward

### 3. dashboard/project.php — Connected Address Only ✅

- Fixed JOIN: `LEFT JOIN npt_company_addresses a ON a.id = r.ref_address_id`
- Was: `ON a.comp_id = c.comp_id` (pulled ALL addresses for company)
- Now: only the specifically connected address shows on project story page

### 4. dashboard/project.php — Project Not Available Page ✅

- Subscribers visiting URL of unpublished project now see elegant "Project Not Available" page
- Shows 📭 icon, message, "← Back to Projects" button
- No redirect to listing — graceful handling

### 5. admin/crud.php + crud_save.php — Updated Date Field ✅

- Added "Updated Date" field in project entry form (below Taken Date)
- Default = today, but editable — staff can set to intended publication date
- This date is shown to subscribers as "Last Updated"
- On INSERT: uses form value instead of hardcoded `$today`
- On EDIT: always uses form value (removed conditional logic)

---

## Address System — How It Works (Final)

```
1. Staff adds project → connects company
2. project_address.php shows legacy address from npis_companies
3. Staff clicks "Yes — Use This Address"
   → Checks if npt_company_addresses record already exists for comp_id
   → If yes: reuses existing record (no duplicate)
   → If no: creates new record
   → Sets npis_refer.ref_address_id = new/existing address ID
4. admin/project.php shows address cards with Connect/Unconnect buttons
5. dashboard/project.php shows ONLY the connected address (ref_address_id JOIN)
6. If address unconnected → project auto-unpublishes → shows "Project Not Available" to subscribers
```

---

## Admin Revamp Status

| # | Page | Status |
|---|------|--------|
| A1 | index.php — Dashboard | ✅ Done |
| A2 | crud.php — Add/Edit Project | ✅ Done + Updated Date field |
| A3 | add_company.php — Add Company | ✅ Done |
| A4 | company.php — View/Edit/Delete | ✅ Done |
| A5 | companies.php — All Companies | 🔲 Pending |
| A6 | projects.php — All Projects | 🔲 Pending |
| A7 | project.php — Project View | ✅ Connect/Unconnect added |
| A8 | project_address.php | ✅ Duplicate fix done |
| A9 | company_addresses.php | ✅ Done |
| A10 | repository / daily / weekly | 🔲 Check |
| A11 | news_crud.php | 🔲 Check |
| A12 | news.php (admin) | 🔲 Check |

---

## Subscriber Dashboard — File Status

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard home | ✅ |
| projects.php | Project listing + date filter + starter gate | ✅ |
| project.php | Project story + freshness + starter gate + unavailable page | ✅ Day 39 |
| companies.php | Companies listing (published only) | ✅ |
| company.php | Company detail page | ✅ |
| news.php | CapEx News listing | ✅ |
| briefcase.php | Saved projects | ✅ |
| pricing.php | Pricing plans | ✅ |
| _auth.php | Auth guard + plan_start session | ✅ |
| _layout.php | Sidebar + topbar | ✅ |

---

## Full Pending Task List

### 🔴 NEXT SESSION
1. Admin revamp — companies.php, projects.php listing pages
2. Check repository / daily / weekly / news pages

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
20. CapEx News ↔ Company linkage

---

## Dependencies Checklist

| Item | Needed For | Status |
|------|-----------|--------|
| kavita@nptonline.com SMTP creds | Email notifications | ⏳ Pending |
| Fast2SMS API key | Registration OTP | ⏳ Pending |
| Razorpay Key Secret | Online payment | ⏳ Pending |
| Client logo image files | Homepage | ⏳ Pending |
| GSTIN of Kariyamangalam Technologies | Tax invoice | ⏳ Pending |
| ~$280 Anthropic API credits | AI Enrichment | 🔲 Post-launch |

---

## Database Tables

### Core
- `npis_projects` — 40,000+ rows ⚠️ CPU risk on full scans
- `npis_companies` — 36,695 rows ✅ contact data re-imported Day 37
- `npis_refer` — ref_ID (capital), ref_address_id, ref_comptype, ref_primary
- `npt_company_addresses` ✅ — deduplication fixed Day 39

### Platform
- `npt_users` — plan_type: basic/starter/premium; source: migrated/lapsed/admin/self_register
- `npt_update_requests` ✅ created Day 38
- `npt_activity_log`, `npt_contact_forms`, `npt_rfq`
- `blog` — 6,537 CapEx news articles

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
| 38 | dashboard/_auth.php | plan_start not in session | Fixed |
| 38 | dashboard/projects.php | Pagination dropping date filter params | Fixed |
| 39 | project_address.php | Duplicate address records on every "Use This Address" | Fixed |
| 39 | dashboard/project.php | All company addresses showing instead of connected only | Fixed |
| 39 | admin/project.php | Connect/Unconnect handler missing, ref_id case wrong | Fixed |
| 39 | dashboard/project.php | Unpublished project URL showing blank | Fixed |
| 39 | crud_save.php | proj_updateddate hardcoded to today | Fixed |

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–8 | Early Apr | Subscriber dashboard, waitlist |
| 9–10 | 14 Apr | Admin panel, Watchlist |
| 11–15 | 15–20 Apr | Import pipeline, admin portal, console, activity, public site |
| 16 | 21 Apr | Branding. Plan rename. crud_save bugs |
| 17–18 | 22–23 Apr | Address system. project_address.php |
| 19–20 | 25–26 Apr | crud.php legacy layout. Planning session |
| 21 | 27 Apr | CapEx News module. blog.sql imported |
| 23–26 | 28 Apr–2 May | Sidebar, dashboard, launch strategy, beta page, lapsed users |
| 27–28 | 4 May | Lapsed system, RFQ, PHPMailer, AI shelved |
| 29–32 | 5–8 May | Dashboard tuning, admin revamp, address system |
| 33–34 | 9–11 May | company.php 500 fix, project.php contacts, data gap found |
| 35–36 | 15 May | Console fixes, add_user simplified, dashboard fixes |
| 37 | 17 May | npis_companies contact re-import complete |
| 38 | 18 May | Freshness badges, date filter, starter gate, login/register fixes |
| 39 | 20 May | Connect/Unconnect address, duplicate address fix, updated date field, project unavailable page, auto-unpublish on unconnect |
