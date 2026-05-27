---
name: seo-sitemap
description: >
  Analyze existing XML sitemaps or generate new ones with industry templates.
  Validates format, URLs, and structure. Use when user says "sitemap",
  "generate sitemap", "sitemap issues", or "XML sitemap".
user-invokable: true
argument-hint: "[url or generate]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Sitemap Analysis & Generation

## Mode 1: Analyze Existing Sitemap

### Validation Checks
- Valid XML format
- URL count < 50,000 per file (protocol limit)
- File size < 50 MB uncompressed (can be hit before 50K URLs if URLs are long)
- All URLs return HTTP 200
- `<lastmod>` dates are accurate (not all identical) — **Google actively verifies lastmod accuracy and will ignore it site-wide if it's routinely inaccurate** (e.g., all pages updated on every deployment regardless of actual content changes). Only update lastmod when content meaningfully changes.
- No deprecated tags: `<priority>` and `<changefreq>` are ignored by Google and Bing (as of 2024). These tags are effectively dead across all major search engines — safe to remove them.
- Sitemap referenced in robots.txt and submitted in Google Search Console
- Compare crawled pages vs sitemap; flag missing pages
- Sitemap index files must NOT be nested (a sitemap index cannot reference another sitemap index — only plain sitemaps)
- Check if sitemap is served compressed (.xml.gz) — reduces bandwidth for large files

### Quality Signals
- Sitemap index file if >50k URLs
- Split by content type (pages, posts, images, videos)
- No non-canonical URLs in sitemap
- No noindexed URLs in sitemap
- No redirected URLs in sitemap
- HTTPS URLs only (no HTTP)

### Common Issues
| Issue | Severity | Fix |
|-------|----------|-----|
| >50k URLs in single file | Critical | Split with sitemap index |
| Non-200 URLs | High | Remove or fix broken URLs |
| Noindexed URLs included | High | Remove from sitemap |
| Redirected URLs included | Medium | Update to final URLs |
| All identical lastmod | Medium | Use actual modification dates; Google ignores lastmod if always identical |
| Lastmod updated on every deploy | Medium | Only update lastmod when content actually changes |
| Priority/changefreq used | Info | Remove — ignored by Google and Bing |
| File > 50 MB uncompressed | High | Split into multiple sitemaps via sitemap index |
| Sitemap index nesting | Critical | A sitemap index cannot reference another sitemap index — only plain sitemaps |
| Uncompressed large sitemap | Low | Serve as .xml.gz to reduce bandwidth |

## Mode 2: Generate New Sitemap

### Process
1. Ask for business type (or auto-detect from existing site)
2. Load industry template from `../seo-plan/assets/` directory
3. Interactive structure planning with user
4. Apply quality gates:
   - ⚠️ WARNING at 30+ location pages (require 60%+ unique content)
   - 🛑 HARD STOP at 50+ location pages (require justification)
5. Generate valid XML output
6. Split at 50k URLs with sitemap index
7. Generate STRUCTURE.md documentation

### Safe Programmatic Pages (OK at scale)
✅ Integration pages (with real setup docs)
✅ Template/tool pages (with downloadable content)
✅ Glossary pages (200+ word definitions)
✅ Product pages (unique specs, reviews)
✅ User profile pages (user-generated content)

### Penalty Risk (avoid at scale)
❌ Location pages with only city name swapped
❌ "Best [tool] for [industry]" without industry-specific value
❌ "[Competitor] alternative" without real comparison data
❌ AI-generated pages without human review and unique value

## Sitemap Format

### Standard Sitemap
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/page</loc>
    <lastmod>2026-02-07</lastmod>
  </url>
</urlset>
```

### Sitemap Index (for >50k URLs)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://example.com/sitemap-pages.xml</loc>
    <lastmod>2026-02-07</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://example.com/sitemap-posts.xml</loc>
    <lastmod>2026-02-07</lastmod>
  </sitemap>
</sitemapindex>
```

## Advanced Sitemap Patterns

### Compressed Sitemaps (.xml.gz)

Large sitemaps should be served compressed to reduce bandwidth and submission time:
```bash
# Gzip a sitemap at generation time
gzip -k sitemap.xml  → produces sitemap.xml.gz

# Nginx: serve compressed sitemap automatically
location /sitemap.xml.gz {
  add_header Content-Encoding gzip;
  add_header Content-Type application/xml;
}
```
Submit the `.xml.gz` URL directly in Google Search Console — GSC accepts compressed sitemaps.

### Hreflang Sitemap (Multilingual Sites)

For large multilingual sites, implementing hreflang via sitemap is often more maintainable than HTML tags (avoids editing every page template):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://example.com/page/</loc>
    <xhtml:link rel="alternate" hreflang="en" href="https://example.com/page/"/>
    <xhtml:link rel="alternate" hreflang="es" href="https://es.example.com/page/"/>
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/page/"/>
  </url>
</urlset>
```
**Requirement:** Every URL in the hreflang set must include the full set of `xhtml:link` alternates — including a self-referencing entry.

### News Sitemap (Google News)

For news publishers: articles MUST appear in the news sitemap within **2 minutes of publication** to be eligible for Google News.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:news="http://www.google.com/schemas/sitemap-news/0.9">
  <url>
    <loc>https://example.com/news/article-title</loc>
    <news:news>
      <news:publication>
        <news:name>Example News</news:name>
        <news:language>en</news:language>
      </news:publication>
      <news:publication_date>2026-04-02T10:30:00+00:00</news:publication_date>
      <news:title>Article Title Here</news:title>
    </news:news>
  </url>
</urlset>
```
- Only include articles published in the last 48 hours
- Maximum 1,000 URLs per news sitemap (separate from main sitemap)

### IndexNow + Sitemap — Complementary Strategy

Sitemaps and IndexNow serve different purposes — use both for maximum indexation speed:

| Mechanism | Purpose | Engines Supported |
|-----------|---------|------------------|
| XML Sitemap | URL discovery, structure | Google, Bing, all |
| IndexNow | Instant change notification | Bing, Yandex, Seznam (NOT Google) |
| Google Indexing API | Fast indexing for eligible content | Google only |

```
Optimal workflow on publish:
1. Add URL to sitemap (or update lastmod)
2. Submit IndexNow ping for Bing/Yandex
3. For Google: rely on sitemap + GSC inspection for time-sensitive content
```

### Dynamic Sitemap Generation

For large sites, never regenerate the full sitemap on every request:

```python
# Pattern: Database-backed sitemap with incremental generation
def generate_sitemap_page(page_num, page_size=5000):
    """Generate one sitemap file from DB, paginated."""
    urls = db.query("""
        SELECT url, updated_at FROM pages
        WHERE indexed = true
        ORDER BY updated_at DESC
        LIMIT %s OFFSET %s
    """, (page_size, page_num * page_size))

    return render_sitemap_xml(urls)

# Cache each page for 1 hour, rebuild on publish event
```

```nginx
# Nginx: cache generated sitemap responses
location /sitemap*.xml {
    proxy_pass http://app;
    proxy_cache sitemap_cache;
    proxy_cache_valid 200 1h;
    proxy_cache_use_stale updating;  # serve stale during rebuild
}
```

## Error Handling

- **URL unreachable**: Report the HTTP status code and suggest checking if the site is live
- **No sitemap found**: Check common locations (/sitemap.xml, /sitemap_index.xml, robots.txt reference) before reporting "not found"
- **Invalid XML format**: Report specific parsing errors with line numbers
- **Rate limiting detected**: Back off and report partial results with a note about retry timing

## Output

### For Analysis
- `VALIDATION-REPORT.md`: analysis results
- Issues list with severity
- Recommendations

### For Generation
- `sitemap.xml` (or split files with index)
- `STRUCTURE.md`: site architecture documentation
- URL count and organization summary
