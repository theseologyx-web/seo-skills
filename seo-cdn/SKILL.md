---
name: seo-cdn
description: >
  Utility técnica de CDN: configuración de caché, headers, redirects y rendimiento
  específico de CDN (Cloudflare, CloudFront, Fastly, Akamai). No es auditoría SEO
  general — se activa solo cuando el CDN es el factor limitante específico. Para
  auditoría de servidor en general usar seo-server. Para CWV y LCP usar
  seo-performance. Se invoca desde seo-performance o seo-server cuando el CDN
  es la causa del problema. Use cuando diga "Cloudflare SEO", "caché headers",
  "CDN redirects", "edge caching", "CDN SEO" o "Rocket Loader".
user-invokable: true
argument-hint: "[url or CDN provider name]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# CDN Configuration & Analysis for SEO

> **Nota de arquitectura:** Este skill cubre un caso muy específico — configuración CDN con impacto SEO. Se activa solo cuando hay problemas de caché, headers o rendimiento atribuibles al CDN. Para análisis de servidor en general usar `seo-server`. Para Core Web Vitals y LCP usar `seo-performance`.

CDNs (Content Delivery Networks) sit between users (and crawlers) and your origin server. Misconfigured CDNs are a silent ranking killer — they can cache wrong responses, break redirects, block bots, or strip critical headers.

---

## 1. How CDNs Affect SEO

### Positive Impacts
- **Faster TTFB**: Content served from edge node nearest the user/bot → directly improves LCP
- **Better availability**: Absorbs traffic spikes, reduces 5xx errors that hurt crawl reliability
- **Global performance**: Consistent speed signals for international targeting
- **DDoS protection**: Keeps site online, which keeps Googlebot happy

### Negative Risks (When Misconfigured)
- **Caching noindex pages**: CDN serves cached 200 after you add noindex → Googlebot caches wrong version
- **Caching redirect responses**: 301 cached by CDN → prevents you from changing it quickly
- **Stripping security headers**: CDN removes X-Robots-Tag, breaking non-HTML noindex signals
- **Breaking canonicals**: CDN modifies URLs in HTML (e.g., image src rewriting) → canonical mismatch
- **Rocket Loader / JS optimization**: Rewrites `<script>` tags async → breaks JS-rendered content for Googlebot
- **Vary header mishandling**: CDN ignores `Vary: Accept-Encoding` or `Vary: User-Agent` → serves wrong cached version

---

## 2. CDN Detection via Response Headers

```bash
# Full header inspection
curl -sI https://example.com

# Filter for CDN-relevant headers
curl -sI https://example.com | grep -i -E '(cf-ray|cf-cache|x-cache|x-served-by|x-fastly|via|x-amz|x-cdn|server|age|x-varnish)'
```

### CDN Fingerprints

| CDN | Identifying Headers |
|-----|-------------------|
| **Cloudflare** | `CF-Ray`, `CF-Cache-Status`, `Server: cloudflare` |
| **Fastly** | `X-Served-By`, `X-Cache`, `X-Cache-Hits`, `Via: 1.1 varnish` |
| **AWS CloudFront** | `X-Amz-Cf-Pop`, `X-Amz-Cf-Id`, `Via: 1.1 *.cloudfront.net` |
| **Akamai** | `X-Check-Cacheable`, `X-Akamai-Transformed`, `Server: AkamaiGHost` |
| **Varnish** | `Via: 1.1 varnish`, `X-Varnish` |
| **Sucuri** | `X-Sucuri-ID`, `Server: Sucuri/Cloudproxy` |
| **Netlify** | `X-Nf-Request-Id`, `Server: Netlify` |
| **Vercel** | `X-Vercel-Id`, `Server: Vercel` |

### Cache Status Values
```bash
# Cloudflare CF-Cache-Status meanings:
# HIT       → Served from CDN cache ✅
# MISS      → Not in cache, fetched from origin
# EXPIRED   → Was in cache, now stale, fetched from origin
# BYPASS    → Cache bypassed (Cloudflare rule or cookie)
# DYNAMIC   → Not cacheable (dynamic content)
# REVALIDATED → Conditional request, content unchanged

# Fastly X-Cache meanings:
# HIT       → Served from Fastly cache
# MISS      → Fetched from origin
```

---

## 3. Cache-Control Headers for SEO

### Critical Cache-Control Directives

| Directive | Meaning | SEO Impact |
|-----------|---------|-----------|
| `max-age=3600` | Cache for 1 hour | After 1h, fresh crawl possible |
| `s-maxage=86400` | CDN cache for 24h (overrides max-age for CDNs) | Controls CDN cache independently |
| `no-store` | Don't cache at all | Origin hit on every crawl |
| `no-cache` | Must revalidate before serving | Allows caching but always checks freshness |
| `private` | Only browser cache, not CDN | CDN will NOT cache |
| `public` | CDN and browser may cache | CDN WILL cache |
| `stale-while-revalidate=60` | Serve stale while fetching fresh in background | Smooth UX, no wait for revalidation |
| `stale-if-error=86400` | Serve stale if origin returns 5xx | **Critical for SEO**: prevents Googlebot from seeing 500/503 during outages |
| `must-revalidate` | Don't serve stale past max-age | Strict freshness |

### Recommended Cache-Control by Content Type

```
# HTML pages (SEO-critical — balance crawl freshness vs. performance)
Cache-Control: public, max-age=0, s-maxage=3600, must-revalidate

# Static assets (CSS, JS — long cache OK)
Cache-Control: public, max-age=31536000, immutable

# Images
Cache-Control: public, max-age=2592000

# API responses (don't cache)
Cache-Control: no-store, no-cache

# Pages with personalization (don't CDN-cache)
Cache-Control: private, max-age=0
```

### Check Current Cache Headers
```bash
curl -sI https://example.com/page | grep -i "cache-control"
curl -sI https://example.com/page | grep -i "age"
# "Age: 3600" means the response has been in CDN cache for 3600 seconds
```

---

## 4. Cloudflare SEO Configuration

### Cloudflare Settings That Affect SEO

**Caching → Cache Level:**
- `Standard` → Caches based on file extension
- `Ignore Query String` → Caches same page regardless of ?params → can cache wrong content!
- `Bypass Cache` → Never caches

**Speed → Rocket Loader:**
```
⚠️ WARNING: Rocket Loader rewrites <script> tags to load asynchronously.
If your site uses JS for critical content that Googlebot needs to index,
Rocket Loader can delay or break rendering.

Recommendation: Test with Rocket Loader OFF first, then enable carefully.
Check: Google's Mobile-Friendly Test or Rich Results Test before/after.
```

**Speed → Auto Minify:**
- Safe for SEO but can occasionally break inline JSON-LD schema
- Test: check `<script type="application/ld+json">` after Cloudflare processes it

**Speed → Polish (Image Optimization):**
- Converts images to WebP/AVIF → good for CWV (LCP improvement)
- Safe for SEO

**Security → Bot Fight Mode / Super Bot Fight Mode:**
```
⚠️ DANGER: Aggressive bot fight settings can block Googlebot.
Check: Firewall → Overview for blocked requests
Verify Googlebot is not being challenged: Security → Bot Analytics

Safe setting: "Verified Bots" should always be set to ALLOW
```

### Cloudflare Rules (Replaces Deprecated Page Rules)

> **Page Rules are deprecated.** Cloudflare is phasing out Page Rules in favor of a more granular Rules system. Use the new rules for all new configurations; migrate existing Page Rules progressively.

| Old (Page Rules) | New Equivalent | Where |
|-----------------|---------------|-------|
| Cache Level = Bypass | Cache Rules → Cache Eligibility: Bypass cache | Rules → Cache Rules |
| Always Use HTTPS | Configuration Rules → HTTPS rewrites | Rules → Configuration Rules |
| Forwarding URL 301 | Redirect Rules → Static redirect | Rules → Redirect Rules |
| Browser Cache TTL | Cache Rules → Browser TTL override | Rules → Cache Rules |

```
# Cache Rules — bypass cache for admin paths
Rule: example.com/wp-admin/*
Action: Bypass cache (origin always hit)

# Redirect Rules — www to non-www
Rule: www.example.com/*
Action: Static redirect 301 → https://example.com/${uri}

# Configuration Rules — force HTTPS
Rule: http://example.com/*
Action: SSL/TLS: Full (Strict) + Redirect to HTTPS
```

### Cloudflare Workers for SEO Redirects
```javascript
// Edge redirect — faster than origin redirects
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url)

  // Redirect /old-path to /new-path
  if (url.pathname === '/old-path') {
    return Response.redirect('https://example.com/new-path', 301)
  }

  return fetch(request)
}
```

### Cloudflare Cache Purging (After SEO Changes)
```bash
# Purge specific URL via API
curl -X POST "https://api.cloudflare.com/client/v4/zones/{zone_id}/purge_cache" \
  -H "Authorization: Bearer {api_token}" \
  -H "Content-Type: application/json" \
  --data '{"files":["https://example.com/changed-page"]}'

# Purge everything (use carefully — increases origin load)
curl -X POST "https://api.cloudflare.com/client/v4/zones/{zone_id}/purge_cache" \
  -H "Authorization: Bearer {api_token}" \
  -H "Content-Type: application/json" \
  --data '{"purge_everything":true}'
```

### Cache Purge Automation (Recommended Patterns)

**Webhook-triggered purge after CMS publish:**
```javascript
// WordPress hook → triggers Cloudflare purge on post save
add_action('save_post', function($post_id) {
  $url = get_permalink($post_id);
  wp_remote_post("https://api.cloudflare.com/client/v4/zones/{zone_id}/purge_cache", [
    'headers' => ['Authorization' => 'Bearer ' . CF_TOKEN],
    'body' => json_encode(['files' => [$url]])
  ]);
});
```

**CI/CD pipeline purge after deployment:**
```yaml
# GitHub Actions step — purge CDN after deploy
- name: Purge Cloudflare cache
  run: |
    curl -X POST "https://api.cloudflare.com/client/v4/zones/$CF_ZONE_ID/purge_cache" \
      -H "Authorization: Bearer $CF_API_TOKEN" \
      -H "Content-Type: application/json" \
      --data '{"purge_everything":true}'
```

**Tag-based purge (selective invalidation — Enterprise):**
```bash
# Tag HTML responses at origin with Cache-Tag header
Cache-Tag: product-123, category-shoes

# Purge all resources tagged with a specific tag
curl -X POST ".../purge_cache" --data '{"tags":["product-123"]}'
# Invalidates all cached responses for product 123 without full cache bust
```

---

## 5. AWS CloudFront SEO Configuration

### Behaviors for SEO
```
# Cache HTML pages with short TTL
- Path Pattern: Default (*)
- Viewer Protocol Policy: Redirect HTTP to HTTPS
- Cache Policy: Custom — TTL min=0, default=3600, max=86400
- Origin Request Policy: Forward all headers needed

# Never cache admin/API paths
- Path Pattern: /wp-admin/*
- Cache Policy: CachingDisabled
```

### Lambda@Edge for SEO Redirects
```javascript
// CloudFront redirect function (faster than Lambda@Edge for simple redirects)
function handler(event) {
  var request = event.request;
  var uri = request.uri;

  // Redirect /old to /new
  if (uri === '/old-path') {
    return {
      statusCode: 301,
      statusDescription: 'Moved Permanently',
      headers: {
        'location': { value: 'https://example.com/new-path' },
        'cache-control': { value: 'max-age=3600' }
      }
    };
  }

  return request;
}
```

### CloudFront Headers to Forward to Origin
For SEO, ensure CloudFront forwards:
- `Host` → critical for multi-tenant / virtual hosting
- `Accept-Encoding` → for Gzip/Brotli response
- `User-Agent` → if your origin does user-agent based rendering

---

## 6. Fastly / Varnish for SEO

### VCL Snippet for SEO Redirects
```vcl
sub vcl_recv {
  # Redirect /old to /new at edge (no origin hit)
  if (req.url == "/old-path") {
    set req.http.Location = "https://example.com/new-path";
    return(synth(301, "Moved Permanently"));
  }
}

sub vcl_synth {
  if (resp.status == 301) {
    set resp.http.Location = req.http.Location;
    return(deliver);
  }
}
```

### Fastly Vary Header Handling
```
# Fastly respects Vary: Accept-Encoding by default
# For mobile/desktop variants, use Vary: User-Agent cautiously
# Better: use responsive design instead of Vary: User-Agent
```

---

## 7. CDN and Core Web Vitals

### LCP (Largest Contentful Paint)
CDN is the most impactful lever for LCP:
- Edge caching reduces TTFB from 800ms → 50ms
- Target: TTFB < 200ms for LCP < 2.5s
```bash
# Measure TTFB from CDN vs. origin
curl -o /dev/null -s -w "TTFB: %{time_starttransfer}s\nTotal: %{time_total}s\n" https://example.com
```

### CLS (Cumulative Layout Shift)
CDN image optimization can cause CLS if:
- Image dimensions change after WebP conversion
- Lazy loading CSS injected by CDN (Cloudflare Polish)
Fix: always set explicit `width` and `height` on `<img>` tags.

### INP (Interaction to Next Paint)
CDN has indirect impact:
- Faster JS delivery → faster Time to Interactive
- Ensure JS files have long `max-age` and are served from edge

### HTTP/3 at CDN Level

All major CDNs support HTTP/3 (QUIC) — typically enabled with one toggle:

```
Cloudflare: Speed → Protocols → HTTP/3 (QUIC) → On (default: On since 2020)
CloudFront: Distribution → Edit → HTTP Versions → HTTP/2 and HTTP/3
Fastly: Contact Fastly support or enable via API (supported since 2023)
Akamai: Property Manager → HTTP/3 (requires Ion or higher plan)
```

```bash
# Verify HTTP/3 at CDN level
curl -sI https://example.com | grep -i "alt-svc"
# alt-svc: h3=":443"; ma=86400  → CDN advertising HTTP/3 ✅

curl --http3 -sI https://example.com | head -1
# HTTP/3 200  → HTTP/3 active ✅
```

### Early Hints (103) at CDN Level

Cloudflare and Akamai can send Early Hints from the edge without origin changes:

```
Cloudflare: Speed → Optimization → Early Hints → On
(Free plan and above. Cloudflare caches and replays `Link` preload headers as 103 from edge)
```

**SEO note:** Googlebot does NOT process Early Hints. Benefit is exclusively for real users / CrUX field LCP improvement.

### Image CDN Optimization (LCP Impact)

Dedicated image CDNs provide automatic format selection, responsive sizing, and quality tuning — directly reducing LCP for image-heavy pages:

| Provider | Automatic Format | Responsive Sizing | LCP Impact |
|----------|-----------------|------------------|-----------|
| **Cloudflare Images** | WebP, AVIF | `width`, `height` params | High |
| **Cloudinary** | WebP, AVIF, auto | `f_auto`, `q_auto`, `w_auto` | High |
| **imgix** | WebP, AVIF, auto | `auto=format`, `w=`, `dpr=` | High |
| **Fastly IO** | WebP, AVIF | Query params | High |

```html
<!-- Cloudinary: auto format + quality + responsive width -->
<img src="https://res.cloudinary.com/demo/image/upload/f_auto,q_auto,w_800/hero.jpg"
     width="800" height="400" alt="Hero image">

<!-- imgix: auto format, responsive, explicit dimensions -->
<img src="https://example.imgix.net/hero.jpg?auto=format,compress&w=800&h=400"
     width="800" height="400" alt="Hero image">
```

**Checklist for image CDN + SEO:**
- Always set explicit `width` and `height` on `<img>` to prevent CLS
- Use `loading="eager"` on LCP image — never lazy-load the LCP element
- Serve AVIF for Chrome/Firefox, WebP for Safari, JPEG as fallback (`<picture>` or CDN auto format)
- Cache hit ratio target for image CDN: > 95%

---

## 8. Common CDN SEO Problems & Fixes

| Problem | Symptom | Diagnosis | Fix |
|---------|---------|-----------|-----|
| CDN caching noindex page | Page indexed despite noindex | `curl -sI [URL]` shows `CF-Cache-Status: HIT`, page has noindex | Purge cache, set `Cache-Control: no-store` for dynamic pages |
| Redirect cached as temporary | 301 behaves like 302 | Check `Cache-Control` on redirect response | Add `Cache-Control: public, max-age=31536000` to 301 responses |
| Bot blocked by WAF | Googlebot can't crawl | Check WAF/firewall logs for blocked user agents | Whitelist verified bots IP ranges |
| Rocket Loader breaks schema | Rich results lost | Compare rendered HTML with JS off vs. on | Disable Rocket Loader or use `data-cfasync="false"` on schema scripts |
| CDN serving stale sitemap | GSC shows old URLs | `Age` header is high on sitemap.xml response | Purge sitemap cache after updates |
| Mixed content after CDN SSL | Browser blocks resources | Check for `http://` URLs in CDN-cached HTML | Enable CDN's automatic HTTPS rewrite |
| Vary header stripped | Mobile/desktop serve same cached version | Check response headers for `Vary` | Configure CDN to preserve and respect `Vary` header |

---

## 9. CDN Analysis Checklist

```bash
# 1. Detect CDN provider
curl -sI https://example.com | grep -i -E '(cf-ray|x-served-by|x-amz|via|server)'

# 2. Check cache status
curl -sI https://example.com | grep -i -E '(cf-cache-status|x-cache|age)'

# 3. Check cache-control headers
curl -sI https://example.com | grep -i "cache-control"

# 4. Check TTFB (CDN impact)
curl -o /dev/null -s -w "TTFB: %{time_starttransfer}s\n" https://example.com

# 5. Check if Googlebot is getting cached response
curl -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" -sI https://example.com

# 6. Check redirect behavior
curl -sI https://example.com/old-page | grep -i "location"

# 7. Check if X-Robots-Tag is preserved
curl -sI https://example.com/robots-test | grep -i "x-robots-tag"

# 8. Check compression
curl -sI -H "Accept-Encoding: gzip, br" https://example.com | grep -i "content-encoding"
```

---

## 10. Tools for CDN Analysis

| Tool | Purpose | Pricing |
|------|---------|---------|
| **KeyCDN Performance Test** | Test page speed from multiple CDN PoPs | Free |
| **CDNPerf** | CDN performance comparison across regions | Free |
| **Pingdom** | Global load time + TTFB from multiple locations | Free + Paid |
| **GTmetrix** | Waterfall chart showing CDN cache hits | Free + Paid |
| **WebPageTest** | Multi-location test, shows CDN headers in waterfall | Free |
| **Cloudflare Analytics** | Native CDN analytics, bot traffic, cache rate | Free (with CF) |
| **Fastly Real-Time Analytics** | Edge analytics, cache hit ratio | Free (with Fastly) |
| **SecurityHeaders.com** | Header audit including CDN-related headers | Free |

### CDN Analytics for SEO — What to Monitor

**Cache Hit Ratio targets:**
| Content Type | Target | SEO Implication |
|-------------|--------|----------------|
| Static assets (CSS/JS/images) | > 95% | LCP stability across crawl sessions |
| HTML pages | > 50% | Lower TTFB for Googlebot + real users |
| Sitemap.xml | > 80% | Consistent sitemap delivery; purge on update |

**How to check Cloudflare cache hit ratio:**
```
Cloudflare Dashboard → Analytics & Logs → Cache → Cache Hit Rate
→ Filter by content type (HTML vs. static assets)
→ Look for unexpectedly low HTML cache rate (< 30%): indicates Cache-Control issues

Security → Bot Analytics → Verified Bots
→ Verify Googlebot / Bingbot are listed as ALLOWED (not challenged/blocked)
```

**Geo cache coverage gaps:**
```bash
# Test TTFB from multiple regions (use WebPageTest or curl from VPS in target country)
# If TTFB > 500ms from target country → CDN may not have cached content in that PoP
# Fix: warm cache by visiting from that region, or check CDN's tiered caching config
```

---

## 11. Cloudflare AI Bot Management

Since **July 1, 2025**, every new Cloudflare domain blocks all known AI crawlers by default. This is a paradigm shift from opt-out to opt-in for AI crawling — sites that WANT AI crawler visibility must explicitly allow specific bots.

### Cloudflare AI Blocking Features

**AI Auditing (default block since July 2025):**
```
Cloudflare Dashboard → Security → Bots → AI Scrapers and Crawlers
→ Toggle per bot: Allow / Block / Challenge
→ Default for new domains: Block ALL AI bots
```

Known AI bots managed by Cloudflare (updated automatically as new bots emerge):
- `ClaudeBot`, `Claude-SearchBot`, `Claude-User` (Anthropic)
- `GPTBot`, `OAI-SearchBot`, `ChatGPT-User` (OpenAI)
- `PerplexityBot`, `Bytespider` (TikTok/ByteDance), `CCBot` (Common Crawl), `Diffbot`
- `Applebot-Extended` (Apple Intelligence), `Meta-ExternalAgent`, `Amazonbot`

**GEO strategy for AI bots:**
```
Sites wanting AI search visibility (Perplexity, ChatGPT browsing, Claude search):
→ Allow: Claude-SearchBot, OAI-SearchBot, PerplexityBot
→ Block: ClaudeBot, GPTBot, CCBot (training-only bots that don't drive traffic)

Sites that want to protect training data:
→ Block ALL AI bots (Cloudflare default for new domains)
→ Add robots.txt rules as belt-and-suspenders (see seo-robots)
```

### Cloudflare Managed robots.txt

Since 2025, Cloudflare can generate and serve a `robots.txt` file on behalf of site owners, specifically for AI bot management:
```
Cloudflare Dashboard → Crawlers → AI Crawlers → Manage robots.txt
→ Cloudflare generates a robots.txt blocking selected AI bots
→ Serves at yourdomain.com/robots.txt (overrides origin file if enabled)
⚠️ WARNING: Enabling this replaces your origin robots.txt — verify nothing else is overridden
```

### Cloudflare Content Signals Policy (TXT Records)

More granular than robots.txt — uses DNS TXT records to signal content permissions:
```dns
; DNS TXT record for content signals
_cf-signals.example.com  TXT  "search=allow ai-input=deny ai-train=deny"
```

| Signal | Meaning |
|--------|---------|
| `search=allow` | Allow search engine indexing |
| `ai-input=deny` | Block use of content as AI input |
| `ai-train=deny` | Block AI training on content |

**Note:** This is a Cloudflare-specific mechanism; not a web standard. Effectiveness depends on AI bots checking TXT records (compliance varies).

---

## 12. Edge SEO Patterns

Beyond caching, CDN edge functions enable advanced SEO patterns that would require origin changes in a traditional server setup.

### Speculation Rules Injection (No Origin Changes)

Inject Speculation Rules API headers at the edge to enable browser prerendering without touching the origin application:

```javascript
// Cloudflare Worker — inject Speculation Rules for known high-intent pages
export default {
  async fetch(request) {
    const response = await fetch(request);
    const newHeaders = new Headers(response.headers);

    // Inject Speculation Rules for product/category pages
    const url = new URL(request.url);
    if (url.pathname.startsWith('/products/') || url.pathname.startsWith('/category/')) {
      newHeaders.set('Speculation-Rules', '"/speculation-rules.json"');
    }

    return new Response(response.body, {
      status: response.status,
      headers: newHeaders
    });
  }
};
```

```json
// /speculation-rules.json — served from CDN
{
  "prerender": [{ "where": { "href_matches": "/products/*" }, "eagerness": "moderate" }],
  "prefetch": [{ "where": { "href_matches": "/*" }, "eagerness": "conservative" }]
}
```

**SEO benefit:** Near-zero navigation time → real user LCP improvement → CrUX field data improvement → Core Web Vitals ranking impact.

### hreflang Injection at Edge

Add or rewrite hreflang tags per geo without touching origin HTML:
```javascript
// Cloudflare Worker — inject hreflang based on CF-IPCountry header
export default {
  async fetch(request) {
    const country = request.headers.get('CF-IPCountry');
    const response = await fetch(request);

    // Only process HTML responses
    const contentType = response.headers.get('Content-Type') || '';
    if (!contentType.includes('text/html')) return response;

    const html = await response.text();
    const hreflang = `
      <link rel="alternate" hreflang="en" href="https://example.com${new URL(request.url).pathname}">
      <link rel="alternate" hreflang="es" href="https://es.example.com${new URL(request.url).pathname}">
      <link rel="alternate" hreflang="x-default" href="https://example.com${new URL(request.url).pathname}">
    `;
    const modified = html.replace('</head>', hreflang + '</head>');
    return new Response(modified, { headers: response.headers });
  }
};
```

### Edge-Side Rendering (ESR)

Serve cached static shell from edge, stream dynamic content from origin:
```javascript
// Cloudflare Worker — ESR pattern
export default {
  async fetch(request) {
    const url = new URL(request.url);

    // Static shell: serve from KV cache (instant, global)
    const cachedShell = await CACHE_KV.get(`shell:${url.pathname}`);
    if (cachedShell) {
      // Start streaming dynamic content from origin while returning shell
      const originFetch = fetch(request);
      // ... merge/stream approach depends on framework
    }
    return fetch(request);
  }
};
```

### Redirect Management at Edge (SEO Best Practice)

Maintain redirects at CDN level — no origin round-trip, faster for Googlebot:
```javascript
// Cloudflare Worker — load redirects from KV store (updatable without code deploy)
export default {
  async fetch(request) {
    const url = new URL(request.url);
    const destination = await REDIRECTS_KV.get(url.pathname);

    if (destination) {
      return Response.redirect(destination, 301);
    }
    return fetch(request);
  }
};
```

```bash
# Update redirects without code deployment
wrangler kv:key put --binding=REDIRECTS_KV "/old-url" "https://example.com/new-url"
```

---

## Output Format

### CDN SEO Audit

**Site:** [domain]
**CDN Detected:** [provider or None]
**Analysis Date:** [date]

#### CDN Detection Results
```
CF-Ray: [value or absent]
CF-Cache-Status: [HIT/MISS/BYPASS/etc. or absent]
Age: [seconds in cache]
Via: [value or absent]
Server: [value]
TTFB: [ms]
```

#### Cache-Control Configuration
| Resource Type | Current Header | Recommended | Status |
|--------------|----------------|-------------|--------|
| HTML pages | [header] | public, s-maxage=3600 | ✅/⚠️/❌ |
| Static assets | [header] | max-age=31536000, immutable | ✅/⚠️/❌ |
| Sitemaps | [header] | max-age=3600 | ✅/⚠️/❌ |

#### Issues Found
| Issue | Severity | Fix |
|-------|----------|-----|
| [issue] | 🔴/🟡/🟢 | [action] |

#### Recommendations
1. [Recommendation]
2. [Recommendation]
