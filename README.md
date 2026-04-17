# NPT Intelligence Platform — Build README

**Last Updated: 17 April 2026 (Day 13)**

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine

---

## Two Separate Systems — Same Database

### Subscriber Dashboard
- **URL:** newprojectstracker.in/dashboard/
- **Files:** /home/newprojectstracker.in/public_html/dashboard/
- **Users:** npt_users table
- **Session namespace:** npt_user_*
- **Purpose:** Paid subscribers browse projects

### Admin Panel
- **URL:** newprojectstracker.in/admin/
- **Files:** /home/newprojectstracker.in/public_html/admin/
- **Users:** npt_admin_users table (completely separate)
- **Session namespace:** npt_admin_*
- **Purpose:** Researchers enter/edit projects daily

Both systems share the same `newp_ai_engine` database. Admin writes, subscribers read.

---

## Admin Panel — Files

| File | Purpose |
|------|---------|
| login.php | Admin login (dark theme) |
| _auth.php | Admin auth guard |
| _layout.php | Admin sidebar + topbar (dark navy + red accent) |
| _layout_end.php | Closes layout |
| index.php | Dashboard — draft queue, published, stats |
| projects.php | All projects list with filters + pagination |
| project.php | **Project view page with Quick Edit** |
| crud.php | Full project entry / edit form |
| crud_save.php | Save handler (new + edit) |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add new company |
| qe_save.php | Quick Edit AJAX save handler |
| qe_milestone.php | Add milestone AJAX handler |
| publish.php | Quick publish handler |
| unpublish.php | Quick unpublish handler |
| logout.php | Admin logout |

---

## Admin Users (npt_admin_users table)

| Email | Name | Role | Password |
|-------|------|------|---------|
| icarusprakash@gmail.com | Jayaprakash | superadmin | (set) |
| indscan.projects@gmail.com | Indscan Admin | superadmin | (set) |
| sbirumca85@gmail.com | Sbiru | editor | NPTEditor2026! |

---

## Project Workflow (Daily)

```
Researcher → crud.php (search first)
    ↓
Case 1: Found, old entry with updates → Edit → Save (takendate preserved)
Case 2: Found, recent duplicate → Skip
Case 3: Not found → New entry → Connect company → Save
    ↓
project.php (view page)
    ↓
Reviewer → Publish Now button
    ↓
Visible to subscribers
```

---

## Project Status Flow
- **Draft** (proj_show = 'NO') — researcher saved, not visible
- **Published** (proj_show = 'YES') — reviewer approved, visible to subscribers
- **proj_publish_date** — can be past or future date

---

## New Fields Added to npis_projects

| Field | Type | Purpose |
|-------|------|---------|
| proj_tags | TEXT | Product tags (comma separated) |
| proj_milestones | TEXT | JSON array of {date, event} milestone history |
| proj_source_tags | TEXT | Source classification tags (comma separated) |
| proj_publish_date | DATE | When project goes live (past or future) |

### npt_source_tags table
Master list of source tags for autocomplete.
Columns: id, tag_name, tag_slug, usage_count, created_at

### proj_milestones JSON format
```json
[
  {"date": "2025-06-15", "event": "Financial closure achieved"},
  {"date": "2026-04-13", "event": "Trial run commenced"}
]
```
Sorted by date ascending. Never overwrites — always appends.

---

## Auto-generation Rules

**proj_pkey:** `P` + comp_pkey[1] + comp_pkey[1] + proj_id
Example: Company pkey `CDA1` → proj_pkey `PDD41004`

**proj_slug:** sanitized(proj_project + ' in ' + proj_district + ' ' + proj_pkey_lowercase)

**comp_pkey:** `C` + company[0] + city[0] + comp_id

**comp_slug:** sanitized company name

**proj_costrange:** Auto-calculated from proj_cost:
- < 100M → Less than 100 Million
- 100–999M → From 100 Million to 999 Million
- 1000–4999M → From 1000 Million to 4999 Million
- 5000–9999M → From 5000 Million to 9999 Million
- 10000–49999M → From 10000 Million to 49999 Million
- ≥ 50000M → More than 49999 Million

---

## Source Tables — Current State
- `npis_projects` — **41,003 rows** (2017–Apr 13, 2026)
- `npis_companies` — **36,695 rows**
- `npis_refer` — **50,845 rows**
- PRIMARY KEY on proj_id and comp_id ✅
- **3 days pending entry (Apr 14–16) via crud.php**

**KEY RULES:**
- `proj_industry` NOT `proj_sector`
- `proj_project` NOT `proj_name`
- `proj_id` NOT `id`

---

## Subscriber Dashboard — Files
- **Files:** /home/newprojectstracker.in/public_html/dashboard/
- **Admin subfolder:** dashboard/admin/ — only index.php + user.php remain (subscriber user management)

### Dashboard Pages Status

| File | Status |
|------|--------|
| _auth.php | ✅ |
| _layout.php | ✅ |
| index.php | ✅ Stats + recent projects |
| projects.php | ✅ Search + filters + access controls |
| project.php | ✅ 4 tabs, credit gate |
| companies.php | ✅ |
| company.php | ✅ |
| briefcase.php | ✅ |
| watchlist.php | ✅ |
| usage.php | ✅ |
| profile.php | ✅ |
| pricing.php | ✅ (needs layout refactor) |
| admin/index.php | ✅ Subscriber user management |
| admin/user.php | ✅ User control panel |

---

## Next Tasks (Priority Order)

| # | Task |
|---|------|
| 1 | **Test CRUD end-to-end** — enter Apr 14–16 backlog via crud.php |
| 2 | **Watchlist trigger** — fire matching when new project saved via crud_save.php |
| 3 | **Staff training** — walk Sbiru + Revathi through admin workflow |
| 4 | **Razorpay** — Jp to get new key pair from Shopify vendor |
| 5 | **Razorpay integration** — create_order → verify → webhook |
| 6 | **login.php** — set credits_reset_date on registration |
| 7 | **Email verification** — AWS SES |
| 8 | **pricing.php** — refactor to _layout.php |
| 9 | **Public pages Phase 3** — company + product tag pages |

---

## Design Systems

### Admin Panel (dark)
- Sidebar: #1a1a2e (very dark navy)
- Accent: #e94560 (red)
- Background: #f0f2f5

### Subscriber Dashboard
- Navy: #1a3c6e, Orange: #e87722, Gray BG: #f5f7fa
- Fonts: DM Serif Display + DM Sans

---

## Razorpay — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending (Jp to get from Shopify vendor)

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–2 | Early Apr | Placeholder + waitlist landing page |
| 3–8 | Apr | Full subscriber dashboard built |
| 9 | 14 Apr AM | Admin panel, access controls |
| 10 | 14 Apr PM | Watchlist, Razorpay domain verified |
| 11 | 15 Apr AM | Import pipeline, PRIMARY KEY fix |
| 12 | 16 Apr | Full dataset imported (41,003 projects). CRUD spec locked. |
| 13 | 17 Apr | Standalone admin panel built at /admin/. Login, dashboard, projects list, project view with Quick Edit, add/edit project form, milestone system, source tags. Completely separate from subscriber dashboard. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- Admin panel: /admin/ — completely separate from /dashboard/
- proj_industry = primary sector (NOT proj_sector)
- proj_project = project name (NOT proj_name)
- Delete SQL dumps immediately after import
- Always verify PRIMARY KEY before importing
