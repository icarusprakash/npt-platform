# NPT Intelligence Platform — Build README

**Last Updated: 21 April 2026 (Day 16)**

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

- DB enum updated: `npt_users.plan_type` = basic / starter / premium
- Updated in: pricing.php, register.php, features.php, index.php, console/index.php, console/users.php

---

## Branding — UPDATED (Day 16)

### Logo
- Blue background: `/assets/img/logo-blue.jpg` — used in Console sidebar
- Orange background: `/assets/img/logo-orange.jpg` — used in all public pages, subscriber portal, register page
- All pages: logo image replaces old "NPT" text badge
- Tagline "EARLY PROJECT INTELLIGENCE" shown below logo

### Company
- NPT Intelligence is a product of **Kariyamangalam Technologies Pvt Ltd**, Chennai
- Website: https://www.kariyamangalam.in
- Mentioned subtly on About, Features pages
- Full footer on all public pages + internal portals:
  - "A product of Kariyamangalam Technologies Pvt Ltd, Chennai · Built with Claude"

### Social Links (on public pages)
- NPT LinkedIn: https://www.linkedin.com/company/nptris/
- NPT Twitter/X: https://x.com/NPTrackerOfcl
- Founder LinkedIn: https://www.linkedin.com/in/jayaprakashsampath (About page + footer)

### Sales Contact
- **Ms. Kavitha Prakash** — Head of Sales
- Phone/WhatsApp: +91 91710 15659
- Prominent banner on Contact page, link on Pricing page

### Address (Contact page)
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

## Public Website — ✅ Complete

| URL | Status |
|-----|--------|
| / | ✅ Home — beta banner, logo, CTA |
| /register.php | ✅ Split-screen registration |
| /about.php | ✅ Story, timeline, Jp LinkedIn linked |
| /features.php | ✅ Feature blocks, plan comparison |
| /contact.php | ✅ Full address, Kavitha banner, social links |
| /login.php | ✅ |
| /forgot-password.php | ✅ |

---

## PLANNED FEATURE SPECS (Next Sprints)

---

### Sprint: Registration Redesign & Legacy Migration
**Status: Planned — Day 17**

**Dependencies to arrange:**
1. SMS Provider — sign up for Fast2SMS, get API key
2. Legacy user export — DBA to export `npis_users` from GoDaddy as SQL dump
3. WhatsApp contact number for paid user manual activation

**Flow:**
1. User enters name + mobile → OTP sent → verified
2. After OTP: fill email, company, designation, password
3. Legacy migration option: enter mobile → check `npis_legacy_users` table
   - Free legacy user: pull data, pre-fill form, register as migrated
   - Paid legacy user: show "Contact us" message for manual activation
4. Welcome splash screen after registration

**DB changes needed:**
- Add `phone_verified` tinyint to `npt_users`
- Create `npt_otp_log` (mobile, otp, expires_at, verified, created_at)
- Import legacy users as `npis_legacy_users` (read-only)
- Console flags: `source` = self_register / migrated / admin

**Files to build:**
- `register.php` — 2-step OTP flow
- `otp_send.php` — Fast2SMS API
- `otp_verify.php` — AJAX OTP check
- `dashboard/welcome.php` — post-registration splash

---

### Sprint: Pricing & Payment Flow
**Status: Planned — Day 18**

**Starter Plan (₹14,950/month)**
- Two payment paths:
  1. Online via Razorpay → instant activation
  2. Offline (NEFT/UPI/Cheque) → activation after payment realisation
- Tax invoice: form (name, company, GSTIN, address) → PDF on Kariyamangalam letterhead → download or email
- Clearly stated: online = instant, offline = manual activation

**Premium Plan (₹99,000/year)**
- Three paths:
  1. Online via Razorpay → instant activation
  2. Offline payment → manual activation
  3. Formal quotation route → fill form → download PDF quotation → offline sales process via Kavitha Prakash → manual activation
- Clearly stated before choosing path

**Project Credits (Basic plan users)**
- Buy credits instead of full subscription
- Packages TBD (100, 200 credits)
- Show as "Coming Soon" teaser on pricing page to gauge interest

**Key principles:**
- No pre-sales pressure anywhere
- Transparent upfront about activation timelines
- Online = instant always
- Offline/quotation = manual activation after payment realisation

---

## CIN — Customer Information Number
- Format: NPT-YYYY-NNNNN
- Column `cin` in `npt_users` ✅
- Auto-generated on registration

---

## Activity Tracking ✅
- dashboard/_auth.php → page_view
- dashboard/login.php → login
- dashboard/project.php → project_view
- register.php → register
- Table: npt_activity_log

---

## NPT Admin Portal (`/admin/`)

### Daily Workflow
```
Mon–Fri: Add projects → REPOSITORY
3:30 PM: Select 15 → Move to Daily & Publish
Email marketer: Download Excel → report → Flush Daily
Friday: Download Weekly → send Monday → Flush Weekly
```

### Key Rules
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page

---

## Database Tables

### Core Data
- `npis_projects` — 40,948 rows
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845 rows

### Platform
- `npt_users` — plan_type: basic/starter/premium
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
| 1 | **Client logos** — Jp to provide, add to index.php |
| 2 | **Registration redesign** — OTP + legacy migration (Day 17) |
| 3 | **Pricing & payment flow** — Razorpay + quotation + credits (Day 18) |
| 4 | **Orders Portal** — build /orders/ |
| 5 | **Watchlist trigger** — fire on new project save |
| 6 | **Email verification** — AWS SES |
| 7 | **Retire dashboard/admin/** → Console |

---

## Design Systems
| Portal | Logo | Accent | Badge |
|--------|------|--------|-------|
| Admin | NPT text (internal only) | #e94560 red | ADMIN |
| Console | logo-orange.jpg | #2d6a4f green | CONSOLE |
| Subscriber | logo-orange.jpg | #e87722 orange | — |
| Public | logo-orange.jpg | navy + orange | — |

---

## Assets
- `/assets/img/logo-blue.jpg` — blue background logo
- `/assets/img/logo-orange.jpg` — orange background logo

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
| 16 | 21 Apr | Branding: logo, Kariyamangalam, social links, address, Kavitha Prakash. Plan rename: Free→Basic, Basic→Starter. Pricing flow spec. Registration redesign spec. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- Three internal portals: Admin, Console, Orders
- Subscriber portal: /dashboard/
- Public site: /, register.php, about.php, features.php, contact.php
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
