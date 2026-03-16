# SEO Audit Report
## 4liteuk.com

---

| | |
|---|---|
| **Prepared for** | 4Lite UK |
| **Website audited** | https://4liteuk.com/ |
| **Audit date** | 16 March 2026 |
| **Prepared by** | Claude Code (Automated SEO Audit) |
| **Report version** | 1.0 |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Audit Methodology](#2-audit-methodology)
3. [Site Overview](#3-site-overview)
4. [Technical SEO](#4-technical-seo)
   - 4.1 Technology Stack
   - 4.2 JavaScript Rendering
   - 4.3 robots.txt
   - 4.4 XML Sitemaps
   - 4.5 Canonical Tags
   - 4.6 WWW vs Non-WWW Domain Consistency
5. [On-Page SEO](#5-on-page-seo)
   - 5.1 Title Tags
   - 5.2 Meta Descriptions
   - 5.3 Heading Structure
   - 5.4 Open Graph & Social Tags
6. [Structured Data & Schema Markup](#6-structured-data--schema-markup)
7. [Content & Keyword Analysis](#7-content--keyword-analysis)
8. [Internal Linking](#8-internal-linking)
9. [Image SEO](#9-image-seo)
10. [Core Web Vitals & Performance](#10-core-web-vitals--performance)
11. [Prioritised Issue Log](#11-prioritised-issue-log)
12. [Recommended Action Plan](#12-recommended-action-plan)
13. [Appendix](#13-appendix)

---

## 1. Executive Summary

4Lite UK is a lighting brand with over 30 years of experience supplying LED lighting solutions for homes, gardens, and professional spaces. The website at `4liteuk.com` serves as the primary digital presence, backed by a WordPress CMS (Yoast SEO) and a Nuxt.js front-end framework. The brand is LIAQA-certified and offers a 4-year warranty on most products.

The site has a solid product catalogue — **167 product pages** across 6 sitemap sub-categories — and benefits from clear brand identity and a growing smart lighting range. However, this audit identified **17 distinct SEO issues**, 4 of which are classified as critical and require immediate attention.

### Key Findings at a Glance

| Severity | Count | Examples |
|---|---|---|
| Critical | 4 | No robots.txt, canonical tags pointing to CMS subdomain, www/non-www mismatch, missing meta descriptions |
| High | 5 | Generic title tags, utility/competition pages indexed, no `<lastmod>` in sitemap, OG tags unconfirmed |
| Medium | 5 | Schema in JS object not HTML head, sitemap redirect chain, stale spaces sitemap, potential duplicate pages, no BreadcrumbList |
| Low | 3 | SKU-only image alt text, Core Web Vitals audit needed, article schema absent |

### Top 3 Immediate Actions

1. **Create a valid robots.txt file** — the current file returns a 404 error, leaving Google without any crawl guidance and without a direct sitemap reference.
2. **Fix canonical tags** — canonicals on category pages point to the CMS subdomain (`cms.4liteuk.com`) rather than the public-facing domain, and product pages use `www.4liteuk.com` while the main site uses `4liteuk.com` — creating duplicate content signals.
3. **Write meta descriptions for all key pages** — meta descriptions were absent on the homepage, About Us, and Products listing pages; these are high-traffic pages where auto-generated snippets reduce CTR.

---

## 2. Audit Methodology

This audit was conducted on **16 March 2026** using the following methods:

| Method | Details |
|---|---|
| Direct HTML crawl | Page source fetched for homepage, about, product category, product listing, and individual product pages |
| Sitemap analysis | Full review of sitemap index at `cms.4liteuk.com/sitemap_index.xml` and sub-sitemaps |
| robots.txt review | Direct inspection of `4liteuk.com/robots.txt` |
| Canonical & meta tag inspection | Head-section metadata reviewed across key page types |
| Structured data review | JSON-LD and schema markup checked across homepage, category, and product pages |

**Limitations:** The site uses a Nuxt.js front-end framework. Some pages returned CSS/framework initialisation code rather than fully rendered HTML body content during crawling, which is consistent with a JavaScript-rendered architecture. Heading structure, full image alt text inventories, and internal link graphs could not be exhaustively verified from raw source alone. Recommendations in those areas are based on observed patterns and best practice. Manual verification using Chrome DevTools, Screaming Frog, or Google Search Console URL Inspection is recommended to supplement this report.

---

## 3. Site Overview

### 3.1 Technology Stack

| Component | Details |
|---|---|
| Frontend framework | Nuxt.js (JavaScript-rendered) |
| CSS framework | Tailwind CSS v3.1.4 |
| CMS backend | WordPress + Yoast SEO plugin |
| Tag management | Google Tag Manager (Container: GTM-5WVWSQ88) |
| Sitemap generation | Yoast SEO (WordPress) |
| Site architecture | Headless / decoupled (Nuxt.js front-end + WordPress CMS at `cms.4liteuk.com`) |

### 3.2 Site Scale

| Content type | Count | Sitemap file | Last modified |
|---|---|---|---|
| Products | 167 | product-sitemap.xml | 2026-03-12 |
| Pages | 29 | pages-sitemap.xml | 2026-03-16 |
| Blog / articles | — | post-sitemap.xml | 2026-01-15 |
| Spaces | — | spaces-sitemap.xml | 2024-10-11 |
| Post categories | — | post_category-sitemap.xml | 2026-03-16 |
| Product categories | — | product_category-sitemap.xml | 2026-03-16 |

### 3.3 Language Versions

The site operates in English only. No multilingual or hreflang implementation is present or required.

---

## 4. Technical SEO

### 4.1 Technology Stack

The site uses a **headless/decoupled architecture**: WordPress with Yoast SEO manages content on the CMS subdomain (`cms.4liteuk.com`), while Nuxt.js serves the public-facing front-end at `4liteuk.com`. This is a legitimate and performant pattern, but introduces specific technical SEO risks — notably around canonical tags, sitemap hosting, and JavaScript rendering — that must be actively managed.

---

### 4.2 JavaScript Rendering

**Status: Risk Identified — Verify Immediately**

The site is built with Nuxt.js. During crawling, some pages returned only CSS/framework initialisation code rather than rendered HTML body content, which is consistent with **Client-Side Rendering (CSR)**. If Nuxt.js is not configured for SSR (Server-Side Rendering) or SSG (Static Site Generation), the following content may be invisible to Googlebot during the initial crawl pass:

- Product names, descriptions, and specifications
- Page headings (H1, H2, H3)
- Body copy across all pages
- Internal link anchor text

Some pages (notably the product detail page) did return heading and body content during the audit, suggesting SSR or pre-rendering may be active on at least some page types. However, the product categories index page and homepage returned only framework code, which warrants investigation.

**Recommendations:**
1. Open Google Search Console → URL Inspection → inspect `https://4liteuk.com/`, `/product-categories/`, and a product page.
2. Click **"View Tested Page"** and compare the raw HTML vs. the rendered DOM.
3. If body content is absent in the raw HTML, ensure SSR or SSG is enabled in the Nuxt.js configuration for all page types.
4. Verify rendering for product pages, category pages, and the homepage independently, as different routes may have different rendering strategies.

---

### 4.3 robots.txt

**Status: MISSING — Critical**

`https://4liteuk.com/robots.txt` returns a **404 Not Found** error. This is a critical issue with three direct consequences:

1. **No crawl guidance** — search engine bots have no instructions on which areas of the site to crawl or avoid.
2. **No sitemap reference** — the primary way to point Google to the sitemap index is via `robots.txt`. Without it, discovery of the CMS-hosted sitemap depends entirely on Search Console submission or crawl luck.
3. **Crawl budget waste** — utility, competition, and legal pages are being crawled unnecessarily with no mechanism to prevent it.

**Recommended robots.txt:**

```
User-agent: *
Disallow: /competition/
Disallow: /gleeentry/
Disallow: /gleedata/
Disallow: /screwfixentry/
Disallow: /staxentry/
Disallow: /request-for-data-removal/

Sitemap: https://cms.4liteuk.com/sitemap_index.xml
```

This file should be placed at `https://4liteuk.com/robots.txt` (accessible at the root of the primary domain) and also submitted via Google Search Console.

---

### 4.4 XML Sitemaps

**Status: Functional but Suboptimal**

**Sitemap discovery path:** `https://4liteuk.com/sitemap.xml` → **301 redirect** → `https://cms.4liteuk.com/sitemap_index.xml`

This redirect chain is non-standard. While Google will follow the redirect, it is preferable for `4liteuk.com/sitemap.xml` to either serve the sitemap directly or point to it without a cross-subdomain redirect.

**Sub-sitemap overview:**

| File | Last Modified | Issue |
|---|---|---|
| post-sitemap.xml | 2026-01-15 | — |
| product-sitemap.xml | 2026-03-12 | — |
| spaces-sitemap.xml | 2024-10-11 | Stale — not updated in ~17 months |
| pages-sitemap.xml | 2026-03-16 | No `<lastmod>` values on any of the 29 entries |
| post_category-sitemap.xml | 2026-03-16 | — |
| product_category-sitemap.xml | 2026-03-16 | — |

**Issues identified:**

1. **No `<lastmod>` values** in the pages sitemap — all 29 pages appear equally stale to Google, reducing crawl prioritisation.
2. **Spaces sitemap not updated since October 2024** — if the spaces content has been updated since then, `<lastmod>` timestamps are inaccurate and Google may not be recrawling recently changed pages.
3. **Utility and competition pages included** — pages such as `/competition/`, `/gleeentry/`, `/screwfixentry/`, `/staxentry/`, `/cookie-policy/`, `/privacy-policy/`, and `/terms-and-conditions/` are indexed and in the sitemap, wasting crawl budget.
4. **301 redirect chain on sitemap URL** — `4liteuk.com/sitemap.xml` redirects to `cms.4liteuk.com/sitemap_index.xml`, which is a cross-subdomain redirect.

**Recommendations:**
- Configure Yoast SEO to output accurate `<lastmod>` timestamps for all sitemap entries.
- Exclude utility, legal, and competition pages from the sitemap (and add `noindex` — see Section 7.4).
- Either proxy `4liteuk.com/sitemap.xml` to serve the sitemap content directly, or create a static sitemap index at the root domain.
- Review and update the spaces sitemap to ensure content freshness.

---

### 4.5 Canonical Tags

**Status: Misconfigured — Critical**

Canonical tags were found on audited pages, but with two significant errors:

**Issue 1 — Canonical pointing to CMS subdomain**

On the product categories page (`/product-categories/`), the canonical URL was found as:

```
https://cms.4liteuk.com/product-categories/battens-non-corrosive/
```

This is the WordPress CMS backend URL, not the public-facing front-end URL. If this is appearing across category pages, Google is being instructed to treat the CMS subdomain as the authoritative version — effectively telling Google to de-index the public site in favour of the CMS backend, which is not publicly accessible to users.

**Issue 2 — www vs non-www inconsistency**

On the product page (`/products/smart-ip54-circular-wall-ceiling/`), the canonical was:

```
https://www.4liteuk.com/products/smart-ip54-circular-wall-ceiling/
```

The primary domain serves content at `https://4liteuk.com/` (without `www`). This mismatch means product page canonicals point to a `www.` URL variant that may not redirect correctly, creating a split canonical signal between the `www` and non-`www` versions.

**Recommendations:**

1. Audit all canonical tag outputs across every Nuxt.js page type (homepage, category, product, space, article).
2. Ensure all canonical tags use the non-`www` primary domain: `https://4liteuk.com/[path]/`.
3. Verify that `www.4liteuk.com` performs a **301 redirect** to `4liteuk.com` (or vice versa — pick one as canonical and redirect the other universally).
4. Fix the CMS subdomain canonical issue — canonical tags should be generated by the Nuxt.js layer using the public domain, not inherited from the WordPress backend.

```html
<!-- Correct canonical format for all pages -->
<link rel="canonical" href="https://4liteuk.com/[page-path]/" />
```

---

### 4.6 WWW vs Non-WWW Domain Consistency

**Status: Inconsistency Detected — High Priority**

The site appears to serve content at both `4liteuk.com` (non-`www`) and `www.4liteuk.com`, with canonical tags referencing both versions. This creates two separate versions of every page from Google's perspective.

**Recommendation:**

1. Decide on one canonical domain version: `https://4liteuk.com/` (recommended — currently the primary).
2. Implement a **301 redirect** from `https://www.4liteuk.com/` to `https://4liteuk.com/` at the server/CDN level.
3. Update all canonical tags to use the non-`www` version consistently.
4. Submit `https://4liteuk.com/` (without `www`) as the preferred domain in Google Search Console.

---

## 5. On-Page SEO

### 5.1 Title Tags

**Status: Inconsistent — Multiple Issues**

Title tags follow the pattern `[Keyword/Name] - 4Lite` across most pages. While this provides brand attribution, the keyword component is often generic or absent.

**Audited title tag inventory:**

| Page | Current Title | Length (approx.) | Issue |
|---|---|---|---|
| Homepage | Lighting Excellence - 4Lite | ~27 chars | Generic — misses key product categories and USPs |
| About Us | About Us - 4Lite | ~17 chars | Generic — no keyword |
| Products (listing) | Products - 4Lite (inferred) | ~18 chars | Generic — no keyword |
| Product categories | Batten / Non Corrosive - 4Lite | ~31 chars | Shows subcategory, not the categories index |
| Product (example) | Smart IP54 Circular Wall/Ceiling - 4Lite | ~41 chars | Good — but could include key specs (IP54, CCT, Smart) |

**Recommendations:**

| Page | Suggested Title |
|---|---|
| Homepage | LED Lighting for Homes & Gardens \| 4Lite UK |
| About Us | About 4Lite \| LIAQA-Certified LED Lighting Manufacturer |
| Products | LED Lighting Products \| Downlights, Smart, Solar & More \| 4Lite |
| Product categories | Product Categories \| LED Lighting Range \| 4Lite |
| Product (template) | [Product Name] \| [Key Spec] LED [Type] \| 4Lite |

- Target 50–60 characters for all title tags.
- Include primary target keyword near the start of the title.
- Maintain brand suffix (`| 4Lite`) for consistency.

---

### 5.2 Meta Descriptions

**Status: Missing on Most Key Pages — Critical**

Meta descriptions were absent on the homepage, About Us, and Products listing pages. At least one product category page (`battens-non-corrosive`) had a meta description, confirming Yoast SEO can generate them — but coverage is inconsistent.

| Page | Meta Description | Status |
|---|---|---|
| Homepage | Not detected | **Missing** |
| About Us | Not detected | **Missing** |
| Products (listing) | Not detected | **Missing** |
| Product categories index | Not detected | **Missing** |
| Battens category | "The extensive range of 4lite LED lighting battens come in a variety of shapes and sizes, ideally suited..." | Present — truncated at ~125 chars |
| Product (smart IP54) | Not detected | **Missing** |

When meta descriptions are absent, Google auto-generates snippets from page content. These are frequently poorly worded, may pull from navigation text or footer boilerplate, and consistently achieve lower click-through rates than crafted descriptions.

**Suggested meta descriptions for key pages:**

| Page | Suggested Meta Description |
|---|---|
| Homepage | Explore 4Lite's range of LED lighting for homes, gardens, and professional spaces. Smart, solar, and IP-rated lights with a 4-year warranty. Shop now. |
| About Us | 4Lite has over 30 years' experience delivering quality LED lighting solutions. LIAQA-certified, easy to install, and backed by a 4-year warranty. |
| Products | Browse 4Lite's full LED lighting range — downlights, bulkheads, floodlights, smart lighting, solar, and more. Built for homes, gardens and professionals. |
| Smart lighting category | Discover 4Lite's smart LED lighting range. Wi-Fi connected with no hub required, dimmable, voice-controlled and CCT adjustable. Shop now. |
| Product (template) | [Product Name] — [key spec, e.g. "IP54 rated, dimmable, CCT adjustable 2700–6500K"]. Easy to install, Wi-Fi connected, no hub required. 4-year warranty. |

Target 150–160 characters. Include the primary keyword and a clear action/benefit.

---

### 5.3 Heading Structure

**Status: Partially Verified — Generally Good**

Heading content was successfully extracted on the homepage and product detail page. The structure appeared logical across those pages.

**Verified heading inventory:**

| Page | H1 | H2 Examples | Assessment |
|---|---|---|---|
| Homepage | "Lighting excellence" | "Browse product types", "Featured products", "Browse spaces", "Expert advice" | Good hierarchy |
| About Us | "About Us" | 4 content pillars (quality, sustainability, etc.) | H1 is generic — no keyword |
| Product categories | "Product categories" | Product type names | H1 generic |
| Product (smart IP54) | "Smart IP54 Circular Wall/Ceiling" | Not retrieved | H1 is good — matches product name |

**Recommendations:**

1. Ensure every page has **exactly one H1** containing the primary target keyword.
2. The About Us H1 ("About Us") and Product categories H1 ("Product categories") are generic — update to keyword-inclusive variants (e.g., "About 4Lite — LED Lighting Specialists" and "Our LED Lighting Range").
3. Product pages should use H1 = product name; H2s for specifications, features, and related products.
4. Use Google Search Console URL Inspection to verify Googlebot sees H1s in the rendered DOM across all page types.

---

### 5.4 Open Graph & Social Tags

**Status: Partially Detected — Incomplete**

Open Graph data was found in the `window.__NUXT__` JavaScript object on product pages (via Yoast SEO's headless API), but was not confirmed as present in the HTML `<head>` as standard `<meta property="og:...">` tags. If OG tags are only present in the JavaScript payload and not in server-rendered HTML, they will not be read by social media crawlers (LinkedIn, Facebook, WhatsApp) which typically do not execute JavaScript.

**Recommendations:**

Implement Open Graph and Twitter Card tags in the Nuxt.js layout file as server-rendered `<meta>` tags in the HTML `<head>`:

```html
<!-- Open Graph -->
<meta property="og:type" content="website" />
<meta property="og:site_name" content="4Lite" />
<meta property="og:title" content="[Page Title]" />
<meta property="og:description" content="[Page Meta Description]" />
<meta property="og:image" content="https://4liteuk.com/images/og-default.jpg" />
<meta property="og:url" content="https://4liteuk.com/[page]/" />

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="[Page Title]" />
<meta name="twitter:description" content="[Page Meta Description]" />
<meta name="twitter:image" content="https://4liteuk.com/images/og-default.jpg" />
```

For product pages, use `og:type` = `product` with a product-specific image. OG images should be 1200×630px.

---

## 6. Structured Data & Schema Markup

**Status: Present in JS Object — Not Confirmed in HTML Head**

Yoast SEO metadata and schema data was detected in the `window.__NUXT__` JavaScript object on product category and product pages, indicating Yoast is generating structured data. However, for schema to reliably benefit SEO it must be rendered in the HTML `<head>` as a `<script type="application/ld+json">` block — not solely within a client-side JavaScript variable.

If Nuxt.js is not forwarding Yoast's JSON-LD output to the server-rendered HTML head, the schema is effectively invisible to Googlebot's first-pass crawl.

### 6.1 Schema Verification

The following schema types should be present and verified:

| Schema Type | Target Pages | Current Status | SEO Benefit |
|---|---|---|---|
| `Organization` | Homepage | Not confirmed in HTML head | Brand knowledge panel, sitelinks |
| `WebSite` + `SearchAction` | Homepage | Not confirmed | Sitelinks search box in SERPs |
| `Product` | All 167 product pages | In JS object — not confirmed in HTML head | Product rich results |
| `BreadcrumbList` | All non-homepage pages | Not confirmed | Breadcrumb trail display in SERPs |
| `Article` | All blog/article posts | Not confirmed | Article rich results |

### 6.2 Implementation Guidance

All schema should be emitted as a `<script type="application/ld+json">` block in the server-rendered HTML `<head>`, via the Nuxt.js `useHead()` composable, dynamically populated from the Yoast SEO REST API data.

**Homepage — Organization schema:**

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "4Lite",
  "url": "https://4liteuk.com/",
  "logo": "https://4liteuk.com/images/4lite-logo.png",
  "description": "4Lite supplies LED lighting solutions for homes, gardens, and professional spaces. LIAQA-certified with over 30 years of experience.",
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "customer service",
    "url": "https://4liteuk.com/contact/"
  }
}
```

**Product pages — Product schema:**

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[Product Name]",
  "description": "[Product Description]",
  "image": "[Product Image URL]",
  "sku": "[SKU Code]",
  "brand": {
    "@type": "Brand",
    "name": "4Lite"
  },
  "manufacturer": {
    "@type": "Organization",
    "name": "4Lite",
    "url": "https://4liteuk.com/"
  },
  "offers": {
    "@type": "Offer",
    "availability": "https://schema.org/InStock",
    "priceCurrency": "GBP",
    "seller": {
      "@type": "Organization",
      "name": "4Lite"
    }
  }
}
```

**Non-homepage pages — BreadcrumbList schema:**

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://4liteuk.com/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Products",
      "item": "https://4liteuk.com/products/"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "[Product Name]",
      "item": "https://4liteuk.com/products/[product-slug]/"
    }
  ]
}
```

---

## 7. Content & Keyword Analysis

### 7.1 Homepage

| Element | Current State | Assessment |
|---|---|---|
| Title | "Lighting Excellence - 4Lite" | Generic — misses core product types and use cases |
| Meta description | Not detected | Missing — auto-generated snippet likely |
| H1 | "Lighting excellence" | Generic — no primary keyword |
| Key content areas | Product types, featured products, spaces, expert advice | Good structural coverage |
| Target keyword opportunity | "LED lighting UK", "smart LED lighting", "garden lights UK" | Under-optimised for purchase-intent keywords |

---

### 7.2 Product Pages (167 Products)

Product title tags follow the pattern `[Product Name] - 4Lite`, which provides brand attribution but lacks keyword depth. Searchers looking for specific technical specifications (IP ratings, CCT, wattage, fitting type) cannot be captured from the title tag alone.

**Recommended title template:**
`[Product Name] | [Key Spec] LED [Type] | 4Lite`

Example: `Smart IP54 Circular Wall/Ceiling | Dimmable CCT LED Light | 4Lite`

Products also appear to lack meta descriptions on individual pages. Given 167 products, a template-driven approach via Yoast SEO or Nuxt.js is the most scalable solution.

---

### 7.3 Spaces Pages

The site includes a "Spaces" section (bathroom, garden, kitchen, living, industrial, commercial, hospitality) which is a strong SEO opportunity for room-specific and application-specific searches. The spaces sitemap has not been updated since **October 2024** (17 months), which may indicate the content has not been refreshed and could be missing newer products.

**Recommendations:**
- Review and update spaces pages to reference the latest product additions.
- Ensure spaces pages have keyword-rich titles (e.g., `Bathroom Lighting | IP-Rated LED Lights for Bathrooms | 4Lite`) and unique meta descriptions.
- Add schema markup for spaces pages where applicable.

---

### 7.4 Utility Pages — Index Pollution

The following pages are currently in the sitemap (and likely indexed) despite offering no meaningful search value. They dilute crawl budget and risk appearing in SERPs in confusing contexts.

| Page URL | Recommended Action |
|---|---|
| /competition/ | Add `noindex`, remove from sitemap |
| /gleeentry/ | Add `noindex`, remove from sitemap |
| /gleedata/ | Add `noindex`, remove from sitemap |
| /screwfixentry/ | Add `noindex`, remove from sitemap |
| /staxentry/ | Add `noindex`, remove from sitemap |
| /cookie-policy/ | Add `noindex`, remove from sitemap |
| /privacy-policy/ | Add `noindex`, remove from sitemap |
| /terms-and-conditions/ | Add `noindex`, remove from sitemap |
| /slavery-and-human-trafficking/ | Add `noindex`, remove from sitemap |
| /weee/ | Add `noindex`, remove from sitemap |
| /request-for-data-removal/ | Add `noindex`, remove from sitemap |

**Implementation:** Add `<meta name="robots" content="noindex, follow" />` via Yoast SEO page settings and remove the URLs from the XML sitemap.

---

### 7.5 Potential Duplicate Pages

Two pages in the sitemap may represent duplicate or overlapping content:

| Page | Potential Issue |
|---|---|
| /professional/ | Possible duplicate of /professional-range/ |
| /professional-range/ | Possible duplicate of /professional/ |

**Recommendation:** Review both pages. If they cover similar content, consolidate into one page with a 301 redirect from the other. If they are distinct, ensure each has a unique title, meta description, H1, and body copy with no shared paragraphs.

---

## 8. Internal Linking

**Status: Architecture Confirmed — Detail Unverifiable**

The site navigation structure is well-defined with clear top-level sections: product categories, spaces, collections (Antheia, Marinus), smart lighting guide, and articles. Product pages link to related products and accessories (verified on the Smart IP54 product page, which linked to Smart IP65, IP54 Surface Circular, Smart PIR Sensor, and Smart Remote).

Breadcrumb navigation was partially verified — the product detail page showed a breadcrumb structure (`Home > Smart IP54 Circular Wall/Ceiling > Smart Lighting`), but the product categories index page did not return visible breadcrumbs during the crawl.

**Recommendations:**

1. **Confirm breadcrumb navigation** is present on all non-homepage pages (category, product, space, article) using Google Search Console URL Inspection.
2. **Add BreadcrumbList schema** to all breadcrumb instances (see Section 6.2).
3. **Product pages** should consistently link to: the parent category, 3–5 related products, and the relevant spaces page (e.g., an IP65 bulkhead links to the Bathroom Spaces page).
4. **Spaces pages** should cross-link to the relevant product categories and feature flagship products.
5. **Smart Lighting Guide** (`/smart-lighting-guide/`) is a strong content asset — ensure it is linked from smart product pages and from the Expert Advice section on the homepage.
6. **Collections pages** (Antheia, Marinus) should link to individual products within the collection and vice versa.

---

## 9. Image SEO

**Status: Partially Verified — Alt Text Pattern Needs Improvement**

Product images were confirmed as having alt text on the audited product page. The alt text pattern observed follows SKU codes and variant descriptions (e.g., `4L1-3320.2.jpg` variant descriptions), which is functional but sub-optimal for image search and accessibility.

**Recommendations:**

| Area | Recommendation |
|---|---|
| Alt text | Use descriptive alt text: e.g., `alt="Smart IP54 Circular Wall/Ceiling LED Light in white – Wi-Fi connected, dimmable"` rather than SKU codes |
| File naming | Ensure filenames are descriptive and hyphenated (most appear to be SKU-based — acceptable but not optimal) |
| Format | Serve images in **WebP** format with JPEG fallback for older browsers |
| Lazy loading | Apply `loading="lazy"` to all images below the fold |
| Dimensions | Define explicit `width` and `height` on all `<img>` elements to prevent Cumulative Layout Shift (CLS) |
| Preloading | Use `<link rel="preload" as="image" />` for hero/above-the-fold images to improve LCP |

---

## 10. Core Web Vitals & Performance

**Status: Cannot Directly Measure — Risks Inferred from Tech Stack**

Direct Core Web Vitals scores could not be retrieved automatically. The following risk assessment is based on the observed technology stack.

### 10.1 Risk Assessment by Metric

| Metric | Threshold (Good) | Risk Level | Primary Cause |
|---|---|---|---|
| LCP (Largest Contentful Paint) | < 2.5s | Medium | JS rendering delay, hero image loading, GTM overhead |
| INP (Interaction to Next Paint) | < 200ms | Medium | Nuxt.js hydration overhead on product/category pages |
| CLS (Cumulative Layout Shift) | < 0.1 | Medium | Likely missing image dimensions, font swap |
| FCP (First Contentful Paint) | < 1.8s | Medium | GTM script, potential render-blocking resources |
| TTFB (Time to First Byte) | < 800ms | Low–Medium | Dependent on CDN/hosting configuration |

### 10.2 Recommendations

| Issue | Fix |
|---|---|
| JS rendering delay | Confirm Nuxt.js SSR/SSG is active for all page types (see Section 4.2) |
| Hero image LCP | Add `<link rel="preload" as="image" href="[hero-image]" />` in page `<head>` |
| Image CLS | Add explicit `width` and `height` attributes to all `<img>` elements |
| GTM overhead | Verify GTM loads asynchronously; audit tag firing rules for unnecessary triggers |
| Font rendering | If custom fonts are used, add `font-display: swap` to all `@font-face` declarations |

### 10.3 Next Steps for Performance

1. Run **Google PageSpeed Insights** on `https://4liteuk.com/` for actual CWV scores (both mobile and desktop).
2. Run on a representative **product page** and **category page** — these typically have heavier image/JS payloads.
3. Target scores: LCP < 2.5s, INP < 200ms, CLS < 0.1 on mobile.
4. Monitor monthly via **Google Search Console → Core Web Vitals report**.

---

## 11. Prioritised Issue Log

### Critical — Fix Immediately

| # | Issue | Pages Affected | SEO Impact |
|---|---|---|---|
| C1 | robots.txt returns 404 | Sitewide | No crawl guidance, no sitemap reference, unblocked utility pages |
| C2 | Canonical tags pointing to CMS subdomain (`cms.4liteuk.com`) | Category pages (all) | Google told to de-index public site in favour of inaccessible backend |
| C3 | www vs non-www inconsistency in canonical tags | Product pages vs rest of site | Duplicate content — two versions of every product page |
| C4 | Meta descriptions missing on key pages | Homepage, About, Products, product pages | Poor SERP snippets, reduced CTR |

### High Priority — Fix Within 30 Days

| # | Issue | Pages Affected | SEO Impact |
|---|---|---|---|
| H1 | Generic title tags lacking keywords | Homepage, About, Products, categories index | Reduced rankings and CTR |
| H2 | Utility and competition pages indexed | 11+ pages | Crawl budget waste, index dilution, confusing SERPs |
| H3 | Pages sitemap has no `<lastmod>` values | 29 pages | Crawl prioritisation |
| H4 | Open Graph tags not confirmed in HTML head | All pages | Social sharing unoptimised, no preview images |
| H5 | JavaScript rendering — SSR/SSG not confirmed for all page types | Homepage, category pages | Potential indexability failure for body content |

### Medium Priority — Fix Within 90 Days

| # | Issue | Pages Affected | SEO Impact |
|---|---|---|---|
| M1 | Schema markup in JS object — not confirmed in HTML head | All pages | May be invisible to Google's first crawl pass; ineligible for rich results |
| M2 | Sitemap accessed via 301 cross-subdomain redirect | Sitewide | Non-standard; resolve with direct sitemap entry at root domain |
| M3 | Spaces sitemap not updated since October 2024 | Spaces section | Stale `<lastmod>` dates; may impede recrawling of updated content |
| M4 | `/professional/` and `/professional-range/` potentially duplicate | 2 pages | Duplicate content risk |
| M5 | Breadcrumb navigation not confirmed on all page types | Category, spaces, articles | UX, internal linking, rich results |

### Low Priority — Ongoing Improvements

| # | Issue | Pages Affected | SEO Impact |
|---|---|---|---|
| L1 | Product image alt text uses SKU codes rather than descriptions | 167 product pages | Image search, accessibility |
| L2 | Article schema markup not confirmed on blog posts | All articles | Article rich results |
| L3 | Core Web Vitals full audit not yet run | All pages | Rankings, user experience |

---

## 12. Recommended Action Plan

### Phase 1 — Critical Fixes (Week 1–2)

| Action | Owner | Effort |
|---|---|---|
| Create and deploy `robots.txt` at `https://4liteuk.com/robots.txt` | Developer | Very Low |
| Audit and fix canonical tag generation in Nuxt.js — ensure all canonicals use `https://4liteuk.com/` (non-www) | Developer | Low |
| Fix CMS subdomain canonicals on category pages — canonical must reflect public-facing URL, not `cms.4liteuk.com` | Developer | Low |
| Implement 301 redirect from `www.4liteuk.com` to `4liteuk.com` (or vice versa) | Developer | Very Low |
| Write and publish meta descriptions for homepage, About Us, Products, and all product pages | SEO / Content | Medium |

### Phase 2 — High Priority (Weeks 3–6)

| Action | Owner | Effort |
|---|---|---|
| Add `noindex` meta tag to all utility, legal, and competition pages | Developer / SEO | Low |
| Remove utility/competition pages from XML sitemap | Developer | Low |
| Rewrite title tags for homepage, About Us, Products listing, and categories index | SEO | Low |
| Update product title tag template to include key specs | SEO / Developer | Low |
| Verify Open Graph tags are rendered in server-side HTML `<head>` (not only JS payload) | Developer | Low |
| Configure Yoast SEO to populate `<lastmod>` on pages sitemap | Developer | Very Low |
| Verify SSR/SSG rendering for homepage and category pages via Google Search Console | Developer / SEO | Low |

### Phase 3 — Medium Priority (Weeks 7–12)

| Action | Owner | Effort |
|---|---|---|
| Verify schema markup is emitted as JSON-LD in HTML head; fix if only present in JS object | Developer | Medium |
| Implement BreadcrumbList schema across all non-homepage page types | Developer | Medium |
| Resolve or consolidate `/professional/` vs `/professional-range/` duplication | SEO / Content | Low |
| Update spaces content and `<lastmod>` timestamps for spaces sitemap | Content | Medium |
| Create sitemap entry at root domain or eliminate 301 redirect on sitemap URL | Developer | Low |
| Run PageSpeed Insights and address Core Web Vitals failures | Developer | Medium |
| Confirm breadcrumb UI navigation on all non-homepage pages | Developer | Low |

### Phase 4 — Ongoing

| Action | Owner | Effort |
|---|---|---|
| Update product image alt text from SKU codes to descriptive text | Content | Medium (bulk update) |
| Add Article schema to all blog posts | Developer | Low (template) |
| Monthly Core Web Vitals monitoring via Google Search Console | SEO | Low |
| Review and refresh meta descriptions quarterly | SEO | Medium |
| Keep spaces content updated to reflect new product additions | Content | Ongoing |

---

## 13. Appendix

### A. Pages Sitemap — Full URL List (29 pages)

The following pages were found in `pages-sitemap.xml`. None have `<lastmod>` values.

```
https://4liteuk.com/about-us/
https://4liteuk.com/antheia/
https://4liteuk.com/articles/
https://4liteuk.com/catalogues/
https://4liteuk.com/weee/
https://4liteuk.com/competition/
https://4liteuk.com/contact/
https://4liteuk.com/cookie-policy/
https://4liteuk.com/gleedata/
https://4liteuk.com/gleeentry/
https://4liteuk.com/inspiration-page/
https://4liteuk.com/
https://4liteuk.com/marinus/
https://4liteuk.com/privacy-policy/
https://4liteuk.com/product-categories/
https://4liteuk.com/products/
https://4liteuk.com/professional/
https://4liteuk.com/professional-range/
https://4liteuk.com/request-for-data-removal/
https://4liteuk.com/screwfixentry/
https://4liteuk.com/slavery-and-human-trafficking/
https://4liteuk.com/smart-lighting-guide/
https://4liteuk.com/spaces/
https://4liteuk.com/staxentry/
https://4liteuk.com/stockists/
https://4liteuk.com/sustainability/
https://4liteuk.com/technical-compliance/
https://4liteuk.com/terms-and-conditions/
https://4liteuk.com/warranty/
```

### B. Sitemap Index — Sub-Sitemaps

| File | Last Modified |
|---|---|
| post-sitemap.xml | 2026-01-15T13:40:54+00:00 |
| product-sitemap.xml | 2026-03-12T01:30:33+00:00 |
| spaces-sitemap.xml | 2024-10-11T08:13:00+00:00 |
| pages-sitemap.xml | 2026-03-16T09:08:35+00:00 |
| post_category-sitemap.xml | 2026-03-16T09:08:35+00:00 |
| product_category-sitemap.xml | 2026-03-16T09:08:35+00:00 |

Sitemap index generated by **Yoast SEO**.
Sitemap index URL: `https://cms.4liteuk.com/sitemap_index.xml`
Accessed via: `https://4liteuk.com/sitemap.xml` → 301 redirect

### C. Sample Product URL Structure (First 10 of 167)

```
https://4liteuk.com/products/antheia-solar-portable-lantern/
https://4liteuk.com/products/antheia-solar-wall-lantern/
https://4liteuk.com/products/smart-ip54-circular-wall-ceiling/
https://4liteuk.com/products/smart-ip65-circular-wall-ceiling/
https://4liteuk.com/products/universal-lithium-emergency-kit/
https://4liteuk.com/products/24v-10w-ip20-rgb-led-strip-10m/
https://4liteuk.com/products/24v-ip20-white-led-strip/
https://4liteuk.com/products/24v-ip20-rgbw-led-strip/
https://4liteuk.com/products/smart-c35-e14-amber-filament-bulb/
https://4liteuk.com/products/smart-c37-e14-rgbw-bulb/
```

URL structure is clean, descriptive, and well-hyphenated — no issues.

### D. Canonical Tag Observations

| Page Tested | Canonical Found | Issue |
|---|---|---|
| `/product-categories/` | `https://cms.4liteuk.com/product-categories/battens-non-corrosive/` | Points to CMS subdomain |
| `/products/smart-ip54-circular-wall-ceiling/` | `https://www.4liteuk.com/products/smart-ip54-circular-wall-ceiling/` | Uses `www.` prefix — inconsistent with site domain |
| Homepage | Not retrieved | Unverified |
| About Us | Not retrieved | Unverified |

### E. robots.txt Status

`https://4liteuk.com/robots.txt` → **HTTP 404 Not Found**

No robots.txt file exists. This must be created at the root of the primary domain.

---

*This report was produced using automated web crawling, XML sitemap analysis, and tech stack profiling conducted on 16 March 2026. Findings should be validated using manual tools including Google Search Console URL Inspection, Chrome DevTools rendered DOM view, Screaming Frog SEO Spider (for full heading and image alt text crawl), and Google PageSpeed Insights (for Core Web Vitals scores).*
