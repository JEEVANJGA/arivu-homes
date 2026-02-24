# Arivu Homes — SEO Implementation Plan

> **Live URL:** https://arivu-homes.vercel.app/  
> **Generated:** February 2026  
> **Purpose:** Client review document — lists every proposed change before implementation

---

## Table of Contents

1. [Summary of Changes](#1-summary-of-changes)
2. [sitemap.ts — Fix URLs & Add Missing Pages](#2-sitemapTs--fix-urls--add-missing-pages)
3. [robots.ts — Fix Sitemap URL](#3-robotsts--fix-sitemap-url)
4. [layout.tsx — Fix URLs, og:image & JSON-LD](#4-layouttsx--fix-urls-ogimage--json-ld)
5. [Homepage — Add Metadata](#5-homepage--add-metadata)
6. [Services Page — Add Metadata](#6-services-page--add-metadata)
7. [Service Slug Pages — generateMetadata & JSON-LD](#7-service-slug-pages--generatemetadata--json-ld)
8. [Projects Page — Add Metadata Layout](#8-projects-page--add-metadata-layout)
9. [Team Page — Add Metadata](#9-team-page--add-metadata)
10. [Team Profile Pages — Metadata & Person JSON-LD](#10-team-profile-pages--metadata--person-json-ld)
11. [Journey Page — Add Metadata](#11-journey-page--add-metadata)
12. [BOQ Page — Add Metadata](#12-boq-page--add-metadata)
13. [Outside Codebase Actions](#13-outside-codebase-actions-client-to-do)

---

## 1. Summary of Changes

| # | File | Change Type | SEO Impact |
|---|---|---|---|
| 1 | `sitemap.ts` | Add 11 missing URLs | 🔴 Critical |
| 2 | `robots.ts` | Fix sitemap URL | 🔴 Critical |
| 3 | `layout.tsx` | Fix all URLs + enhance JSON-LD | 🔴 Critical |
| 4 | `page.tsx` (home) | Add canonical + enhance metadata | 🟡 Important |
| 5 | `services/page.tsx` | Add page-level metadata | 🟡 Important |
| 6 | `services/[slug]/page.tsx` | Add `generateMetadata` + `generateStaticParams` + Service JSON-LD | 🔴 Critical |
| 7 | `projects/page.tsx` | Add server-side metadata (page is client-side, note limitations) | 🟡 Important |
| 8 | `team/page.tsx` | Add page-level metadata | 🟡 Important |
| 9 | `team/rohith-gopal/page.tsx` | Add metadata + Person JSON-LD | 🟡 Important |
| 10 | `team/chethan-kumar-s/page.tsx` | Add metadata + Person JSON-LD | 🟡 Important |
| 11 | `team/shashank-d/page.tsx` | Add metadata + Person JSON-LD | 🟡 Important |
| 12 | `journey/page.tsx` | Add page-level metadata | 🟡 Important |
| 13 | `boq/page.tsx` | Add page-level metadata | 🟡 Important |

**Total files changed: 13**  
**New pages added to sitemap: 11** (7 service subpages + 3 team profiles + journey)

---

## 2. sitemap.ts — Fix URLs & Add Missing Pages

**File:** `src/app/sitemap.ts`

### Current State
Only 5 URLs are in the sitemap. Missing: `/journey`, all 7 service subpages, all 3 team profile pages.

### Proposed Change

```typescript
// BEFORE (current)
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
    const baseUrl = 'https://arivuhomes.com'  // ← WRONG URL

    return [
        { url: baseUrl, ... },
        { url: `${baseUrl}/services`, ... },
        { url: `${baseUrl}/projects`, ... },
        { url: `${baseUrl}/team`, ... },
        { url: `${baseUrl}/boq`, ... },
        // MISSING: /journey, /services/[slug] x7, /team/[slug] x3
    ]
}
```

```typescript
// AFTER (proposed)
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
    const baseUrl = 'https://arivu-homes.vercel.app'  // ← CORRECT URL

    return [
        { url: baseUrl, lastModified: new Date(), changeFrequency: 'monthly', priority: 1 },
        { url: `${baseUrl}/services`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.9 },
        { url: `${baseUrl}/services/residential-construction`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/services/commercial-construction`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/services/farm-house-construction`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/services/architectural-design`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/services/structural-engineering`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/services/renovation-remodeling`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/services/project-management`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.7 },
        { url: `${baseUrl}/projects`, lastModified: new Date(), changeFrequency: 'weekly', priority: 0.9 },
        { url: `${baseUrl}/journey`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
        { url: `${baseUrl}/team`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.7 },
        { url: `${baseUrl}/team/rohith-gopal`, lastModified: new Date(), changeFrequency: 'yearly', priority: 0.6 },
        { url: `${baseUrl}/team/chethan-kumar-s`, lastModified: new Date(), changeFrequency: 'yearly', priority: 0.6 },
        { url: `${baseUrl}/team/shashank-d`, lastModified: new Date(), changeFrequency: 'yearly', priority: 0.6 },
        { url: `${baseUrl}/boq`, lastModified: new Date(), changeFrequency: 'yearly', priority: 0.5 },
    ]
}
```

**Impact:** Google will now discover and index all 16 pages instead of just 5.

---

## 3. robots.ts — Fix Sitemap URL

**File:** `src/app/robots.ts`

### Current State
```typescript
sitemap: 'https://arivuhomes.com/sitemap.xml',  // ← WRONG URL
```

### Proposed Change
```typescript
sitemap: 'https://arivu-homes.vercel.app/sitemap.xml',  // ← CORRECT URL
```

**Impact:** Google's crawler will find the correct sitemap URL.

---

## 4. layout.tsx — Fix URLs, og:image & JSON-LD

**File:** `src/app/layout.tsx`

### Current State Issues
- `openGraph.url` points to `https://arivuhomes.com` (wrong)
- JSON-LD `url` and `image` point to `https://arivuhomes.com` (wrong)
- No `og:image` defined (no preview when sharing on WhatsApp/social)
- JSON-LD missing `description`, `@id`, `priceRange`
- `sameAs` only has Instagram and WhatsApp — missing LinkedIn

### Proposed Changes

**A. Fix Open Graph URL + Add og:image:**
```typescript
// BEFORE
openGraph: {
    title: "Arivu Homes Private Limited",
    description: "End-to-end construction and architectural design services in Bangalore.",
    url: "https://arivuhomes.com",   // ← WRONG
    siteName: "Arivu Homes Private Limited",
    locale: "en_IN",
    type: "website",
},

// AFTER
openGraph: {
    title: "Arivu Homes Private Limited",
    description: "End-to-end construction and architectural design services in Bangalore.",
    url: "https://arivu-homes.vercel.app",   // ← CORRECT
    siteName: "Arivu Homes Private Limited",
    locale: "en_IN",
    type: "website",
    images: [
        {
            url: "https://arivu-homes.vercel.app/logo-home.jpg",
            width: 1200,
            height: 630,
            alt: "Arivu Homes Private Limited - Construction Company in Bangalore",
        }
    ],
},
```

**B. Fix JSON-LD + Add missing fields:**
```typescript
// BEFORE
const jsonLd = {
    "@context": "https://schema.org",
    "@type": "LocalBusiness",
    "name": "Arivu Homes Private Limited",
    "image": "https://arivuhomes.com/logo-home.jpg",   // ← WRONG
    "url": "https://arivuhomes.com",                    // ← WRONG
    ...
    "sameAs": [
        "https://www.instagram.com/arivuhomes/",
        "https://wa.me/916361867464"
    ],
}

// AFTER
const jsonLd = {
    "@context": "https://schema.org",
    "@type": "LocalBusiness",
    "@id": "https://arivu-homes.vercel.app/#business",
    "name": "Arivu Homes Private Limited",
    "description": "End-to-end construction, architectural design, structural engineering, and project management services in Bangalore.",
    "image": "https://arivu-homes.vercel.app/logo-home.jpg",   // ← CORRECT
    "url": "https://arivu-homes.vercel.app",                    // ← CORRECT
    "telephone": "+916361867464",
    "email": "contact.arivuhomes@gmail.com",
    "priceRange": "₹₹₹",
    "address": {
        "@type": "PostalAddress",
        "addressLocality": "Bangalore",
        "addressRegion": "Karnataka",
        "addressCountry": "IN"
    },
    "sameAs": [
        "https://www.instagram.com/arivuhomes/",
        "https://wa.me/916361867464"
        // Add LinkedIn URL here when company page is created
    ],
    "openingHoursSpecification": { ... }  // unchanged
}
```

**Impact:** Correct URLs for Google indexing; preview image when sharing on WhatsApp/LinkedIn; richer Google Knowledge Panel eligibility.

---

## 5. Homepage — Add Metadata

**File:** `src/app/page.tsx`

### Current State
No `metadata` export — inherits generic metadata from `layout.tsx`.

### Proposed Addition
```typescript
// ADD at top of file (before the component)
import type { Metadata } from "next";

export const metadata: Metadata = {
    title: "Arivu Homes Private Limited | Construction Company in Bangalore",
    description: "Arivu Homes — Building Dreams with Precision. End-to-end residential & commercial construction, architectural design, structural engineering in Bangalore. Contact us today.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app",
    },
};
```

**Impact:** Homepage gets a unique, keyword-rich title that appears in Google search results.

---

## 6. Services Page — Add Metadata

**File:** `src/app/services/page.tsx`

### Current State
No `metadata` export.

### Proposed Addition
```typescript
export const metadata: Metadata = {
    title: "Construction Services in Bangalore | Arivu Homes",
    description: "Explore Arivu Homes' full range of services: residential construction, commercial construction, farm house construction, architectural design, structural engineering, and renovation in Bangalore.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/services",
    },
};
```

---

## 7. Service Slug Pages — generateMetadata & JSON-LD

**File:** `src/app/services/[slug]/page.tsx`

### Current State
- No `metadata` export — all 7 service pages show the same generic title
- No `generateStaticParams` — pages are not pre-rendered at build time
- No structured data for individual services

### Proposed Changes

**A. Add `generateStaticParams` (pre-renders all service pages at build time):**
```typescript
export function generateStaticParams() {
    return Object.keys(serviceContent).map((slug) => ({ slug }));
}
```

**B. Add `generateMetadata` (unique title/description per service):**
```typescript
const serviceMetadata: Record<string, { title: string; description: string }> = {
    "residential-construction": {
        title: "Residential Construction in Bangalore | Arivu Homes",
        description: "Turnkey villas and apartments in Bangalore. RCC M25+ structure, branded materials, weekly progress reports. Arivu Homes Private Limited.",
    },
    "commercial-construction": {
        title: "Commercial Construction in Bangalore | Arivu Homes",
        description: "Office, retail, and industrial construction in Bangalore. Pre-construction planning, MEP coordination, fire-safety compliance. Arivu Homes.",
    },
    "farm-house-construction": {
        title: "Farm House Construction in Bangalore | Arivu Homes",
        description: "Eco-friendly farm house and weekend home construction near Bangalore. Sustainable design, quality materials. Arivu Homes Private Limited.",
    },
    "architectural-design": {
        title: "Architectural Design Services in Bangalore | Arivu Homes",
        description: "Concept to GFC drawings, 3D views, Vastu-aligned planning, authority submission support. Arivu Homes architectural design in Bangalore.",
    },
    "structural-engineering": {
        title: "Structural Engineering Services in Bangalore | Arivu Homes",
        description: "ETABS/STAAD analysis, seismic design to IS 1893, peer review, value engineering. Arivu Homes structural engineering in Bangalore.",
    },
    "renovation-remodeling": {
        title: "Renovation & Remodeling in Bangalore | Arivu Homes",
        description: "Structural retrofits, interior remodeling, phased execution with dust control. Arivu Homes renovation services in Bangalore.",
    },
    "project-management": {
        title: "Construction Project Management in Bangalore | Arivu Homes",
        description: "Transparent cost control, scheduling, QA/QC, vendor management. Arivu Homes project management services in Bangalore.",
    },
};

export async function generateMetadata({ params }: { params: Promise<{ slug: string }> }): Promise<Metadata> {
    const { slug } = await params;
    const meta = serviceMetadata[slug];
    if (!meta) return {};
    return {
        title: meta.title,
        description: meta.description,
        alternates: {
            canonical: `https://arivu-homes.vercel.app/services/${slug}`,
        },
    };
}
```

**C. Add Service JSON-LD structured data to each service page:**
```typescript
// Inside the component, add before </main>:
<script
    type="application/ld+json"
    dangerouslySetInnerHTML={{
        __html: JSON.stringify({
            "@context": "https://schema.org",
            "@type": "Service",
            "name": data.title,
            "description": data.intro,
            "provider": {
                "@type": "LocalBusiness",
                "name": "Arivu Homes Private Limited",
                "url": "https://arivu-homes.vercel.app"
            },
            "areaServed": {
                "@type": "City",
                "name": "Bangalore"
            }
        })
    }}
/>
```

**Impact:** Each service page gets a unique Google listing with relevant keywords. "Residential Construction in Bangalore" can rank independently from "Structural Engineering in Bangalore".

---

## 8. Projects Page — Add Metadata Layout

**Files:** `src/app/projects/layout.tsx` *(new file)* + `src/app/projects/page.tsx`

### Current State
The projects page is `"use client"` with `useEffect` data fetching. Google's crawler sees an empty page shell — no project names, locations, or descriptions are visible to search engines.

### Proposed Change
Add a `metadata` export at the top of the file. Note: because this is a client component, we cannot add server-side metadata directly. The fix requires either:

**Option A (Simple — just metadata):** Add a separate `layout.tsx` inside `/app/projects/` that exports metadata. This is the least invasive change.

```typescript
// NEW FILE: src/app/projects/layout.tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
    title: "Construction Projects in Bangalore | Arivu Homes",
    description: "View Arivu Homes' ongoing and completed construction projects in Bangalore — residential villas, commercial buildings, and more.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/projects",
    },
};

export default function ProjectsLayout({ children }: { children: React.ReactNode }) {
    return <>{children}</>;
}
```

**Option B (Full fix — better for SEO):** Convert the page to a hybrid: server component for initial render + client component for admin interactions. This is a larger refactor and can be done in a separate phase.

**Recommendation:** Implement Option A now (quick win), plan Option B for a future sprint.

---

## 9. Team Page — Add Metadata

**File:** `src/app/team/page.tsx`

### Current State
No `metadata` export.

### Proposed Addition
```typescript
export const metadata: Metadata = {
    title: "Our Team | Arivu Homes Private Limited",
    description: "Meet the experienced team behind Arivu Homes — Rohith Gopal (Managing Partner, 16+ years), Chethan Kumar S (Managing Partner, 20+ years), and Shashank D (Senior Architect, 7+ years).",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/team",
    },
};
```

---

## 10. Team Profile Pages — Metadata & Person JSON-LD

**Files:** `src/app/team/rohith-gopal/page.tsx`, `src/app/team/chethan-kumar-s/page.tsx`, `src/app/team/shashank-d/page.tsx`

### Current State
All three team profile pages have no `metadata` export and no structured data.

### Proposed Changes for Each Profile

**Rohith Gopal (`/team/rohith-gopal/page.tsx`):**
```typescript
export const metadata: Metadata = {
    title: "Rohith Gopal — Managing Partner | Arivu Homes",
    description: "Rohith Gopal, Managing Partner at Arivu Homes Private Limited. 16+ years of experience in project management, business development, and strategic planning in Bangalore construction.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/team/rohith-gopal",
    },
};

// Person JSON-LD (add inside component before </main>):
const personJsonLd = {
    "@context": "https://schema.org",
    "@type": "Person",
    "name": "Rohith Gopal",
    "jobTitle": "Managing Partner",
    "worksFor": {
        "@type": "Organization",
        "name": "Arivu Homes Private Limited",
        "url": "https://arivu-homes.vercel.app"
    },
    "image": "https://arivu-homes.vercel.app/profile/rohith.jpg",
    "url": "https://arivu-homes.vercel.app/team/rohith-gopal",
    "address": {
        "@type": "PostalAddress",
        "addressLocality": "Bangalore",
        "addressRegion": "Karnataka",
        "addressCountry": "IN"
    }
};
```

**Chethan Kumar S (`/team/chethan-kumar-s/page.tsx`):**
```typescript
export const metadata: Metadata = {
    title: "Chethan Kumar S — Managing Partner | Arivu Homes",
    description: "Chethan Kumar S, Managing Partner at Arivu Homes Private Limited. 20+ years of experience in construction management, quality control, and technical leadership in Bangalore.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/team/chethan-kumar-s",
    },
};
```

**Shashank D (`/team/shashank-d/page.tsx`):**
```typescript
export const metadata: Metadata = {
    title: "Shashank D — Senior Architect | Arivu Homes",
    description: "Shashank D, Senior Architect & Civil Engineer at Arivu Homes Private Limited. 7+ years of experience in architectural design, structural engineering, and sustainable design in Bangalore.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/team/shashank-d",
    },
};
```

---

## 11. Journey Page — Add Metadata

**File:** `src/app/journey/page.tsx`

### Current State
No `metadata` export. This page is completely missing from the sitemap.

### Proposed Addition
```typescript
export const metadata: Metadata = {
    title: "Your Construction Journey | Arivu Homes Bangalore",
    description: "Understand the 5-step construction process with Arivu Homes — from consultation & design to final handover. Clear payment milestones and timelines for your dream home in Bangalore.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/journey",
    },
};
```

---

## 12. BOQ Page — Add Metadata

**File:** `src/app/boq/page.tsx`

### Current State
No `metadata` export.

### Proposed Addition
```typescript
export const metadata: Metadata = {
    title: "BOQ Calculator — Construction Cost Estimator | Arivu Homes",
    description: "Free Bill of Quantities (BOQ) calculator for construction projects in Bangalore. Estimate costs for earthwork, concrete, masonry, finishing, electrical, and plumbing. Arivu Homes.",
    alternates: {
        canonical: "https://arivu-homes.vercel.app/boq",
    },
};
```

---

## 13. Outside Codebase Actions (Client To-Do)

These actions **cannot be done in code** — they require the client to take action:

### A. Google Search Console *(Do this first — free, ~10 minutes)*

| Step | Action |
|---|---|
| 1 | Go to https://search.google.com/search-console |
| 2 | Click "Add Property" → choose "URL prefix" → enter `https://arivu-homes.vercel.app` |
| 3 | Choose "HTML tag" verification method → copy the `<meta name="google-site-verification" content="...">` tag |
| 4 | Share the verification code with the developer — it will be added to `layout.tsx` |
| 5 | After verification, go to Sitemaps → enter `sitemap.xml` → click Submit |
| 6 | Go to URL Inspection → enter `https://arivu-homes.vercel.app` → click "Request Indexing" |

**Why:** Without this, Google has no way to know the site exists. This is the single most important step.

### B. Google Business Profile *(Highest impact for local Bangalore searches)*

| Step | Action |
|---|---|
| 1 | Go to https://business.google.com |
| 2 | Search for "Arivu Homes Private Limited" — claim if it exists, or create new |
| 3 | Business category: "Construction Company" (primary) + "Architect" (secondary) |
| 4 | Add: Bangalore address, +91-6361867464, website URL, Mon–Sat 9am–6pm hours |
| 5 | Upload 5–10 project photos |
| 6 | Ask 3–5 past clients to leave Google Reviews |

**Why:** This is what makes Arivu Homes appear in Google Maps and "construction company near me" searches in Bangalore. This is the highest-impact action for local business SEO.

### C. Custom Domain (When Ready)

| Step | Action |
|---|---|
| 1 | Purchase `arivuhomes.com` or `arivuhomes.in` from GoDaddy/Namecheap/Google Domains |
| 2 | In Vercel dashboard → Settings → Domains → Add domain |
| 3 | Update DNS records as instructed by Vercel |
| 4 | Inform developer to update all URLs in codebase from `arivu-homes.vercel.app` to new domain |

**Why:** A custom domain looks more professional and builds brand trust. `arivuhomes.com` is also easier to remember and share.

### D. Bing Webmaster Tools

| Step | Action |
|---|---|
| 1 | Go to https://bing.com/webmasters |
| 2 | Add site: `https://arivu-homes.vercel.app` |
| 3 | Submit sitemap: `https://arivu-homes.vercel.app/sitemap.xml` |

**Why:** Covers Bing, DuckDuckGo, and Yahoo search engines.

### E. Local Business Directory Listings *(Backlinks = trust signals)*

| Directory | URL | Priority |
|---|---|---|
| Justdial | justdial.com | 🔴 High |
| Sulekha | sulekha.com | 🔴 High |
| IndiaMART | indiamart.com | 🟡 Medium |
| Houzz | houzz.com | 🟡 Medium (high authority) |
| 99acres | 99acres.com | 🟡 Medium |

For each: create a free business listing with name, address, phone, website URL, and category.

### F. Social Media Profiles

| Platform | Action |
|---|---|
| Instagram `@arivuhomes` | Ensure bio links to `https://arivu-homes.vercel.app` |
| LinkedIn | Create Company Page for "Arivu Homes Private Limited" — link to website |
| Facebook | Create Business Page — link to website |

---

## Approval Checklist

Please review and confirm which changes to implement:

### Codebase Changes
- [ ] Fix URLs in `sitemap.ts`, `robots.ts`, `layout.tsx`
- [ ] Add per-page metadata to all 12 pages
- [ ] Add `generateStaticParams` + `generateMetadata` to service slug pages
- [ ] Add Service JSON-LD to service subpages
- [ ] Add Person JSON-LD to team profile pages
- [ ] Create `projects/layout.tsx` for projects page metadata
- [ ] Add `og:image` to layout Open Graph metadata

### Outside Actions (Client)
- [ ] Set up Google Search Console
- [ ] Create/claim Google Business Profile
- [ ] Purchase custom domain (when ready)
- [ ] Submit to Bing Webmaster Tools
- [ ] Create directory listings (Justdial, Sulekha, etc.)
- [ ] Complete social media profiles with website link

---

*Once approved, all codebase changes can be implemented in a single Act mode session.*
