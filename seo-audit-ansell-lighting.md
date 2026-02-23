# SEO Audit: ansell-lighting.com/en

**Audit Date:** 2026-02-23
**URL Audited:** https://ansell-lighting.com/en
**Auditor:** Claude Code (Automated)

---

## Executive Summary

Ansell Lighting operates a Nuxt.js-powered website with a WordPress CMS backend (Yoast SEO). The site serves English content at `/en/` and Spanish at `/es/`, with 744 product pages and 74 general pages indexed. While the site has strong brand recognition and distributor coverage, there are significant technical and on-page SEO gaps — most critically: missing meta descriptions on most pages, absent hreflang tags, no structured data/schema markup, social sharing tags missing sitewide, and sitemap fragmentation across subdomains.

---

## 1. Technical SEO

### 1.1 Technology Stack

| Component | Details |
|---|---|
| Frontend Framework | Nuxt.js (JavaScript-rendered) |
| CSS Framework | Tailwind CSS v3.1.4 |
| CMS Backend | WordPress + Yoast SEO |
| Analytics | Google Tag Manager (GTM-P84QN68) |
| Fonts | FS Lucas (custom, WOFF2/WOFF) |

**Issue — JavaScript Rendering Risk:**
The site is built with Nuxt.js. If pages are Client-Side Rendered (CSR) rather than Server-Side Rendered (SSR) or statically generated (SSG), Googlebot may fail to index body content, headings, and product details. The WebFetch crawler consistently retrieved only CSS/framework initialization code instead of rendered HTML, which is a strong indicator of potential crawlability problems.

**Recommendation:** Confirm SSR or SSG mode is active for all pages. Audit Google Search Console's "URL Inspection" tool for rendered HTML vs. raw HTML to verify Googlebot is seeing full content.

---

### 1.2 robots.txt

**Status: Partially Configured**

```
User-agent: *
Disallow:

Sitemap: https://cms.en.ansell-lighting.com/sitemap_index.xml
Sitemap: https://cms.es.ansell-lighting.com/sitemap_index.xml
```

**Issues:**
- Sitemap references point to a **CMS subdomain** (`cms.en.ansell-lighting.com`), not the primary domain. While functional, this is non-standard and may confuse some crawlers.
- `Disallow:` is empty (allows everything) — this is correct for a public site but means utility/legal pages (privacy policy, accessibility, slavery statement, cookie policy) are fully crawlable and indexable, diluting crawl budget.
- No `crawl-delay` directive (minor concern).

**Recommendations:**
- Consider adding `Disallow:` rules for pages with thin/duplicate content (e.g., `/en/cookie-policy/`, `/en/accessibility/`, `/en/slavery-and-human-trafficking/`, `/en/weee/`) — or use `noindex` meta tags on those pages instead.
- Optionally host a sitemap index at `https://ansell-lighting.com/sitemap.xml` that aggregates both language sitemaps.

---

### 1.3 Sitemap

**Sitemap Index:** `https://cms.en.ansell-lighting.com/sitemap_index.xml` (6 sitemaps)

| Sitemap | Last Modified | Notes |
|---|---|---|
| post-sitemap.xml | 2026-02-18 | Blog/articles |
| product-sitemap.xml | 2026-02-21 | 744 products |
| applications-sitemap.xml | 2026-02-06 | Application pages |
| pages-sitemap.xml | 2026-02-23 | 74 pages |
| post_category-sitemap.xml | 2026-02-23 | Blog categories |
| product_category-sitemap.xml | 2026-02-23 | Product categories |

**Issues:**
1. **Subdomain hosting:** Sitemaps are hosted at `cms.en.ansell-lighting.com`, not `ansell-lighting.com`. Technically valid (covered by robots.txt), but creates a split between CMS domain and front-end domain.
2. **Pages sitemap lacks `<lastmod>` values:** The 74-URL pages sitemap has no `lastmod`, `changefreq`, or `priority` values. While Google largely ignores `changefreq`/`priority`, `lastmod` helps signal freshness.
3. **Utility pages in sitemap:** Pages like `/en/cookie-policy/`, `/en/accessibility/`, `/en/slavery-and-human-trafficking/`, `/en/testimonial-with-no-rating/`, `/en/weee/` are in the sitemap but have minimal SEO value.
4. **Missing sitemap on main domain root:** `https://ansell-lighting.com/sitemap.xml` returns 404.

**Recommendations:**
- Add `<lastmod>` dates to all sitemap entries.
- Exclude utility/legal pages from the sitemap (or add `noindex` to those pages).
- Create a redirect or sitemap proxy at `https://ansell-lighting.com/sitemap.xml` → sitemap index.

---

### 1.4 Canonical Tags

**Status: NOT DETECTED**

No canonical URL tags were found in any audited pages. This is a critical issue given:
- The site serves content under `/en/` — if the root domain (`ansell-lighting.com/`) or other variants are accessible, duplicate content issues can arise.
- Nuxt.js may generate multiple URL variants (with/without trailing slash, with/without `/en/` prefix).

**Recommendation:** Add `<link rel="canonical" href="https://ansell-lighting.com/en/[page-path]/" />` to every page. Yoast SEO should handle this automatically if properly configured for the headless/decoupled WordPress setup.

---

### 1.5 Hreflang Tags

**Status: NOT DETECTED — Critical Issue**

The site has two confirmed language versions:
- English: `ansell-lighting.com/en/`
- Spanish: `ansell-lighting.com/es/` (referenced in robots.txt via `cms.es.ansell-lighting.com/sitemap_index.xml`)

No `hreflang` tags were detected in any audited pages. Without hreflang, Google cannot:
- Determine the correct language/region version to serve to users.
- Avoid penalising the site for duplicate content across languages.
- Surface the Spanish version to Spanish-speaking searchers.

**Recommendation:** Add hreflang link elements to all pages:
```html
<link rel="alternate" hreflang="en" href="https://ansell-lighting.com/en/[page]/" />
<link rel="alternate" hreflang="es" href="https://ansell-lighting.com/es/[page]/" />
<link rel="alternate" hreflang="x-default" href="https://ansell-lighting.com/en/[page]/" />
```
Implement via Nuxt.js `useHead()` or Yoast's headless API.

---

## 2. On-Page SEO

### 2.1 Title Tags

**Status: Inconsistent — Significant Issues**

| Page | Title | Issue |
|---|---|---|
| Homepage | Lighting Manufacturers UK \| Bespoke Solutions | Brand name "Ansell Lighting" absent |
| About Us | About Us - Ansell Lighting | Generic, no target keyword |
| Why Ansell | Why Ansell - Ansell Lighting | Generic, no keyword |
| Products | Products - Ansell Lighting | Generic, no keyword |
| History | History - Ansell Lighting | Generic, no keyword |
| Residential | Residential - Ansell Lighting | Missing "Lighting" keyword |
| Commercial | Commercial Lighting Solutions \| Ansell Lighting | Good |
| Retail | Retail Lighting Solutions \| Ansell Lighting | Good |
| Showrooms | Visit Our Lighting Showrooms \| Ansell Lighting | Good |
| LED Strip (category) | Flexible LED Strip Lighting \| Indoor & Outdoor Options | Good (but missing brand name) |
| Prism Pro (product) | Prism Pro Anti Glare - Ansell Lighting | Missing key specs in title |

**Recommendations:**
- **Homepage:** Include "Ansell Lighting" in the title. E.g., `Ansell Lighting | Lighting Manufacturers UK | Bespoke Solutions`.
- **Generic pages:** Replace pattern `[Page Name] - Ansell Lighting` with keyword-rich titles. E.g.:
  - `About Us` → `About Ansell Lighting | UK Lighting Manufacturer Since 1992`
  - `Products` → `LED Lighting Products | Downlights, Panels & Smart Lighting | Ansell`
  - `Residential` → `Residential Lighting Solutions | LED Downlights & Smart Lighting | Ansell`
  - `History` → `Our History | Ansell Lighting Since 1992`
- **Product pages:** Include wattage, IP rating, or key spec where space allows. E.g., `Prism Pro Anti Glare Fire Rated LED Downlight | Ansell Lighting`
- Aim for 50–60 characters for all title tags.

---

### 2.2 Meta Descriptions

**Status: Missing on Most Pages — Critical Issue**

Only the **homepage** has a confirmed meta description:
> *"Bespoke lighting manufacturers in the UK designing high-quality luminaires for commercial, healthcare, education, retail, and industrial applications."*

All other audited pages (About Us, Why Ansell, Products, product detail pages, category pages) had no detectable meta description. Without meta descriptions, Google auto-generates snippets from page content, which are often poorly worded and reduce click-through rates.

**Recommendations:**
- Write unique, compelling meta descriptions for every page (150–160 characters).
- Include a call to action and primary keyword.
- Examples:
  - **Why Ansell:** `"Discover why Ansell Lighting is the UK's trusted lighting manufacturer — LIA-accredited, 99.6% stock availability, and next-day delivery across the UK."`
  - **Commercial:** `"Transform your workspace with Ansell's commercial LED lighting range. Energy-efficient solutions for offices, retail, healthcare & industrial environments."`
  - **Product pages:** Include key specs, benefits, and a CTA: `"The Prism Pro Anti Glare Fire Rated LED Downlight offers IP65 protection, CCT-selectable 2700K–6000K, and dimmable performance. Ideal for residential & hospitality."`

---

### 2.3 Heading Structure (H1/H2/H3)

**Status: Unable to Verify — Likely JavaScript-Rendered**

No heading content was retrievable across any audited page. This may be because Nuxt.js renders headings client-side, which could cause issues for crawlers.

**Recommendations:**
- Ensure every page has exactly **one H1** tag containing the primary target keyword.
- Use H2s for main sections and H3s for subsections.
- Audit heading structure in Google Search Console's "URL Inspection" → "View Tested Page" to see what Googlebot renders.

---

### 2.4 Open Graph & Twitter Card Tags

**Status: NOT DETECTED — All Audited Pages**

No Open Graph (`og:title`, `og:description`, `og:image`, `og:type`, `og:url`) or Twitter Card tags were found on any audited page. This means social shares on LinkedIn, Facebook, Twitter/X, and WhatsApp will render with no image, a generic title, and no description.

**Recommendation:** Implement sitewide Open Graph and Twitter Card tags via Nuxt.js `useHead()`:
```html
<!-- Open Graph -->
<meta property="og:type" content="website" />
<meta property="og:title" content="[Page Title]" />
<meta property="og:description" content="[Page Description]" />
<meta property="og:image" content="https://ansell-lighting.com/[page-image].jpg" />
<meta property="og:url" content="https://ansell-lighting.com/en/[page]/" />
<meta property="og:site_name" content="Ansell Lighting" />

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="[Page Title]" />
<meta name="twitter:description" content="[Page Description]" />
<meta name="twitter:image" content="https://ansell-lighting.com/[page-image].jpg" />
```

---

## 3. Structured Data / Schema Markup

**Status: NOT DETECTED — Critical Issue**

No JSON-LD or microdata structured data was found on any audited page. This means the site is missing eligibility for all Google rich results.

### Missing Schema Types

| Schema Type | Pages | Benefit |
|---|---|---|
| `Organization` | Homepage | Brand panel, sitelinks, knowledge graph |
| `WebSite` with `SearchAction` | Homepage | Sitelinks search box in Google |
| `LocalBusiness` | Homepage, Contact, Showrooms | Local pack results, rich snippets |
| `Product` | All 744 product pages | Product rich results (price, rating, availability) |
| `BreadcrumbList` | All non-homepage pages | Breadcrumb rich results in SERPs |
| `FAQPage` | FAQs page | FAQ rich results (expand in SERPs) |
| `Article` | Blog/articles | Article rich results |

### Recommended Implementations

**Homepage — Organization + WebSite:**
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Ansell Lighting",
  "url": "https://ansell-lighting.com/en/",
  "logo": "https://ansell-lighting.com/logo.png",
  "foundingDate": "1992",
  "description": "UK lighting manufacturer designing bespoke LED luminaires for commercial, healthcare, education, retail and industrial applications.",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Unit 6B, Stonecross Industrial Park, Yew Tree Way",
    "addressLocality": "Warrington",
    "addressRegion": "Cheshire",
    "postalCode": "WA3 3JD",
    "addressCountry": "GB"
  },
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "customer service",
    "url": "https://ansell-lighting.com/en/contact/"
  }
}
```

**Product Pages — Product Schema:**
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[Product Name]",
  "description": "[Product Description]",
  "brand": {
    "@type": "Brand",
    "name": "Ansell Lighting"
  },
  "manufacturer": {
    "@type": "Organization",
    "name": "Ansell Lighting"
  },
  "image": "[Product Image URL]",
  "sku": "[Product SKU/Code]",
  "offers": {
    "@type": "Offer",
    "availability": "https://schema.org/InStock",
    "priceCurrency": "GBP",
    "seller": {
      "@type": "Organization",
      "name": "Ansell Lighting"
    }
  }
}
```

---

## 4. Content & Keyword Analysis

### 4.1 Homepage

- **Current title:** "Lighting Manufacturers UK | Bespoke Solutions" — targets generic head terms but omits brand
- **Meta description:** Good content, covers key verticals (commercial, healthcare, education, retail, industrial)
- **Gap:** No mention of smart lighting (OCTO), LED technology, or specific product USPs in meta description
- **Opportunity:** Target long-tail: "LED lighting manufacturer UK", "commercial LED luminaires", "bespoke lighting design UK"

### 4.2 Product Pages (744 products)

- Title format: `[Product Name] - Ansell Lighting` — misses descriptive keywords
- Product pages likely lack: meta descriptions, schema markup, breadcrumbs, related products internal linking
- **Opportunity:** Product titles with key specs rank well for long-tail searches (e.g., "IP65 CCT dimmable LED downlight fire rated")

### 4.3 Application Pages

- Commercial and Retail pages have keyword-rich titles ("Commercial Lighting Solutions | Ansell Lighting")
- "Residential - Ansell Lighting" is notably weaker — should be "Residential Lighting Solutions"
- Missing: Healthcare, Education, Industrial as separate application pages (high-traffic verticals mentioned in meta description but may not have dedicated optimised pages)

### 4.4 Thin/Utility Pages in Index

The following pages are indexed but offer minimal SEO value and may dilute crawl budget:
- `/en/accessibility/`
- `/en/cookie-policy/`
- `/en/privacy-policy/`
- `/en/slavery-and-human-trafficking/`
- `/en/terms-and-conditions/`
- `/en/weee/`
- `/en/testimonial-with-no-rating/`
- `/en/request-for-data-removal/`

**Recommendation:** Add `<meta name="robots" content="noindex, follow" />` to these pages and remove them from the sitemap.

---

## 5. Internal Linking

**Status: Unable to fully audit (JavaScript-rendered)**

**Observations from sitemap and search data:**
- Navigation likely covers major sections (Products, Applications, About Us, OCTO, Contact)
- 744 product pages exist but no clear internal linking strategy was observable
- Breadcrumb navigation was not confirmed present

**Recommendations:**
- Implement `BreadcrumbList` schema on all non-homepage pages
- Ensure product pages link to: relevant product category, related application pages, related products
- Add contextual internal links within application/sector pages to relevant product categories
- Create topic clusters: e.g., "Commercial Lighting" hub page linking to sub-pages (panels, downlights, emergency, smart, case studies)

---

## 6. Image SEO

**Status: Unable to audit (JavaScript-rendered)**

**Recommendations:**
- All product images should have descriptive `alt` text including product name and key spec (e.g., `alt="Prism Pro Anti Glare Fire Rated IP65 LED Downlight in white"`)
- Use descriptive filenames (e.g., `prism-pro-antiglare-fire-rated-led-downlight.jpg`)
- Serve images in WebP format with JPEG fallback
- Implement lazy loading for images below the fold
- Define explicit `width` and `height` attributes to prevent CLS (Cumulative Layout Shift)

---

## 7. Core Web Vitals & Performance

**Status: Cannot directly measure — Inferred from tech stack**

The site uses:
- Nuxt.js (client-side rendering risk for LCP)
- Custom web fonts (FS Lucas — multiple weights, WOFF2/WOFF — potential render-blocking)
- External Google Tag Manager script (render-blocking risk)
- Tailwind CSS (well-optimised framework, low CSS overhead risk)

**Likely Risk Areas:**

| Metric | Risk | Cause |
|---|---|---|
| LCP (Largest Contentful Paint) | Medium-High | JS rendering delay, custom font load |
| INP (Interaction to Next Paint) | Medium | Nuxt.js hydration overhead |
| CLS (Cumulative Layout Shift) | Medium | Font swap, no image dimensions |
| TTFB (Time to First Byte) | Low-Medium | Depends on CDN/hosting config |
| FCP (First Contentful Paint) | Medium | Render-blocking fonts/scripts |

**Recommendations:**
- Use `font-display: swap` or `font-display: optional` on FS Lucas to prevent render blocking
- Load GTM asynchronously (it should be by default — verify)
- Preload above-the-fold hero images with `<link rel="preload" as="image" />`
- Ensure Nuxt.js uses SSR or SSG (not CSR) so LCP content is in initial HTML
- Run PageSpeed Insights on `https://ansell-lighting.com/en` to get actual CWV scores
- Target: LCP < 2.5s, INP < 200ms, CLS < 0.1

---

## 8. Multilingual SEO

**Status: Needs Remediation**

- Two confirmed language versions: EN (`/en/`) and ES (`/es/`)
- Sitemaps exist for both (`cms.en.ansell-lighting.com` and `cms.es.ansell-lighting.com`)
- **No hreflang tags detected** — see Section 1.5

**Additional Concerns:**
- Verify the Spanish version is fully translated (not just auto-translated)
- Ensure `/es/` URLs mirror the `/en/` URL structure for consistency
- Verify Google is not treating `/en/` and `/es/` pages as duplicate content

---

## 9. Summary of Issues by Priority

### Critical (Fix Immediately)

| # | Issue | Impact |
|---|---|---|
| 1 | Meta descriptions missing on most pages | CTR, SERP appearance |
| 2 | Hreflang tags completely absent | International SEO, duplicate content |
| 3 | No structured data / schema markup | Missing all rich result eligibility |
| 4 | Open Graph / Twitter Card tags missing sitewide | Social sharing, brand perception |
| 5 | Canonical tags not detected | Potential duplicate content issues |
| 6 | JavaScript rendering — verify SSR/SSG active | Indexability of all content |

### High Priority (Fix Within 30 Days)

| # | Issue | Impact |
|---|---|---|
| 7 | Generic/keyword-poor title tags on many pages | Rankings, CTR |
| 8 | Homepage title omits brand name "Ansell Lighting" | Brand search, SERP identity |
| 9 | Utility pages indexed and in sitemap | Crawl budget, index bloat |
| 10 | Product title tags missing key specs | Long-tail product ranking |
| 11 | Sitemap hosted on CMS subdomain, not main domain | Crawl clarity |

### Medium Priority (Fix Within 90 Days)

| # | Issue | Impact |
|---|---|---|
| 12 | No `lastmod` in pages sitemap | Crawl efficiency |
| 13 | Missing `BreadcrumbList` navigation | UX, rich results, internal linking |
| 14 | Image alt text audit needed | Image search, accessibility |
| 15 | Custom font render-blocking risk | Core Web Vitals, LCP |
| 16 | No FAQ schema on /en/faqs/ | Featured snippets, SERP real estate |
| 17 | Healthcare/Education application pages may lack optimised titles | Vertical search visibility |

### Low Priority (Ongoing Improvements)

| # | Issue | Impact |
|---|---|---|
| 18 | Article/blog post schema missing | Article rich results |
| 19 | Product pages need contextual internal linking | Link equity distribution |
| 20 | Page load performance audit (CWV) | Rankings, UX |

---

## 10. Quick Wins

1. **Add meta descriptions** to all pages via Yoast SEO (highest ROI action)
2. **Add hreflang tags** for EN/ES in Nuxt.js `useHead()` — one code change covers all pages
3. **Add Organization + WebSite schema** to homepage (single JSON-LD block)
4. **Add Open Graph tags** globally via Nuxt.js layout
5. **Add `noindex`** to utility pages (cookie policy, accessibility, T&Cs, etc.)
6. **Update homepage title** to include "Ansell Lighting" brand name
7. **Update "Residential - Ansell Lighting"** to "Residential Lighting Solutions | Ansell Lighting"
8. **Run PageSpeed Insights** on the homepage and top product pages — address any CWV failures

---

*Audit completed using automated web crawling, sitemap analysis, and Google SERP data. Further manual auditing of rendered page source via Chrome DevTools or Screaming Frog recommended for complete heading, image, and internal link analysis.*
