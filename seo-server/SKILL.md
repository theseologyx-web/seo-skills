---
name: seo-server
description: >
  Server infrastructure audit for SEO. Covers HTTP response headers (Cache-Control,
  X-Robots-Tag, Vary, ETag), TTFB optimization, hosting type and geo-targeting
  signals, IP reputation, compression (Gzip/Brotli), HTTP/2 vs HTTP/3, SSR vs CSR
  implications, CDN/WAF/proxy detection, and uptime impact on crawl frequency.
  Use when user says "server audit", "hosting SEO", "HTTP headers", "TTFB",
  "server configuration", "X-Robots-Tag", "server infrastructure", or "hosting analysis". Not for CDN-specific configuration — use seo-cdn. Not for Core Web Vitals optimization — use seo-performance.
user-invokable: true
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Server Infrastructure Audit for SEO

The server layer is often overlooked in SEO audits, but it directly affects crawlability, indexability, page speed, and geo-targeting signals. This skill covers everything between DNS resolution and HTML delivery.

---

## 1. Full HTTP Headers Audit

### Master Diagnostic Command
```bash
# Full header inspection with redirect following
curl -sIL https://example.com

# Filter for SEO-relevant headers
curl -sI https://example.com | grep -i -E '(cache-control|vary|etag|last-modified|x-robots-tag|content-type|content-encoding|server|via|x-cache|cf-ray|age|strict-transport|location|transfer-encoding|server-timing|alt-svc)'

# Check specific page (POST redirect, etc.)
curl -sI -X GET "https://example.com/page" -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
```

### Critical SEO Headers

#### Cache-Control
Controls how long content is cached by browsers and CDNs.
```
Cache-Control: public, max-age=3600         → Cache for 1 hour
Cache-Control: no-store                     → Never cache (Googlebot will always fetch fresh)
Cache-Control: no-cache                     → Always revalidate before serving
Cache-Control: private, max-age=0           → User-specific, don't cache on CDN
Cache-Control: public, s-maxage=86400       → CDN caches 24h, browser default
```
**SEO impact:** Crawlers respect cache headers. Very long `max-age` on HTML pages means Googlebot may not re-crawl after you make content updates.

#### X-Robots-Tag
HTTP header equivalent of `<meta name="robots">`. Critical for non-HTML files (PDFs, images).
```bash
curl -sI https://example.com/document.pdf | grep -i "x-robots-tag"
```
```
X-Robots-Tag: noindex                       → Don't index this resource
X-Robots-Tag: noindex, nofollow            → Don't index, don't follow links
X-Robots-Tag: none                          → Same as noindex, nofollow
X-Robots-Tag: noarchive                     → Don't show cached version
X-Robots-Tag: unavailable_after: 2025-12-31 → Remove from index after date
```
**Use case:** Block PDFs, internal documents, print pages from indexing without touching the HTML.

#### Vary
Tells caches which request headers affect the response.
```
Vary: Accept-Encoding   → Different cache per encoding (gzip vs brotli) ✅ Correct
Vary: User-Agent        → Different cache per user agent ⚠️ Problematic for CDNs
Vary: Accept-Language   → Different cache per language (use for i18n) ✅ Correct
Vary: Cookie            → Never cache for CDN (every user gets own version) ❌ Problematic
```

#### ETag and Last-Modified
Freshness signals that allow conditional requests (saves bandwidth).
```
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d"
Last-Modified: Thu, 01 Jan 2025 00:00:00 GMT
```
Googlebot uses these for efficient re-crawling. Sites without ETag/Last-Modified force Googlebot to download full content every time.

#### Content-Type + Charset
```
Content-Type: text/html; charset=UTF-8   → Correct for HTML ✅
Content-Type: text/html                  → Missing charset ⚠️ (browser guesses encoding)
```
Wrong charset → garbled characters in foreign languages → crawl and indexing issues.

#### Content-Encoding (Compression)
```
Content-Encoding: gzip    → Gzip compression active ✅
Content-Encoding: br      → Brotli compression active ✅ (better than gzip)
                           → No compression header = uncompressed ❌
```

---

## 2. Security Headers (SEO Context)

Security headers affect trust signals and occasionally crawlability:

```bash
curl -sI https://example.com | grep -i -E '(strict-transport|x-frame|x-content-type|content-security|referrer-policy|permissions-policy)'
```

| Header | Recommended Value | SEO Impact |
|--------|-----------------|-----------|
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` | HTTPS enforcement → Google ranking signal |
| `X-Content-Type-Options` | `nosniff` | Prevents MIME type confusion |
| `X-Frame-Options` | `SAMEORIGIN` | Prevents clickjacking (no direct SEO impact) |
| `Content-Security-Policy` | Configured per site | Too strict CSP can block inline JSON-LD schema |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Affects referral attribution in analytics |
| `Permissions-Policy` | Restrict unused features | Controls browser feature access — see below |
| `Cross-Origin-Opener-Policy` | `same-origin` | Required for SharedArrayBuffer (high-res timers) |
| `Cross-Origin-Embedder-Policy` | `require-corp` | Required alongside COOP for SharedArrayBuffer |
| `Server-Timing` | Per-request metrics | Exposes server-side timing to DevTools/RUM |

**Permissions-Policy** controls browser API access (geolocation, camera, microphone, payment). Restricting unused features reduces attack surface and signals security posture:
```
# Block features not used by your site
Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=()
```

**CSP and JSON-LD — nonce-based approach (recommended):**
```
If CSP has strict-dynamic or script-src rules,
inline <script type="application/ld+json"> may be blocked.
Test: Rich Results Test → check if schema is detected.

❌ Avoid: Add 'unsafe-inline' to script-src (weakens CSP)
✅ Prefer: Use nonce-based CSP
```
```nginx
# Nginx — generate nonce per request and inject into CSP header
# (Requires Nginx scripting or dynamic header generation)
add_header Content-Security-Policy "script-src 'nonce-$request_id' 'strict-dynamic'; object-src 'none';";
```
```html
<!-- In HTML: include nonce on JSON-LD script tag -->
<script type="application/ld+json" nonce="SAME_NONCE_AS_CSP_HEADER">
{ "@context": "https://schema.org", ... }
</script>
```

**Server header (version disclosure):**
```
Server: Apache/2.4.54 (Ubuntu)   ← Exposes version, security risk
Server: Apache                    ← Acceptable
Server: nginx                     ← Acceptable
```
Suppress version in server config:
```nginx
# Nginx
server_tokens off;

# Apache
ServerTokens Prod
```

---

## 3. TTFB (Time to First Byte) Analysis

TTFB is Google's primary server performance signal and directly affects LCP.

### Measure TTFB
```bash
# Basic TTFB measurement
curl -o /dev/null -s -w "TTFB: %{time_starttransfer}s\nDNS: %{time_namelookup}s\nConnect: %{time_connect}s\nTLS: %{time_appconnect}s\nTotal: %{time_total}s\n" https://example.com

# Multi-location TTFB (use WebPageTest or GTmetrix for this)
# From US: curl output
# From EU: different result if CDN geo-distribution is active
```

### TTFB Benchmarks

> **Lab vs field:** Lab tools (curl, Lighthouse, WebPageTest) measure from a single location and do not include DNS pre-resolution or TCP pre-connection that browsers cache. CrUX (field) TTFB includes full DNS + TCP + TLS + server response time from real users. Field TTFB is typically higher than lab — compare like-for-like.

| TTFB | Assessment | Impact on LCP |
|------|-----------|--------------|
| < 200ms | Excellent | LCP can achieve < 2.5s |
| 200-500ms | Good | LCP achievable with optimized assets |
| 500ms - 1s | Needs improvement | LCP likely 2.5-4s |
| > 1s | Poor | LCP likely > 4s, Core Web Vitals fail |

### TTFB Root Causes & Fixes

| Cause | Diagnosis | Fix |
|-------|-----------|-----|
| No CDN | High TTFB from distant locations | Add CDN (Cloudflare, Fastly, CloudFront) |
| Slow database queries | TTFB high, server CPU normal | Query optimization, database caching |
| No server-side caching | Every request hits application | Add Redis/Memcached, full-page cache |
| Shared hosting | Inconsistent TTFB, spikes | Upgrade to VPS/cloud |
| Unoptimized PHP/Python | High CPU, slow response | Code profiling, framework optimization |
| No PHP OPcache | Slow PHP execution | Enable OPcache |
| Large uncompressed responses | High transfer time | Enable Gzip/Brotli |
| DNS resolution slow | High `time_namelookup` | Change DNS provider (Cloudflare: 1.1.1.1, Google: 8.8.8.8) |

---

## 4. Hosting Type & Geo-Targeting Signal

### Hosting Location as SEO Signal

Server IP location is a geo-targeting signal, especially for:
- ccTLD sites (`.de` hosted in Germany → stronger German signal)
- Sites without clear country targeting in content

```bash
# Find hosting IP
dig +short example.com

# Identify hosting provider and location from IP
curl -s "https://ipinfo.io/[IP_ADDRESS]"
# Returns: city, region, country, org (hosting provider)
```

### Hosting Types: SEO Implications

| Hosting Type | TTFB | Uptime | SEO Risk |
|-------------|------|--------|---------|
| **Shared hosting** | High (100-800ms) | Variable | High — neighbor sites affect your performance; resource limits can cause 500 errors |
| **VPS (Virtual Private Server)** | Medium (50-200ms) | Good | Low — dedicated resources |
| **Dedicated server** | Low (20-100ms) | Good | Low |
| **Cloud (AWS/GCP/Azure)** | Low-Medium | Excellent | Low — auto-scaling prevents downtime |
| **Managed WordPress hosting** (Kinsta, WP Engine) | Low | Excellent | Very low — optimized for WordPress SEO |
| **Edge/Serverless** (Vercel, Netlify, Cloudflare Pages) | Very low | Excellent | Very low — global edge deployment |

### Uptime and Crawl Frequency

Googlebot adjusts crawl frequency based on server reliability:
- **Frequent 5xx errors** → Googlebot reduces crawl rate → slower indexation of new content
- **Consistent uptime** → Googlebot crawls more aggressively

```bash
# Test uptime with repeated requests
for i in {1..10}; do
  status=$(curl -o /dev/null -s -w "%{http_code}" https://example.com)
  echo "Request $i: $status"
  sleep 2
done
```

Recommended monitoring: StatusCake, UptimeRobot, Better Uptime (all have free tiers).

---

## 5. IP Reputation Check

If your server IP has been flagged for spam or abuse, it can affect:
- Email deliverability (indirect SEO impact)
- Access from corporate networks
- Some web filters blocking your domain

```bash
# Get your server IP
dig +short example.com

# Check against common blacklists
# Manual: check on MXToolbox → Blacklist Check
# Or use API: https://api.mxtoolbox.com/api/v1/lookup/blacklist/[IP]
```

### IP Reputation Tools
| Tool | Check |
|------|-------|
| MXToolbox Blacklist Check | Check IP against 100+ blacklists |
| Spamhaus | Most authoritative spam IP database |
| AbuseIPDB | Community abuse reports |
| Google Transparency Report | Malware/phishing check for domains |

---

## 6. Compression Configuration

### Enable Brotli (Better than Gzip)

**Brotli quality levels:** Use different levels for dynamic vs static assets:
- **Level 4–6:** Dynamic content (HTML, API responses) — good compression with low CPU overhead
- **Level 11:** Pre-compressed static assets (CSS, JS bundles) — maximum compression, CPU-intensive (run at build time, not on-the-fly)

```nginx
# Nginx — enable Brotli (requires ngx_brotli module)
brotli on;
brotli_comp_level 6;          # Use 4-6 for dynamic content
brotli_static on;              # Serve pre-compressed .br files for static assets (level 11)
brotli_types text/html text/css application/javascript application/json text/xml;

# Fallback to Gzip if Brotli not available
gzip on;
gzip_comp_level 5;
gzip_types text/html text/css application/javascript application/json text/xml application/xml;
```

```apache
# Apache — enable Brotli
LoadModule brotli_module modules/mod_brotli.so
BrotliCompressionQuality 6    # 4-6 for dynamic; pre-compress static at level 11
AddOutputFilterByType BROTLI_COMPRESS text/html text/css application/javascript

# Fallback Gzip
LoadModule deflate_module modules/mod_deflate.so
AddOutputFilterByType DEFLATE text/html text/css application/javascript
```

### Zstandard (zstd) — Emerging Compression

Chrome 123+ supports `zstd` (Zstandard) compression. Better compression ratios than Brotli at lower CPU cost, making it attractive for dynamic content:
```bash
# Check zstd support in browser Accept-Encoding
curl -sI -H "Accept-Encoding: zstd, br, gzip" https://example.com | grep -i "content-encoding"

# Nginx zstd (requires ngx_brotli equivalent module or Cloudflare automatic)
# Cloudflare enables zstd automatically for supported browsers since 2024
```
**Status:** Not yet widely deployed at origin level — Cloudflare handles it transparently at edge for most sites. Monitor adoption as server-side support matures.

### Test Compression
```bash
curl -sI -H "Accept-Encoding: br" https://example.com | grep -i "content-encoding"
# → content-encoding: br  (Brotli active ✅)

curl -sI -H "Accept-Encoding: gzip" https://example.com | grep -i "content-encoding"
# → content-encoding: gzip  (Gzip active ✅)
```

### Compression Benchmarks
| Content Type | Typical Compression Ratio |
|-------------|--------------------------|
| HTML | 70-80% reduction |
| CSS | 75-85% reduction |
| JavaScript | 60-75% reduction |
| JSON | 70-85% reduction |
| Images | 0% (already compressed) |

---

## 7. HTTP Protocol Version (HTTP/2 and HTTP/3)

### Check HTTP Version
```bash
curl -sI --http2 https://example.com | head -1
# HTTP/2 200 → HTTP/2 active ✅

curl -sI --http3 https://example.com | head -1
# HTTP/3 200 → HTTP/3 active ✅

# Verify HTTP/3 alt-svc advertisement
curl -sI https://example.com | grep -i "alt-svc"
# alt-svc: h3=":443"; ma=86400  → HTTP/3 available on port 443
```

### Protocol Comparison

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 (QUIC) |
|---------|---------|--------|--------------|
| Multiplexing | No (1 req/connection) | Yes (HPACK) | Yes (independent streams) |
| Head-of-line blocking | Yes | Yes (at TCP level) | **No** (per-stream) |
| Header compression | No | HPACK | QPACK |
| Server push | No | ~~Yes~~ (deprecated, Chrome 106+) | No |
| Transport layer | TCP | TCP | **UDP (QUIC)** |
| 0-RTT connection | No | No | **Yes** |
| Connection migration | No | No | **Yes** (WiFi → cellular) |
| HTTPS requirement | Optional | Effectively required | Required |
| Global adoption (2026) | Legacy | ~65% | ~35% |

**HTTP/2 is now standard.** If your site is on HTTP/1.1, that's a performance and crawl efficiency issue.

### HTTP/3 / QUIC — What It Means for SEO

HTTP/3 runs on QUIC (UDP-based transport) and is now mainstream, not "cutting edge":
- **35% global adoption** across all major CDNs (Cloudflare, Fastly, CloudFront, Akamai)
- **25% faster average downloads**, 52% faster on unstable mobile networks
- **0-RTT connection establishment** — QUIC skips TCP+TLS handshake on return visits
- **No head-of-line blocking** — a lost packet stalls only its stream, not the full connection
- **Connection migration** — device switching from WiFi to cellular reconnects instantly

**Googlebot and HTTP/3:** Googlebot does not currently use HTTP/3. The benefit is exclusively for real users / CrUX field data (not lab tools). HTTP/3 support still signals modern infrastructure.

### HTTP/2 Server Push — Deprecated
Server Push (`PUSH_PROMISE`) was listed as an HTTP/2 feature but was **removed in Chrome 106 (2022)**. It is effectively dead:
- Chrome, Firefox, and Safari no longer support it
- **Replacement:** Use Early Hints (HTTP 103) to achieve the same pre-loading benefit — see Section 12.

### Enable HTTP/2
```nginx
# Nginx — requires OpenSSL 1.0.2+
listen 443 ssl http2;
```
```apache
# Apache — requires mod_http2
Protocols h2 http/1.1
```

### Enable HTTP/3
```nginx
# Nginx with QUIC support (nginx 1.25+)
listen 443 quic reuseport;
listen 443 ssl;
http2 on;
ssl_protocols TLSv1.3;
add_header Alt-Svc 'h3=":443"; ma=86400';
```
```
# Cloudflare: Settings → Speed → Protocols → HTTP/3 (QUIC) → On
# (Enabled by default on all Cloudflare plans since 2020)
```

### Diagnostic: Which Protocol Is Active?
```bash
# Verbose check shows negotiated protocol
curl -v --http3 https://example.com 2>&1 | grep -E "HTTP/[123]|ALPN|h3"

# Alt-Svc header tells browser to use HTTP/3 next visit
curl -sI https://example.com | grep -i alt-svc
```

---

## 8. Server-Side vs. Client-Side Rendering

### The SEO Problem with CSR (Client-Side Rendering)
```
Google's rendering process:
1. Fetch HTML → immediately indexes visible text
2. Queue for rendering → renders JavaScript (can take days to weeks for large sites)
3. Index rendered content

With CSR (React/Vue/Angular default):
- Initial HTML is nearly empty: <div id="app"></div>
- All content loaded via JavaScript
- Googlebot must wait for rendering queue

Result: Content takes longer to index, may not be indexed if rendering fails.
```

### Rendering Options

| Method | How It Works | SEO Status |
|--------|-------------|-----------|
| **SSR (Server-Side Rendering)** | HTML generated on server, fully rendered | ✅ Best for SEO |
| **SSG (Static Site Generation)** | Pre-built HTML files | ✅ Best for SEO |
| **ISR (Incremental Static Regeneration)** | SSG with periodic regeneration (Next.js) | ✅ Excellent |
| **CSR (Client-Side Rendering)** | JS renders in browser | ⚠️ Risk — depends on JS execution |
| **Pre-rendering** | Static HTML generated at build time | ✅ Good |
| **Dynamic rendering** | SSR for bots, CSR for users | ✅ Acceptable but complex |

### Detect Rendering Method
```bash
# If initial HTML has content → SSR or SSG ✅
curl -sL https://example.com | grep -i '<h1'
curl -sL https://example.com | wc -w  # word count in raw HTML

# If initial HTML is nearly empty → CSR ⚠️
# Compare raw HTML word count vs. browser-rendered word count
# Use Google's Rich Results Test to see what Googlebot renders
```

---

## 9. CDN / WAF / Proxy Detection from Headers

```bash
curl -sI https://example.com | grep -i -E '(server|via|x-cache|cf-ray|x-powered-by|x-varnish|x-served-by|x-amz|x-sucuri|x-nf|x-vercel)'
```

### What Each Header Reveals
| Header | Reveals |
|--------|---------|
| `Server: cloudflare` | Cloudflare CDN/proxy |
| `CF-Ray: *` | Behind Cloudflare |
| `X-Served-By: cache-*` | Behind Fastly |
| `Via: 1.1 *.cloudfront.net` | Behind AWS CloudFront |
| `X-Sucuri-ID: *` | Behind Sucuri WAF |
| `X-Powered-By: PHP/8.1` | PHP version exposed (security risk) |
| `X-Powered-By: Express` | Node.js/Express backend |
| `X-Varnish: *` | Varnish cache active |
| `Age: 3600` | Response from cache (seconds old) |

### Suppress Sensitive Headers
```nginx
# Nginx — hide PHP version and other sensitive headers
proxy_hide_header X-Powered-By;
more_clear_headers Server;
```
```php
# PHP — suppress X-Powered-By
header_remove("X-Powered-By");
# OR in php.ini:
expose_php = Off
```

---

## 10. Complete Server SEO Audit Checklist

```bash
DOMAIN="https://example.com"

echo "=== 1. BASIC RESPONSE ==="
curl -o /dev/null -s -w "Status: %{http_code}\nTTFB: %{time_starttransfer}s\nTotal: %{time_total}s\n" $DOMAIN

echo "=== 2. HTTPS REDIRECT ==="
curl -sI http://$(echo $DOMAIN | sed 's|https://||') | grep -i "location"

echo "=== 3. CACHE HEADERS ==="
curl -sI $DOMAIN | grep -i -E '(cache-control|etag|last-modified|age)'

echo "=== 4. COMPRESSION ==="
curl -sI -H "Accept-Encoding: br, gzip" $DOMAIN | grep -i "content-encoding"

echo "=== 5. HTTP VERSION ==="
curl -sI --http2 $DOMAIN | head -1

echo "=== 6. SECURITY HEADERS ==="
curl -sI $DOMAIN | grep -i -E '(strict-transport|x-frame|x-content-type|content-security|referrer)'

echo "=== 7. X-ROBOTS-TAG ==="
curl -sI $DOMAIN | grep -i "x-robots-tag"

echo "=== 8. CDN DETECTION ==="
curl -sI $DOMAIN | grep -i -E '(server|via|cf-ray|x-cache|x-served-by|x-amz)'

echo "=== 9. VARY HEADER ==="
curl -sI $DOMAIN | grep -i "vary"

echo "=== 10. SENSITIVE HEADERS ==="
curl -sI $DOMAIN | grep -i "x-powered-by"
```

---

## 11. Tools for Server Auditing

| Tool | Purpose | Pricing |
|------|---------|---------|
| **SecurityHeaders.com** | Full HTTP header grade (A-F) | Free |
| **Mozilla Observatory** | Security header audit | Free |
| **GTmetrix** | TTFB + waterfall + server config | Free + Paid |
| **WebPageTest** | Multi-location TTFB, HTTP version detection | Free |
| **Pingdom** | Global TTFB monitoring + uptime | Free + Paid |
| **StatusCake** | Uptime monitoring + TTFB alerts | Free + Paid |
| **UptimeRobot** | Free uptime monitoring (5-min intervals) | Free |
| **Better Uptime** | Status page + uptime monitoring | Free + Paid |
| **MXToolbox** | IP blacklist check, DNS audit | Free |
| **Shodan** | Server/port exposure audit | Free + Paid |
| **KeyCDN HTTP/2 Test** | Check HTTP version support | Free |

---

## 12. Early Hints (HTTP 103)

Early Hints is an HTTP informational response (103 status) that lets the server send `preload` and `preconnect` headers to the browser **before** the final response is ready. The browser can start loading critical resources during server think time, reducing effective LCP.

```
HTTP/1.1 103 Early Hints
Link: </styles.css>; rel=preload; as=style
Link: <https://fonts.googleapis.com>; rel=preconnect

HTTP/1.1 200 OK
Content-Type: text/html
...
```

**Key facts:**
- Browser support: ~89% globally (Chrome, Firefox, Safari)
- **Googlebot does NOT process Early Hints** — benefit is purely for real users / CrUX field data, not lab tools
- Early Hints replaces HTTP/2 Server Push as the recommended pre-loading mechanism
- Cloudflare and Akamai support Early Hints at CDN level (no origin changes needed)

### Enable Early Hints

```nginx
# Nginx 1.25.1+ (official support added 2023)
location / {
    add_header Link "</style.css>; rel=preload; as=style" always;
    add_header Link "<https://fonts.googleapis.com>; rel=preconnect" always;
    # Nginx sends 103 automatically when using HTTP/2+
    proxy_pass http://backend;
}
```

```apache
# Apache (mod_http2 required)
# Via mod_headers in .htaccess or VirtualHost:
Header add Early-Hint "Link: </style.css>; rel=preload; as=style"
```

```
# Cloudflare: Speed → Optimization → Early Hints → On
# (Free plan and above, sends hints automatically from cache)
```

### Verify Early Hints
```bash
# Check if server sends 103 response
curl -v --http2 https://example.com 2>&1 | grep -A5 "< HTTP/2 103"

# Lighthouse now measures Early Hints impact on LCP
# Chrome DevTools → Network → filter by "103" status
```

### Server-Timing Header

`Server-Timing` exposes server-side performance metrics to browser DevTools and Real User Monitoring tools — useful for diagnosing TTFB breakdown (DB query time, cache hit, render time):

```nginx
# Nginx — add timing metrics to response header
add_header Server-Timing 'db;dur=12.5, cache;desc="HIT";dur=0.5, total;dur=45';
```
```
# Resulting header:
Server-Timing: db;dur=12.5, cache;desc="HIT";dur=0.5, total;dur=45

# Visible in Chrome DevTools → Network → [request] → Timing tab
# Also accessible via PerformanceServerTiming API for RUM
```

---

## 13. Edge Computing for SEO

Edge computing runs server logic at CDN PoPs (300+ global locations) instead of a single origin, delivering sub-50ms TTFB from anywhere in the world.

### Key Platforms
| Platform | Runtime | Locations | Cold Start |
|---------|---------|-----------|-----------|
| Cloudflare Workers | V8 Isolates | 300+ | ~0ms (always warm) |
| Vercel Edge Functions | V8 Isolates | ~80 | ~0ms |
| Deno Deploy | V8 Isolates | ~35 | ~0ms |
| AWS Lambda@Edge | Node.js | ~450 | ~100ms (containers) |
| Fastly Compute | Wasm | ~80 | <1ms |

**V8 Isolates (Cloudflare, Vercel, Deno)** have no cold starts and ~0ms startup — always preferred over container-based edge functions for latency-sensitive use cases.

### Edge SEO Use Cases

**Redirect management at edge:**
```javascript
// Cloudflare Worker — handle redirects without hitting origin
export default {
  async fetch(request) {
    const url = new URL(request.url);
    const redirects = {
      '/old-page': '/new-page',
      '/legacy/url': '/current/url',
    };
    if (redirects[url.pathname]) {
      return Response.redirect(redirects[url.pathname], 301);
    }
    return fetch(request);
  }
};
```

**Edge-side rendering (ESR):** Pre-render the static shell at edge (cached), stream dynamic personalization from origin:
- Static header/footer/nav: served from edge cache (0ms latency)
- Dynamic content (prices, user data): streamed from origin

**A/B testing at edge:** Run experiments without JS flicker (no layout shift, invisible to crawlers):
```javascript
// Cloudflare Worker — server-side A/B without client JS
const variant = Math.random() < 0.5 ? 'control' : 'test';
const response = await fetch(`${request.url}?variant=${variant}`);
```

**Geo-based content at edge:** Serve country-specific content (pricing, language) without origin round-trip.

### SEO Considerations
- **Googlebot sees edge-rendered output** — ensure rendered HTML is complete and canonical
- **Crawl budget:** Edge reduces TTFB for Googlebot; more pages crawled per session
- **Hreflang at edge:** Can inject or rewrite hreflang tags per geo without touching origin code
- **Edge caching vs. CDN caching:** Edge functions run on every request by default — cache aggressively at edge to avoid origin calls

---

## Output Format

### Server Infrastructure SEO Audit

**Site:** [domain]
**Hosting Provider:** [detected from IP or headers]
**CDN/Proxy:** [detected or None]
**Analysis Date:** [date]

#### Response Headers Summary
```
HTTP Version: [1.1 / 2 / 3]
TTFB: [ms]
Compression: [gzip / brotli / none]
Cache-Control: [value]
Vary: [value]
ETag: [present / absent]
X-Robots-Tag: [value or absent]
Security Headers Grade: [A-F from SecurityHeaders.com]
```

#### Issues Found
| Header / Setting | Current Value | Issue | Fix | Priority |
|-----------------|--------------|-------|-----|---------|
| Cache-Control | [value] | [issue] | [fix] | 🔴/🟡/🟢 |
| Compression | None | Not compressed | Enable Brotli/Gzip | 🔴 |
| HTTP version | HTTP/1.1 | Outdated protocol | Enable HTTP/2 | 🟡 |

#### TTFB Analysis
| Location | TTFB | Assessment |
|----------|------|-----------|
| Local | Xms | ✅/⚠️/❌ |
| Remote (estimate) | Xms | ✅/⚠️/❌ |

#### Rendering Method
**Detected:** [SSR / SSG / CSR / Unknown]
**Assessment:** [OK / Risk — content may not be indexed]

#### Priority Fixes
1. [Most critical fix]
2. [Second fix]
3. [Third fix]
