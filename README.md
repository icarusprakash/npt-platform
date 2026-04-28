# NPT Intelligence Platform — Build README

**Last Updated: 27 April 2026 (Day 22)**

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
- **GitHub:** https://github.com/icarusprakash/npt-platform

---

## Login Credentials

### Jayaprakash (icarusprakash@gmail.com / Npt@2026)
- Admin: https://newprojectstracker.in/admin/
- Console: https://newprojectstracker.in/console/
- Subscriber: https://newprojectstracker.in/dashboard/

### Indscan (indscan.projects@gmail.com)
- Admin: NPTAdmin2026! · Console: NPTConsole2026! · Subscriber: Npt@2026

### Sbiru (sbirumca85@gmail.com)
- Admin: NPTEditor2026! · Console: NPTConsole2026!

---

## Plan Names
| Plan | Price |
|------|-------|
| Basic | ₹0 / forever |
| Starter | ₹14,950/month |
| Premium | ₹99,000/year |

---

## Day 19 — What Was Done

### 1. crud.php — Form Restructured to Match Legacy Layout
Field order now matches legacy NPT system:
1. Project Name
2. Synopsis
3. Teaser
4. Ownership + Project Type (side by side)
5. Industry
6. Production Capacity
7. End Product
8. Investment + Cost Range (side by side, cost range is auto/readonly)
9. Expected Completion + Key Equipment (side by side)
10. Project Stage
11. Current Status
12. Product Tags
13. Full Project Details (with AI Populate button)
14. CIN Number
15. Location section (separate block): Location, District, Pincode, State, Region, Plant Address

### 2. Bug Fix — saveAndConnectCompany
JS function was trying to read deleted address fields (addr1, addr2, city etc.)
Fixed to send empty strings for those fields — company name only saves correctly now.

### 3. Cancel Button Fixed
Was pointing to `/dashboard/admin/` — now correctly points to `/admin/projects.php`

### 4. Old Description Section Removed
Duplicate Synopsis/Teaser/Details block (lines 415-440) removed — was causing double field submission risk.

---

## Complete Project Entry Workflow (Final)

```
Step 1: /admin/crud.php
  - Search/connect company OR add new (name only)
  - Fill all project fields in legacy order
  - Save New Project → auto-redirects to project_address.php

Step 2: /admin/project_address.php
  - Legacy address shown if available → "Yes — Use This Address"
  - OR add new address → auto-connects
  - Add another company if needed (EPC, Consultant etc.)
  - "Done — Move to Repository" → project goes to Repository

Step 3: /admin/projects.php
  - Draft: View + Delete
  - Repository: View + Publish + Delete
  - Published: View only

Step 4: /admin/project.php?id=X
  - Full view of all fields
  - Quick Edit on every section
  - Connected Address block
  - Full Edit button
```

---

## NPT Admin Portal — All Files

| File | Purpose |
|------|---------|
| login.php | Login |
| _auth.php | Auth guard |
| _layout.php | Sidebar |
| _layout_end.php | Footer |
| index.php | Dashboard |
| projects.php | Projects list |
| project.php | Project view + Quick Edit |
| crud.php | Full entry/edit form (restructured Day 19) |
| crud_save.php | Save → redirects to project_address.php |
| crud_search.php | AJAX project search |
| crud_company_search.php | AJAX company search |
| crud_add_company.php | AJAX add company (name only) |
| project_address.php | Address connection screen |
| company_address.php | Company address form |
| ajax_save_address.php | AJAX address save |
| ajax_update_comptype.php | AJAX company role update |
| delete_project.php | Delete draft project |
| qe_save.php | Quick Edit AJAX |
| qe_milestone.php | Milestone AJAX |
| repository.php | Repository bucket |
| daily.php | Daily workflow |
| weekly.php | Weekly workflow |
| download.php | CSV download |
| publish.php | Quick publish |
| unpublish.php | Unpublish |
| companies.php | Companies list |
| company.php | Company view |
| logout.php | Logout |

---

## Company Address System
- `npt_company_addresses` — address records per company
- `npis_refer.ref_address_id` — links address to project-company connection
- Legacy fallback: shows `npis_companies` address if no new record exists
- "Yes — Use This Address" copies legacy to new table and connects

---

## Critical Bug Fixes Log
### Day 16 — crud_save.php
1. Email whitelist blocking indscan — removed
2. Extra '$today' (41 vs 40 columns) — removed
3. proj_recently_viewed DATE getting string — NULL
4. npis_refer ref_ID missing — MAX+1 logic added

### Day 21 — dashboard/_auth.php + admin/_layout.php
6. `localhost` markdown corruption in mysqli calls — fixed via regex replace in both files
7. `session_start()` missing from news.php and news_article.php — added

### Day 22 — crud.php + crud_save.php + project_address.php
8. Bug 1 — proj_takendate hardcoded to today on INSERT — now uses form value; visible date field added to crud.php
9. Bug 2 — proj_updateddate updating on every edit — now only updates when proj_details or proj_stage changes
10. Bug 4 — address save redirecting back to project_address.php — now redirects to project.php
11. Bug 3 — phone field missing in address screen — PENDING (Jp to observe via AnyDesk)

---

## Branding
- Logo: `/assets/img/logo-orange.jpg` (public/subscriber), `/assets/img/logo-blue.jpg` (console)
- Footer: "A product of Kariyamangalam Technologies Pvt Ltd, Chennai · Built with Claude"
- Sales: Ms. Kavitha Prakash — +91 91710 15659

---

## PLANNED FEATURE SPECS

---

### SPEC 1 — Registration Redesign (Revised)

**Dependencies:**
- Fast2SMS API key (Jp to obtain)
- `npis_users` SQL dump from GoDaddy VPS (DBA to export)
- WhatsApp number for manual paid account activation message

**Registration Flow:**
```
1. User fills registration form (name, email, mobile, company, designation, password)
2. OTP sent to mobile via Fast2SMS
3. OTP verified → account created (plan_type = 'basic', unactivated)
4. Welcome splash screen shown
5. Redirected to Pricing Plans page
   - Basic (Free) → "Start Free — No credit card needed" → Activate Basic Plan
   - Starter (₹14,950) → Razorpay checkout
   - Premium (₹99,000) → Razorpay checkout
6. User MUST activate Basic plan to access free projects (5/month)
7. User can skip Basic and go directly to Starter or Premium
```

**Key UX note:** Basic plan button must say "Start Free — No credit card needed"
to avoid confusion that it costs money.

**Legacy Migration Lookup:**
- On registration, check mobile against `npis_legacy_users`
- If found + paid legacy user → show manual activation message with WhatsApp contact
- If found + free legacy user → auto-migrate, show "Welcome back" message
- If not found → fresh registration, proceed normally

**DB Changes Needed:**
- Add `phone_verified` TINYINT to `npt_users`
- Add `plan_activated` TINYINT to `npt_users` (0 = registered but not activated)
- Create `npt_otp_log` table: (id, mobile, otp, expires_at, verified, created_at)
- Import legacy users as `npis_legacy_users` (read-only reference table)

**Files to Build:**
- `register.php` — 2-step form: mobile OTP → profile completion
- `otp_send.php` — Fast2SMS API call
- `otp_verify.php` — AJAX OTP verification
- `dashboard/welcome.php` — splash screen / modal
- `dashboard/pricing.php` — plan selection page (also used post-registration)

**Console Flags:**
- `source` field in `npt_users`: `self_register` / `migrated` / `admin`
- Console shows badge: "New" or "Migrated" on user profile and list

---

### SPEC 2 — Pricing & Payment Flow (Detailed)

**Razorpay Key ID:** rzp_live_D1cUKmkOav2Xx9
**Razorpay Key Secret:** PENDING (Jp to retrieve from Shopify vendor)

---

#### Plan Positioning — Unlimited with Fair Use (IMPORTANT)
- **Basic (Free):** Hard cap — 5 project views per month. Clearly shown as a limit.
- **Starter:** Displayed as **"Unlimited\*"** — no cap shown to user.
- **Premium:** Displayed as **"Unlimited\*"** — no cap shown to user.
- Fine print on Starter and Premium cards: *"Fair usage policy applies."*
- The current internal credit limits (400/month for Starter, 10,000/month for Premium)
  are retained in the DB as fair use thresholds — NOT shown to users as caps.
- If a user hits the fair use threshold, show a soft warning:
  *"You've reached your fair usage limit for this month. Contact us if you need more."*
- Rationale: Psychological — "Unlimited" removes the biggest objection at point of purchase.

---

#### Plan Summary Card (shown before any payment action)
Each plan card on the pricing page must clearly show:
- Basic: "5 project views per month"
- Starter: "Unlimited project access\* · Monthly subscription"
- Premium: "Unlimited project access\* · Annual subscription"
- What is included vs not included (Key Persons, News, Downloads etc.)
- Validity period
- Fine print: \*Fair usage policy applies
This is mandatory — shown before the user clicks any payment button.

---

#### Basic Plan (Free)
```
Pricing Page → "Start Free — No credit card needed" button
→ One-click activation → plan_activated = 1
→ Redirect to dashboard
```

---

#### Paid Plans — Online Payment (Razorpay) — PLACEHOLDER FOR NOW
```
Pricing Page → "Pay Online" button (greyed out)
Note below button: "Online payment coming soon. Use 'Pay Offline' option below."
```
Full Razorpay integration to be wired once Key Secret is available.
Flow is designed and ready — button will go live without page redesign.

---

#### Paid Plans — Offline Payment Flow
```
Pricing Page → "Pay Offline / Get Quotation" button
→ Warning shown:
  "This is a manual process. Your account will be activated only after payment
   is received and verified by our team. This may take 1–2 business days."
→ User selects document type:
   Option A — Formal Quotation (for corporate PO process)
   Option B — Proforma Invoice (for direct bank transfer)
→ Pre-filled form pulled from npt_users:
   (Name, Company, GST Number, Billing Address — all editable. Phone locked.)
   GST Number: optional here, mandatory only at tax invoice download stage.
→ PDF generated (Quotation or Proforma) → downloadable immediately
→ Offline payment instructions shown:
   - Bank name, account number, IFSC
   - NEFT / RTGS / UPI details
   - Reference: auto-generated order code (e.g. NPT-2026-00142)
→ "I have made the payment" button
   → order status set to 'payment_pending'
   → email notification sent to Kavitha Prakash (+91 91710 15659)
→ User sees dashboard banner:
  "Your payment is being verified. Expected activation: 1–2 business days.
   Questions? WhatsApp Kavitha: +91 91710 15659"
→ Sales team verifies payment → manually activates from Console
→ User gets email confirmation + tax invoice link
→ Tax invoice available in My Orders
```

---

#### My Orders (inside subscriber dashboard — Profile section)
- Lists all transactions: online + offline
- Columns: Reference No, Date, Plan, Amount, Status, Documents
- Status badges: Paid / Pending Verification / Activated / Expired
- Download buttons per row: Tax Invoice / Quotation / Proforma Invoice
- GST number required to download Tax Invoice (prompt if missing)

---

#### Console — Orders Management (for sales team)
- Orders tab: lists all pending offline payments
- Columns: User, Plan, Reference No, Document Type, Date Requested, Status
- "Activate Account" button → sets plan_type, credits, validity dates
- "Generate Tax Invoice" → GST-compliant PDF generated, linked to user's My Orders
- Tax Invoice fields: Kariyamangalam Technologies GSTIN, HSN/SAC code for
  software subscription, 18% GST line item, buyer GSTIN if provided

---

#### DB Tables for Orders
- `npt_orders` — order reference, user_id, plan, amount, status, created_at
- `npt_payments` — payment records (online/offline), linked to order
- `npt_invoices` — invoice records, PDF path, linked to order
- `npt_quotations` — quotation/proforma records, PDF path, linked to order

---

#### Files to Build
| File | Purpose |
|------|---------|
| `dashboard/pricing.php` | Plan cards with summary + payment buttons |
| `dashboard/checkout.php` | Pre-filled checkout form (phone locked) |
| `dashboard/offline_payment.php` | Doc type selection + bank details + confirmation |
| `dashboard/generate_doc.php` | Quotation / Proforma PDF via mPDF |
| `dashboard/orders.php` | My Orders section |
| `dashboard/invoice.php` | GST tax invoice PDF via mPDF |
| `dashboard/pay.php` | Razorpay handler (placeholder → live when key ready) |
| `webhook_razorpay.php` | Razorpay webhook (placeholder) |
| `console/orders.php` | Orders management for sales team |
| `console/activate.php` | Manual activation + invoice trigger |

**Build order:** DB tables → pricing.php → checkout.php → offline flow →
My Orders → Console orders tab → Console activation → Razorpay (last, needs key)

---

### SPEC 3 — Key Persons DB (Subscriber Dashboard) — ✅ LIVE

**Data source:** Contact person fields inside `npis_companies` table
- `comp_person1` / `comp_person2` — names
- `comp_kpdesignation1` / `comp_kpdesignation2` — designations
- `comp_kptitle1` / `comp_kptitle2` — titles (Mr/Ms/Dr)
- `comp_tel1` / `comp_tel2` — direct numbers
- `comp_email` / `comp_email2` — emails
- 36,651 person1 records + 31,354 person2 records = ~68,000 total

**Access:** Starter + Premium only. Basic users see upgrade banner.

**Files built:**
- `dashboard/keypersons.php` — listing, card grid, 4 filters (name/company/desig, state, city, designation)
- `dashboard/keyperson.php` — detail page with hero card, company details, contact, connected projects, sidebar

---

### SPEC 4 — CapEx News Module

**Three deliverables:**

**A. Public News Magazine** (`newprojectstracker.in/capex-news/`) — 🔲 Build next week
- Modern news/magazine UI, publicly accessible, SEO-optimised
- New stories only (from new admin CRUD, starting this week)
- Public sees excerpt only — "Login to read full story" for non-logged-in visitors
- Full archive available inside subscriber dashboard (free for Basic users)
- Pop-up notice on new public page explaining move from legacy site (plain message, no redirects)
- Right sidebar: Monthly Archive, Industry filter with counts, Company search
- Clean URL structure: `/capex-news/{slug}`

**B. Searchable News Archive (Subscriber Dashboard)** — ✅ LIVE
- `dashboard/news.php` — listing with keyword, industry, company, date filters
- `dashboard/news_article.php` — full article view with related sidebar
- Available to ALL users including Basic (free) — no credit consumption
- 6,537 articles imported from legacy blog.sql

**C. News CRUD (NPT Admin Portal)** — ✅ LIVE
- `admin/news.php` — all articles listing with search + delete
- `admin/news_crud.php` — add/edit with Quill rich text editor
- Auto-slug generation from headline
- Sidebar link added under CapEx News section

**Legacy Migration Status:**
- Post transition message on legacy site, LinkedIn, Twitter (messages drafted — ready to post)
- Stop new entries in legacy admin after transition message
- New entries go only into new admin CRUD from this week

**URL Strategy:**
- New site URL: `newprojectstracker.in/capex-news/{slug}`
- No programmatic redirects — plain human message on new site explaining the move
- Slugs already stored in blog table — imported as-is

**Known issue to fix:**
- `&apos;` HTML entities showing in article sidebar titles — fix with `html_entity_decode()`

---

### SPEC 5 — Access Control Rules (Inline Upgrade Banners)

**Rule table:**

| Section | Basic (Free) | Starter | Premium |
|---------|-------------|---------|---------|
| Projects — Page 1 | ✅ Listing | ✅ Listing | ✅ Listing |
| Projects — Pages 2–40 | ❌ Inline banner | ✅ Listing | ✅ Listing |
| Projects — Pages 41+ | ❌ Inline banner | ❌ Inline banner | ✅ Listing |
| Projects — Detail | ✅ If credits available | ✅ If credits available | ✅ Unlimited* |
| Promoters DB — Any page | ❌ Inline banner | ❌ Inline banner | ✅ |
| Promoters DB — Detail | ❌ Inline banner | ❌ Inline banner | ✅ |
| Key Contacts — Any page | ❌ Inline banner | ❌ Inline banner | ✅ |
| Key Contacts — Detail | ❌ Inline banner | ❌ Inline banner | ✅ |
| CapEx News | ✅ All users | ✅ All users | ✅ All users |

**Inline banner design:**
- Shown instead of results (not over them)
- Navy gradient banner with lock icon
- Message: "Upgrade to [Starter/Premium] to continue browsing"
- Two buttons: "View Pricing Plans" and "Request a Quote"
- Shows what they are missing (e.g. "Access 40,000+ projects across all pages")
- Never redirects — stays on same page

**Implementation:**
- Check in `projects.php`: if page > 1 && basic → show banner; if page > 40 && starter → show banner
- Check in `companies.php`: if basic or starter → show banner on page load
- Check in `keypersons.php`: if basic or starter → show banner on page load
- Detail pages: existing credit gate handles project.php; add plan check to company.php and keyperson.php

| # | Task | Dependencies | Status |
|---|------|-------------|--------|
| 1 | **Access control rules** — inline upgrade banners on Projects/Companies/KeyPersons | No blocker | 🔲 Ready |
| 2 | Bug 3 — Phone field missing in address screen | AnyDesk observation | ⏳ Pending |
| 3 | **Dashboard home page redesign** — teaser cards, infographics, welcome banner | No blocker | 🔲 Ready |
| 4 | **Book a Demo form** | No blocker | 🔲 Ready |
| 5 | **Request for Quote standalone form** | No blocker | 🔲 Ready |
| 6 | **Downloads section** — admin upload + user download | No blocker | 🔲 Ready |
| 7 | **CapEx News — Public Magazine** (`/capex-news/`) | No blocker | 🔲 Ready |
| 8 | **Registration Redesign** — OTP + pricing page + activation flow | Fast2SMS key + npis_users dump | 🔲 Pending |
| 9 | **Pricing & Payment Flow** — offline flow + My Orders + Console activation | No blocker | 🔲 Ready |
| 10 | **Orders Portal** — /orders/ | After payment flow | 🔲 Pending |
| 11 | Client logos for homepage | Jp to provide | 🔲 Pending |

---

## Dependencies Checklist (Jp to Arrange)

| Item | Needed For | Status |
|------|-----------|--------|
| Fast2SMS API key | Registration OTP | ⏳ Pending |
| `npis_users` SQL dump (GoDaddy) | Legacy migration | ⏳ Pending |
| WhatsApp number for paid migration message | Registration | ⏳ Pending |
| Razorpay Key Secret | Payment flow | ⏳ Pending |
| `news.sql` dump from legacy system | CapEx News module | ⏳ Pending |
| Key Persons table name from DBA | Key Persons DB | ⏳ Pending |

---

## Database Tables

### Core
- `npis_projects` — 40,948+ rows
- `npis_companies` — 36,695 rows
- `npis_refer` — 50,845+ rows + ref_address_id
- `npt_company_addresses` ✅

### Platform
- `npt_users` — plan_type: basic/starter/premium
- `npt_admin_users`, `npt_console_users`
- `npt_activity_log`, `npt_contact_forms`

### To Create
- `npt_otp_log` — registration OTP verification
- `npis_legacy_users` — imported from GoDaddy for migration lookup
- `npt_quotations`, `npt_orders`, `npt_payments`, `npt_invoices` — Orders Portal

### Imported
- `blog` — 6,537 CapEx news articles (imported 27 Apr 2026)

---

## Session Log
| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1–8 | Early Apr | Subscriber dashboard, waitlist |
| 9–10 | 14 Apr | Admin panel, Watchlist |
| 11–12 | 15–16 Apr | Import pipeline, full dataset |
| 13 | 17–18 Apr | Admin portal. Repository → Daily → Weekly. |
| 14 | 19 Apr | Console. CIN. Companies. |
| 15 | 20 Apr | Activity tracking. Public website. |
| 16 | 21 Apr | Branding. Plan rename. 4 crud_save bugs fixed. |
| 17 | 22 Apr | Address system foundation. npt_company_addresses table. |
| 18 | 23 Apr | Full workflow. project_address.php. Delete drafts. |
| 19 | 25 Apr | crud.php restructured to legacy layout. Cancel fix. Duplicate section removed. saveAndConnectCompany bug fixed. |
| 20 | 25–26 Apr | Planning session. Specs drafted: Key Persons DB, CapEx News Module, Registration redesign revised (OTP + Basic activation flow). Payment flow fully specced: offline flow, My Orders, Console orders + activation, Razorpay placeholder. |
| 21 | 27 Apr | CapEx News module built. blog.sql imported (6,537 articles). dashboard/news.php + news_article.php live. Admin news.php + news_crud.php built with Quill editor. News Feed sidebar link activated. _auth.php localhost bug fixed. session_start() added to news pages. Migration messages drafted for legacy site, LinkedIn, Twitter. |
| 23 | 28 Apr | Full sidebar redesign with collapsible sections. coming_soon.php with full content for About, History, Terms, Refund, Plan Compare, FAQ, Payment Methods. Legacy Welcome + FAQ pages. Project Hat Tip form + admin hattips.php. Projects/Companies/KeyPersons table+card toggle. Card view redesigned (IIG style, 3-col portrait). Project detail banner shows company name instead of investment. Access control rules defined. |

---

## Key Rules
- **newprojectstracker.com: NEVER TOUCH**
- proj_updateddate changes ONLY on proj_details or proj_stage edit
- Unpublish only from project view page
- proj_industry NOT proj_sector
- Delete SQL dumps immediately after import
- Only Repository projects can be published — not Drafts
- Company cannot be changed after connecting to a project
- address saves go to npt_company_addresses — NOT npis_companies
