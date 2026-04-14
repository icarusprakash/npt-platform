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

---

## Platform Tables (npt_ prefix)
- `npt_users` — registered users
- `npt_waitlist` — waitlist captures
- `npt_password_resets` — reset tokens
- `npt_briefcase` — saved projects (user_id, proj_id, saved_at)

---

## npt_users Schema
id, email, username, password_hash, full_name, company, designation, phone,
plan_type (free/basic/premium), plan_status (active/expired/suspended),
plan_start, plan_end, credits_monthly, credits_used, credits_reset_date,
daily_limit, daily_used, daily_reset, weekly_limit, weekly_used, weekly_reset,
source, imported_from_npt, email_verified, created_at, last_login

---

## Test User
- Email: icarusprakash@gmail.com
- Plan: basic, 1000 credits/month
- Reset credits: `UPDATE npt_users SET credits_used = 0 WHERE email = 'icarusprakash@gmail.com';`

---

## Credit System
| Plan    | Monthly | Daily | Weekly |
|---------|---------|-------|--------|
| Free    | 5       | —     | —      |
| Basic   | 400     | 30    | 100    |
| Premium | 10,000  | 50    | 600    |

Credits deducted for ALL plan types on project.php load.
Free users: credit bar shown in dashboard.
Basic/Premium: no credit bar — "My Usage" link in topbar instead.
Alert shown at 80%+ daily/weekly cap for paid users.

---

## Dashboard Pages — All Built & Deployed

| File | Status | Notes |
|------|--------|-------|
| _auth.php | ✅ | Auth guard — include at top of every page |
| _layout.php | ✅ | Shared sidebar + topbar + CSS. No-cache headers added. |
| _layout_end.php | ✅ | Closes dashboard body + mobile sidebar JS |
| index.php | ✅ | Stat cards + recent projects table |
| projects.php | ✅ | Search + 4 filters (industry/state/stage/cost) + pagination |
| project.php | ✅ | 4 tabs: Overview, Intelligence, Contacts, Related. Credit gate modal. Save to Briefcase button. |
| companies.php | ✅ | Card grid, search, state filter. Deduplicated by company name. |
| company.php | ✅ | Company profile + all associated projects from npis_refer |
| briefcase.php | ✅ | Saved projects list with AJAX remove |
| briefcase_toggle.php | ✅ | AJAX save/unsave handler |
| usage.php | ✅ | Monthly/daily/weekly usage stats + plan details |
| profile.php | ✅ | Edit profile, change password, subscription info |
| pricing.php | ✅ | Plans & pricing (legacy standalone file, needs layout refactor later) |
| admin/index.php | ⚠️ BROKEN | File uploaded and renamed correctly. Gives "DB error". Root cause: $_SESSION['npt_user_email'] likely not being set in login.php — admin guard checks this field. Fix: verify login.php sets npt_user_email in session, or change guard to use npt_user_id instead. |

---

## IMMEDIATE FIX NEEDED — Admin Panel

**Problem:** /dashboard/admin/ gives "DB error"
**File:** /home/newprojectstracker.in/public_html/dashboard/admin/index.php
**Root cause:** Admin guard at top of file:
```php
$allowed_admins = ['icarusprakash@gmail.com'];
if (!isset($_SESSION['npt_user_id']) || !in_array($_SESSION['npt_user_email'] ?? '', $allowed_admins)) {
    header('Location: /dashboard/'); exit;
}
```
If $_SESSION['npt_user_email'] is not set, user gets redirected to /dashboard/ — but the error shown is "DB error" which means the redirect isn't happening and something else is failing.

**Fix to try:**
```bash
grep -n "npt_user_email\|npt_user_id\|npt_user_name\|npt_plan_type" /home/newprojectstracker.in/public_html/login.php
```
This will show what session variables login.php sets. Then update the admin guard to match.

---

## Key Rules (Non-Negotiable)
- **newprojectstracker.com: NEVER TOUCH** (legacy site on GoDaddy)
- Company names: ONLY visible on project.php story page. NEVER in listings.
- `proj_industry` = primary sector field (NOT proj_sector)
- No public dynamic SEO pages yet — all content inside dashboard (Phase 3 later)
- Public listing pages (states_slug.php etc) exist but are parked — not linked from anywhere

---

## Public Pages (Parked — Phase 3)
- /home/newprojectstracker.in/public_html/states_slug.php — state listing (working, parked)
- /home/newprojectstracker.in/public_html/projects_slug.php — project detail (working, parked)
- /home/newprojectstracker.in/public_html/companies_slug.php — company listing (parked)
- /home/newprojectstracker.in/public_html/index.php — waitlist landing page (live)

---

## Next Tasks (In Priority Order)
1. **Fix admin panel** — debug session variable issue (first task next session)
2. **Razorpay payment integration** — upgrade flow from pricing.php
3. **Data import pipeline** — import 2022-2023 batch data
4. **Daily 15 projects CRUD** — simple admin form to add new projects daily
5. **login.php** — update last_login timestamp on each login
6. **Register flow** — set credits_reset_date on registration
7. **Email verification** — send verification email on register (AWS SES configured)
8. **Pricing.php** — refactor to use shared _layout.php
9. **Dashboard home** — enrich with news feed, recently added companies once data imported

---

## Design System (Locked)
- Navy: #1a3c6e, Orange: #e87722, Gray BG: #f5f7fa
- Fonts: DM Serif Display (headings) + DM Sans (body)
- Base font size: 16px in dashboard
- Reference: Biltrax.com (public), Adminex/ByeWind (dashboard)

---

## Sidebar Nav Structure
MAIN: Dashboard, Projects (Soon), Companies (Soon), Briefcase (Soon)
INTELLIGENCE: News Feed (Soon), Analytics (Soon)
ACCOUNT: My Profile (Soon), Plans & Pricing
Premium only: AI Apps, Workspaces

---

## Session Variables Set by Login
(Verify by checking login.php — expected: npt_user_id, npt_user_name, npt_user_email, npt_plan_type)

