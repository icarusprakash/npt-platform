# NPT Project Vendor Directory — Build README

**Last Updated: 14 April 2026 (Day 1 — Planning)**

---

## Overview

A standalone, publicly accessible directory of project vendors — companies that supply equipment, services, and contracting to industrial and infrastructure projects across India. Built as a subdomain of newprojectstracker.in but completely independent in codebase, database, and user base.

**Core concept:**
- Vendors register and submit their company details
- Staff screens and approves each listing before it goes live
- Anyone can browse and filter the directory publicly
- Mobile number and email are hidden behind a free login wall
- Project promoters and purchase managers register (free) just to access contact details

This is the supply-side complement to the NPT Intel platform. NPT Intel helps vendors find projects. This directory helps project buyers find vendors.

---

## Server Details

- **Subdomain:** vendors.newprojectstracker.in
- **IP:** 31.97.228.143
- **Stack:** AlmaLinux 9, CyberPanel, LiteSpeed, PHP 8.0, MariaDB
- **DB:** nptvd_db (separate from NPT Intel's newp_ai_engine)
- **DB User:** nptvd_user / (to be set on server)
- **MySQL root config:** /root/.my_temp.cnf
- **GitHub:** https://github.com/icarusprakash/npt-vendor-directory (to be created)
- **All files live at:** /home/vendors.newprojectstracker.in/public_html/

---

## What This Site Is NOT

- Not connected to the NPT Intel dashboard or its database
- Not a marketplace — no RFQs, no transactions, no messaging between parties
- Not a paid listing service at launch — free to list, free to browse (monetisation later)
- Not a project database — it lists vendors only, not projects

---

## User Types

### 1. Vendor (listing owner)
- Registers to submit their company for listing
- Fills out vendor registration form
- Waits for staff approval before listing goes live
- Can log in to edit their listing after approval

### 2. Browser (project promoter / purchase manager)
- Registers for free just to view contact details
- No listing, no fee, no approval needed
- Sees full vendor profile including mobile number and email once logged in

### 3. Staff / Admin
- Reviews pending vendor submissions
- Approves or rejects with optional rejection note
- Can edit, suspend, or delete any listing
- Manages user accounts

---

## Database — nptvd_db

### Table: vpd_users
Stores all registered users (both vendors and browsers).

```sql
CREATE TABLE vpd_users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  full_name VARCHAR(255),
  phone VARCHAR(20),
  company VARCHAR(255),
  designation VARCHAR(255),
  user_type ENUM('vendor', 'browser') NOT NULL DEFAULT 'browser',
  email_verified TINYINT(1) DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_login TIMESTAMP NULL
);
```

### Table: vpd_vendors
One row per vendor listing. Linked to vpd_users via user_id.

```sql
CREATE TABLE vpd_vendors (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  company_name VARCHAR(255) NOT NULL,
  slug VARCHAR(255) NOT NULL UNIQUE,
  tagline VARCHAR(255),
  description TEXT,
  category_id INT,
  industry_tags VARCHAR(500),
  state VARCHAR(100),
  city VARCHAR(100),
  website VARCHAR(255),
  contact_person VARCHAR(255),
  contact_phone VARCHAR(20),
  contact_email VARCHAR(255),
  year_established YEAR,
  logo_path VARCHAR(255),
  status ENUM('pending', 'approved', 'rejected', 'suspended') DEFAULT 'pending',
  rejection_note TEXT,
  submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  approved_at TIMESTAMP NULL,
  updated_at TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES vpd_users(id) ON DELETE CASCADE
);
```

### Table: vpd_categories
Master list of vendor categories (equipment type, service type, etc.)

```sql
CREATE TABLE vpd_categories (
  id INT AUTO_INCREMENT PRIMARY KEY,
  category_name VARCHAR(255) NOT NULL,
  parent_id INT DEFAULT NULL,
  sort_order INT DEFAULT 0
);
```

### Table: vpd_password_resets

```sql
CREATE TABLE vpd_password_resets (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(255) NOT NULL,
  token VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  expires_at TIMESTAMP NOT NULL
);
```

---

## Vendor Registration Form Fields

**Mandatory:**
- Company Name
- Category (dropdown from vpd_categories)
- State (dropdown — all Indian states)
- City
- Contact Person Name
- Contact Phone (hidden from public — login required)
- Contact Email (hidden from public — login required)
- Brief Description (max 500 characters)

**Optional:**
- Tagline (one line — max 100 characters)
- Industry Tags (multi-select — Hospitality, Chemicals, Cement, Power, Pharma, Oil & Gas, Steel, Infrastructure, etc.)
- Website URL
- Year Established
- Company Logo (image upload)

---

## Directory Page — Public

**URL:** vendors.newprojectstracker.in/

**What's visible to everyone (no login):**
- Company name
- Category
- Tagline
- State / City
- Industry tags
- Year established
- Website link (if provided)
- "View Profile" button

**What requires login:**
- Contact person name
- Mobile number
- Email address

**Filters available:**
- Category (dropdown)
- Industry (multi-select)
- State (dropdown)
- Keyword search (searches company name + description + tagline)

**Default sort:** Newest approved first
**Pagination:** 20 listings per page

---

## Vendor Profile Page — Public

**URL:** vendors.newprojectstracker.in/vendor/[slug]/

**Sections:**
1. Company header — logo, name, tagline, category badge, location
2. About — full description
3. Industries served — tag pills
4. Details — year established, website
5. Contact block — shown only to logged-in users. If not logged in, shows "Login to view contact details" prompt with register/login CTA.

---

## Auth Pages

| Page | URL | Notes |
|------|-----|-------|
| Register | /register/ | Captures name, email, phone, password. User selects type: "I am a vendor" or "I want to find vendors". Vendor type → redirected to vendor submission form after registration. Browser type → redirected to directory. |
| Login | /login/ | Standard email + password |
| Forgot Password | /forgot-password/ | Token-based reset |
| My Listing | /my-listing/ | Vendor only — view and edit their approved listing. Shows pending/rejected status if not yet approved. |
| My Account | /my-account/ | Edit profile details, change password |

---

## Admin Panel

**URL:** vendors.newprojectstracker.in/admin/

**Access:** Hardcoded admin emails — icarusprakash@gmail.com, sbirumca85@gmail.com

### Admin Pages

| Page | Purpose |
|------|---------|
| admin/index.php | Dashboard — stat cards (total vendors, pending, approved, rejected, total users, browsers, vendors) + recent submissions table |
| admin/pending.php | Queue of pending vendor submissions — view details, approve or reject (with note) |
| admin/vendors.php | All approved listings — search, filter, edit, suspend, delete |
| admin/users.php | All registered users — search, filter by type, suspend |
| admin/categories.php | Manage vendor categories — add, edit, delete, reorder |

### Approval Flow
1. Vendor submits form → status = `pending`
2. Admin reviews in pending.php
3. Admin approves → status = `approved`, listing goes live, approval email sent to vendor
4. Admin rejects → status = `rejected`, rejection note saved, rejection email sent to vendor
5. Vendor can resubmit after editing (status resets to `pending`)

---

## File Structure

```
/public_html/
│
├── index.php               ← Directory listing page (public)
├── vendor.php              ← Individual vendor profile (public)
├── register.php            ← Registration page
├── login.php               ← Login page
├── logout.php              ← Logout handler
├── forgot-password.php     ← Password reset request
├── reset-password.php      ← Password reset form
├── my-listing.php          ← Vendor's own listing management (auth required)
├── my-account.php          ← User profile edit (auth required)
├── submit-vendor.php       ← Vendor submission form (auth required)
│
├── _auth.php               ← Auth guard — include at top of protected pages
├── _layout.php             ← Shared header + nav + CSS
├── _layout_end.php         ← Shared footer + JS
│
├── admin/
│   ├── index.php           ← Admin dashboard
│   ├── pending.php         ← Approval queue
│   ├── vendors.php         ← All listings management
│   ├── users.php           ← User management
│   └── categories.php      ← Category management
│
├── uploads/
│   └── logos/              ← Vendor logo uploads
│
└── assets/
    ├── css/
    └── js/
```

---

## Design System

Consistent with newprojectstracker.in:
- **Navy:** #1a3c6e
- **Orange:** #e87722
- **Gray BG:** #f5f7fa
- **Fonts:** DM Serif Display (headings) + DM Sans (body)
- Clean, minimal, professional — not flashy
- Mobile responsive throughout

---

## Key Rules (Non-Negotiable)

- Contact phone and email NEVER visible without login — enforced server-side, not just CSS
- All vendor listings go through staff approval before going live — no auto-publish
- Admin panel accessible only to whitelisted email addresses
- Slug generated from company name — URL-safe, lowercase, hyphenated
- Logo uploads: max 1MB, JPG/PNG only, stored in /uploads/logos/
- newprojectstracker.com: NEVER TOUCH (legacy site on GoDaddy)
- This site's DB (nptvd_db) is fully separate — never query newp_ai_engine from this codebase

---

## Phase Plan

### Phase 1 — Core Directory (Build First)
- [ ] Server setup — subdomain, DB, file structure
- [ ] DB tables created
- [ ] Auth system — register, login, logout, forgot password
- [ ] Vendor submission form
- [ ] Admin panel — pending queue, approve/reject
- [ ] Public directory page with filters
- [ ] Individual vendor profile page
- [ ] My Listing page for vendors
- [ ] Basic email notifications (approval, rejection)

### Phase 2 — Enrichment (After Initial Traction)
- [ ] Logo upload on vendor profiles
- [ ] Featured listings (manual admin flag — for later monetisation)
- [ ] Search engine optimisation — meta tags, clean URLs, sitemap
- [ ] "Claim your listing" flow — staff seeds a listing, vendor claims it
- [ ] Social share buttons on vendor profiles

### Phase 3 — Monetisation (When Ready)
- [ ] Paid featured placement
- [ ] Premium vendor profiles (enhanced fields, gallery, brochure upload)
- [ ] Lead tracking — how many times was your contact viewed?
- [ ] Cross-link with NPT Intel — "This vendor is also on NPT Intel"

---

## Launch Campaign Plan

1. Export all hospitality + industrial vendor contacts from NPT subscriber base
2. Email campaign: "List your company free on NPT Vendor Directory — get discovered by project buyers"
3. Incentive: Registered vendors get 5 free NPT project leads per month
4. Separately target project promoters and purchase managers to register as browsers

---

## Session Log

| Day | Date | Key Deliverables |
|-----|------|-----------------|
| 1 | 14 Apr 2026 | README drafted. Concept finalised. Scope locked. |
