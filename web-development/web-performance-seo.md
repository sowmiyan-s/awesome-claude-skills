---
name: web-performance-seo
description: Master web performance optimization, Core Web Vitals (LCP, INP, CLS), asset pipelines, bundle splitting, critical rendering path tuning, and comprehensive technical SEO (structured data JSON-LD, Open Graph, meta tags, sitemaps, robots.txt, canonical URLs, semantic HTML). Use this skill when auditing page speed, optimizing slow web apps, fixing Core Web Vitals issues, improving search engine crawlability and indexation, or setting up enterprise-grade SEO architecture.
---

# Web Performance & Technical SEO Optimizer

An advanced engineering skill for diagnosing performance bottlenecks, maximizing Google Core Web Vitals scores, streamlining asset delivery, and implementing comprehensive technical SEO strategies that drive search rankings and lightning-fast user experiences.

---

## 1. Core Web Vitals (CWV) Optimization Guide

Target thresholds for 75th percentile of real users (CrUX):

| Metric | Target | Focus Area | Key Fixes |
| :--- | :--- | :--- | :--- |
| **LCP** (Largest Contentful Paint) | `< 2.5s` | Server response, resource load, render blocking | Preload hero images, optimize TTFB, eliminate render-blocking CSS/JS, use modern AVIF/WebP. |
| **INP** (Interaction to Next Paint) | `< 200ms` | Main thread blocking, JS execution, event latency | Break long tasks with `scheduler.yield()`, debounce high-frequency events, web workers. |
| **CLS** (Cumulative Layout Shift) | `< 0.1` | Visual stability, unexpected shifts | Set explicit `width`/`height` on images & videos, reserve skeleton space for dynamic ads/widgets, `font-display: swap` with size-adjust. |

---

## 2. Technical Performance Playbook

### Critical Rendering Path & Asset Delivery
1. **Fonts Optimization**:
   - Self-host fonts rather than requesting external third-party CDNs.
   - Preload primary WOFF2 fonts with `<link rel="preload" as="font" type="font/woff2" crossorigin>`.
   - Prevent FOIT/FOUT using CSS `@font-face { font-display: swap; size-adjust: 100%; }`.
2. **Image & Media Pipeline**:
   - Serve next-gen formats (`.avif`, `.webp`) with responsive `srcset` and `sizes` attributes.
   - Set `loading="lazy"` and `decoding="async"` for below-the-fold media; explicitly use `priority` or `fetchpriority="high"` for hero visuals.
   - Use vector SVGs with inline critical paths or clean sprite systems.
3. **JavaScript & Bundle Reduction**:
   - Implement dynamic `import()` code splitting for modals, non-critical tab views, and heavy charting libraries.
   - Tree-shake barrel files and remove unused npm dependencies.
   - Offload heavy compute tasks (crypto, syntax highlighting, data parsing) to Web Workers.
4. **Caching & Edge CDN Strategy**:
   - Static assets (immutable hashed files): `Cache-Control: public, max-age=31536000, immutable`.
   - HTML / Dynamic SSR responses: `Cache-Control: public, s-maxage=60, stale-while-revalidate=600`.

---

## 3. Technical SEO & Discoverability Architecture

### Semantic HTML & Document Hierarchy
- Exactly one `<h1>` per page reflecting the main topic.
- Strict heading tree (`<h1>` $\rightarrow$ `<h2>` $\rightarrow$ `<h3>`) without skipping levels.
- Use landmark semantic tags: `<header>`, `<nav>`, `<main>`, `<article>`, `<aside>`, `<footer>`.

### Metadata & Social Graph
Include in the `<head>` of every production page:

```html
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Page Title (50-60 chars) | Brand Name</title>
  <meta name="description" content="Engaging, accurate summary containing target keywords (150-160 characters)." />
  <link rel="canonical" href="https://example.com/page-url" />

  <!-- Open Graph / Facebook / LinkedIn -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://example.com/page-url" />
  <meta property="og:title" content="Engaging Open Graph Title" />
  <meta property="og:description" content="Engaging description for social previews." />
  <meta property="og:image" content="https://example.com/og-image.jpg" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:site" content="@brandhandle" />
  <meta name="twitter:title" content="Engaging Twitter Title" />
  <meta name="twitter:description" content="Engaging description for Twitter." />
  <meta name="twitter:image" content="https://example.com/og-image.jpg" />
</head>
```

### Structured Data (JSON-LD)
Embed rich schema markup inside `<script type="application/ld+json">` for Google Rich Snippets:
- **Organization / WebSite**: Company identity, logo, search actions.
- **Article / BlogPosting**: Author, publication date, modified date, publisher.
- **Product / SoftwareApplication**: Price, currency, rating, aggregate reviews.
- **FAQPage / BreadcrumbList**: Accordion FAQs, hierarchical breadcrumb trails.

---

## 4. Crawlability & Indexation Standards

- **`robots.txt`**: Ensure crawler access to public assets while protecting private admin, search query, and staging paths:
  ```text
  User-agent: *
  Allow: /
  Disallow: /api/
  Disallow: /admin/
  Disallow: /search?*
  Sitemap: https://example.com/sitemap.xml
  ```
- **`sitemap.xml`**: Auto-generated dynamic XML sitemap listing canonical URLs with `<lastmod>` timestamps.
- **Status Code Correctness**:
  - Valid pages: `200 OK`
  - Moved permanently: `301 Moved Permanently`
  - Broken / missing: `404 Not Found` with helpful navigation
  - Gone permanently: `410 Gone`
