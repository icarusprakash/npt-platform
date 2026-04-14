# NPT Intelligence Platform — Build README

**Last Updated: 14 April 2026 (Day 9 — Session End)**

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

## Source Tables
- `npis_projects` — 6,511 rows (2024-2025 batch only)
- `npis_companies` — 4,602 rows
- `npis_refer` — 9,226 rows (project-company relationships)
- **KEY RULE:** Use `proj_industry` NOT `proj_sector` (proj_sector is mostly NULL)
- **KEY RULE:** Project name column is `proj_project` NOT `proj_name`
- **KEY RULE:** Project primary key is `proj_id` NOT `id`

---

## Platform Tables (npt_ prefix)
- `npt_users` — registered users
- `npt_waitlist` — waitlist captures
- `npt_password_resets` — reset tokens
- `npt_briefcase` — saved projects (user_id, proj_id, saved_at)
- `npt_watchlist_phrases` — (TO BUILD) user watch phrases
- `npt_watchlist_projects` — (TO BUILD) matched projects per user

---

## npt_users Schema
id, email, username, password_hash, full_name, company, designation, phone,
plan_type (free/basic/premium), plan_status (active/expired/suspended),
plan_start, plan_end, credits_monthly, credits_used, credits_reset_date,
daily_limit, daily_used, daily_reset, weekly_limit, weekly_used, weekly_reset,
**state_access** (VARCHAR 500, default 'all'),
**industry_access** (VARCHAR 500, default 'all'),
source, imported_from_npt, email_verified, created_at, last_login

### state_access / industry_access values
- `all` = no restriction, user sees everything permitted by their plan
- `gujarat` = single state restriction
- `gujarat,maharashtra` = comma-separated multi-state restriction
- Same pattern for industry_access
- Enforced in projects.php query WHERE clause
- Restriction modal shown on project.php if user tries to access outside scope

---

## Test Users
| Email | Role | Password | Plan |
|-------|------|----------|------|
| icarusprakash@gmail.com | Super Admin | (existing) | Basic, 1000 credits |
| sbirumca85@gmail.com | Admin Manager | NPTAdmin@2026 | Basic, 1000 credits |
| revathid883@gmail.com | Test Subscriber | (existing) | Basic |
| kavi.santhi@gmail.com | Test Free User | (existing) | Free |

### Reset credits for any user:
```sql
UPDATE npt_users SET credits_used = 0 WHERE email = 'email@example.com';
```

---

## Admin Panel
- **URL:** /dashboard/admin/
- **Allowed admins:** icarusprakash@gmail.com, sbirumca85@gmail.com
- **Files:** /dashboard/admin/index.php, /dashboard/admin/user.php

### Admin index.php features:
- Stat cards: total users, free, basic active, premium active, projects, waitlist, saves
- User table with search + plan filter
- User name is clickable — links to user.php?id=X
- Quick actions: set plan, activate, suspend, reset credits

### Admin user.php features (NEW — Day 9):
- Profile header with avatar, plan badge, usage stats
- 3 tabs: Activity | Subscription Controls | Access Controls
- **Activity tab:** credit usage bars (monthly/daily/weekly), saved projects list
- **Subscription Controls tab:** plan type, status, start/end dates, credit limits (monthly/daily/weekly)
- **Access Controls tab (inline in Subscription Controls form):**
  - State access: All India OR restricted to selected states (checkbox grid)
  - Industry access: All Industries OR restricted to selected industries (checkbox grid)
  - Single save button updates all fields together
- Access snapshot shown in Activity tab for quick reference

---

## Credit System
| Plan    | Monthly | Daily | Weekly |
|---------|---------|-------|--------|
| Free    | 5       | —     | —      |
| Basic   | 400     | 30    | 100    |
| Premium | 10,000  | 50    | 600    |

- Credits deducted for ALL plan types on project.php load
- Free users: credit bar shown in dashboard
- Basic/Premium: no credit bar — "My Usage" link in topbar instead
- Alert shown at 80%+ daily/weekly cap for paid users
- Admin can override any limit per user via user.php

---

## Dashboard Pages — All Built & Deployed

| File | Status | Notes |
|------|--------|-------|
| _auth.php | ✅ | Auth guard — include at top of every page |
| _layout.php | ✅ | Shared sidebar + topbar + CSS. No-cache headers added. |
| _layout_end.php | ✅ | Closes dashboard body + mobile sidebar JS |
| index.php | ✅ | Stat cards + recent projects table |
| projects.php | ✅ | Search + 4 filters (industry/state/stage/cost) + pagination. Enforces state_access + industry_access. |
| project.php | ✅ | 4 tabs: Overview, Intelligence, Contacts, Related. Credit gate modal. Save to Briefcase. Restriction modal if outside access scope. |
| companies.php | ✅ | Card grid, search, state filter. Deduplicated by company name. |
| company.php | ✅ | Company profile + all associated projects from npis_refer |
| briefcase.php | ✅ | Saved projects list with AJAX remove |
| briefcase_toggle.php | ✅ | AJAX save/unsave handler |
| usage.php | ✅ | Monthly/daily/weekly usage stats + plan details |
| profile.php | ✅ | Edit profile, change password, subscription info |
| pricing.php | ✅ | Plans & pricing (legacy standalone file, needs layout refactor later) |
| watchlist.php | 🔲 TO BUILD | User watchlist — see spec below |
| admin/index.php | ✅ | Fixed Day 9. User list, stat cards, quick actions. |
| admin/user.php | ✅ | NEW Day 9. Full user control panel with access controls. |

---

## Public Pages — REVISED SCOPE (Day 9 Decision)

### What changed
Original plan included ~37,000 individual project detail pages as public SEO magnets with paywall.
**This has been scrapped.**

### New public page strategy
Only two types of public listing pages will be built:

**1. Company pages** — one page per company
- URL pattern: /companies/[company-slug]/
- Shows company name, sector, associated project count
- Lists project titles (name only, no details)
- Clicking any project → redirects to login page
- ~4,600 pages total

**2. Product tag pages** — one page per product/keyword tag
- URL pattern: /products/[tag-slug]/
- Lists projects associated with that tag (titles only)
- Clicking any project → redirects to login page
- Volume TBD based on tag taxonomy

### What this means
- No project detail content is publicly accessible
- SEO value comes from company + product tag pages driving registrations
- All project intelligence stays inside the dashboard (behind login)
- Simpler to build and maintain — no paywall complexity on public side
- Public pages to be built in Phase 3 (after payments and core dashboard complete)

### Files parked (to be refactored or replaced in Phase 3)
- /public_html/states_slug.php — parked, original design, will be reconsidered
- /public_html/projects_slug.php — parked, original design, SCRAPPED per new decision
- /public_html/companies_slug.php — parked, will be rebuilt per new spec
- /public_html/index.php — waitlist landing page (live, untouched)

---

## Watchlist Feature Spec (TO BUILD — Phase 2)

### Concept
Each user maintains a personal watchlist of up to 5 watch phrases. When new projects are uploaded matching any phrase, they are automatically added to the user's watchlist. Users log in periodically to review only matched projects without having to search.

### Rules
- Max 5 watch phrases per user (all plan types)
- Phrase types: company name, keyword, industry, or state
- Max 50 projects in watchlist at any time (oldest auto-removed when full)
- Projects listed in reverse chronological order (newest match first)
- Project viewability governed by existing access controls (state_access, industry_access, credits)
- If user has no permission to view a project, it shows in the list but is credit-gated or access-restricted on click — same behaviour as projects.php
- Available to all plan types (free, basic, premium)

### Watchlist page actions per project
- View project (subject to normal access controls)
- Push to Briefcase (AJAX toggle)
- Remove from watchlist (AJAX)

### DB tables to create
```sql
-- User watch phrases
CREATE TABLE npt_watchlist_phrases (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  phrase VARCHAR(255) NOT NULL,
  phrase_type ENUM('keyword','company','industry','state') NOT NULL DEFAULT 'keyword',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES npt_users(id) ON DELETE CASCADE
);

-- Matched projects per user
CREATE TABLE npt_watchlist_projects (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  proj_id INT NOT NULL,
  matched_phrase VARCHAR(255),
  matched_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE KEY unique_user_proj (user_id, proj_id),
  FOREIGN KEY (user_id) REFERENCES npt_users(id) ON DELETE CASCADE
);
```

### Matching logic (to run on each new project upload)
- For each active user, check their watch phrases against new project's:
  - proj_project (title) — keyword match
  - proj_company — company match
  - proj_industry — industry match
  - proj_state — state match
- If match found: INSERT into npt_watchlist_projects
- If user already has 50 projects in watchlist: delete oldest before inserting new

### Dashboard integration
- Watchlist link added to sidebar under MAIN nav
- Badge showing count of new matches since last visit
- watchlist.php page: phrase manager (add/delete phrases) + matched projects list

---

## Next Tasks (In Priority Order)

| # | Task | Notes |
|---|------|-------|
| 1 | **Enforce access controls in dashboard** | projects.php and project.php to respect state_access + industry_access from npt_users. Restriction modal on project.php. |
| 2 | **Razorpay payment integration** | Upgrade flow from pricing.php. Webhook to update plan_type + plan_status + plan_end on successful payment. |
| 3 | **Watchlist feature** | DB tables + watchlist.php + sidebar badge + matching logic hook |
| 4 | **Data import pipeline** | Import 2022-2023 batch data |
| 5 | **Daily 15 projects CRUD** | Simple admin form to add new projects daily |
| 6 | **login.php** | Set credits_reset_date on registration (register flow fix) |
| 7 | **Email verification** | Send verification email on register (AWS SES configured) |
| 8 | **Pricing.php** | Refactor to use shared _layout.php |
| 9 | **Public pages Phase 3** | Company pages + product tag pages per revised spec |
| 10 | **Dashboard home enrichment** | News feed, recently added companies once data imported |

---

## Design System (Locked)
- Navy: #1a3c6e, Orange: #e87722, Gray BG: #f5f7fa
- Fonts: DM Serif Display (headings) + DM Sans (body)
- Base font size: 16px in dashboard
- Reference: Biltrax.com (public), Adminex/ByeWind (dashboard)

---

## Sidebar Nav Structure
```
MAIN:         Dashboard, Projects, Companies, Briefcase, Watchlist (soon)
INTELLIGENCE: News Feed (soon), Analytics (soon)
ACCOUNT:      My Profile, Plans & Pricing
Premium only: AI Apps, Workspaces
```

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1 | Early Apr | Placeholder page deployed |
| 2 | Early Apr | Waitlist landing page live, PHP form writing to npt_waitlist |
| 3–8 | Apr | Full dashboard built: projects, project detail, companies, briefcase, usage, profile, pricing, auth, layout |
| 9 | 14 Apr 2026 | Fixed admin panel (joined_at column bug). Built admin/user.php — full user control panel with state/industry access controls. Added state_access + industry_access columns to npt_users. Created sbirumca85 as admin manager. Generated test brief documents for staff. Revised public pages scope. Defined watchlist feature spec. |

---

## Key Rules (Non-Negotiable)
- **newprojectstracker.com: NEVER TOUCH** (legacy site on GoDaddy)
- Company names: ONLY visible on project.php story page. NEVER in listings.
- `proj_industry` = primary sector field (NOT proj_sector)
- `proj_project` = project name field (NOT proj_name)
- `proj_id` = primary key (NOT id)
- No public project detail pages — all project content inside dashboard only
- Public listing pages (Phase 3): companies + product tags only
