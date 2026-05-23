# NPT Intelligence Platform — Build README

**Last Updated: 23 May 2026 (Day 41)**

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
13. **npis_companies** = unique company name registry ONLY. Never for display.
14. **npt_company_addresses** = complete company database. Used everywhere for display.
15. **⚠️ CPU WARNING** — npis_projects has 40,000+ rows. Full-table scans = CPU risk.
16. **proj_industry NOT proj_sector** — proj_sector NULL for ~95% of records.
17. **ref_ID** — column name in npis_refer is `ref_ID` (capital ID).
18. **proj_updateddate** — staff sets manually. Shown to subscribers as "Last Updated".
19. **Starter plan cutoff** = plan_start minus 3 months.
20. **Login URL** = /login.php (NOT /dashboard/login.php)
21. **Unconnect address** → auto-unpublishes project.
22. **Frontend developer Revathi** — working on UI redesign locally. Never touches live server until Jp approves.

---

## Project Status / Workflow Flags

| Flag | Values | Meaning |
|------|--------|---------|
| proj_show | YES / NO | Published / Draft |
| proj_repository | YES / NO | In repository (drafts only) |
| proj_today | YES / NULL | In daily newsletter folder |
| proj_weekly | YES / NULL | In weekly newsletter folder |

**Rules:**
- New project → proj_show='NO', proj_repository='YES'
- On publish → proj_show='YES', proj_repository='NO'
- Push to newsletter → proj_today='YES', proj_weekly='YES' (does NOT change proj_show)
- Flush daily → proj_today=NULL
- Flush weekly → proj_weekly=NULL
- Daily/Weekly flags are INTERNAL ONLY — subscribers see all published projects regardless

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

## Day 41 — What Was Done (23 May 2026)

### Admin projects.php revamped ✅
- Columns: Project, Industry, State, Stage, Source Tags, Taken/Updated, Status, Actions
- Sorted by proj_updateddate DESC
- Source shows proj_source_tags

### Admin companies.php improved ✅
- Add Company button, clickable names, telephone shown
- Reads address from npt_company_addresses

### dashboard/project.php — Taken On + Updated On ✅
- Fact strip now shows both dates (5 columns)
- Updated On shown in orange if different

### Repository/Daily/Weekly workflow fully rebuilt ✅
- DB cleaned: 313 stale daily/weekly flags flushed
- repository.php: shows only drafts, Publish button per project
- daily.php: two tabs — Select for Newsletter + Daily Dispatch
- weekly.php: flush sets proj_weekly=NULL
- publish.php: sets proj_repository='NO', does NOT auto-set proj_updateddate
- crud_save.php: new projects always saved as Draft

### Admin sidebar redesigned ✅
- All counts visible: Draft Queue, Published (41,304), Daily, Weekly
- All links accessible: Add Project, All Projects, Draft Queue, Published, Repository, Daily, Weekly, Projects by Day
- Single DB query for all counts

### Staff Instructions document created ✅
- NPT_Staff_Instructions.docx — full Word document for data entry staff
- Covers: Project entry, Repository, Daily workflow, Weekly workflow, Projects by Day, Date fields, Rules

---

## Admin Portal — File Status

| File | Purpose | Status |
|------|---------|--------|
| index.php | Dashboard | ✅ |
| crud.php | Add/Edit Project | ✅ |
| crud_save.php | Save handler | ✅ Fixed Day 41 |
| project.php | Project view + Connect/Unconnect | ✅ |
| project_address.php | Address connection | ✅ |
| projects.php | All projects listing | ✅ Revamped Day 41 |
| projects_by_day.php | Projects by date | ✅ |
| add_company.php | Add company | ✅ |
| company.php | Company view/edit | ✅ |
| companies.php | All companies listing | ✅ |
| company_addresses.php | Address search | ✅ |
| repository.php | Draft queue | ✅ Rebuilt Day 41 |
| daily.php | Newsletter workflow | ✅ Rebuilt Day 41 |
| weekly.php | Weekly report | ✅ Fixed Day 41 |
| publish.php | Quick publish | ✅ Fixed Day 41 |
| news.php | CapEx News list | 🔲 Check pending |
| news_crud.php | Add/edit news | ✅ |
| _layout.php | Sidebar with counts | ✅ Redesigned Day 41 |
| download.php | Excel export | ✅ |

---

## 🗓️ NEXT SESSION AGENDA (Day 42)

### Priority 1 — Pending from Day 41
1. **crud.php — Multiple company connection** — can currently only connect one company per project. Need to allow connecting EPC contractor, consultant etc. alongside promoter.
2. **news.php** — Check admin news list works correctly
3. **download.php** — Verify Excel download works for daily and weekly

### Priority 2 — Subscriber Dashboard
4. **Project PDF download** — elegant 2-page printable
5. **&apos; entity fix** — CapEx News sidebar HTML entities
6. **My Profile page** — edit profile, change password, usage stats

### Priority 3 — Pre-July 1
7. **Razorpay integration** (Key Secret pending)
8. **Registration OTP** (Fast2SMS key pending)

---

## Full Pending Task List

### 🔴 ADMIN
- crud.php multiple company connection bug
- news.php check
- download.php verify

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

---

## Key Rules (Always Apply)
- newprojectstracker.com: NEVER TOUCH
- proj_history — INTERNAL ONLY
- proj_industry NOT proj_sector
- npis_companies = name registry only, npt_company_addresses = display everywhere
- Company names never on listing pages
- Only draft projects in repository (proj_repository='YES' + proj_show='NO')
- publish.php sets proj_show='YES', proj_repository='NO' — never auto-sets proj_updateddate
- Daily/weekly flags are newsletter-only — never affect subscriber visibility
- ref_ID (capital ID) in npis_refer
- proj_updateddate — manually set by staff
- Starter cutoff = plan_start - 3 months

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–37 | Apr–17 May | Full platform build |
| 38 | 18 May | Freshness badges, date filter, starter gate |
| 39 | 20 May | Connect/Unconnect, duplicate fix, unavailable page |
| 40 | 22 May | CapEx News nav, projects_by_day.php, Revathi onboarded |
| 41 | 23 May | projects.php revamp, Taken/Updated dates, repository/daily/weekly rebuilt, sidebar redesigned, staff instructions Word doc |
