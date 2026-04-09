# NPT Intelligence — AI-Powered Capital Investment Intelligence Platform
## Master Build Document
**Version 2.0 — Complete Platform Rebuild**
**Last Updated: 09 April 2026 (Day 5 Complete)**

---

## IMPORTANT — READ FIRST

This README replaces all previous session logs. Version 1.0 (the experimental AI engine at newprojectstracker.in) has been retired. All databases from V1.0 are preserved and reused in V2.0.

**The plan is locked. No mid-build changes. New ideas go to a separate ideas log.**

---

## Day 1 Summary (06 April 2026) ✅

- **Brand name locked:** NPT Intelligence
- **Domain locked:** `newprojectstracker.in` — building here now. Premium `.ai` domain deferred to post-launch (cost prohibitive at this stage)
- **Legacy site clarified:** `newprojectstracker.com` is the existing production site — 50k+ ranked public pages, 30k registered users, live paid dashboard. **Never touch this.**
- **public_html wiped clean:** Only `dumps/` and `logs/` folders retained on server
- **Placeholder live:** `newprojectstracker.in` is live with navy holding page — "NPT Intelligence / Something powerful is coming."
- **Logo:** Brief handed to designer — expected this evening
- **Tagline shortlisted:** *"Where India Invests Next"* (preferred) — to confirm with designer
- **Company rename:** Founder considering renaming to NPT Intelligence Pvt Ltd — deferred, no action needed now
- **Day 2 complete:** Home page live at newprojectstracker.in — full waitlist landing page with form capturing to npt_waitlist table. Server permissions verified clean.

---

## What We Are Building

**India's leading B2B capital investment intelligence platform.**

A full SaaS product with:
- A public corporate website (lead generation + SEO)
- A subscriber dashboard (core product)
- A CRM subdomain for internal operations
- Newsletter + alerts system
- Mini B2B apps for premium users
- Razorpay subscription payments
- AI-generated intelligence layers

---

## Domain & Brand (Locked ✅)

- **Platform name:** NPT Intelligence
- **Build domain:** `newprojectstracker.in`
- **Legacy domain:** `newprojectstracker.com` — production site, 50k+ ranked pages, 30k users. **Never touch.**
- **Premium domain:** `nptintelligence.ai` — desirable, deferred to post-launch (cost prohibitive now)
- **Tagline:** *"Where India Invests Next"* — preferred, confirm with designer

**Context:** This new platform is a complementary AI-enriched product alongside the legacy .com site. Both coexist. Existing NPT users get access to this as a complement; new subscribers come in independently.

---

## Server Details
- **Hosting:** Hostinger VPS — AlmaLinux 9, CyberPanel
- **IP:** 31.97.228.143
- **Web server:** LiteSpeed | **PHP:** 8.0.30 | **DB:** MariaDB 10.11.13

## Database Details
- **DB name:** `newp_ai_engine` (preserved from V1.0)
- **DB user:** `newp_npt_ai_user` | **Password:** `npt_ai_user@123`
- **MySQL root password:** `1a626244c103903e66b39c5973e7f0d73e9915ec0f83878e`
- **MySQL config:** `/root/.my_temp.cnf`

## GitHub Repository
`https://github.com/icarusprakash/npt-ai-engine`
**Session Rule:** Download README at session start, upload updated README at session end.

---

## Database State (Preserved from V1.0)

### Source Tables (read-only — from GoDaddy export)
| Table | Rows | Notes |
|-------|------|-------|
| `npis_projects` | 6,511 | 2024-2025 data. proj_show='YES' filter. |
| `npis_companies` | 4,602 | comp_slug column added |
| `npis_refer` | 9,226 | Links projects ↔ companies |

### AI Engine Tables
| Table | Rows | Notes |
|-------|------|-------|
| `npt_ai_enrichment` | 475 done, 5,994 failed | API credits exhausted — top up $20 |
| `npt_product_tags` | 0 | Script ready — needs API credits |
| `npt_project_tags` | 0 | Script ready — needs API credits |
| `npt_ai_users` | 1 | Test user only — will rebuild user system |

### Data Remaining to Import
| Batch | Period | Status |
|-------|--------|--------|
| Batch 1 | 2024-2025 | ✅ Done |
| Batch 2 | 2022-2023 | ⬜ Pending |
| Batch 3 | 2020-2021 | ⬜ Pending |
| Batch 4 | 2017-2019 | ⬜ Pending |

### SQL Import Process (Lessons Learned)
GoDaddy exports need Python cleaning before import:
1. Replace `\'` with `''`
2. Convert multi-row INSERTs to individual row INSERTs (semicolons in text fields break parsers)
3. Fix DD-MM-YYYY → YYYY-MM-DD (companies table)
4. Fix smart quotes, remove `\r` chars
Import via phpMyAdmin with Partial Import ON.

---

## API Credits (Anthropic)
**Current balance: $0.00 — needs top up**
- Go to `https://console.anthropic.com` → Billing → Buy Credits
- Add $20 USD to start (covers tagging + enrichment for current batch)

**After top up — run enrichment:**
```bash
mysql --defaults-file=/root/.my_temp.cnf -e "UPDATE newp_ai_engine.npt_ai_enrichment SET enrichment_status='pending' WHERE enrichment_status='failed';"

nohup bash -c 'while true; do php /var/www/html/enrich.php --cron >> /var/log/npt_enrich.log 2>&1; pending=$(mysql --defaults-file=/root/.my_temp.cnf -se "SELECT COUNT(*) FROM newp_ai_engine.npt_ai_enrichment WHERE enrichment_status='"'"'pending'"'"';"); if [ "$pending" -eq "0" ]; then break; fi; sleep 2; done' &
```

**After top up — run product tagging:**
```bash
nohup bash -c 'while true; do php /var/www/html/tag.php --cron >> /var/log/npt_tag.log 2>&1; remaining=$(mysql --defaults-file=/root/.my_temp.cnf -se "SELECT COUNT(*) FROM newp_ai_engine.npis_projects WHERE proj_show='"'"'YES'"'"' AND proj_id NOT IN (SELECT DISTINCT proj_id FROM newp_ai_engine.npt_project_tags);"); if [ "$remaining" -eq "0" ]; then break; fi; sleep 2; done' &
```

---

## Day 1 Startup Checklist ✅ COMPLETE

### Step 1 — Clean up public_html
Login to CyberPanel → File Manager → `public_html/`
Delete ALL files and folders except:
- Keep: `dumps/` folder (SQL files)
- Keep: `logs/` folder
- Delete everything else

**Terminal alternative (faster):**
```bash
cd /home/newprojectstracker.in/public_html
find . -maxdepth 1 -not -name '.' -not -name 'dumps' -not -name 'logs' -exec rm -rf {} +
```

### Step 2 — Domain ✅ Locked: newprojectstracker.in
If keeping `.in` → no action needed
If switching to `.ai`:
- Purchase `newprojectstracker.ai`
- Point DNS A record to `31.97.228.143`
- Add domain to CyberPanel
- SSL certificate will auto-generate

### Step 3 — Confirm logo
Share logo PNG file. If no logo ready, we'll create a text-based logo in code.

### Step 4 — Begin Day 1 module
Build design system + home page (which doubles as the waitlist landing page — see below).

---

## Design System (Locked)

### Fonts
- **Primary:** Inter (Google Fonts) — used throughout
- **Weights:** 400 (body), 600 (subheadings), 700 (headings), 800 (hero)

### Colors
| Name | Hex | Usage |
|------|-----|-------|
| Navy Primary | `#1a3c6e` | Headers, sidebar, primary buttons |
| Navy Dark | `#0d2340` | Hero gradient, footer |
| Navy Light | `#e8f0fb` | Tag backgrounds, hover states |
| Orange Accent | `#e87722` | CTAs, highlights, active states |
| Orange Dark | `#cf6a1a` | Hover on orange |
| Gray BG | `#f5f7fa` | Dashboard content area background |
| Gray Border | `#e0e0e0` | Card borders, dividers |
| Text Primary | `#1a1a2e` | Main body text |
| Text Secondary | `#666` | Secondary text |
| Text Muted | `#999` | Meta, timestamps |
| White | `#ffffff` | Cards, sidebar |
| Success | `#2e7d32` | Active status, positive signals |
| Warning | `#e65100` | Alerts, beta notices |
| Danger | `#c62828` | Errors |

### Spacing Scale
- xs: 4px | sm: 8px | md: 16px | lg: 24px | xl: 32px | 2xl: 48px | 3xl: 64px

### Border Radius
- Cards: 8px | Buttons: 4px | Tags: 20px | Inputs: 4px

### Shadows
- Card: `0 2px 8px rgba(0,0,0,0.08)`
- Card hover: `0 4px 16px rgba(0,0,0,0.12)`
- Modal: `0 8px 32px rgba(0,0,0,0.16)`

### Public Website Style (inspired by Biltrax)
- Clean white body, generous whitespace
- Large bold hero H1 — 48px desktop, 32px mobile
- H2 sections — 32px, dark navy
- Paragraph — 16px, line-height 1.8
- Full-width section alternating white/light-gray
- Animated counter stats on scroll
- Sticky header that adds shadow on scroll

### Dashboard Style (inspired by Adminex + ByeWind)
- Left sidebar: 240px wide, white, icon + label nav
- Collapsible to 60px (icons only)
- Active item: navy fill, white text
- Header: white, 60px height, search + notifications + avatar
- Content area: gray `#f5f7fa` background
- Stat cards: white, border, rounded, icon in colored circle
- Charts: Chart.js, minimal, clean

---

## Platform Architecture

```
newprojectstracker.in (or .ai)
├── / (public home page — also serves as waitlist landing page in Phase 1)
├── /early-access (waitlist landing page — Phase 2 onwards, after corporate site is live)
├── /about
├── /solutions
├── /pricing
├── /contact
├── /projects/{slug} (SEO project pages — public)
├── /companies/{slug} (SEO company pages — public)
├── /tags/{slug} (SEO tag pages — public)
├── /register
├── /login
├── /forgot-password
└── /dashboard (subscriber area)
    ├── /dashboard (entry — feed + stats)
    ├── /dashboard/projects (search + filter)
    ├── /dashboard/projects/{slug} (full project page)
    ├── /dashboard/companies/{slug} (full company page)
    ├── /dashboard/briefcase (saved projects)
    ├── /dashboard/news (project news feed)
    ├── /dashboard/analytics (charts + insights)
    ├── /dashboard/apps/meeting-prep
    ├── /dashboard/apps/tender-summarizer
    ├── /dashboard/apps/contact-manager
    ├── /dashboard/profile
    ├── /dashboard/subscription
    └── /dashboard/workspaces (Premium — 5 custom dashboards)

crm.newprojectstracker.in (internal)
├── / (activity dashboard)
├── /payments
├── /quotations
├── /users
└── /sales-calls
```

---

## User Types & Access

| Type | Projects/Month | Daily Cap | Weekly Cap | Price | Notes |
|------|---------------|-----------|------------|-------|-------|
| Free | 5 | No cap | No cap | Free | No rollover. Briefcase available. |
| Basic | 400 | 30 | 100 | ₹14,950 + GST / month | 1 user. Briefcase. |
| Premium | 10,000 | 50 | 600 | ₹99,000 + GST / year | 1 login, 5 workspaces. All apps. |

**Free user guardrails:**
- "Viewing this project will use 1 of your 5 monthly credits. You have X remaining. Continue?"
- After 5 used: "You've used all free credits this month. Upgrade to continue."
- Imported old NPT users: shown onboarding message explaining why they're here

**Premium workspaces:**
- 1 login, 5 named saved filter configurations
- Each workspace remembers: industry filter, state filter, product tags, last viewed
- Switch via dropdown in dashboard header
- User can name each workspace (e.g. "Chemicals Gujarat", "Steel Projects")

---

## Waitlist Landing Page Strategy

### Phase 1 — Home page IS the landing page
After Week 1 (public corporate pages) is complete, the site goes live immediately with the home page doubling as a waitlist capture page.

**Goal:** Build an audience while the dashboard is being built.

**The home page will have:**
- Strong hero — what NPT is, who it's for, why it matters
- Enough signal to kindle curiosity — project count stats, industry coverage, key use cases
- A prominent capture form: name + email + company + "I'm an existing NPT subscriber" checkbox
- Two queues stored in DB:
  - General waitlist (new visitors)
  - Priority queue (existing NPT subscribers — checkbox triggers this)
- CTA where the dashboard link would normally be → routes to the capture form instead
- No login, no dashboard access — just the waitlist

**What the home page will NOT have in Phase 1:**
- Separate About / Solutions / Pricing pages (not yet built)
- Any working dashboard links

### Phase 2 — Full corporate site goes live
Once enough dashboard progress exists to show prospects:
- Build About, Solutions, Pricing, Contact pages
- Home page becomes a proper corporate homepage
- Waitlist capture form moves to `/early-access`
- `/early-access` remains live even after full launch for drip campaigns

### Database Table: npt_waitlist
```
id, email, full_name, company, is_existing_subscriber (boolean),
queue_type (general/priority), ip_address, joined_at
```

### Rules for the waitlist page
- Content for the page decided at build time (not pre-planned)
- Keep it curiosity-first — don't reveal the full product
- One clear CTA above the fold
- Mobile responsive — many visitors will be on phones

---

## Subscription & Payments

### Online (Razorpay)
- User selects plan → Razorpay checkout → webhook → auto-activate

### Offline
- User pays via bank/cheque → informs via email/WhatsApp
- Admin panel → Payments → Mark as paid → Activate subscription

### Quotation Generator
- User fills form (name, company, designation, GST number)
- System generates branded PDF quote
- Quote stored in DB, downloadable anytime

---

## New Database Tables (to create in V2.0)

### npt_waitlist (Phase 1 — build on Day 1)
```
id, email, full_name, company, is_existing_subscriber (boolean),
queue_type (general/priority), ip_address, joined_at
```

### npt_users (replaces npt_ai_users)
```
id, email, username, password_hash, full_name, company,
designation, phone, plan_type (free/basic/premium),
plan_status (active/expired/suspended), plan_start, plan_end,
credits_monthly, credits_used, credits_reset_date,
daily_limit, daily_used, daily_reset,
weekly_limit, weekly_used, weekly_reset,
source (self_register/imported/admin),
imported_from_npt (boolean),
created_at, last_login
```

### npt_subscriptions
```
id, user_id, plan_type, amount, currency, gst_amount,
payment_method (online/offline), razorpay_order_id,
razorpay_payment_id, status, activated_by, activated_at,
starts_at, ends_at, created_at
```

### npt_quotations
```
id, user_id, quote_number, company_name, contact_name,
designation, gst_number, plan_type, amount, gst_amount,
total_amount, pdf_path, created_at, downloaded_at
```

### npt_briefcase
```
id, user_id, proj_id, added_at
```

### npt_workspaces (Premium)
```
id, user_id, workspace_name, industry_filter, state_filter,
tag_filter, investment_filter, is_active, created_at
```

### npt_news
```
id, news_slug, news_title, company_name, company_slug,
news_details, news_excerpt, industry, category,
published_date, source, created_at
```

### npt_hattips
```
id, user_id, company_name, project_description,
status (pending/researching/added/rejected),
proj_id_added, credits_given, created_at
```

---

## Execution Plan

### Week 1 — Foundation & Public Website
| Day | Module | Task |
|-----|--------|------|
| 1 | Setup + Design | ✅ public_html wiped, domain locked, placeholder live at newprojectstracker.in. Logo with designer. Day 2: design system + home page. |
| 2 | Waitlist Landing Page | ✅ Home page built and live at newprojectstracker.in — hero, problem section, timeline visual, platform features, 16 industries grid, who-its-for, waitlist form with DB capture, footer. DM Serif Display + DM Sans fonts. Fully mobile responsive. |
| 3 | Auth Pages | ✅ Register, Login, Forgot Password, Logout, Dashboard placeholder — all live. npt_users + npt_password_resets tables auto-created. Session auth working. |
| 4 | Pricing Page | ✅ dashboard/pricing.php — 3 plan cards (Free/Basic/Premium), GST calculated, current plan highlighted, upgrade modal with email fallback, FAQ accordion, contact band. Razorpay integration deferred — placeholder in place. |
| 5 | SEO Pages | ✅ projects_slug.php, companies_slug.php, states_slug.php live. Clean public titles (no company names), content gate for guests, pagination lock on page 2+. proj_industry used as primary sector field (proj_sector is NULL for most records). |

**→ After Day 5: Go live. Home page serves as waitlist landing page. Begin building audience.**

### Week 2 — Dashboard Core
| Day | Module | Task |
|-----|--------|------|
| 6 | Dashboard Layout | Sidebar, header, routing system |
| 7 | Dashboard Entry | Feed page — project cards + news + stats widgets |
| 8 | Project Listing | Search + 4 filters + results table |
| 9 | Project Story | Full project page (dashboard version) |
| 10 | Briefcase | Save/remove/view saved projects |

### Week 3 — User System & Credits
| Day | Module | Task |
|-----|--------|------|
| 11 | User Registration | Self-register flow + email verification |
| 12 | Credit System | Per-project deduction, guardrails, confirmations |
| 13 | Free User Flow | Onboarding, guardrail messages, upgrade prompts |
| 14 | Subscription Payment | Razorpay checkout + webhook + auto-activate |
| 15 | Offline Payment | Admin activates manually + notification to user |

### Week 4 — Admin Panel
| Day | Module | Task |
|-----|--------|------|
| 16 | Admin Layout | Sidebar, user management table |
| 17 | User Control | View/edit/override credits per user |
| 18 | Payment Dashboard | Online + offline payment records |
| 19 | Daily Projects CRUD | Add 15 new projects daily form |
| 20 | Quotation Generator | Form + branded PDF generation |

### Week 5 — News + Data
| Day | Module | Task |
|-----|--------|------|
| 21 | News Import | Import news DB from GoDaddy |
| 22 | News Public | Excerpts on public website |
| 23 | News Dashboard | Full news feed inside dashboard |
| 24 | Import Old Users | 20,000 NPT users as free users |
| 25 | Import Balance Data | 2022-2023 project batch |

### Week 6 — Premium Features
| Day | Module | Task |
|-----|--------|------|
| 26 | Workspaces | 5 named custom dashboards for Premium |
| 27 | Newsletter System | Daily digest email setup (Amazon SES) |
| 28 | Alert Preferences | Paid users configure newsletter filters |
| 29 | Analytics Page | Charts: industry breakdown, state map, investment trends |
| 30 | Report Generator | Filter projects + generate branded PDF report |

### Week 7 — Mini Apps + CRM
| Day | Module | Task |
|-----|--------|------|
| 31 | AI Meeting Prep | Input company → get briefing doc |
| 32 | Tender Summarizer | Paste tender → structured summary |
| 33 | Smart Contact Manager | Save contacts with notes |
| 34 | CRM Subdomain | Setup + user activity dashboard |
| 35 | CRM Sales Log | Sales call log for staff |

### Week 8 — Polish + Launch
| Day | Module | Task |
|-----|--------|------|
| 36 | Hat-tip System | Project request form + internal workflow |
| 37 | Anti-scraping | Rate limiting, honeypots, bot detection |
| 38 | Security Audit | SQL injection, XSS, session security |
| 39 | Performance | Indexes, caching, image optimization |
| 40 | Full Launch | Move waitlist form to /early-access, onboard existing NPT users, send emails |

### Background (runs parallel, when API credits available)
- AI enrichment — Layer 2 (opportunity intelligence)
- Product tag generation
- Company profile AI layer
- Layer 3 — Live project status tracker

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Backend | PHP 8.0 |
| Database | MariaDB (existing) |
| Frontend | HTML + CSS (Inter font) + Vanilla JS |
| Charts | Chart.js |
| PDF | mPDF (PHP) |
| Payments | Razorpay |
| Email | Amazon SES (to set up) |
| Icons | Heroicons or Lucide (SVG inline) |
| CRM | Same PHP stack, subdomain |

---

## Rules (Non-Negotiable)

1. Plan is locked — no changes mid-build
2. New ideas → noted separately, built after launch
3. 3 hours/day — 1-2 modules per session
4. Complete each module before moving to next
5. Design references approved — no redesigns mid-build
6. Test each module before marking complete
7. README updated at end of every session

---

## Key Business Decisions (Locked)

| Decision | Choice |
|----------|--------|
| Domain | newprojectstracker.in ✅ Locked |
| Pricing — Basic | ₹14,950 + GST / month / 1 user |
| Pricing — Premium | ₹99,000 + GST / year / 1 user |
| Free plan | 5 projects/month, no rollover |
| Payment gateway | Razorpay (existing account) |
| Email service | Amazon SES |
| Premium workspaces | 1 login + 5 named filter sets |
| Offline payment | Admin activates manually from panel |
| Old NPT users | Import as free users near launch |
| Newsletter | Daily digest — free gets standard, paid gets filtered |
| nptstore.in | Integrate report store after core launch |
| News | Free forever for all users — daily hook |
| Waitlist home page | Live after Week 1 — moves to /early-access at full launch ✅ |

---

## Anthropic API Cost Estimate (Full Platform)

| Task | Cost |
|------|------|
| Layer 2 — 5,994 remaining projects | ~$18 |
| Product tagging — 6,511 projects | ~$13 |
| Layer 3 status — 6,469 projects | ~$78 |
| Layer 2 — future 30,000 projects | ~$90 |
| Layer 3 — future 30,000 projects | ~$360 |
| Company profile AI — 4,602 companies | ~$30 |
| **Total** | **~$589** |
Credits never expire. Buy in $20-50 batches as needed.
