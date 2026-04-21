# NPT Intelligence Platform — Build README

**Last Updated: 21 April 2026 (Day 16 — Session End)**

---

## Four Systems — One Database

| System | URL | Team | Status |
|--------|-----|------|--------|
| NPT Subscriber Portal | /dashboard/ | Paid subscribers | ✅ Live |
| NPT Admin Portal | /admin/ | Researchers | ✅ Live |
| NPT Console | /console/ | Marketing | ✅ Live |
| NPT Orders Portal | /orders/ | Sales & Finance | 🔲 To Build |
| Public Website | / | Public | ✅ Live |

---

## Server Details
- **Domain:** newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** newp_ai_engine
- **DB User:** newp_npt_ai_user / npt_ai_user@123
- **GitHub:** https://github.com/icarusprakash/npt-ai-engine

---

## Login Credentials — All Portals

### Jayaprakash (icarusprakash@gmail.com / Npt@2026)
- NPT Admin: https://newprojectstracker.in/admin/
- NPT Console: https://newprojectstracker.in/console/
- NPT Subscriber: https://newprojectstracker.in/dashboard/

### Indscan Admin (indscan.projects@gmail.com)
- NPT Admin: NPTAdmin2026!
- NPT Console: NPTConsole2026!
- NPT Subscriber: Npt@2026

### Sbiru (sbirumca85@gmail.com)
- NPT Admin: NPTEditor2026! (editor)
- NPT Console: NPTConsole2026! (marketing)

---

## Plan Names — UPDATED (Day 16)

| Old Name | New Name | Price |
|----------|----------|-------|
| Free | Basic | ₹0 / forever |
| Basic | Starter | ₹14,950/month |
| Premium | Premium | ₹99,000/year |

- DB enum: `npt_users.plan_type` = basic / starter / premium ✅
- Updated in: pricing.php, register.php, features.php, index.php, console/index.php, console/users.php

---

## Branding — UPDATED (Day 16)

### Logo
- Blue background: `/assets/img/logo-blue.jpg` — Console sidebar
- Orange background: `/assets/img/logo-orange.jpg` — all public pages, subscriber portal
- Tagline "EARLY PROJECT INTELLIGENCE" shown below logo on all pages

### Company
- NPT Intelligence is a product of **Kariyamangalam Technologies Pvt Ltd**, Chennai
- Website: https://www.kariyamangalam.in
- Footer on ALL pages (public + portals): "A product of Kariyamangalam Technologies Pvt Ltd, Chennai · Built with Claude"

### Social Links
- NPT LinkedIn: https://www.linkedin.com/company/nptris/
- NPT Twitter/X: https://x.com/NPTrackerOfcl
- Founder LinkedIn: https://www.linkedin.com/in/jayaprakashsampath

### Sales Contact
- **Ms. Kavitha Prakash** — Head of Sales
- Phone/WhatsApp: +91 91710 15659
- Prominent banner on Contact page

### Address
```
Kariyamangalam Technologies Pvt Ltd
NPT Intelligence Division
II Floor, Opp to RTO Office
9/25, Raghavendra Colony, Kaliamman Koil Street
Chinmaya Nagar, Virugambakkam
Chennai, Tamil Nadu 600092
```
Google Maps: https://maps.app.goo.gl/abGz671s6F77KTns6

---

## Critical Bug Fixes (Day 16) — crud_save.php

Four bugs were found and fixed in `/admin/crud_save.php`:

**Bug 1: Old email whitelist blocking all saves**
- Lines 3-6 had hardcoded check: only `icarusprakash@gmail.com` and `sbirumca85@gmail.com` allowed
- `indscan.projects@gmail.com` was silently redirected without saving
- Fix: Removed whitelist entirely — auth handled by `_auth.php`

**Bug 2: Extra value in INSERT (column count mismatch)**
- VALUES had 41 entries vs 40 columns
- Extra `'$today'` was the culprit
- Fix: Removed the extra `'$today'`

**Bug 3: Wrong data type for proj_recently_viewed**
- `'$taken_by_esc'` (name string) was being inserted into a DATE column
- Fix: Changed to `NULL`

**Bug 4: npis_refer INSERT missing ref_ID**
- `ref_ID` is NOT NULL with no AUTO_INCREMENT
- INSERT was failing silently
- Fix: Added `MAX(ref_ID) + 1` logic before inserting

**Result:** New project save confirmed working end-to-end:
Admin saves → project appears in Repository → Publish Now → visible in Subscriber Portal ✅

---

## Public Website — ✅ Complete

| URL | Status |
|-----|--------|
| / | ✅ Home |
| /register.php | ✅ Registration |
| /about.php | ✅ Story, timeline |
| /features.php | ✅ Feature blocks |
| /contact.php | ✅ Full address, Kavitha banner |
| /login.php | ✅ |
| /forgot-password.php | ✅ |

---

## PLANNED FEATURE SPECS

### Sprint: Registration Redesign & Legacy Migration
**Status: Planned — Day 17+**

**Dependencies to arrange BEFORE this sprint:**
1. Sign up for Fast2SMS — get API key
2. DBA to export `npis_users` table from GoDaddy as SQL dump
3. Decide WhatsApp number for paid user manual activation

**Flow:**
1. User enters name + mobile → OTP sent → verified
2. After OTP: email, company, designation, password
3. Legacy check: enter mobile → search `npis_legacy_users`
   - Free legacy user: pre-fill form → register as migrated
   - Paid legacy user: "Contact us" message → manual activation
4. Welcome splash after registration

**DB changes needed:**
- Add `phone_verified` tinyint to `npt_users`
- Create `npt_otp_log` (mobile, otp, expires_at, verified, created_at)
- Import legacy as `npis_legacy_users`
- `source` field: self_register / migrated / admin

**Files to build:**
- `register.php` — 2-step OTP flow
- `otp_send.php` — Fast2SMS API
- `otp_verify.php` — AJAX OTP check
- `dashboard/welcome.php` — splash screen

---

### Sprint: Pricing & Payment Flow
**Status: Planned — Day 18+**

**Starter Plan (₹14,950/month)**
- Online via Razorpay → instant activation
- Offline (NEFT/UPI/Cheque) → manual activation after realisation
- Tax invoice: form → PDF on Kariyamangalam letterhead → download/email

**Premium Plan (₹99,000/year)**
- Online via Razorpay → instant activation
- Offline payment → manual activation
- Formal quotation route → PDF quotation → offline sales via Kavitha → manual activation

**Project Credits (Basic plan)**
- Buy credits instead of subscription
- Packages TBD (100, 200 credits)
- "Coming Soon" teaser on pricing page

**Key principles:**
- No pre-sales pressure
- Online = instant activation (always stated clearly)
- Offline/quotation = manual activation after payment realisation

---

## NPT Admin Portal — Daily Workflow

```
Mon–Fri: Researcher adds projects → REPOSITORY (proj_repository = 'YES')
3:30 PM: Open Repository → select 15 → "Move to Daily & Publish"
         → proj_show = YES, proj_today = YES, proj_weekly = YES
         → Visible to subscribers immediately
Email marketer: Open Daily → Download Excel → create PDF report → Flush Daily
Friday: Open Weekly → Download Excel → send report Monday → Flush Weekly
```

### Project Flags
| Field | YES means |
|-------|-----------|
| proj_repository | In bucket, not published |
| proj_show | Published, visible |
| proj_today | In daily section |
| proj_weekly | In weekly section |

### Key Rules
- `proj_updateddate` changes ONLY on `proj_details` or `proj_stage` edit
- Unpublish only from project view page (never from listing)
- `proj_industry` NOT `proj_sector`
- `proj_project` NOT `proj_name`

---

## Tomorrow's Agenda — 22 April 2026 (Morning Session with Staff)

### Data Entry Backlog (with Jp + data entry staff)
1. Enter all backlog projects from Apr 14–21 (4 days) via `/admin/crud.php`
2. Pre-date each project to correct taken date
3. Publish each immediately after saving
4. Verify each appears in Subscriber Portal

### Daily Workflow Test (Afternoon)
5. Add today's fresh projects → Repository
6. Open Repository → select 15 → Move to Daily & Publish
7. Verify Daily section shows 15 projects
8. Download Excel from Daily
9. Generate PDF manually from Excel (staff task)
10. Flush Daily → verify empties
11. Verify Weekly accumulated
12. Debug any errors in real time

### If time permits
13. Test edit workflow — change proj_details → verify updateddate changes
14. Test Quick Edit on project view page
15. Test Unpublish from project view page

---

## Database Tables

### Core Data
- `npis_projects` — 40,948 rows + new entries from Day 16
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845 rows

### Platform
- `npt_users` — plan_type: basic/starter/premium ✅
- `npt_admin_users`
- `npt_console_users`
- `npt_activity_log` ✅
- `npt_contact_forms` ✅
- `npt_source_tags`
- `npt_waitlist`
- `npt_briefcase`
- `npt_watchlist_phrases`
- `npt_watchlist_projects`
- `npt_password_resets`

### Orders (TO CREATE)
- `npt_quotations`
- `npt_orders`
- `npt_payments`
- `npt_invoices`

---

## Next Tasks (Priority Order)

| # | Task |
|---|------|
| 1 | **Tomorrow AM** — backlog entry + daily workflow test with staff |
| 2 | **Client logos** — Jp to provide, add to index.php |
| 3 | **Registration redesign** — OTP + legacy migration (needs Fast2SMS + SQL dump) |
| 4 | **Pricing & payment flow** — Razorpay + quotation + credits |
| 5 | **Orders Portal** — build /orders/ |
| 6 | **Watchlist trigger** — fire on new project save |
| 7 | **Email verification** — AWS SES |
| 8 | **Retire dashboard/admin/** → Console |

---

## Design Systems
| Portal | Logo | Accent |
|--------|------|--------|
| Admin | NPT text (internal) | #e94560 red |
| Console | logo-orange.jpg | #2d6a4f green |
| Subscriber | logo-orange.jpg | #e87722 orange |
| Public | logo-orange.jpg | navy + orange |

---

## Razorpay — PENDING
- Domain verified ✅
- Key ID: rzp_live_D1cUKmkOav2Xx9
- Key Secret: pending (Jp to get from Shopify vendor)

---

## Session Log
| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–2 | Early Apr | Placeholder + waitlist |
| 3–8 | Apr | Full subscriber dashboard |
| 9 | 14 Apr AM | Admin panel, access controls |
| 10 | 14 Apr PM | Watchlist, Razorpay domain |
| 11 | 15 Apr AM | Import pipeline, PRIMARY KEY fix |
| 12 | 16 Apr | Full dataset — 41,003 projects |
| 13 | 17–18 Apr | NPT Admin portal. Repository → Daily → Weekly. |
| 14 | 19 Apr | NPT Console. CIN system. Companies in Admin. |
| 15 | 20 Apr | Activity tracking. proj_updateddate bug fix. Public website complete. |
| 16 | 21 Apr | Branding: logo, Kariyamangalam, social links, Kavitha. Plan rename: Free→Basic, Basic→Starter. 4 critical bugs fixed in crud_save.php. Save→Publish→Subscriber flow confirmed working end-to-end. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- Admin: /admin/ · Console: /console/ · Subscriber: /dashboard/ · Public: /
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
