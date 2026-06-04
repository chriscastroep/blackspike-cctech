# SEO Design — chriscastrotech.com

**Date:** 2026-06-03
**Goal:** Improve local search visibility for "IT support Los Angeles" and related queries.
**Scope:** Option B — technical foundations + on-page signals. No visible copy rewrites.

---

## 1. Technical Crawling & Indexing

### `public/robots.txt` (new file)
```
User-agent: *
Allow: /
Disallow: /doom

Sitemap: https://chriscastrotech.com/sitemap-index.xml
```

### Sitemap
- Install `@astrojs/sitemap` integration
- Add to `astro.config.mjs`: `site: 'https://chriscastrotech.com'` and `integrations: [sitemap({ filter: (page) => !page.includes('/doom') })]`
- Generates `/sitemap-index.xml` and `/sitemap-0.xml` automatically on build

### Doom page noindex
- Add `<meta name="robots" content="noindex, nofollow">` to `src/pages/doom.astro` `<head>`

---

## 2. LocalBusiness JSON-LD Schema

Add a `<script type="application/ld+json">` block in `src/layouts/Layout.astro`.

```json
{
  "@context": "https://schema.org",
  "@type": "ProfessionalService",
  "name": "Chris Castro Technical Solutions",
  "url": "https://chriscastrotech.com",
  "telephone": "+12133080127",
  "email": "chris@chriscastrotech.com",
  "description": "Expert IT support and consulting in Los Angeles, CA. Hard drive recovery, computer repair, network setup, and emergency support.",
  "priceRange": "$$",
  "areaServed": {
    "@type": "City",
    "name": "Los Angeles",
    "sameAs": "https://www.wikidata.org/wiki/Q65"
  },
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Los Angeles",
    "addressRegion": "CA",
    "addressCountry": "US"
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
      "opens": "08:00",
      "closes": "23:00"
    },
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Saturday","Sunday"],
      "opens": "11:00",
      "closes": "17:00"
    }
  ],
  "sameAs": [
    "https://www.yelp.com/biz/mid-city-it-los-angeles",
    "https://www.linkedin.com/company/mid-city-it"
  ]
}
```

The schema values for `telephone`, `email`, `sameAs`, and `description` should be pulled from `global_settings.json` where possible to keep a single source of truth.

---

## 3. Meta Tags & On-Page Signals

### Title tag
Change `global_settings.json` `"title"` from:
```
Chris Castro Technical Solutions
```
To:
```
Chris Castro Technical Solutions — IT Support Los Angeles
```

### Meta description
Change `global_settings.json` `"description"` to:
```
Expert IT support in Los Angeles. Hard drive recovery, computer repair, network setup, and emergency support. Available 7 days a week. Call 213-308-0127.
```

### Canonical URL
Add to `src/layouts/Layout.astro` `<head>`:
```html
<link rel="canonical" href={ global_settings.base_url + Astro.url.pathname } />
```

### Twitter/X card
Add to `src/layouts/Layout.astro` `<head>`:
```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content={ title ?? global_settings.title } />
<meta name="twitter:description" content={ description ?? global_settings.description } />
```

### `public/manifest.json`
Update the upstream placeholder values:
- `"name"`: `"Chris Castro Technical Solutions"`
- `"short_name"`: `"CCTS"`
- `"description"`: `"Expert IT support in Los Angeles — hard drive recovery, computer repair, network setup, and emergency support."`

---

## Files Changed

| File | Action |
|---|---|
| `public/robots.txt` | Create |
| `astro.config.mjs` | Add `site` + `@astrojs/sitemap` integration |
| `src/pages/doom.astro` | Add noindex meta tag |
| `src/layouts/Layout.astro` | Add JSON-LD, canonical, Twitter/X card tags |
| `src/data/global_settings.json` | Update title, description |
| `public/manifest.json` | Update name, short_name, description |

---

## Out of Scope

- Visible copy rewrites in section JSON files (defer until rankings are established)
- Google Search Console setup (manual step outside the codebase)
- Google Business Profile updates (manual step)
