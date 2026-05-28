---
name: seo-migrations
description: >
  SEO site migration planning and execution. Covers domain changes, HTTP to HTTPS,
  URL restructuring, CMS migrations, platform changes, redirect mapping, and
  post-migration recovery. Use when user says "site migration", "domain change",
  "URL restructure", "HTTPS migration", "CMS migration", "redirect map",
  "migrating to new platform", "website redesign SEO", or "301 redirects".
  Not for general IA planning or URL taxonomy design (without URL changes) — use seo-architecture.
user-invokable: true
argument-hint: "[current url and/or new url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# SEO Site Migration Guide

Site migrations are the highest-risk SEO operation. A botched migration can destroy years of ranking equity in days. This guide covers the full lifecycle: pre-migration audit → execution → validation → recovery.

---

## Migration Types & Risk Level

| Migration Type | Risk Level | Typical Traffic Impact | Recovery Time |
|---------------|-----------|----------------------|--------------|
| HTTP → HTTPS (same domain) | Low | 0-5% temporary dip | 1-4 weeks |
| URL restructure (same domain) | Medium | 10-30% dip | 4-12 weeks |
| Domain change (old → new domain) | High | 20-50% dip | 3-6 months |
| CMS/Platform change (same URLs) | Medium | 5-20% dip | 4-8 weeks |
| CMS + URL restructure | Very High | 30-60% dip | 6-12 months |
| Full rebuild (new domain + new URLs) | Critical | 50-80% dip | 4-12 months (well-executed) / 12-18+ months (poor redirect mapping or AI citation loss) |

**Rule:** Never combine multiple migration types unless absolutely necessary. Each migration compounds risk.

---

## Phase 1: Pre-Migration Audit (Non-Negotiable)

Do this BEFORE touching anything. You need a complete baseline to measure against post-migration.

### 1.1 URL Inventory

Crawl the entire current site and export all URLs:
```bash
# Using wget for a basic crawl (free)
wget --spider -r --no-verbose -o crawl.log https://old-site.com
grep -E "^--" crawl.log | grep -v "^-- " | awk '{ print $3 }' > all-urls.txt

# Or use Screaming Frog (most reliable)
# Export: All URLs → 200 status codes only → CSV
```

**Export and save:**
- All URLs returning 200 (live pages)
- All URLs returning 301/302 (existing redirects)
- All URLs returning 404 (broken pages)
- All canonical URLs
- All URLs in XML sitemap

### 1.2 Performance Baseline

Capture BEFORE migration begins. You'll need this to diagnose post-migration drops.

**From Google Search Console:**
```
Performance → Download last 12 months of:
- Total clicks by URL
- Total impressions by URL
- Average position by URL
- Keywords driving traffic to each page
```

**From GA4:**
```
Pages and screens → Last 90 days:
- Sessions by landing page
- Conversions by landing page
- Bounce/engagement rate by page
```

**From rank tracker:**
- Export current rankings for all tracked keywords
- Note SERP features owned (snippets, PAA)

### 1.3 Backlink Audit

Export all backlinks to the current domain — these need to be preserved.
```bash
# Ahrefs: Site Explorer → Backlinks → Export all
# SEMrush: Backlinks → Export
# Google Search Console: Links → External links → Download
```

**Priority backlinks (highest DR, most traffic referral) must be 301-redirected to equivalent new URLs.**

### 1.4 Technical Baseline
```bash
# Record current Core Web Vitals
# PageSpeed Insights → run for homepage + 5 key pages
# Save: LCP, INP, CLS for mobile and desktop

# Record robots.txt
curl -sL https://old-site.com/robots.txt > old-robots.txt

# Record XML sitemap
curl -sL https://old-site.com/sitemap.xml > old-sitemap.xml

# Record canonical tags for top 50 pages
for url in $(head -50 all-urls.txt); do
  echo "$url: $(curl -sL $url | grep -i 'rel="canonical"')"
done
```

---

## Phase 2: Redirect Mapping

The most critical deliverable for any migration. A missing redirect = dead link = lost ranking equity.

### 2.1 Redirect Map Structure

Create a spreadsheet with these columns:
| Old URL | New URL | HTTP Status | Notes | Priority |
|---------|---------|-------------|-------|---------|
| /old-page | /new-page | 301 | Exact equivalent | High |
| /blog/post-1 | /insights/post-1 | 301 | URL structure change | High |
| /deleted-page | /most-relevant-page | 301 | No equivalent | Medium |

### 2.2 Mapping Strategy

**1:1 redirects (best):** Old URL maps exactly to equivalent new URL
```
/services/seo → /seo-services
/blog/2024/post-1 → /blog/post-1
```

**Category-level redirects (acceptable):** When individual pages are consolidated
```
/blog/category/seo/* → /seo-resources/
```

**Homepage fallback (last resort):** Only for truly deleted content with no equivalent
```
/old-product-discontinued → / (homepage)
```

**Avoid redirect chains — ALWAYS:**
```
❌ CHAIN: /old → /intermediate → /new  (crawl waste: each hop costs budget; Googlebot stops after 5-10 hops)
✅ DIRECT: /old → /new               (1 hop = efficient)

Note: Google has confirmed that 3xx redirects do NOT lose PageRank per hop. Redirect chains are harmful for crawl budget efficiency and user TTFB — not for direct link equity loss.
```

### 2.3 Priority Tier for Redirect Implementation

**Tier 1 (Critical — do first):**
- Homepage
- Top 50 pages by organic traffic (from GSC export)
- Top 50 pages by backlinks (from backlink export)
- All pages currently ranking in top 10

**Tier 2 (High — do before launch):**
- All remaining pages in XML sitemap
- All pages receiving backlinks (any DR)
- Product/service pages

**Tier 3 (Standard):**
- All other 200-status pages

**Do not redirect:**
- Already-404 pages (unless they have backlinks)
- Paginated pages (/page/2, /page/3)
- Temporary URLs, staging URLs

---

## Phase 3: HTTP → HTTPS Migration (Specific)

Most common migration. Treated as low risk but commonly botched.

### Checklist

**Server setup:**
- [ ] SSL certificate installed and valid (check expiry date)
- [ ] Certificate covers www and non-www versions
- [ ] Wildcard cert for subdomains (if applicable)
- [ ] HSTS header enabled: `Strict-Transport-Security: max-age=31536000; includeSubDomains`

**Redirect implementation:**
```nginx
# Nginx: Redirect HTTP → HTTPS
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$host$request_uri;
}
```
```apache
# Apache: .htaccess
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

**Mixed content fix:**
After HTTPS migration, scan for HTTP references:
```bash
curl -sL https://example.com | grep -i 'http://' | grep -v 'https://'
# Fix: update all hardcoded HTTP:// references in content and CMS
```

**CMS settings:**
- [ ] WordPress: Update Site URL and WordPress URL to https://
- [ ] Update all internal links (use Search & Replace in DB or plugin)
- [ ] Update canonical tags to https://
- [ ] Update sitemap to https://
- [ ] Update robots.txt sitemap reference to https://

**GSC verification:**
- Add https:// property (separate from http:// property)
- Set https:// as preferred domain
- Submit https:// sitemap
- Monitor for crawl errors

**Analytics:**
- Update GA4 data stream URL to https://
- Update any goal/conversion URLs to https://

---

## Phase 4: Domain Migration (Specific)

Highest-risk migration. Google must re-evaluate the new domain from scratch, using link equity passed via 301s.

### Pre-Launch

**DNS preparation:**
- Lower TTL to 300 seconds (5 min) 48 hours before migration
- This allows faster DNS propagation during cutover

**Staging validation:**
- Full new site built on staging environment
- ALL redirects tested and working
- No 404s for Tier 1 and Tier 2 URLs
- Canonical tags pointing to new domain
- robots.txt NOT blocking anything (check staging robots first!)

**Common staging mistake:**
```
# Staging robots.txt (WRONG if accidentally pushed to prod):
User-agent: *
Disallow: /

# Must verify production robots.txt after launch:
curl -sL https://new-domain.com/robots.txt
```

### Launch Day Checklist
- [ ] DNS changed to point to new server
- [ ] Old domain 301-redirects all pages to new domain equivalents
- [ ] New domain accessible and loading correctly
- [ ] SSL valid on new domain
- [ ] robots.txt correct (nothing blocked)
- [ ] sitemap.xml updated with new domain URLs
- [ ] canonical tags updated to new domain
- [ ] GSC: Use Change of Address tool (Settings → Change of Address)
- [ ] Bing Webmaster Tools: Submit change of address
- [ ] Update Google Business Profile with new domain
- [ ] Update all backlinks you control (social profiles, directories)
- [ ] Notify high-DR backlink sources of domain change

### GSC Change of Address Tool
```
Google Search Console:
1. Verify both old and new domain in GSC
2. Old domain property → Settings → Change of Address
3. Select new property → Submit
This tells Google to transfer ranking signals from old to new domain
```

---

## Phase 5: CMS / Platform Migration

Moving from WordPress to Webflow, Shopify to WooCommerce, etc.

### URL Preservation Priority
**Best case:** Keep all URLs identical — zero redirect needed.
```
Old: mysite.com/blog/post-title
New: mysite.com/blog/post-title  ← same
```

**If URLs must change:** Follow full redirect mapping (Phase 2).

### Platform-Specific Gotchas

**WordPress → Other platform:**
- Yoast/RankMath SEO data (title, description, canonical) must be migrated
- Post slugs must match or be redirected
- Image URLs change if media library moves
- Custom post types need equivalent URL structures

**Shopify → WooCommerce (or vice versa):**
- Product URL structure typically changes (`/products/name` vs. `/shop/name`)
- Collection/category pages restructure
- Customer account pages differ

**Static site → Dynamic (or vice versa):**
- Check JavaScript rendering — static site → next.js may break indexing if SSR not configured
- Server-side rendering required for Google to index JS-rendered content reliably

---

## Phase 6: Post-Migration Validation

Run within 24-48 hours of launch.

### Immediate Checks (within 1 hour)
```bash
# Check homepage returns 200
curl -I https://new-site.com
# Expected: HTTP/2 200

# Check old domain redirects correctly
curl -I https://old-site.com
# Expected: HTTP/1.1 301 → Location: https://new-site.com/

# Check a sample of old URLs redirect correctly
curl -IL https://old-site.com/old-page
# Expected: 301 → 200 at new equivalent URL

# Check robots.txt is correct
curl -sL https://new-site.com/robots.txt

# Check sitemap accessible
curl -I https://new-site.com/sitemap.xml
# Expected: 200
```

### 48-Hour Checks
- [ ] GSC: Submit new sitemap
- [ ] GSC: Check Coverage for crawl errors
- [ ] GSC: URL Inspection on top 10 pages — verify indexed
- [ ] GA4: Verify traffic data flowing (not zero sessions)
- [ ] Check 404 spike in server logs or GSC Coverage
- [ ] Verify all canonical tags pointing to correct URLs
- [ ] Check structured data is preserved (Rich Results Test on key pages)
- [ ] Verify Core Web Vitals comparable to pre-migration baseline

### 2-Week Checks
- [ ] GSC Coverage: all Tier 1 pages indexed
- [ ] Rankings: compare to pre-migration baseline (expect 10-30% temporary drop)
- [ ] Traffic: compare week-over-week (expect dip, check recovery trend)
- [ ] 404 errors resolved (find in GSC → Pages → Not Found)
- [ ] Any ranking losses beyond expected? → diagnose immediately

### Monitoring Schedule
| Timeframe | What to Monitor | Acceptable Loss |
|-----------|----------------|----------------|
| Week 1 | Traffic, crawl errors | 10-20% dip normal |
| Weeks 2-4 | Rankings, indexation | Slow recovery expected |
| Month 2-3 | Traffic recovery | Should return to 80-90% |
| Month 4-6 | Full recovery | Should match or exceed baseline |
| Month 6+ | If still below baseline | Re-audit for issues |

---

## Phase 7: Post-Migration Recovery

If rankings drop more than expected or don't recover:

### Diagnosis Checklist
```bash
# 1. Check new URLs are indexed
# GSC → Coverage → Indexed

# 2. Check for accidental noindex tags
curl -sL https://new-site.com/page | grep -i 'noindex'

# 3. Check redirects are 301 (not 302)
curl -I https://old-site.com/page
# Look for: HTTP/1.1 301 (not 302)

# 4. Check canonical tags correct
curl -sL https://new-site.com/page | grep -i 'canonical'

# 5. Check no orphan pages (not linked from anywhere)
# Screaming Frog: Crawl → filter for inlinks = 0

# 6. Check for redirect chains
curl -IL https://old-url.com  # Follow all hops
```

### Common Post-Migration Fixes

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Pages not indexed | robots.txt blocking, noindex tag | Remove block |
| Rankings lost, redirects present | Redirect chains | Fix to 1-hop redirects |
| Traffic down 50%+ after HTTPS | Mixed content | Fix all HTTP:// references |
| Duplicate content | Canonical pointing to old domain | Update canonicals |
| GSC errors spike | Sitemap has old URLs | Regenerate + resubmit sitemap |
| Structured data lost | Schema not migrated | Re-implement schema on new CMS |

---

## Tools for Migration

### Crawling & Auditing
| Tool | Purpose | Pricing |
|------|---------|---------|
| Screaming Frog | Full site crawl, redirect audit, URL mapping | Free (500 URLs) + Paid |
| Sitebulb | Visual crawl + redirect map visualization | Paid |
| DeepCrawl / Lumar | Enterprise crawling + migration validation | Paid |
| Ahrefs Site Audit | Crawl + broken links + redirect detection | Paid |

### Redirect Testing
| Tool | Purpose | Pricing |
|------|---------|---------|
| Redirect Path (Chrome ext.) | Visual redirect chain checker | Free |
| httpstatus.io | Bulk URL redirect checker | Free |
| curl (command line) | Single URL redirect inspection | Free (built-in) |
| Link Redirect Trace (ext.) | Full redirect chain + HTTP headers | Free |

### Monitoring Post-Migration
| Tool | Purpose | Pricing |
|------|---------|---------|
| Google Search Console | Indexation, crawl errors, clicks | Free |
| GA4 | Traffic, conversions before/after | Free |
| Ahrefs Rank Tracker | Keyword ranking before/after | Paid |
| SERPWatcher | Daily rank tracking for recovery | Paid |
| Uptime Robot | Monitor new site availability | Free + Paid |

---

## Output Format

### Migration Plan Document

**Migration Type:** [Type]
**Old URL:** [domain/URL]
**New URL:** [domain/URL]
**Target Launch Date:** [date]
**Risk Level:** [Low/Medium/High/Critical]

#### Pre-Migration Baseline
| Metric | Value (Pre-Migration) |
|--------|----------------------|
| Pages indexed | X |
| Monthly organic traffic | X |
| Keywords in top 10 | X |
| Referring domains | X |
| Top 5 traffic pages | [list] |

#### Redirect Map Summary
| Tier | # URLs | Status |
|------|--------|--------|
| Tier 1 (Critical) | X | Complete/Pending |
| Tier 2 (High) | X | Complete/Pending |
| Tier 3 (Standard) | X | Complete/Pending |

#### Launch Day Checklist Progress
[Checklist items with ✅/❌ status]

#### Post-Migration Monitoring Log
| Date | Traffic | Rankings | Indexed Pages | Issues Found |
|------|---------|---------|--------------|-------------|
| Launch | X | X | X | [list] |
| Week 1 | X | X | X | [list] |
| Week 2 | X | X | X | [list] |
| Month 1 | X | X | X | [list] |

---

## A/B Testing — SEO Safe Implementation

A/B testing can accidentally destroy rankings if done incorrectly. Google explicitly addresses this in their documentation.

### Safe A/B Testing Methods

| Method | SEO Safety | How It Works |
|--------|-----------|-------------|
| **rel="canonical" pointing to original** | ✅ Safe | Variant pages canonical → original. Google attributes signals to original. |
| **302 redirect to variant** | ✅ Safe | Temporary redirect — Google understands it's a test, doesn't transfer PageRank permanently |
| **Server-side split** (same URL, different content) | ✅ Safe if not cloaking | Same URL serves variant to % of users. NOT cloaking if Googlebot sees the same. |
| **301 redirect to variant** | ❌ Dangerous | Permanent redirect — transfers rankings to variant permanently |
| **Cloaking** (different content for Googlebot vs users) | ❌ Manual action risk | Google penalizes sites that show different content to crawlers |
| **JavaScript-only testing** without server-side canonical | ⚠️ Risk | Googlebot may see original or variant inconsistently |

### Google's A/B Testing Rules (Official)
1. **Don't cloak** — if Googlebot visits during the test, it should see a version of the test (original or variant), not a special "bot-only" page
2. **Use rel=canonical** on variant pages pointing to the original URL
3. **Use 302s** (not 301s) for redirect-based tests
4. **End tests promptly** — don't leave tests running indefinitely; Google detects long-running tests as permanent changes
5. **Don't test noindex/nofollow** — changes to meta robots in tests confuse indexation

### Example: Safe A/B Test Setup
```html
<!-- Variant page (/landing-v2) → canonical to original -->
<link rel="canonical" href="https://example.com/landing" />

<!-- OR: use 302 redirect from variant to control during test -->
<!-- Redirect: /landing-v2 → 302 → /landing -->
```

### What Happens if Test Stays Live Too Long
- Google stops treating 302 as temporary → begins treating as 301
- Variant content starts competing with original for the same keyword
- Potential cannibalization between original and variant

### A/B Testing Tools — SEO Compatibility
| Tool | Safe Default? | Notes |
|------|--------------|-------|
| ~~Google Optimize (sunset 2023)~~ | — | Removed. Use alternatives below |
| **Statsig** | ✅ Safe | Feature flags + A/B, server-side |
| **PostHog** | ✅ Safe | Open-source, server-side experiments |
| **GrowthBook** | ✅ Safe | Open-source, SQL-based, no vendor lock-in |
| VWO | ✅ Safe | Supports canonical + 302 approach |
| Optimizely | ✅ Safe | Server-side testing available |
| AB Tasty | ✅ Safe | Supports server-side rendering |
| Custom JS split | ⚠️ Verify | Must not cloak; canonical must be correct |

---

## IndexNow for Faster Re-Indexing Post-Migration

IndexNow allows instant URL submission to Bing, Yandex, Seznam, and Naver (not Google as of 2026). For migrations, submit all new URLs via IndexNow alongside sitemap resubmission to accelerate re-indexing by up to 95% on supported engines.

```bash
# Bulk submit via IndexNow API
curl -X POST "https://api.indexnow.org/indexnow" \
  -H "Content-Type: application/json" \
  -d '{
    "host": "www.example.com",
    "key": "YOUR_KEY",
    "urlList": ["https://example.com/new-url-1", "https://example.com/new-url-2"]
  }'
```

**Migration-specific workflow:**
1. Day of go-live: submit ALL new URLs via IndexNow + resubmit sitemap in GSC
2. Check GSC URL Inspection for key pages within 48 hours
3. Monitor AI citation sources for stale old URLs appearing in AI Overviews/Perplexity

---

## Rollback Strategy

18% of migration projects require rolling back at least some components. Define rollback before go-live.

| Component | Rollback action | Timeline |
|-----------|----------------|----------|
| DNS | Revert A/CNAME to old server | Minutes (TTL-dependent; lower TTL to 300s before migration) |
| Redirects | Disable redirect rules at CDN/server level | Minutes |
| CDN cache | Purge all cached new URLs | Minutes |
| GSC Change of Address | Cancel via GSC UI (available up to 180 days post-submission) | Immediate |
| Sitemap | Resubmit old sitemap | Minutes |
| Internal links | Revert to pre-migration CMS snapshot | Hours |

**Pre-migration rollback checklist:**
- [ ] Old server kept live and accessible (just with DNS diverted)
- [ ] CDN configured for instant cache purge capability
- [ ] DNS TTL reduced to 300s at least 48h before migration
- [ ] Redirect rules stored in version control (not just server config)
- [ ] Database backup of old CMS state

---

## Related Skills

| Need | Skill |
|------|-------|
| Validate old vs new architecture, detect unauthorized changes | `/seo architecture audit [old-sitemap] [new-sitemap]` |
| Analyze internal link breakage after URL changes | `/seo internal-linking` |
| Technical audit of new site (crawlability, canonicals, indexation) | `/seo technical <url>` |
| Post-migration GSC performance comparison | `/seo google gsc <property>` |
| Validate hreflang if migration includes locale changes | `/seo hreflang <url>` |
