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
13. **npis_companies data model** — One company = multiple rows, each = different address.
14. **⚠️ CPU WARNING** — npis_projects has 40,000+ rows. Full-table scans = CPU risk. Always flag before building.
15. **Age badges use proj_updateddate** — not proj_takendate.
16. **Correct login URL** — `/login.php` (NOT `/dashboard/login.php`)
17. **Address deduplication** — project_address.php checks existing record before INSERT. No duplicates.
18. **Dashboard project.php JOIN** — `a.id = r.ref_address_id` (connected address only).
19. **ref_ID** — column name in npis_refer is `ref_ID` (capital ID) — always use exact case.
20. **Unconnect address** → auto-unpublishes project (proj_show='NO', proj_repository='YES').
21. **proj_updateddate** — staff sets manually in crud.php. Shown to subscribers as "Last Updated".
22. **Starter plan cutoff** = plan_start minus 3 months (from user's plan_start date, not rolling).
23. **Technical Base Project** — "NPT 2.0 Technical Base" created on Claude.ai with NPT_Master_Technical_Document.md as Project Knowledge. Update it when architecture changes.

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
- Admin / Console / Subscriber — all portals

### Indscan (indscan.projects@gmail.com)
- Admin: NPTAdmin2026! · Console: NPTConsole2026! · Subscriber: Npt@2026

### Sbiru (sbirumca85@gmail.com)
- Admin: NPTEditor2026! · Console: NPTConsole2026!

---

## Plan Names & Access Rules

| Plan | Price | Archive Access | Credits |
|------|-------|---------------|---------|
| Basic | ₹0 | None | 5/month |
| Starter | ₹14,950/month | plan_start - 3 months | 99999 (unlimited) |
| Premium | ₹99,000/year | Full — all projects | 99999 (unlimited) |

---

## Day 39 — What Was Done (20 May 2026)

### Admin portal — project.php
- Connect/Unconnect address buttons added to each address card
- Unconnect sets ref_address_id=0 AND auto-unpublishes project
- Connect sets ref_address_id=addr_id
- Handler uses ref_ID (capital) — correct column name
- Forms POST to /admin/project.php?id=X with proj_id_hidden field

### project_address.php — Duplicate address fix
- Root cause: "Yes — Use This Address" was always INSERTing new record
- Fix: checks if npt_company_addresses record already exists for comp_id
- If exists → reuses existing ID. If not → creates new.

### dashboard/project.php — Connected address only
- JOIN fixed: `LEFT JOIN npt_company_addresses a ON a.id = r.ref_address_id`
- Was pulling all addresses for company — now pulls only the connected one

### dashboard/project.php — Project Not Available page
- Unpublished project URLs now show elegant "Project Not Available" page
- No redirect — graceful 📭 message with Back to Projects button

### admin/crud.php + crud_save.php — Updated Date field
- New "Updated Date" field in project entry form
- Default = today but editable — staff sets intended publication date
- This is what subscribers see as "Last Updated"
- INSERT and EDIT both use form value, not hardcoded today

### NPT 2.0 Technical Base — Created
- New Claude Project created: "NPT 2.0 Technical Base"
- NPT_Master_Technical_Document.md uploaded as Project Knowledge (755 lines)
- Instructions added defining purpose and usage rules
- This is the permanent technical constitution — updated only on architectural changes
- README_NPTINTEL.md remains the daily journal

---

## Admin Revamp Status

| # | Page | Status |
|---|------|--------|
| A1 | index.php — Dashboard | ✅ Done |
| A2 | crud.php — Add/Edit Project | ✅ Done + Updated Date field |
| A3 | add_company.php — Add Company | ✅ Done |
| A4 | company.php — View/Edit/Delete | ✅ Done |
| A5 | companies.php — All Companies | 🔲 **Tomorrow** |
| A6 | projects.php — All Projects listing | 🔲 **Tomorrow** |
| A7 | project.php — Project View | ✅ Connect/Unconnect done |
| A8 | project_address.php | ✅ Duplicate fix done |
| A9 | company_addresses.php | ✅ Done |
| A10 | repository.php / daily.php / weekly.php | 🔲 Check |
| A11 | news_crud.php | 🔲 Check |
| A12 | news.php (admin) | 🔲 Check |

---

## 🗓️ TOMORROW'S WORK PLAN (Day 40)

### Priority 1 — Admin Revamp (complete these)
1. **companies.php** — All companies listing with search, state filter, pagination
2. **projects.php** — All projects listing with filters (industry, state, stage, status), pagination
3. **repository.php / daily.php / weekly.php** — Check they work correctly, fix if needed
4. **news.php + news_crud.php** — Confirm admin news management works

### Priority 2 — Subscriber Dashboard
5. **Project PDF download** — elegant 2-page printable premium report
6. **&apos; entity fix** — CapEx News sidebar showing HTML entities

### Priority 3 — If time permits
7. **My Profile page** — edit profile, change password, usage stats

---

## Full Pending Task List

### 🔴 ADMIN REVAMP (In Progress)
- companies.php, projects.php listing pages
- repository/daily/weekly/news pages check

### 🟠 SUBSCRIBER DASHBOARD
- Project PDF download
- Email SMTP fix (kavita@nptonline.com creds pending)
- My Profile page
- Downloads section
- Book a Demo form
- Fix &apos; in CapEx News sidebar

### 🟠 PRE-JULY 1
- Registration redesign — OTP (Fast2SMS key pending)
- Razorpay online payment (Key Secret pending)
- Client logos for homepage
- GSTIN for Kariyamangalam Technologies

### 🔵 ORDERS PORTAL (Post-Beta)
- Tables: npt_quotations, npt_orders, npt_payments, npt_invoices
- /orders/ portal build

### ⚫ POST-LAUNCH
- AI Enrichment Pipeline (~$280)
- Tag auto-population (~$8-10)
- nptintelligence.ai domain
- PitchOS + AI Tools
- SEO public pages
- CapEx News ↔ Company linkage

---

## Dependencies Checklist

| Item | Needed For | Status |
|------|-----------|--------|
| kavita@nptonline.com SMTP creds | Email | ⏳ Pending |
| Fast2SMS API key | OTP registration | ⏳ Pending |
| Razorpay Key Secret | Online payment | ⏳ Pending |
| Client logo image files | Homepage | ⏳ Pending |
| GSTIN of Kariyamangalam Technologies | Tax invoice | ⏳ Pending |
| ~$280 Anthropic API credits | AI Enrichment | 🔲 Post-launch |

---

## Database Tables — Quick Reference

### Core
- `npis_projects` — 40,000+ rows ⚠️ CPU risk
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845+ rows (ref_ID capital, ref_address_id, ref_comptype, ref_primary)
- `npt_company_addresses` ✅

### Platform
- `npt_users` — source: migrated/lapsed/admin/self_register
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_briefcase`, `npt_rfq`
- `npt_update_requests` ✅
- `blog` — 6,537 CapEx news articles

---

## Key Rules (Always Apply)
- newprojectstracker.com: NEVER TOUCH
- proj_history — INTERNAL ONLY
- proj_industry NOT proj_sector
- proj_equipments — ignore, use proj_equip_tags
- Company names never on listing pages
- Address saves → npt_company_addresses only
- Only Repository projects can be published
- ref_ID (capital ID) in npis_refer
- Unconnect address → auto-unpublishes
- proj_updateddate — manually set by staff, shown to subscribers
- Starter cutoff = plan_start - 3 months
- Login URL = /login.php

---

## Bug Fixes Log

| Day | File | Bug | Fix |
|-----|------|-----|-----|
| 16 | crud_save.php | 4 bugs — email whitelist, column count, date, ref_ID | Fixed |
| 21 | _auth.php | localhost corruption | Fixed |
| 33 | admin/company.php | Mixed quotes → HTTP 500 | Fixed |
| 34 | dashboard/project.php | Missing contact fields in SQL | Fixed |
| 35 | console/_layout_end.php | Footer floating | Fixed |
| 36 | dashboard/companies.php | Unpublished companies showing | Fixed |
| 36 | dashboard/company.php | Wrong address source + empty pages | Fixed |
| 38 | dashboard/_auth.php | plan_start missing from session | Fixed |
| 38 | dashboard/projects.php | Pagination dropping date params | Fixed |
| 39 | project_address.php | Duplicate address on every "Use This" | Fixed |
| 39 | dashboard/project.php | All addresses showing instead of connected | Fixed |
| 39 | admin/project.php | Connect/Unconnect handler missing | Fixed |
| 39 | dashboard/project.php | Unpublished URL showing blank | Fixed |
| 39 | crud_save.php | proj_updateddate hardcoded to today | Fixed |

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–8 | Early Apr | Subscriber dashboard, waitlist |
| 9–15 | 14–20 Apr | Admin, console, import pipeline, public site |
| 16–19 | 21–25 Apr | Bug fixes, address system, crud.php |
| 20–26 | 25 Apr–2 May | Planning, CapEx News, launch strategy, beta |
| 27–28 | 4 May | Lapsed system, RFQ, PHPMailer, AI shelved |
| 29–32 | 5–8 May | Dashboard tuning, admin revamp |
| 33–34 | 9–11 May | Bugs fixed, contacts block, data gap found |
| 35–36 | 15 May | Console fixes, add_user simplified |
| 37 | 17 May | Contact data re-import complete |
| 38 | 18 May | Freshness badges, date filter, starter gate |
| 39 | 20 May | Connect/Unconnect, duplicate fix, updated date field, unavailable page, NPT 2.0 Technical Base project created |
