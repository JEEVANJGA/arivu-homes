# Arivu Homes — Development Lifecycle Report

> **Generated:** February 2026  
> **Repository:** https://github.com/Rohithg86/arivu-homes  
> **Live URL:** https://arivu-homes.vercel.app//  
> **Total Commits:** ~90 commits from initial scaffold to current production state

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Tech Stack](#2-tech-stack)
3. [Phase 1 — Foundation & Scaffold](#3-phase-1--foundation--scaffold)
4. [Phase 2 — Core Pages & Admin Panel](#4-phase-2--core-pages--admin-panel)
5. [Phase 3 — UI & UX Overhaul](#5-phase-3--ui--ux-overhaul)
6. [Phase 4 — Integrations (Email, Storage, Database)](#6-phase-4--integrations-email-storage-database)
7. [Phase 5 — Content, Team & Testimonials](#7-phase-5--content-team--testimonials)
8. [Phase 6 — Navigation, BOQ & Mobile Experience](#8-phase-6--navigation-boq--mobile-experience)
9. [Phase 7 — SEO & Launch Preparation](#9-phase-7--seo--launch-preparation)
10. [Current State of the Codebase](#10-current-state-of-the-codebase)
11. [Known Gaps & What Needs to Be Done Next](#11-known-gaps--what-needs-to-be-done-next)

---

## 1. Project Overview

**Arivu Homes Private Limited** is a Bangalore-based construction and architectural design company. This website serves as their primary digital presence — a marketing site, lead generation tool, and internal project management platform.

### Business Goals the Website Serves
- Showcase services (residential, commercial, farm house, architectural design, structural engineering, renovation)
- Display ongoing and completed projects with images and progress tracking
- Introduce the founding team with individual profile pages
- Educate potential clients on the construction journey (5-step process with payment milestones)
- Provide a BOQ (Bill of Quantities) calculator for cost estimation
- Capture leads via a floating contact widget (email + WhatsApp + Instagram)
- Allow admins to manage projects, team members, and assets via a protected admin panel

---

## 2. Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | Next.js 15 (App Router, React 19) |
| **Language** | TypeScript |
| **Styling** | Tailwind CSS v4 |
| **Database** | PostgreSQL via Prisma ORM |
| **File Storage** | Vercel Blob Storage |
| **Email** | Resend API |
| **Lead Capture** | Google Sheets API (primary) + Resend email (fallback) |
| **Auth** | Custom HMAC-SHA256 signed session tokens (cookie-based) |
| **Deployment** | Vercel |
| **Node Version** | 20.x (pinned via `.nvmrc`) |
| **Animation** | Framer Motion |
| **Forms** | React Hook Form + Zod validation |

---

## 3. Phase 1 — Foundation & Scaffold

**Commits:** `6d05395` → `7c493e9`  
**Key commits:** `chore: initial commit`, `Rebrand to Arivu Homes Private Limited`

### What Was Built
- Next.js 15 project initialized with TypeScript, Tailwind CSS, and ESLint
- Basic homepage with company name, tagline, and service cards
- Initial branding: "Arivu Homes Private Limited" with tagline "Building Dreams with Precision"
- Prisma ORM configured with PostgreSQL schema (TeamMember, Asset, Service, Project models)
- Node 20 pinned for Vercel compatibility (`.nvmrc`)
- TypeScript and ESLint errors fixed for clean deployment

### Database Schema (established in this phase)
```
TeamMember  — id, name, role, bio, photoUrl
Asset       — id, type (SITE_PHOTO/DESIGN/ARCHITECTURE/INNOVATION), title, description, url
Service     — id, name, description, iconKey
Project     — id, name, location, client, type, startDate, expectedCompletion,
              completionPercentage, status, description, images (JSON array)
```

---

## 4. Phase 2 — Core Pages & Admin Panel

**Commits:** `a06dd8f` → `65b21f6`  
**Key commits:** `feat(admin): auth + admin-only projects`, `feat(admin): login with username + password`, `feat(projects): add Jigani and Magadi project images`

### What Was Built

#### Admin Authentication System
- Custom JWT-style session tokens using HMAC-SHA256 (no external auth library)
- Cookie-based session management (`arivu_admin` cookie)
- Admin login page at `/admin-login` with username + password
- Protected admin panel at `/admin` (redirects to login if unauthenticated)
- Logout endpoint at `/api/admin/logout`
- Session verification middleware in `src/lib/adminAuth.ts`

#### Admin Panel Features
Three-tab interface:
1. **Team tab** — Add team members (name, role, bio, photo URL)
2. **Assets tab** — Upload site photos, designs, architecture images, innovation assets
3. **Projects tab** — Full CRUD: create, edit, delete projects with all fields

#### Projects API (`/api/projects`)
- `GET` — Public: fetch all projects ordered by last updated
- `POST` — Admin-only: create new project
- `PUT` — Admin-only: update existing project
- `DELETE` — Admin-only: delete project by ID

#### First Real Project Data
- **Jigani project** — Images added: `1.jpg`, `2.jpg`, `elevation.jpg`
- **Magadi project** — Images added: `1.jpg`, `2.jpg`, `elevation.jpg`
- Images stored in `public/uploads/projects/` directory

---

## 5. Phase 3 — UI & UX Overhaul

**Commits:** `a3ad6e0` → `83ae3a8`  
**Key commits:** `ui: light tile palettes; minimal logo`, `ui: new house logo; clearer hero`, `feat: mobile responsiveness`, `715b313 add home logo component`

### What Was Built

#### Branding & Logo Evolution
- Multiple iterations of the logo: text-only → icon + text → house SVG icon
- `HomeLogo` component created (`src/components/HomeLogo.tsx`) — renders the SVG wordmark
- House icon integrated into the header nav bar (SVG path for a home shape)
- Logo files added: `logo.svg`, `logo-house.svg`, `logo-simple.svg`, `logo-home.jpg`

#### Hero Section
- Full-viewport hero with video background (later replaced with slideshow)
- Overlay text with company name, tagline, and CTA buttons
- Managing Partner and Senior Architect names linked to individual profile pages
- CTA buttons: Explore Services, View Projects, Your Journey, Contact Us

#### Services Grid
- 6 service cards on the homepage:
  1. Residential Construction
  2. Commercial Construction
  3. Farm House Construction
  4. Architectural Design
  5. Structural Engineering
  6. Renovation, Remodeling & PM
- Each card links to a dedicated service subpage at `/services/[slug]`

#### Mobile Responsiveness
- Responsive nav with hamburger menu (`MobileNav` component)
- Responsive hero text sizing (5xl → 7xl → 8xl)
- Responsive grid layouts across all pages
- Mobile-specific font size fixes and spacing adjustments

#### Services Subpages
- Dynamic route `/services/[slug]` with detailed content per service
- Async route params handling for Next.js 15 compatibility

---

## 6. Phase 4 — Integrations (Email, Storage, Database)

**Commits:** `d66aa77` → `422540f`  
**Key commits:** `feat: integrate Vercel Blob storage`, `feat: switch to Resend API`, `feat: add home icon to logo, fix database for Vercel`

### What Was Built

#### Vercel Blob Storage
- Replaced local file storage with Vercel Blob for production image uploads
- Upload API at `/api/upload/route.ts` — accepts base64-encoded images, stores to Blob
- Admin panel asset upload now uses Blob URLs instead of local paths
- `@vercel/blob` package added as dependency

#### Resend Email Integration
- Contact form submissions trigger email notifications via Resend API
- Email sent to `contact.arivuhomes@gmail.com` with lead details
- Requires `RESEND_API_KEY` environment variable
- Graceful fallback: if Resend fails, Google Sheets still captures the lead

#### Google Sheets Lead Capture
- Primary lead storage: Google Sheets via `googleapis` package
- Appends rows: `[timestamp, name, phone, email, requirement]`
- Requires `GOOGLE_SHEETS_CLIENT_EMAIL`, `GOOGLE_SHEETS_PRIVATE_KEY`, `GOOGLE_SHEET_ID` env vars
- Dual-write strategy: Sheets + email notification for redundancy

#### Database Migration to PostgreSQL
- Switched from SQLite (local dev) to PostgreSQL for Vercel production
- `prisma/schema.prisma` updated: `provider = "postgresql"`
- `DATABASE_URL` environment variable required
- `prisma generate` runs automatically via `postinstall` script

---

## 7. Phase 5 — Content, Team & Testimonials

**Commits:** `e9f60b5` → `83ed310`  
**Key commits:** `feat: update team experience`, `feat: testimonials, farm house service, video only home`, `feat: phase 8 - larger title, bold names`

### What Was Built

#### Team Profile Pages
Three individual team member pages created:

| Team Member | URL | Role |
|---|---|---|
| Rohith Gopal | `/team/rohith-gopal` | Managing Partner |
| Chethan Kumar S | `/team/chethan-kumar-s` | Managing Partner |
| Shashank D | `/team/shashank-d` | Senior Architect |

- Profile photos added to `public/profile/` directory
- Team listing page at `/team`
- Multiple iterations of profile photos and name corrections

#### Testimonials Section
- `TestimonialSlideshow` component created (`src/components/TestimonialSlideshow.tsx`)
- Auto-rotating testimonials from past clients
- Displayed on homepage in a blue-tinted section
- "What Our Clients Say" heading with client stories

#### Completed Projects Gallery
- "Yash" completed project added with 10 photos (`public/uploads/completed/yash/1-10.jpg`)
- Projects page updated to show completed projects section

#### Journey Page (`/journey`)
- 5-step construction journey with detailed breakdown:
  1. Consultation & Design (10% payment)
  2. Finalizing Plan & Mobilization (20% payment)
  3. Sub-Structure Construction (25% payment)
  4. Super-Structure Construction (25% payment)
  5. Finishes & Handover (20% payment)
- Each step shows: description, detailed checklist, payment milestone, and step image
- Step images: `public/images/journey/step-1.png` through `step-5.png`
- Client journey video: `public/videos/client-journey.mp4`
- Vertical timeline visual with gradient line

---

## 8. Phase 6 — Navigation, BOQ & Mobile Experience

**Commits:** `288d8cb` → `c1f2170`  
**Key commits:** `feat: move BOQ to top nav`, `feat: rename completed projects`, `fix: mobile visibility issues`, `fix: clean header typography`

### What Was Built

#### BOQ Calculator (`/boq`)
Full Bill of Quantities calculator with:
- **6 construction categories:** Earthwork, Concrete Work, Masonry, Finishing, Electrical, Plumbing
- **20 pre-populated line items** with editable description, unit, quantity, and rate
- Real-time amount calculation (quantity × rate)
- Category subtotals and grand total
- Summary panel: Contingency (5%), GST (18%), Grand Total
- Print functionality (`window.print()`)
- Add/remove items per category
- Project information header (name, location, client, date)

#### Navigation Restructuring
Multiple iterations to find the right nav structure:
- BOQ Calculator moved to top navigation bar
- Journey page moved to hero section CTAs
- Final nav: **Services | BOQ Calculator | Team | Projects | Admin**
- Mobile nav (`MobileNav` component) with hamburger menu
- Sticky header with glass-morphism effect (`bg-white/80 backdrop-blur`)

#### GSTIN Display
- GSTIN placeholder added to header: shows "PENDING..." in monospace font
- Positioned next to logo with a divider line
- Visible on desktop, hidden on very small screens

#### Hero Slideshow
- `HeroSlideshow` component created (`src/components/HeroSlideshow.tsx`)
- Replaced static video background with auto-rotating image/video slideshow
- Videos: `public/videos/home-1.mp4`, `public/videos/home-2.mp4`
- Smooth crossfade transitions between slides

#### Quick Access Section
- Three-card grid below hero: Current Projects, Your Journey, Meet Our Team
- Hover animations with color-coded borders and shadows
- Replaced earlier "BOQ highlight" design

---

## 9. Phase 7 — SEO & Launch Preparation

**Commits:** `e044313` → `7f6268e`  
**Key commits:** `feat: SEO improvements`, `feat: Launch prep: Content updates`, `UI tweak: Always show Stay Tuned msg; SEO: Added footer social links`

### What Was Built

#### Technical SEO
- **Sitemap** (`/sitemap.ts`) — Auto-generated XML sitemap covering:
  - Homepage (priority 1.0, yearly)
  - Services (priority 0.8, monthly)
  - Projects (priority 0.8, weekly)
  - Team (priority 0.5, monthly)
  - BOQ (priority 0.5, yearly)
- **Robots.txt** (`/robots.ts`) — Allows all crawlers, points to sitemap
- **JSON-LD Structured Data** — `LocalBusiness` schema in `layout.tsx`:
  - Business name, URL, phone, email, address
  - Opening hours (Mon–Sat, 9am–6pm)
  - Social profiles (Instagram, WhatsApp)

#### Open Graph & Meta Tags
- `layout.tsx` metadata object with:
  - Title: "Arivu Homes Private Limited - Building Dreams with Precision"
  - Description with service keywords
  - Keywords array: 10 targeted search terms for Bangalore construction
  - Open Graph: title, description, URL, site name, locale (en_IN), type
  - Robots: index + follow

#### Social Links in Footer
- Instagram link: `https://www.instagram.com/arivuhomes/`
- WhatsApp link: `https://wa.me/916361867464`
- Both open in new tab with `rel="noopener noreferrer"`

#### Floating Contact Widget (`ContactWidget`)
- Three floating buttons (bottom-right corner):
  1. **Instagram** — gradient button linking to Instagram profile
  2. **WhatsApp** — green button linking to WhatsApp chat
  3. **Contact Us** — blue button opening lead capture modal
- Modal form with validation:
  - Name (required)
  - Phone (required, 10-digit Indian number validation)
  - Email (required, format validation)
  - Requirement (optional textarea)
- ESC key closes modal
- Success/error state handling
- Submits to `/api/contact` → Google Sheets + Resend email

#### Content Updates for Launch
- "Stay Tuned" message always visible on projects page (not conditional)
- Service descriptions refined
- Team bios and experience updated
- Contact information verified

---

## 10. Current State of the Codebase

### Pages & Routes

| Route | Status | Description |
|---|---|---|
| `/` | ✅ Complete | Homepage with hero slideshow, quick access, services grid, testimonials, contact |
| `/services` | ✅ Complete | Services listing page |
| `/services/[slug]` | ✅ Complete | 6 individual service detail pages |
| `/projects` | ✅ Complete | Projects gallery with ongoing + completed sections |
| `/team` | ✅ Complete | Team listing page |
| `/team/rohith-gopal` | ✅ Complete | Rohith Gopal profile |
| `/team/chethan-kumar-s` | ✅ Complete | Chethan Kumar S profile |
| `/team/shashank-d` | ✅ Complete | Shashank D profile |
| `/journey` | ✅ Complete | 5-step construction journey with payment milestones |
| `/boq` | ✅ Complete | Bill of Quantities calculator with print support |
| `/admin` | ✅ Complete | Protected admin panel (projects, team, assets) |
| `/admin-login` | ✅ Complete | Admin login page |

### API Endpoints

| Endpoint | Method | Auth | Description |
|---|---|---|---|
| `/api/projects` | GET | Public | Fetch all projects |
| `/api/projects` | POST | Admin | Create project |
| `/api/projects` | PUT | Admin | Update project |
| `/api/projects` | DELETE | Admin | Delete project |
| `/api/team` | POST | Public | Add team member |
| `/api/assets` | GET | Public | Fetch assets |
| `/api/upload` | POST | Public | Upload image to Vercel Blob |
| `/api/boq` | GET/POST | Public | BOQ data |
| `/api/contact` | POST | Public | Submit contact form → Sheets + Email |
| `/api/admin/login` | POST | — | Admin login |
| `/api/admin/logout` | POST | — | Admin logout |
| `/api/admin/me` | GET | Admin | Check session |

### Components

| Component | Description |
|---|---|
| `HeroSlideshow` | Auto-rotating video/image slideshow for homepage hero |
| `TestimonialSlideshow` | Auto-rotating client testimonials |
| `ContactWidget` | Floating contact buttons + lead capture modal |
| `MobileNav` | Hamburger menu for mobile navigation |
| `HomeLogo` | SVG wordmark logo component |

### Environment Variables Required

```env
DATABASE_URL=                    # PostgreSQL connection string
ADMIN_USERNAME=                  # Admin login username
ADMIN_PASSWORD=                  # Admin login password
ADMIN_AUTH_SECRET=               # Secret for HMAC session signing
GOOGLE_SHEETS_CLIENT_EMAIL=      # Google service account email
GOOGLE_SHEETS_PRIVATE_KEY=       # Google service account private key
GOOGLE_SHEET_ID=                 # Target Google Sheet ID
RESEND_API_KEY=                  # Resend API key for email notifications
BLOB_READ_WRITE_TOKEN=           # Vercel Blob storage token
```

---

## 11. Known Gaps & What Needs to Be Done Next

### 🔴 Critical / Blocking

| Item | Details |
|---|---|
| **GSTIN Registration** | Header shows "PENDING..." — needs to be updated once GSTIN is obtained from GST portal |
| **Admin credentials in env** | `ADMIN_USERNAME` and `ADMIN_PASSWORD` must be set in Vercel environment variables for production security |
| **`ADMIN_AUTH_SECRET`** | Currently falls back to a hardcoded default — must be set to a strong random string in production |

### 🟡 Important Improvements

| Item | Details |
|---|---|
| **Projects page — "Stay Tuned" message** | Currently shows a static "Stay Tuned" message when no projects are loaded. Real project data should be seeded/added via admin panel |
| **BOQ — No save/export to PDF** | The BOQ calculator only supports browser print. A proper PDF export (using a library like `jsPDF` or server-side PDF generation) would be more professional |
| **BOQ — No persistence** | BOQ data is purely client-side state — refreshing the page loses all entered data. Consider adding save-to-database or local storage persistence |
| **Admin panel — No image upload for projects** | Project images are entered as URL strings (one per line). A proper drag-and-drop image uploader connected to Vercel Blob would be much better UX |
| **Team API — No auth protection** | `/api/team` POST is not protected by admin auth — anyone can add team members. Should add the same `verifyAdminSession` check |
| **Upload API — No auth protection** | `/api/upload` POST is also unprotected. Should require admin session |
| **Sitemap — Missing service subpages** | The sitemap only includes top-level routes. The 6 service subpages (`/services/residential-construction`, etc.) are not included |

### 🟢 Nice-to-Have / Future Features

| Item | Details |
|---|---|
| **Blog / News section** | Construction tips, project updates, company news — great for SEO |
| **Client portal** | Allow clients to log in and track their specific project progress, view photos, download documents |
| **WhatsApp Business API integration** | Auto-send WhatsApp message to leads when they submit the contact form |
| **Project detail pages** | Currently projects are shown in a grid. Individual project pages (`/projects/[id]`) with full gallery, timeline, and description would improve UX |
| **Google Analytics / Vercel Analytics** | No analytics tracking currently. Add to measure traffic, lead conversion, and popular pages |
| **Image optimization** | Profile photos and project images are served as-is. Using Next.js `<Image>` component with proper `sizes` and `priority` attributes throughout would improve Core Web Vitals |
| **Contact form — SMS notification** | In addition to email, send an SMS to the team's phone when a new lead comes in (Twilio or MSG91) |
| **Testimonials — CMS-backed** | Testimonials are currently hardcoded in the component. Moving them to the database (admin-managed) would allow easy updates |
| **Services — CMS-backed** | Service content is hardcoded. Moving to DB would allow admin to update service descriptions |
| **404 page** | No custom 404 page exists. Add `not-found.tsx` for better user experience |
| **Loading states** | No loading skeletons on the projects page while data fetches from the API |
| **Error boundaries** | No React error boundaries — a component crash would break the whole page |

### 🔧 Technical Debt

| Item | Details |
|---|---|
| **`contacts.csv` in repo root** | A `contacts.csv` file exists in the repo root — likely an early lead capture artifact. Should be removed from the repo (add to `.gitignore`) as it may contain PII |
| **`build_log.txt` in repo root** | Build log file committed to the repo — should be in `.gitignore` |
| **Prisma migrations** | Only one migration exists (`20250811020030_init`). As the schema evolves, proper migration files should be created rather than using `prisma db push` |
| **`prisma/prisma/dev.db`** | SQLite dev database committed to the repo — should be in `.gitignore` |
| **Hardcoded Google Sheet ID** | The Sheet ID is hardcoded as a fallback in `contact/route.ts` — should only come from env vars |
| **`framer-motion` installed but minimal use** | Framer Motion is a large dependency. If only used for simple transitions, consider replacing with CSS animations |

---

## Summary

The Arivu Homes website has been built from scratch over ~90 commits into a **fully functional, production-deployed marketing and lead generation platform**. The core product is complete and live. The immediate priorities are:

1. ✅ Get GSTIN and update the header
2. ✅ Secure all admin environment variables in Vercel
3. 🔲 Add auth protection to team and upload APIs
4. 🔲 Add service subpages to the sitemap
5. 🔲 Clean up committed sensitive/generated files from the repo
6. 🔲 Add project detail pages for better UX
7. 🔲 Implement BOQ PDF export

---

*Document generated from codebase analysis of commit `6d05395` (initial) through `7f6268e` (latest).*
