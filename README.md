# NPT Intelligence Platform — Build README

**Last Updated: 18 April 2026 (Day 13 — Session End)**

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

### NPT Subscriber Portal
- **URL:** newprojectstracker.in/dashboard/
- **Files:** /home/newprojectstracker.in/public_html/dashboard/
- **Users:** npt_users table
- **Session:** npt_user_*
- **Purpose:** Paid subscribers browse projects

### NPT Admin Portal
- **URL:** newprojectstracker.in/admin/
- **Files:** /home/newprojectstracker.in/public_html/admin/
- **Users:** npt_admin_users table (completely separate)
- **Session:** npt_admin_*
- **Purpose:** Researchers enter/edit projects daily

Both share `newp_ai_engine` database. Admin writes, subscribers read.

---

## NPT Admin Portal — All Files

| File | Purpose |
|------|---------|
| login.php | Admin login (dark theme) |
| _auth.php | Admin auth guard |
| _layout.php | Sidebar + topbar (dark navy + red accent) |
| _layout_end.php | Closes layout |
| index.php | Dashboard — draft queue, stats, today summary |
| projects.php | All projects list — search, filter, paginate |
| project.php | Project view with Quick Edit sections |
| crud.php | Full project entry / edit form |
| crud_save.php | Save handler — new + edit |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add new company inline |
| qe_save.php | Quick Edit AJAX save |
| qe_milestone.php | Add milestone AJAX |
| repository.php | Researcher's working bucket |
| daily.php | Today's 15 published projects + download + flush |
| weekly.php | Week's accumulated projects + download + flush |
| download.php | CSV/Excel download handler |
| publish.php | Quick publish a project |
| unpublish.php | Unpublish a project |
| logout.php | Admin logout |

---

## Admin Users (npt_admin_users table)

| Email | Name | Role |
|-------|------|------|
| icarusprakash@gmail.com | Jayaprakash | superadmin |
| indscan.projects@gmail.com | Indscan Admin | superadmin |
| sbirumca85@gmail.com | Sbiru | editor |

---

## Daily Workflow (Researcher)

```
Monday — Research lead provides source links

Mon–Fri (daily):
1. Researcher adds new projects via crud.php
   → All new entries go to REPOSITORY (proj_repository = 'YES')
2. At 3:30 PM — open Repository page
3. Review entries, select 15 (spread across states/industries)
4. Click "Move to Daily & Publish"
   → proj_today = YES, proj_weekly = YES, proj_show = YES
   → Projects visible to subscribers immediately

Email Marketer (daily after 3:30 PM):
5. Open Daily page → verify 15 entries
6. Click "Download Excel" → raw data for PDF report + newsletter
7. Create PDF report and newsletter (manual)
8. Send to subscribers and prospects
9. Click "Flush Daily" → clears daily section for tomorrow

Friday:
10. Open Weekly page → 75 entries accumulated
11. Click "Download Weekly Excel"
12. Format and send weekly report on Monday morning
13. Click "Flush Weekly" → resets for next week
```

---

## Project Status Flags

| Field | Value | Meaning |
|-------|-------|---------|
| proj_repository | YES | In researcher's bucket, not published |
| proj_show | YES | Published, visible to subscribers |
| proj_show | NO/NULL | Draft, not visible |
| proj_today | YES | In today's daily section |
| proj_weekly | YES | Accumulated in weekly section |
| proj_publish_date | date | Can be past or future |

---

## Project Workflow (Three Cases)

When researcher has a new project lead:

**Case 1 — Existing project with updates**
Search → find it → Edit → update fields → Save
`proj_takendate` unchanged, `proj_updateddate` = today

**Case 2 — Recent duplicate**
Search → found recently → Skip

**Case 3 — New project**
Search → not found → Add New Entry → Connect Company → Save
Goes to Repository → reviewed → published

---

## New Fields Added to npis_projects

| Field | Type | Purpose |
|-------|------|---------|
| proj_tags | TEXT | Product tags (comma separated) |
| proj_milestones | TEXT | JSON array — milestone history |
| proj_source_tags | TEXT | Source classification tags |
| proj_publish_date | DATE | Go-live date (past or future) |

### proj_milestones JSON format
```json
[
  {"date": "2025-06-15", "event": "Financial closure achieved"},
  {"date": "2026-04-13", "event": "Trial run commenced"}
]
```
Sorted by date. Never overwrites — always appends.

### npt_source_tags table
Master list for autocomplete: id, tag_name, tag_slug, usage_count, created_at

---

## Auto-generation Rules

**proj_pkey:** `P` + comp_pkey[1] + comp_pkey[1] + proj_id
Example: comp_pkey `CDA1` → proj_pkey `PDD41004`

**proj_slug:** sanitized(proj_project + ' in ' + proj_district + ' ' + proj_pkey)

**comp_pkey:** `C` + company[0] + city[0] + comp_id

**proj_costrange:** Auto from proj_cost:
- < 100M → Less than 100 Million
- 100–999M → From 100 Million to 999 Million
- 1000–4999M → From 1000 Million to 4999 Million
- 5000–9999M → From 5000 Million to 9999 Million
- 10000–49999M → From 10000 Million to 49999 Million
- ≥ 50000M → More than 49999 Million

---

## Source Tables — Current State
- `npis_projects` — **40,948 rows** (all published, 2017–Apr 13, 2026)
- `npis_companies` — **36,695 rows**
- `npis_refer` — **50,845 rows**
- PRIMARY KEY on proj_id ✅ and comp_id ✅
- 55 unpublished entries deleted — clean slate for new workflow

**KEY RULES:**
- `proj_industry` NOT `proj_sector`
- `proj_project` NOT `proj_name`
- `proj_id` NOT `id`

---

## Staff Instructions for Monday (One-time)

1. Check legacy system for projects published Apr 14–18
2. For each one — add via NPT Admin crud.php
3. Set `proj_publish_date` to the correct date
4. Publish immediately
5. From Tuesday — use new workflow (Repository → Daily → Weekly)

---

## NPT Subscriber Portal — Pages Status

| File | Status | Notes |
|------|--------|-------|
| _auth.php | ✅ | |
| _layout.php | ✅ | |
| index.php | ✅ | Stats + recent projects |
| projects.php | ✅ | Search + filters + access controls |
| project.php | ✅ | 4 tabs, credit gate, restriction modal |
| companies.php | ✅ | |
| company.php | ✅ | |
| briefcase.php | ✅ | |
| watchlist.php | ✅ | |
| usage.php | ✅ | |
| profile.php | ✅ | |
| pricing.php | ✅ | Needs layout refactor |
| admin/index.php | ✅ | Subscriber user management |
| admin/user.php | ✅ | User control panel |

---

## Next Tasks (Priority Order)

| # | Task |
|---|------|
| 1 | **Staff Monday task** — enter Apr 14–18 backlog via NPT Admin |
| 2 | **Watchlist trigger** — fire matching on new project save in crud_save.php |
| 3 | **Razorpay keys** — Jp to get from Shopify vendor |
| 4 | **Razorpay integration** — create_order → verify → webhook |
| 5 | **login.php** — set credits_reset_date on registration |
| 6 | **Email verification** — AWS SES |
| 7 | **pricing.php** — refactor to _layout.php |
| 8 | **Public pages Phase 3** — company + product tag pages |
| 9 | **Weekly report** — fix weekly flag for legacy daily entries |

---

## Design Systems

### NPT Admin Portal
- Sidebar: #1a1a2e, Accent: #e94560 (red)
- Background: #f0f2f5

### NPT Subscriber Portal
- Navy: #1a3c6e, Orange: #e87722
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
| 12 | 16 Apr | Full dataset imported — 41,003 projects |
| 13 | 17–18 Apr | Standalone NPT Admin built. Project view + Quick Edit. Repository → Daily → Weekly workflow. Download Excel. Flush. 55 unpublished deleted. Clean slate for Monday. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- NPT Admin: /admin/ — completely separate from /dashboard/
- Unpublish only from project view page — not from listing
- proj_industry = primary sector (NOT proj_sector)
- Delete SQL dumps immediately after import
- Always verify PRIMARY KEY before importing
