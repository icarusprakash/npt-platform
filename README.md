# NPT Intelligence Platform — Build README

**Last Updated: 4 May 2026 (Day 28)**

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

---

## LAUNCH STRATEGY — Decided 29 April 2026

### Two-Phase Plan

---

### SPEC 6 — Lapsed User Engagement System

**Who they are:**
- 650+ past NPT subscribers who paid once or multiple times and did not renew
- They know NPT — no need to explain the product
- Being reached via email, WhatsApp, phone and in person

**The offer:**
- Free 15-day Premium trial on NPT Intelligence
- No credit card, no obligation
- Positioned as: *"We've rebuilt NPT from scratch. Come see what's new — on us."*
- Credits set individually per user based on their history

**Source flag:** `source = 'lapsed'` — separate from `migrated`

**Their dashboard experience (different from migrated):**
- DO see pricing page — mild nudges to convert
- Welcome banner specific to lapsed users: acknowledges they are returning, explains free trial
- Trial countdown in sidebar: *"Your free trial ends in X days"*
- Upgrade prompts appear when trial < 5 days remaining
- When trial expires → conversion page: *"Your trial has ended. Renew to keep access."*

**Console — adding lapsed users:**
- Same add_user.php form — select `source = 'lapsed'`
- Set `plan_type = 'premium'`, `plan_status = 'active'`
- Set `plan_start` = today, `plan_end` = 15 days from today
- Set credits individually per user (Jp decides)
- State/industry access as per their legacy subscription history

**What to track:**
- Activity during trial (logins, projects viewed, searches)
- Console shows trial status, days remaining, activity count per user
- Heavy users → personal call from Jp/Kavitha
- Inactive users → WhatsApp nudge on day 7 and day 13

**DB changes needed:**
- `npt_users.source` ENUM: add 'lapsed' value
- Trial countdown uses existing `plan_end` field — no new fields needed

**Files to build/modify:**
- `console/add_user.php` — add 'lapsed' to source dropdown
- `dashboard/_auth.php` — detect lapsed source, compute trial days remaining
- `dashboard/_layout.php` — trial countdown for lapsed users, upgrade nudge < 5 days
- `dashboard/index.php` — lapsed-specific welcome banner
- `dashboard/pricing.php` — returning user message for lapsed users
- `dashboard/trial_expired.php` — new page shown when trial ends

---

### SPEC 7 — Public CapEx News Magazine

**URL:** `newprojectstracker.in/capex-news/`
**Status:** ✅ LIVE

**What it does:**
- Public page — no login required
- Shows CapEx news from April 23, 2026 onwards only
- No archives (avoids SEO duplication with legacy site's 25k indexed pages)
- Listing page: 10 articles per page, industry tag, company, date, excerpt
- Single article page: full content, prev/next nav, latest news sidebar
- Archive link points to legacy site (`newprojectstracker.com/capex-news`)
- No login prompts until July 1 public launch

**Legacy site notice (to be added by Jp/team):**
Post a message on `newprojectstracker.com/capex-news` saying:
*"We have moved. Read the latest CapEx News at newprojectstracker.in/capex-news"*
Manual link — no automatic redirect.

**Post July 1 changes:**
- Add login/register prompts for archive access
- Add search and filter on public page

---

**Who can access the new site:**
- Only manually migrated paid legacy users (120 users)
- No public registration — anyone who lands on the site sees a closed beta message
- Legacy free users (25k+) stay on old site completely — no knowledge of new site
- No links to newprojectstracker.in from the public website or anywhere

**How migrated users are identified:**
- `npt_users.source = 'migrated'` — set manually when adding via Console
- Migrated users get `plan_type = 'premium'` or `plan_type = 'starter'` as appropriate
- Custom plan users (state/industry restricted) — set `state_access` and `industry_access` fields

**Experience for migrated users:**
- Pricing Plans page — completely hidden from sidebar
- No upgrade banners anywhere on the dashboard
- No "Soon" prompts related to pricing or plans
- Welcome message on dashboard explaining the complimentary access
- Clear messaging: legacy subscription continues unchanged, this is a free bonus
- Contact details for support: Kavitha +91 91710 15659

**Experience for non-migrated visitors:**
- `/register` shows closed beta page — not a registration form
- `/login` works normally (for migrated users who have credentials)
- Closed beta page message: "NPT Intelligence is currently in private beta for existing NPT subscribers. If you are an NPT subscriber, please contact us to get access."
- No public-facing marketing content on new site during this period

**Legacy site (newprojectstracker.com) — NO CHANGES:**
- Public website stays exactly as is
- Legacy dashboard stays exactly as is
- Free users continue using legacy dashboard
- No links added to newprojectstracker.in anywhere
- New subscriptions still accepted on legacy site during this period

**Goal of Phase 1:**
- Get real feedback from 120 actual users
- Stabilise the platform
- Complete remaining features
- Decide public launch strategy with confidence

---

#### Phase 2 — Public Launch (July 1, 2026 — Decision Point)

By June 30, review feedback and decide:
- Open public registration on new site
- Change CTA on legacy public website to point to new site
- Stop accepting new subscriptions on legacy site
- All renewals and new sales through new site only
- Legacy dashboard shows migration notice

*Detailed Phase 2 plan to be drafted closer to June 30.*

---

### Build Tasks for Beta Launch (Priority Order)

| # | Task | What it does | Status |
|---|------|-------------|--------|
| 1 | **Closed beta page** — `/register` | Non-migrated visitors see beta message instead of registration form | 🔲 Build next |
| 2 | **Migrated user experience** — hide pricing | If `source = 'migrated'`, remove Subscription section from sidebar, hide all upgrade prompts | 🔲 Build next |
| 3 | **Welcome message for migrated users** | Dismissible banner explaining complimentary access, legacy subscription continues | 🔲 Build next |
| 4 | **Console — user addition** | Ensure Console allows setting source=migrated, state_access, industry_access when adding users | 🔲 Check/fix |
| 5 | **Downloads section** | Daily PDF + Weekly Excel + Special Reports (last 5 each) | 🔲 Ready |
| 6 | **Book a Demo form** | Collect demo requests | 🔲 Ready |
| 7 | **Request for Quote form** | Standalone RFQ form | 🔲 Ready |
| 8 | **Access control banners** | Inline upgrade prompts for non-migrated users on Projects/Companies/KeyPersons | 🔲 Ready |
| 9 | **My Profile page** | Edit details, change password, My Orders | 🔲 Ready |
| 10 | **Fix &apos; entities in news sidebar** | html_entity_decode() fix | 🔲 Quick fix |

---

### Migrated User Rules (Technical)

**Sidebar — hide for `source = 'migrated'`:**
- Entire Subscription section (Pricing Plans, Payment Methods, RFQ, Book a Demo)
- No upgrade banners in credit bar or elsewhere

**Welcome banner — show for `source = 'migrated'` only:**
- Show once per session (or until dismissed via cookie)
- Message: "Welcome to NPT Intelligence — complimentary access for NPT subscribers"
- Sub-text: "Your existing NPT subscription continues unchanged. This platform is available to you as a bonus service. Use both platforms freely until further notice."
- Contact line: "Questions? WhatsApp Kavitha: +91 91710 15659"

**Dashboard credit bar — hide for `source = 'migrated'`:**
- No credit countdown shown
- No "X of Y views remaining" message

**Pricing page (`/dashboard/pricing.php`) — redirect for `source = 'migrated'`:**
- If migrated user somehow navigates to pricing.php → redirect to dashboard with a message

---

### Console — Adding Migrated Users

When adding a legacy paid user to the new site via Console:
- Set `source = 'migrated'`
- Set `plan_type = 'premium'` (or 'starter' depending on their legacy plan)
- Set `plan_status = 'active'`
- Set `state_access` = their legacy state restriction (or 'all')
- Set `industry_access` = their legacy industry restriction (or 'all')
- Set `credits_monthly = 99999` (effectively unlimited for migrated users)
- No OTP required — admin creates account directly
- Send credentials to user manually (Kavitha does this)

---

**What it does:**
Bulk enrichment of existing project records using Claude API. Many records have sparse
data — short synopsis, missing teaser, thin proj_details. Claude will read the raw
project data and generate structured, readable content for missing fields.

**Fields to enrich:**
- `proj_synopsis` — one-line summary if missing or too short
- `proj_teaser` — 2-3 sentence teaser for dashboard listing
- `proj_details` — full structured project description

**Implementation plan:**
- Python script: `nptcleaner/enrich_projects.py`
- Reads unpopulated or thin records from `npis_projects` in batches of 50
- Calls `claude-sonnet-4-6` via Anthropic API for each batch
- Writes enriched content back to DB
- Logs enriched project IDs to avoid re-processing
- Rate limited to stay within API budget

**Budget:** ~$280 Anthropic API credits for ~40,000 project records

**Admin hook:** "AI Populate" button already exists in `admin/crud.php` as a stub.
This will be wired to the same enrichment logic for single-record use.

**Trigger:** Run after platform launch is stable and subscriber base is onboarded.
Do not run before launch — enrichment is a background data quality task.

---

## PENDING CONTENT FROM JP

| Item | Needed For | Status |
|------|-----------|--------|
| Bank name, account number, IFSC, UPI ID | Payment Methods page + offline payment flow | ⏳ Pending |
| GSTIN of Kariyamangalam Technologies | Tax invoice generator | ⏳ Pending |
| HSN/SAC code for software subscription | Tax invoice | ⏳ Pending |
| Client logos (image files) | Our Clients page | ⏳ Pending |
| Sales Tools intro text (PitchOS, Tender Summarizer, Vendor Reg, Custom Tools) | Coming Soon pages | ⏳ Pending |

---

## Next Tasks — Priority Order

| # | Task | Dependencies | Status |
|---|------|-------------|--------|
| 1 | Closed beta page — /register | No blocker | ✅ Done |
| 2 | Migrated user experience — hide pricing + upgrade prompts | No blocker | ✅ Done |
| 3 | Welcome banner for migrated users | No blocker | ✅ Done |
| 4 | Console add_user.php — fully rebuilt with 4 source types | No blocker | ✅ Done |
| 5 | Public CapEx News magazine — /capex-news/ | No blocker | ✅ Done |
| 6 | Lapsed user engagement system — trial, countdown, expired page | No blocker | ✅ Done |
| 7 | RFQ form + Payment Methods page | No blocker | ✅ Done |
| 8 | **Admin RFQ management** — view/manage RFQs in Console | No blocker | 🔲 Next |
| 9 | **Console plan upgrade flow** — change lapsed→paid, extend dates | No blocker | 🔲 Next |
| 10 | **Email SMTP** — verify PHP mail() delivery, switch to Gmail SMTP if needed | No blocker | 🔲 Next |
| 11 | **Downloads section** — admin upload + user download (Daily PDF, Weekly Excel) | No blocker | 🔲 Ready |
| 12 | **Book a Demo form** | No blocker | 🔲 Ready |
| 13 | **Access control banners** — Projects/Companies/KeyPersons | No blocker | 🔲 Ready |
| 14 | **My Profile page** — edit details, change password | No blocker | 🔲 Ready |
| 15 | Fix `&apos;` entities in CapEx News article sidebar | No blocker | 🔲 Quick fix |
| 16 | **Registration Redesign** — OTP + activation flow (for July 1 public launch) | Fast2SMS key + npis_users dump | 🔲 Pre July 1 |
| 17 | **Orders Portal** — /orders/ | After payment flow | 🔲 Post-beta |
| 18 | **AI Enrichment Pipeline** — bulk project data enrichment via Claude API | ~$280 Anthropic credits | 🔲 Post-launch |
| 19 | Client logos for homepage | Jp to provide | ⏳ Pending |
| 20 | GSTIN + HSN/SAC code for tax invoices (Kariyamangalam Tech) | Jp to obtain | ⏳ Pending |

---

## AI Features — Phase 2 (Post-Launch, Shelved for Now)

Explored in a side thread on 4 May 2026. **Decision: shelved until NPT 2.0 is live and stable.**
Will be revisited in this thread, one feature at a time, after public launch.

Concepts noted for future: Natural Language Search, Live Project Intel, Company Intelligence Cards,
Project Fit Scoring, AI Daily Brief, Geo-Proximity Discovery, Bid Prep Assistant, Smart Alert Engine,
Ask-About-A-Project Chat, Market Intelligence Dashboard.

No code changes made. `npis_projects` schema reviewed — no changes.

---

## Dependencies Checklist (Jp to Arrange)

| Item | Needed For | Status |
|------|-----------|--------|
| Fast2SMS API key | Registration OTP | ⏳ Pending |
| `npis_users` SQL dump (GoDaddy) | Legacy migration | ⏳ Pending |
| Razorpay Key Secret | Payment flow | ⏳ Pending |
| ~$280 Anthropic API credits | AI Enrichment pipeline | 🔲 Post-launch |

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
- `npt_downloads` — admin-uploaded reports for Downloads section
- `npt_demo_requests` — Book a Demo form submissions
- `npt_rfq` — Request for Quote form submissions

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
| 23 | 28 Apr | Full sidebar redesign with collapsible sections. coming_soon.php with full content for About, History, Terms, Refund, Plan Compare, FAQ, Payment Methods. Legacy Welcome + FAQ pages. Project Hat Tip form + admin hattips.php. Projects/Companies/KeyPersons table+card toggle. Card view redesigned (IIG style, 3-col portrait). Project detail banner shows company name. Access control rules defined. Dashboard home page redesigned (DB teaser cards, industry bars, top states, recent activity, latest news). fmt_cost updated globally to show actual cost in Mn. Pricing page rebuilt with new sidebar. |
| 24 | 29 Apr | Launch strategy finalised. Two-phase plan: Closed Beta (now→Jun 30) for 120 paid legacy users only, Public Launch decision point July 1. Migrated user experience spec written. Closed beta page spec written. Build priority reordered around beta launch. |
| 25 | 30 Apr | Priority 1 beta launch prep completed. Closed beta page live at /register.php (original backed up as register_full.php). _auth.php updated with $auth_source detection from DB. _layout.php updated: Subscription section hidden for migrated users, credit bar + upgrade alerts hidden for migrated users. pricing.php redirects migrated users to dashboard. index.php: migrated-specific welcome banner added. npt_users.source ENUM updated to include 'migrated'. console/add_user.php rebuilt: Source dropdown (Migrated Legacy User default), state/industry access fields, unlimited credits for migrated users. |
| 26 | 2 May | Launch strategy discussion. Decided: no public registrations till June 30. Two new user groups planned: (1) Lapsed premium users — 650+ past subscribers, 15-day free trial, source='lapsed'. (2) Post July 1 — lukewarm legacy free users (~4k). Public CapEx News magazine built at /capex-news/ — clean public page, no login required, shows news from April 23 onwards, archive link to legacy site. Single article page with full content, prev/next nav, latest news sidebar. |
| 27 | 4 May | Lapsed user system fully built: source='lapsed' ENUM added, _auth.php trial detection, trial countdown bar in _layout.php (with credits/daily usage), lapsed welcome banner in index.php, trial_expired.php new page, pricing.php returning user message. Console add_user.php fully rebuilt: 4 source type cards (Migrated/Lapsed/Paid/Self-register), smart auto-fill defaults, Generate Password button, collapsed access restrictions. RFQ form built (rfq.php) with npt_rfq table + email notification to Kavitha. Payment Methods page built (paymentmethods.php) with full SBI bank details, copy buttons, UPI section, step-by-step process. Sidebar links updated for RFQ and Payment Methods. |
| 28 | 4 May | AI feature exploration done in a side thread — 10 concepts discussed (Natural Language Search, Live Project Intel, Company Intelligence Cards, Project Fit Scoring, AI Daily Brief, Geo-Proximity Discovery, Bid Prep Assistant, Smart Alert Engine, Ask-About-A-Project Chat, Market Intelligence Dashboard). Decision: ALL AI features shelved. Ship NPT 2.0 first. AI features to be explored as clean Phase 2 in this thread, one at a time, after launch. No code changes made. npis_projects schema reviewed — no changes. Resuming NPT 2.0 development. |

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
