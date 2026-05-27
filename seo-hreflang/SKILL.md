---
name: seo-hreflang
description: >
  Referencia técnica de hreflang integrada en seo-international (sección 11).
  Toda la implementación técnica — validación, métodos HTML/HTTP/XML sitemap,
  debugging, notas CMS — vive en seo-international para mantener contexto
  completo de estrategia internacional. Este skill redirige a seo-international.
  Usar seo-international directamente para cualquier tarea de hreflang.
user-invokable: false
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Hreflang — Referencia movida a seo-international

> **Este skill ha sido integrado en `seo-international` (sección 11).** Usar `seo-international` para cualquier tarea de hreflang.

Toda la documentación técnica de hreflang vive ahora en `seo-international` sección 11:
- Validación: self-referencing tags, return tags, x-default, ISO codes, canonical alignment
- Métodos: HTML link tags, HTTP headers, XML sitemap
- Tabla de errores comunes con severidad y fix
- Quick validation commands + tools
- Debugging step-by-step
- CMS-specific notes (WordPress, Shopify, Webflow, Next.js, Nuxt)

Para estrategia internacional completa (estructura de URLs, priorización de mercados, localización de contenido), `seo-international` es el skill principal.

> **Critical expectation-setting:** Google treats hreflang as a **hint, not a directive** (reiterated May 2025). Canonical tags, content similarity, site structure, and indexation status can all override hreflang annotations. Hreflang does not guarantee correct locale serving — it improves the probability. Set client expectations accordingly.

## Validation Checks

### Self-Referencing Tags
- Every page must include an hreflang tag pointing to **itself**
- The self-referencing URL must exactly match the page's canonical URL
- Missing self-referencing tags cause Google to ignore the entire hreflang set

### Return Tags (Bidirectional Requirement)
- If page A links to page B with hreflang, page B must link back to page A
- Every hreflang relationship must be bidirectional (A→B **and** B→A)
- Missing return tags invalidate the hreflang signal for both pages

### x-default Tag
- Required: designates the fallback page for unmatched languages/regions
- Only one x-default per set of alternates
- Must also receive return tags from all other language versions

**x-default target decision tree:**
```
Does the site have a language selector page? → point x-default to selector
Is the site English-only with regional variants? → point to English/global version
Does the site geo-redirect automatically? → point to geo-redirect entry point
Is there a truly global version? → point to it
```
- Do NOT use a specific locale page (e.g., `/en-us/`) as x-default unless that page genuinely serves all unmatched locales
- Missing x-default does not break hreflang, but Google may pick incorrect fallback for unmatched regions

### Language Code Validation
Must use ISO 639-1 two-letter codes:
```
✅ en, fr, de, ja, zh-Hans, zh-Hant, pt-BR, pt-PT
❌ eng (ISO 639-2, invalid)
❌ jp (should be ja for Japanese)
❌ zh (ambiguous — specify zh-Hans or zh-Hant)
```

### Region Code Validation
Optional region qualifier uses ISO 3166-1 Alpha-2:
```
✅ en-US, en-GB, pt-BR, es-MX, es-ES
❌ en-uk (should be en-GB — UK is not a valid ISO code)
❌ es-LA (Latin America is not a country)
```

### Canonical URL Alignment
- Hreflang tags must only appear on **canonical** URLs
- If a page has `rel=canonical` pointing elsewhere, hreflang on that page is ignored
- Canonical URL and hreflang URL must match exactly (including trailing slashes)

### Protocol Consistency
- All URLs in an hreflang set must use the same protocol (HTTPS)
- Mixed HTTP/HTTPS causes validation failures

## Implementation Methods

### Method 1: HTML Link Tags
Best for sites with fewer than 50 language/region variants per page.
```html
<head>
  <link rel="alternate" hreflang="en-US" href="https://example.com/page" />
  <link rel="alternate" hreflang="en-GB" href="https://example.co.uk/page" />
  <link rel="alternate" hreflang="fr"    href="https://example.com/fr/page" />
  <link rel="alternate" hreflang="de"    href="https://example.de/page" />
  <link rel="alternate" hreflang="x-default" href="https://example.com/page" />
</head>
```

### Method 2: HTTP Headers
Best for non-HTML files (PDFs, documents):
```
Link: <https://example.com/doc.pdf>; rel="alternate"; hreflang="en-US",
      <https://example.com/fr/doc.pdf>; rel="alternate"; hreflang="fr",
      <https://example.com/doc.pdf>; rel="alternate"; hreflang="x-default"
```
Set via Nginx/Apache config or CDN rules.

### Method 3: XML Sitemap (Recommended for large sites)
Best for 50+ pages, cross-domain setups, or sites where editing `<head>` at scale is impractical:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://example.com/page</loc>
    <xhtml:link rel="alternate" hreflang="en-US" href="https://example.com/page" />
    <xhtml:link rel="alternate" hreflang="fr"    href="https://example.com/fr/page" />
    <xhtml:link rel="alternate" hreflang="de"    href="https://example.de/page" />
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/page" />
  </url>
  <url>
    <loc>https://example.com/fr/page</loc>
    <xhtml:link rel="alternate" hreflang="en-US" href="https://example.com/page" />
    <xhtml:link rel="alternate" hreflang="fr"    href="https://example.com/fr/page" />
    <xhtml:link rel="alternate" hreflang="de"    href="https://example.de/page" />
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/page" />
  </url>
</urlset>
```
Rules:
- Include `xmlns:xhtml` namespace declaration
- Every `<url>` entry must list ALL language alternates (including itself)
- Split at 50,000 URLs per sitemap file

### Method Comparison
| Method | Best For | Pros | Cons |
|--------|----------|------|------|
| HTML link tags | Small sites | Easy, visible in source | Bloats `<head>`, hard at scale |
| HTTP headers | Non-HTML files | Works for PDFs | Complex server config |
| XML sitemap | Large/cross-domain | Scalable, centralized | Not visible on page |

## Common Hreflang Mistakes

| Issue | Severity | Fix |
|-------|----------|-----|
| Missing self-referencing tag | Critical | Add hreflang pointing to same page URL |
| Missing return tags (A→B but no B→A) | Critical | Add matching return tags on all alternates |
| Missing x-default | High | Add x-default pointing to fallback/selector page |
| Invalid language code (`eng`, `jp`) | High | Use ISO 639-1 two-letter codes |
| Invalid region code (`en-uk`, `es-LA`) | High | Use ISO 3166-1 Alpha-2 codes |
| Hreflang on non-canonical URL | High | Move hreflang to canonical URL only |
| HTTP/HTTPS mismatch in URL set | Medium | Standardize all URLs to HTTPS |
| Trailing slash inconsistency | Medium | Match canonical URL format exactly |
| Hreflang in both HTML and sitemap | Medium | Creates conflicting signals — choose one method only |
| Hreflang on non-200 pages | High | Remove hreflang from redirected or 404 pages |
| Hreflang pointing to noindex page | High | All hreflang target pages must be indexable |
| Content parity < 70% between variants | Medium | Google may ignore hreflang if variants are too dissimilar |
| HTML hreflang on JS-rendered SPA | Critical | Must be in initial HTML response — not injected by JS |

## Quick Validation
```bash
# Check if hreflang tags exist on a page
curl -sL https://example.com/page | grep -i "hreflang"

# Check lang attribute
curl -sL https://example.com/page | grep -i '<html'

# Verify canonical matches hreflang self-reference
curl -sL https://example.com/page | grep -i -E '(canonical|hreflang)'
```

**Tools:** hreflang.org validator (free), Merkle hreflang tester (free), Ahrefs Site Audit hreflang report (paid).

## Debugging Hreflang Issues

**Step-by-step diagnosis for "wrong language version ranking":**

1. **Verify tags exist and are valid** — `curl -sL [URL] | grep hreflang` + hreflang.org validator
2. **Check GSC International Targeting report** — GSC → Settings → International Targeting → Language tab. Verify Google recognizes the hreflang annotations (shows "detected" status per locale).
3. **Screaming Frog hreflang audit** — Crawl → Reports → Hreflang → filter for issues (missing return tags, invalid codes, missing self-referencing)
4. **Ahrefs hreflang report** — Site Audit → Issues → filter "hreflang" for site-wide coverage
5. **Check canonical conflict** — if canonical points to a different URL than hreflang self-reference, Google follows canonical and may ignore hreflang
6. **Check content similarity** — if locale pages have <70% content parity, Google may serve the version it judges most relevant regardless of hreflang
7. **Check indexation** — URL Inspection tool for each locale variant. Non-indexed pages will not be served even with valid hreflang.

**Performance considerations at scale:** For sites with 10+ language versions, HTML hreflang adds ~2–4KB to every page's `<head>`. Switch to XML sitemap method at 10+ locales to avoid head bloat and keep under the 2MB Googlebot crawl limit.

**JavaScript SPAs:** hreflang must be present in the initial server-side HTML response. Client-side injection via JS will not be reliably picked up. For SPAs, use the XML sitemap method as the primary hreflang delivery mechanism.

## CMS-Specific Notes

| Platform | Approach |
|----------|---------|
| **WordPress + WPML** | WPML auto-generates hreflang; verify in Siteground/hreflang validator post-install |
| **WordPress + Polylang** | Free tier handles hreflang; PRO adds sitemap integration |
| **Shopify** | No native hreflang — requires Shopify Markets (2022+) or Langify/Weglot app |
| **Webflow** | Localization feature (2024) generates hreflang automatically |
| **Next.js** | `next/head` + `i18n` config in `next.config.js`; use `next-sitemap` for multilingual sitemap |
| **Nuxt** | `@nuxtjs/i18n` module handles hreflang + sitemap automatically |
