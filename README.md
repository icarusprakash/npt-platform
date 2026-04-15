# NPT Intelligence Platform — Build README

**Last Updated: 15 April 2026 (Day 11 — Forenoon Session End)**

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **MySQL root config:** /root/.my_temp.cnf
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine
- **All dashboard files live at:** /home/newprojectstracker.in/public_html/dashboard/

---

## Source Tables — Current State
- `npis_projects` — **0 rows** (wiped, awaiting full clean import)
- `npis_companies` — **0 rows** (wiped)
- `npis_refer` — **0 rows** (wiped)
- PRIMARY KEY added on `npis_projects(proj_id)` ✅
- PRIMARY KEY added on `npis_companies(comp_id)` ✅
- **KEY RULE:** Use `proj_industry` NOT `proj_sector`
- **KEY RULE:** Project name column is `proj_project` NOT `proj_name`
- **KEY RULE:** Project primary key is `proj_id` NOT `id`

---

## Data Import — Current Status

### What happened
- Batch 0 (Jan 2024–Dec 2025) was pre-loaded — 6,511 projects
- Batch 1 (Jan 2023–Dec 2024) was imported today — caused overlap and duplicates
- Root cause: npis_projects had no PRIMARY KEY, so INSERT IGNORE did not prevent duplicates
- Decision: wipe all three tables and start fresh with one complete export

### What to do next
Ask DBA to export **complete dataset — all years 2017 to April 2026 — in one single export**. No year splitting. Three files: `npis_projects.sql`, `npis_companies.sql`, `npis_refer.sql`. Upload to Claude, clean, import once.

### Import command (use for all imports)
```bash
mysql -u newp_npt_ai_user -p'npt_ai_user@123' newp_ai_engine --default-character-set=utf8mb4 < /home/newprojectstracker.in/public_html/dumps/filename.sql 2>&1 | tail -5
```

### Import order (always)
1. npis_companies_clean.sql
2. npis_projects_clean.sql
3. npis_refer_clean.sql

### After import — verify counts
```bash
mysql -u newp_npt_ai_user -p'npt_ai_user@123' newp_ai_engine -e "
SELECT COUNT(*) as projects FROM npis_projects;
SELECT COUNT(*) as companies FROM npis_companies;
SELECT COUNT(*) as refer FROM npis_refer;
"
```

### After import — delete dump files immediately
```bash
rm /home/newprojectstracker.in/public_html/dumps/npis_companies_clean.sql
rm /home/newprojectstracker.in/public_html/dumps/npis_projects_clean.sql
rm /home/newprojectstracker.in/public_html/dumps/npis_refer_clean.sql
```

**See DATA_IMPORT_GUIDE.md for full error solutions and cleaning process.**

---

## Platform Tables (npt_ prefix)
- `npt_users` — registered users
- `npt_waitlist` — waitlist captures
- `npt_password_resets` — reset tokens
- `npt_briefcase` — saved projects (user_id, proj_id, saved_at)
- `npt_watchlist_phrases` — user watch phrases (max 5 per user)
- `npt_watchlist_projects` — matched projects per user (max 50 per user)

---

## npt_users Schema
id, email, username, password_hash, full_name, company, designation, phone,
plan_type (free/basic/premium), plan_status (active/expired/suspended),
plan_start, plan_end, credits_monthly, credits_used, credits_reset_date,
daily_limit, daily_used, daily_reset, weekly_limit, weekly_used, weekly_reset,
**state_access** (VARCHAR 500, default 'all'),
**industry_access** (VARCHAR 500, default 'all'),
source, imported_from_npt, email_verified, created_at, last_login

---

## npt_watchlist_phrases Schema
id, user_id, phrase (VARCHAR 255), phrase_type (ENUM: keyword/company/industry/state), created_at

## npt_watchlist_projects Schema
id, user_id, proj_id, matched_phrase (VARCHAR 255), matched_at, is_new (TINYINT, default 1)
- UNIQUE KEY on (user_id, proj_id)
- is_new = 1 until user visits watchlist page (powers sidebar badge)

---

## Test Users
| Email | Role | Password | Plan |
|-------|------|----------|------|
| icarusprakash@gmail.com | Super Admin | (existing) | Basic, 1000 credits |
| sbirumca85@gmail.com | Admin Manager | NPTAdmin@2026 | Basic, 1000 credits |
| revathid883@gmail.com | Test Subscriber | (existing) | Basic |
| kavi.santhi@gmail.com | Test Free User | (existing) | Free |

---

## Admin Panel
- **URL:** /dashboard/admin/
- **Allowed admins:** icarusprakash@gmail.com, sbirumca85@gmail.com
- **Files:** /dashboard/admin/index.php, /dashboard/admin/user.php

### Admin index.php features:
- Stat cards, user table, search + plan filter, quick actions

### Admin user.php features:
- 3 tabs: Activity | Subscription Controls | Access Controls
- State + industry access controls with checkbox grid
- Single save updates all fields

---

## Credit System
| Plan    | Monthly | Daily | Weekly |
|---------|---------|-------|--------|
| Free    | 5       | —     | —      |
| Basic   | 400     | 30    | 100    |
| Premium | 10,000  | 50    | 600    |

---

## Dashboard Pages — Status

| File | Status | Notes |
|------|--------|-------|
| _auth.php | ✅ | Auth guard |
| _layout.php | ✅ | Sidebar + topbar. Watchlist nav item + badge. |
| _layout_end.php | ✅ | Closes dashboard body |
| index.php | ✅ | Stat cards + recent projects |
| projects.php | ✅ | Search + filters + pagination. Enforces state_access + industry_access. |
| project.php | ✅ | 4 tabs. Credit gate. Restriction modal. |
| companies.php | ✅ | Card grid, search, state filter |
| company.php | ✅ | Company profile + associated projects |
| briefcase.php | ✅ | Saved projects with AJAX remove |
| briefcase_toggle.php | ✅ | AJAX save/unsave handler |
| watchlist.php | ✅ | Phrase manager + matched projects |
| usage.php | ✅ | Monthly/daily/weekly usage stats |
| profile.php | ✅ | Edit profile, change password |
| pricing.php | ✅ | Plans & pricing (needs layout refactor) |
| admin/index.php | ✅ | User list, stat cards, quick actions |
| admin/user.php | ✅ | Full user control panel with access controls |

---

## Watchlist Feature
- Max 5 watch phrases per user (all plan types)
- Industry + State use autocomplete; Keyword + Company are free text
- Forward-only matching — no retroactive search on existing data
- Matching logic fires when new projects added via daily CRUD (TO BUILD)
- Max 50 projects per watchlist, shows 5 most recent
- Sidebar badge shows new match count (clears on visit)

---

## Access Controls
- `state_access` and `industry_access` columns in `npt_users`
- `all` = no restriction; `gujarat,maharashtra` = comma-separated list
- Enforced in projects.php (hard WHERE clause) and project.php (restriction modal)
- Admin can set per user via admin/user.php

---

## Public Pages — REVISED SCOPE
- No individual project detail pages publicly
- Phase 3: company listing pages + product tag pages only
- Parked: states_slug.php, projects_slug.php (scrapped), companies_slug.php

---

## Razorpay Payment Integration — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending — Jp to coordinate with Shopify vendor to regenerate safely
- Build plan ready: create_order.php → pricing.php checkout → payment_verify.php → webhook

---

## Next Tasks (In Priority Order)

| # | Task | Notes |
|---|------|-------|
| 1 | **Full data import** | DBA to export complete 2017–Apr 2026 dataset in one shot. Upload to Claude → clean → import. |
| 2 | **Staff testing** | Revathi + Sbiru to test access controls. Report bugs. |
| 3 | **Daily projects CRUD** | Admin form to add projects. Triggers watchlist matching. |
| 4 | **Razorpay keys** | Jp to sort with Shopify vendor. |
| 5 | **Razorpay payment integration** | Once keys available |
| 6 | **login.php** | Set credits_reset_date on registration |
| 7 | **Email verification** | Send on register via AWS SES |
| 8 | **Pricing.php** | Refactor to shared _layout.php |
| 9 | **Public pages Phase 3** | Company + product tag pages |
| 10 | **Dashboard home enrichment** | News feed, recently added companies |

---

## Design System (Locked)
- Navy: #1a3c6e, Orange: #e87722, Gray BG: #f5f7fa
- Fonts: DM Serif Display (headings) + DM Sans (body)
- Base font size: 16px in dashboard

---

## Sidebar Nav Structure
```
MAIN:         Dashboard, Projects (Soon), Companies (Soon), Briefcase (Soon), Watchlist
INTELLIGENCE: News Feed (Soon), Analytics (Soon)
ACCOUNT:      My Profile (Soon), Plans & Pricing
Premium only: AI Apps, Workspaces
```

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1 | Early Apr | Placeholder page deployed |
| 2 | Early Apr | Waitlist landing page live |
| 3–8 | Apr | Full dashboard built |
| 9 | 14 Apr AM | Fixed admin panel. Built admin/user.php. State/industry access controls. Test briefs. |
| 10 | 14 Apr PM | Access controls enforced. Watchlist feature built. Razorpay domain verified. |
| 11 | 15 Apr AM | Data import pipeline built. Cleaning script written. Import attempted — discovered no PRIMARY KEY on npis_projects causing duplicates. Tables wiped. PRIMARY KEYs added on proj_id and comp_id. DBA to re-export complete dataset (2017–Apr 2026) as one single file. |

---

## Key Rules (Non-Negotiable)
- **newprojectstracker.com: NEVER TOUCH**
- Company names: ONLY visible on project.php. NEVER in listings.
- `proj_industry` = primary sector field (NOT proj_sector)
- `proj_project` = project name field (NOT proj_name)
- `proj_id` = primary key (NOT id)
- No public project detail pages
- Delete SQL dump files from server immediately after import
- Always verify PRIMARY KEY exists before importing: `SHOW KEYS FROM npis_projects WHERE Key_name = 'PRIMARY';`
- Never split exports by year — always import complete dataset in one shot
