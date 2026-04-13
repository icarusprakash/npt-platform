# NPT Intelligence Platform — Build README

**Last Updated: 13 April 2026 (Day 7 + Day 8 Complete)**

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **MySQL config:** /root/.my_temp.cnf
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine

---

## Source Tables
- `npis_projects` — 6,511 rows (2024-2025 batch)
- `npis_companies` — 4,602 rows
- `npis_refer` — 9,226 rows (project-company relationships)
- **KEY:** Use `proj_industry` NOT `proj_sector` (proj_sector is mostly NULL)

---

## Platform Tables
- `npt_users` — registered users
- `npt_waitlist` — waitlist captures
- `npt_password_resets` — reset tokens
- `npt_briefcase` — saved projects (user_id, proj_id, saved_at)

---

## Test User
- Email: icarusprakash@gmail.com
- Plan: basic, 1000 credits, credits_used = 0

Reset credits:
```sql
UPDATE npt_users SET credits_used = 0 WHERE email = 'icarusprakash@gmail.com';
```

---

## Dashboard Pages Built

| File | Status | Notes |
|------|--------|-------|
| _auth.php | ✅ | Auth guard — include at top of every page |
| _layout.php | ✅ | Shared sidebar + topbar. Free: credit bar. Paid: usage alert at 80%+ |
| _layout_end.php | ✅ | Closes dashboard body + mobile JS |
| index.php | ✅ | Stat cards + recent projects table |
| projects.php | ✅ | Search + 4 filters + pagination. No company names in listing |
| project.php | ✅ | 4 tabs: Overview, Intelligence, Contacts, Related. Credit gate. Save to Briefcase button |
| companies.php | ✅ | Card grid, search by name/city/group, filter by state. Deduplicated by name |
| company.php | ✅ | Company profile + all associated projects |
| briefcase.php | ✅ | Saved projects list with remove button |
| briefcase_toggle.php | ✅ | AJAX handler for save/unsave |
| usage.php | ✅ | Monthly/daily/weekly usage stats + plan details |
| pricing.php | ✅ | Plans & pricing (legacy, needs layout refactor) |

---

## Key Rules (Non-Negotiable)
- **newprojectstracker.com: NEVER TOUCH**
- Company names: ONLY visible on project story page (project.php). Never in listings.
- `proj_industry` = primary sector field (NOT proj_sector)
- Credits deducted for ALL plan types on project.php load
- Free users: credit bar in dashboard. Basic/Premium: no credit bar, "My Usage" link in topbar
- Page 1 of public listing pages: free for all. Page 2+: login required

---

## Credit System
| Plan | Monthly | Daily | Weekly |
|------|---------|-------|--------|
| Free | 5 | — | — |
| Basic | 400 | 30 | 100 |
| Premium | 10,000 | 50 | 600 |

---

## Next Session Agenda
- company.php — already built, test View Projects links from companies.php
- Profile page (my_profile.php)
- Admin panel (admin/)
- Razorpay payment integration
- Import balance data (2022-2023 batches)
- Daily 15 projects CRUD workflow
