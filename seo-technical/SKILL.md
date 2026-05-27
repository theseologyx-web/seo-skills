---
name: seo-technical
description: >
  Technical SEO audit across 9 categories: crawlability, indexability, security,
  URL structure, mobile, Core Web Vitals, structured data, JavaScript rendering,
  and IndexNow protocol. Use when user says "technical SEO", "crawl issues",
  "robots.txt", "Core Web Vitals", "site speed", or "security headers".
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Technical SEO Audit

> **Skill relacionado:** `seo-performance` cubre Core Web Vitals con profundidad (diagnóstico lab vs field, LCP/CLS/INP por elemento, optimización detallada). Este skill (`seo-technical`) hace un check superficial de CWV (pass/fail) dentro de la auditoría técnica general. Si los CWV fallan, delegar el análisis profundo a `seo-performance`.

## Categories

### 1. Crawlability
- robots.txt: exists, valid, not blocking important resources
- XML sitemap: exists, referenced in robots.txt, valid format
- Noindex tags: intentional vs accidental
- Crawl depth: important pages within 3 clicks of homepage
- JavaScript rendering: check if critical content requires JS execution
- Crawl budget: for large sites (>10k pages), efficiency matters

> **Deep dives:** For full robots.txt validation, generation, and audit workflows (including AI crawler management and CMS-specific patterns), use `seo-robots`. For crawl budget analysis using real server logs (Googlebot frequency, crawl wasters, redirect chains in logs), use `seo-logs`. For sitemap validation, generation, and coverage analysis, use `seo-sitemap`. For full-site URL discovery and crawling (requires Firecrawl MCP), use `seo-firecrawl`. To see exactly what Google reads from a page (Cloudflare-rendered HTML), use `seo-crawler`. For crawl depth optimization, orphan pages, and link equity distribution via internal linking, use `seo-internal-linking`.

#### AI Crawler Management

As of 2025-2026, AI companies actively crawl the web to train models and power AI search. Managing these crawlers via robots.txt is a critical technical SEO consideration.

**Known AI crawlers (updated 2026):**

| Crawler | Company | robots.txt token | Purpose |
|---------|---------|-----------------|---------|
| GPTBot | OpenAI | `GPTBot` | Model training |
| OAI-SearchBot | OpenAI | `OAI-SearchBot` | Search index (ChatGPT search results) |
| ChatGPT-User | OpenAI | `ChatGPT-User` | Real-time browsing (user-initiated) |
| ClaudeBot | Anthropic | `ClaudeBot` | Model training |
| Claude-SearchBot | Anthropic | `Claude-SearchBot` | Search indexing |
| Claude-User | Anthropic | `Claude-User` | User-initiated fetches |
| PerplexityBot | Perplexity | `PerplexityBot` | Search index + training |
| Bytespider | ByteDance | `Bytespider` | Model training |
| Google-Extended | Google | `Google-Extended` | Gemini training (NOT search) |
| Applebot-Extended | Apple | `Applebot-Extended` | Apple Intelligence training (separate from Applebot for Siri/Spotlight) |
| Meta-ExternalAgent | Meta | `Meta-ExternalAgent` | Meta AI training |
| Amazonbot | Amazon | `Amazonbot` | Alexa/Amazon AI |
| CCBot | Common Crawl | `CCBot` | Open dataset |

**Three-bot frameworks (2026):**
- **OpenAI:** `GPTBot` (training) / `OAI-SearchBot` (search index) / `ChatGPT-User` (browsing). Block GPTBot to stop training; block OAI-SearchBot to stop ChatGPT search index; ChatGPT-User may not always honor robots.txt.
- **Anthropic (Feb 2026):** `ClaudeBot` (training) / `Claude-SearchBot` (search) / `Claude-User` (user fetches). All three honor robots.txt.

**Key distinctions:**
- Blocking `Google-Extended` prevents Gemini training use but does NOT affect Google Search indexing or AI Overviews (those use `Googlebot`)
- Blocking `GPTBot` prevents OpenAI training but does NOT prevent ChatGPT from citing your content via browsing (`ChatGPT-User`) or search (`OAI-SearchBot`)
- ClaudeBot blocked by ~69% of sites, GPTBot by ~62% (2026 data)
- Perplexity has been found using undeclared crawlers with generic user-agent strings — robots.txt is advisory; WAF/Cloudflare is the only reliable enforcement
- Since July 2025, every new Cloudflare domain blocks all known AI crawlers by default (opt-in, not opt-out)
- Maintain an up-to-date list via knownagents.com (formerly Dark Visitors)

**Example, selective AI crawler blocking:**
```
# Allow search indexing, block AI training crawlers
User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: Bytespider
Disallow: /

# Allow all other crawlers (including Googlebot for search)
User-agent: *
Allow: /
```

**Recommendation:** Consider your AI visibility strategy before blocking. Being cited by AI systems drives brand awareness and referral traffic. Cross-reference the `seo-geo` skill for full AI visibility optimization.

### 2. Indexability
- Canonical tags: self-referencing, no conflicts with noindex
- Duplicate content: near-duplicates, parameter URLs, www vs non-www
- Thin content: pages below minimum word counts per type
- Pagination: rel=next/prev or load-more pattern
- Hreflang: correct for multi-language/multi-region sites → full implementation and validation: `/seo hreflang <url>`
- Index bloat: unnecessary pages consuming crawl budget

### 3. Security
- HTTPS: enforced, valid SSL certificate, no mixed content
- Security headers:
  - Content-Security-Policy (CSP)
  - Strict-Transport-Security (HSTS)
  - X-Frame-Options
  - X-Content-Type-Options
  - Referrer-Policy
- HSTS preload: check preload list inclusion for high-security sites

> **Deep dive:** For complete HTTP header audit (Cache-Control, X-Robots-Tag, Vary, ETag, compression, HTTP/2 vs HTTP/3, TTFB analysis, hosting types, SSR/CSR detection, and IP reputation), use `seo-server`.

### 4. URL Structure
- Clean URLs: descriptive, hyphenated, no query parameters for content
- Hierarchy: logical folder structure reflecting site architecture
- Redirects: no chains (max 1 hop), 301 for permanent moves
- URL length: flag >100 characters
- Trailing slashes: consistent usage

> **Deep dive:** For domain changes, HTTPS migrations, URL restructuring, CMS migrations, redirect mapping, and post-migration recovery, use `seo-migrations`.

### 5. Mobile Optimization
- Responsive design: viewport meta tag, responsive CSS
- Touch targets: minimum 48x48px with 8px spacing
- Font size: minimum 16px base
- No horizontal scroll
- Mobile-first indexing: Google indexes mobile version. **Mobile-first indexing is 100% complete as of July 5, 2024.** Google now crawls and indexes ALL websites exclusively with the mobile Googlebot user-agent.

#### max-image-preview — Required for Google Discover & Social
```html
<meta name="robots" content="max-image-preview:large" />
```
- Without this: Google shows small thumbnails in Discover (kills CTR)
- With this: full 1200px+ card in Discover and large previews in Search
- Values: `none` (no preview), `standard` (default small), `large` (recommended)
- Add to ALL pages with editorial content (blog, articles, guides)
- Compatible with other robots directives: `content="index, follow, max-image-preview:large"`

#### rel Link Attributes — Nofollow, Sponsored, UGC
Google uses these to understand link context. Apply correctly to avoid spam signals.

| Attribute | When to Use | Google Treatment |
|-----------|------------|-----------------|
| `rel="nofollow"` | Links you don't want to vouch for; general catch-all | Hint (not directive) — Google may still follow and pass PageRank |
| `rel="sponsored"` | Paid links, affiliate links, advertisements | Hint — Google identifies paid relationships |
| `rel="ugc"` | User-generated content (comments, forum posts) | Hint — Google understands these are not editorial endorsements |

```html
<!-- Affiliate link -->
<a href="/product" rel="sponsored">Buy now</a>

<!-- Comment section link -->
<a href="https://user-site.com" rel="ugc nofollow">User's site</a>

<!-- Link you don't endorse -->
<a href="https://example.com" rel="nofollow">Reference</a>
```

**Key rule (Google 2019+):** All three are treated as **hints**, not hard directives. Google may choose to follow and pass PageRank to nofollow links if it deems them valuable. The primary purpose is to communicate link intent, not to block PageRank 100%.

**Audit check:**
```bash
# Find all external links without rel attributes
curl -sL https://example.com | grep -o '<a [^>]*href="http[^"]*"[^>]*>' | grep -v 'rel='
# These external links have no rel attribute — review if they need nofollow/sponsored/ugc
```

> **Deep dive:** For image formats (WebP/AVIF), responsive images (`srcset`/`sizes`), lazy loading, and CLS prevention via explicit dimensions, use `seo-images`.

### 6. Core Web Vitals
- **LCP** (Largest Contentful Paint): target <2.5s
- **INP** (Interaction to Next Paint): target <200ms
  - INP replaced FID on March 12, 2024. FID was fully removed from all Chrome tools (CrUX API, PageSpeed Insights, Lighthouse) on September 9, 2024. Do NOT reference FID anywhere.
  - **INP two-level percentile:** INP takes the p98 of interactions during a single page visit (near-worst interaction), then CrUX reports the p75 of those per-visit INP values across all visits. This means CrUX INP = p75 of p98 values.
- **CLS** (Cumulative Layout Shift): target <0.1
- Evaluation uses 75th percentile of real user data (p75 across all page loads in 28-day window)
- Use PageSpeed Insights API or CrUX data if MCP available
- **Threshold verification:** Always verify thresholds against official web.dev/articles/vitals — some SEO blogs incorrectly claim tighter thresholds (LCP 2.0s, INP 150ms). As of April 2026, official thresholds remain LCP ≤2.5s, INP ≤200ms, CLS ≤0.1.
- **Bing note:** Bing's JS rendering lags significantly behind Google's. Sites relying on CSR may rank on Google but be invisible on Bing. Consider multi-engine impact.

> **Deep dives:** For CDN impact on LCP/INP (edge caching, TTFB reduction) and provider-specific configuration (Cloudflare, AWS CloudFront, Fastly, Varnish), use `seo-cdn`. For image-specific CLS prevention (explicit width/height, lazy loading, WebP/AVIF formats, image CDN), use `seo-images`.

### 7. Structured Data
- Detection: JSON-LD (preferred), Microdata, RDFa
- Validation against Google's supported types
- See seo-schema skill for full analysis

> **Deep dive:** For VideoObject schema, video sitemaps, and multi-platform video SEO (YouTube, TikTok, Google Video Search), use `seo-video`.

### 8. JavaScript Rendering — React / Angular / SPA Complete Audit

> **Deep dive:** To see exactly what Google reads after JS execution (Cloudflare-rendered HTML, head tags, structured data), use `seo-crawler`.

#### 8.1 How Google Processes JavaScript (2025-2026)

Google uses a **two-wave indexing process** for JS sites:

- **Wave 1 (Crawl):** Googlebot fetches raw HTML immediately. Parses links and basic content without executing JavaScript. This happens fast.
- **Wave 2 (Render):** Pages queued for Google's Web Rendering Service (WRS) — a headless Chromium instance. Runs JavaScript, builds DOM, extracts content. This can take **hours to days** after initial crawl.

**Implication:** Content only visible after JS execution = delayed indexing. If rendering fails or times out, content may never be indexed.

**Google's current JS capability (2026):** WRS runs evergreen Chromium, supports React/Angular/Vue and modern ES6+. Google explicitly stated (Dec 2025) that its JS rendering capabilities have "significantly improved" and that earlier warnings about building sites usable without JavaScript are now "outdated." However, complex rendering can still timeout or fail on large SPAs with heavy client-side state.

**Bing limitation:** Bing's JS rendering capabilities lag significantly behind Google's. Sites relying purely on CSR may rank on Google but be invisible on Bing. Always validate with multi-engine testing.

---

#### 8.1b Performance Hints (2025-2026)

**Speculation Rules API** — prerender/prefetch the next likely page for near-instant navigation:
```html
<script type="speculationrules">
{
  "prerender": [{"where": {"href_matches": "/products/*"}, "eagerness": "moderate"}],
  "prefetch": [{"where": {"href_matches": "/blog/*"}, "eagerness": "moderate"}]
}
</script>
```
- Chrome-primary (2026), emerging cross-browser adoption
- Ray-Ban case study: 156% desktop conversion lift with prerendering
- `eagerness`: `immediate` (on page load), `eager` (soon), `moderate` (on hover), `conservative` (on click)
- Pattern: start with prefetch moderate, upgrade to prerender on hover for high-intent links
- Googlebot does NOT execute Speculation Rules — benefit is purely for real users and CrUX field data

**Early Hints (103 status code)** — server sends preliminary headers before the full response:
```
HTTP/1.1 103 Early Hints
Link: </style.css>; rel=preload; as=style
Link: <https://fonts.googleapis.com>; rel=preconnect

HTTP/1.1 200 OK
...
```
- Browser support ~89% globally (2026)
- Reduces effective LCP by starting resource loads during server think time
- Supported by Cloudflare (automatic), NGINX (2025+), Akamai
- **Googlebot does NOT process Early Hints** — benefit is purely for real users/CrUX field data, not lab data

---

#### 8.1c Schema Deprecations (2025-2026)

**June 2025 deprecations:** Book Actions, Course Info, Claim Review, Estimated Salary, Learning Video, Special Announcement, Vehicle Listing — Google no longer generates rich results for these types.

**New schema types:** MemberProgram, return policy structured data.

**Always check current supported types** via `seo-schema` before recommending new markup.

---

#### 8.2 Rendering Methods — SEO Impact by Type

| Method | Description | SEO Status | Best For |
|--------|-------------|-----------|----------|
| **SSR** (Server-Side Rendering) | Full HTML on server per request | ✅ Best | Content sites, SaaS landing pages |
| **SSG** (Static Site Generation) | Pre-built HTML at build time | ✅ Best | Blogs, docs, marketing pages |
| **ISR** (Incremental Static Regeneration) | SSG + background revalidation (Next.js) | ✅ Excellent | E-commerce, frequently updated content |
| **CSR** (Client-Side Rendering) | JS renders everything in browser | ⚠️ Risk | Internal apps (not public sites) |
| **Pre-rendering** | Static HTML generated at build for bots | ✅ Good | React apps without SSR framework |
| **Dynamic rendering** | SSR for bots, CSR for users | ⚠️ Deprecated workaround | Legacy SPAs only — migrate to SSR/SSG |
| **Partial hydration** | SSR shell + selective JS hydration | ✅ Excellent | Islands architecture (Astro, Qwik) |
| **React Server Components (RSC)** | Server-rendered components, zero client JS for server parts | ✅ Best | Next.js App Router (default 2025+) |
| **Streaming SSR** | Progressive HTML sent to browser as it renders | ✅ Excellent | Next.js, Remix — faster LCP, crawlable |
| **Partial Pre-Rendering (PPR)** | Static shell at build + dynamic content streamed from server | ✅ Excellent | Next.js 15+ (experimental → stable 2026) |
| **Islands Architecture** | Zero JS by default, selective interactive islands | ✅ Best | Astro 5+, content-driven sites |
| **View Transitions API** | Smooth MPA page transitions (SPA-like UX, full HTML per page) | ✅ Excellent | Astro, Chrome-native — crawlable MPA with SPA feel |

**How to detect rendering method:**
```bash
# Raw HTML word count — if very low = CSR
curl -sL https://example.com | wc -w

# Check for SSR signals in raw HTML
curl -sL https://example.com | grep -i '<h1\|<main\|<article\|<p'

# Check framework signals in headers
curl -sI https://example.com | grep -i 'x-powered-by\|server'

# Next.js SSR: look for __NEXT_DATA__ in raw HTML
curl -sL https://example.com | grep '__NEXT_DATA__'

# Angular: look for ng-version in HTML
curl -sL https://example.com | grep 'ng-version'
```

---

#### 8.3 Critical Canonical & Meta Directives Rule

Google updated JavaScript SEO guidance in December 2025:

1. **Canonical conflicts:** Canonical in raw HTML ≠ canonical injected by JS → Google may use EITHER. Must match.
2. **noindex via JS:** Raw HTML has `noindex` but JS removes it → Google MAY still honor the raw HTML noindex.
3. **Non-200 + JS:** Google does NOT render JS on non-200 pages. Meta tags injected by JS on error pages are invisible to Googlebot.
4. **Structured data timing:** JSON-LD injected via JS may face delayed processing. Include in initial server-rendered HTML for Product/Offer.

**Rule:** All critical SEO elements must be in the initial server-rendered HTML — never rely solely on JS injection:
- `<title>`, `<meta name="description">`, `<link rel="canonical">`
- `<meta name="robots">`, hreflang, OG tags
- JSON-LD structured data (especially Product, Article, SoftwareApplication)

---

#### 8.4 Soft 404s in SPAs — High Risk Pattern

**Soft 404 = page returns HTTP 200 but shows "not found" content.** This is one of the most damaging and hardest-to-detect patterns in React/Angular apps.

**Why it happens:**
- Angular/React router handles the "not found" state client-side → browser URL changes, content shows "Page not found", but server returns 200
- Google indexes an empty/useless page consuming crawl budget
- Tools like Screaming Frog report 200 — the issue is invisible to standard crawlers

**Diagnosis:**
```bash
# Test a URL that should 404
curl -o /dev/null -s -w "%{http_code}" https://example.com/nonexistent-page
# If it returns 200 → SOFT 404 RISK

# For Angular (common pattern): check if the app catches unknown routes
# Look in app-routing.module.ts for wildcard route:
# { path: '**', component: NotFoundComponent }
# This catches everything client-side but the server still returns 200
```

**Fix by framework:**

*Next.js:*
```javascript
// pages/404.js → automatically returns 404 status
// OR in App Router:
// app/not-found.js → returns 404

// For dynamic routes that don't exist:
export async function getServerSideProps({ params }) {
  const data = await fetchData(params.slug)
  if (!data) {
    return { notFound: true }  // → returns real 404
  }
  return { props: { data } }
}
```

*Angular (with Angular Universal/SSR):*
```typescript
// In the component for 404 page:
import { Response } from 'express'
import { RESPONSE } from '@nguniversal/express-engine/tokens'

constructor(@Optional() @Inject(RESPONSE) private response: Response) {
  if (this.response) {
    this.response.status(404)  // → returns real 404 to Googlebot
  }
}
```

---

#### 8.5 HTTP Status Codes in SPA Frameworks

SPAs must return correct HTTP status codes from the server, not just handle them client-side.

| Scenario | Required Status | How to Implement |
|----------|----------------|-----------------|
| Page not found | 404 | Next.js: `notFound: true` in getServerSideProps. Angular Universal: inject RESPONSE and set status |
| Permanent redirect | 301 | Next.js: `redirect: { destination, permanent: true }`. Angular: server-side redirect in Express |
| Temporary redirect | 302 | Next.js: `redirect: { destination, permanent: false }` |
| Unauthorized | 401 | Server must return 401 — not just JS hide content |
| Gone | 410 | Use for permanently deleted content (stronger signal than 404) |

**Google does NOT render JS on non-200 pages.** If your SPA returns 200 for all routes and handles routing client-side, Googlebot never sees the correct status code.

---

#### 8.6 Head Management by Framework

Critical: meta tags must be in server-rendered HTML, not injected client-side only.

**Next.js (App Router — Next.js 13+):**
```javascript
// app/[page]/page.js
export async function generateMetadata({ params }) {
  return {
    title: 'Page Title',
    description: 'Meta description',
    alternates: { canonical: 'https://example.com/page' },
    openGraph: { title: 'OG Title', description: 'OG desc', images: ['/og.jpg'] },
    robots: { index: true, follow: true }
  }
}
// ✅ Rendered server-side, in <head> on initial HTML
```

**Next.js (Pages Router):**
```javascript
import Head from 'next/head'
export default function Page() {
  return (
    <Head>
      <title>Page Title</title>
      <meta name="description" content="..." />
      <link rel="canonical" href="https://example.com/page" />
    </Head>
  )
}
// ✅ Rendered server-side with SSR/SSG
// ⚠️ If used with CSR only, meta tags inject client-side = delayed
```

**React + React Helmet (non-Next.js):**
```javascript
import { Helmet } from 'react-helmet-async'
// ⚠️ Client-side only by default — Googlebot sees empty <head>
// ✅ With SSR (react-helmet-async + server renderToString): works correctly
// Verify: curl -sL https://example.com | grep '<title'
```

**Angular (Angular Universal/SSR required for meta tags):**
```typescript
import { Meta, Title } from '@angular/platform-browser'

constructor(private meta: Meta, private title: Title) {
  this.title.setTitle('Page Title')
  this.meta.updateTag({ name: 'description', content: '...' })
  this.meta.updateTag({ property: 'og:title', content: '...' })
}
// ⚠️ Without Angular Universal: meta tags inject client-side only
// ✅ With Angular Universal (SSR): meta tags present in initial HTML
// Verify: curl -sL https://example.com | grep 'meta name'
```

**Audit check:**
```bash
# Verify meta tags are in raw HTML (not JS-injected)
curl -sL https://example.com | grep -i '<title\|meta name="description"\|rel="canonical"\|og:title'
# If empty → meta tags are JS-injected only = SEO risk
```

---

#### 8.7 Client-Side Routing vs Real Links

**SPA routing problem:** `<a href>` tags work correctly for Googlebot. But some SPAs use programmatic navigation that doesn't produce crawlable links.

```javascript
// ✅ Crawlable by Googlebot:
<Link href="/products">Products</Link>   // Next.js
<a routerLink="/products">Products</a>   // Angular
<a href="/products">Products</a>         // Standard HTML

// ⚠️ NOT crawlable by Googlebot:
<button onClick={() => router.push('/products')}>Products</button>
// → No <a> tag, Googlebot cannot discover this URL
```

**Audit check:**
```bash
# Check if internal links use <a> tags
curl -sL https://example.com | grep -o 'href="[^"]*"' | head -30
# All internal navigation should appear here
# If key links are missing → programmatic navigation issue
```

---

#### 8.8 Hydration Issues and SEO

Hydration = process where SSR-rendered HTML is "taken over" by JavaScript in the browser. SEO issues:

| Hydration Issue | SEO Impact | Fix |
|----------------|-----------|-----|
| **Hydration mismatch** | Server HTML ≠ JS-rendered HTML → React re-renders, causing CLS | Ensure server and client render identical content |
| **Hydration delay** | Critical content disappears then reappears during hydration | Avoid `typeof window !== 'undefined'` guards around critical content |
| **Full page re-render** | JS replaces all SSR content → Googlebot may see different content | Use partial hydration, avoid unnecessary client-side re-renders |
| **Content flash** | SSR renders "logged out" state, JS hydrates with "logged in" → layout shift | Only show user-specific content after client-side hydration is confirmed |

---

#### 8.9 Pagination in SPAs

| Pattern | SEO Compatibility | Implementation |
|---------|-----------------|---------------|
| **URL-based pagination** (`/page/2`, `?page=2`) | ✅ Best | Each page has unique URL, crawlable |
| **Load More button** | ✅ Good if links exist | Must have `<a href="/page/2">` as fallback |
| **Infinite scroll** | ⚠️ Risk | Googlebot doesn't scroll — implement URL-based fallback |
| **Client-side pagination** (no URL change) | ❌ Bad | Googlebot only sees page 1 content |

**Google's current guidance (2025):** Paginated content should use unique URLs. Infinite scroll is indexable ONLY if each "chunk" has a corresponding URL that Googlebot can crawl independently.

```bash
# Check if pagination changes URL
# Navigate to page 2 in browser — does URL change?
# curl -sL "https://example.com/products?page=2" | wc -w
# → Low word count = page 2 content isn't server-rendered
```

---

#### 8.10 Crawl Budget in SPAs

SPAs can waste crawl budget severely:

- **Faceted navigation without `noindex`:** `/products?color=red&size=M&brand=Nike` = thousands of duplicate URLs
- **Parameterized state in URL:** Sort/filter parameters creating infinite URL variations
- **Hash-based routing:** `/#/products` — hash fragment is ignored by Googlebot (it never changes)
- **Trailing slash inconsistency:** `/page/` and `/page` as separate URLs

```bash
# Check if hash routing is used (critical issue)
curl -sL https://example.com | grep 'hashbang\|#/'
# If site uses /#/route pattern → URLs invisible to Googlebot

# Check for parameter pollution
curl -sI "https://example.com/products?sort=asc&filter=red"
# → Should have canonical pointing to /products (or noindex)
```

**Fix for faceted navigation:**
```html
<!-- Add canonical to all filter/sort variations -->
<link rel="canonical" href="https://example.com/products" />
<!-- Or noindex filter pages -->
<meta name="robots" content="noindex, follow" />
```

---

#### 8.11 Bundle Size & Core Web Vitals in React/Angular

**LCP (Largest Contentful Paint) — React/Angular specific risks:**
- Large JS bundle blocks rendering → LCP element appears late
- Hero image loaded by JS (not in initial HTML) → invisible to LCP attribution
- Fonts loaded by JS → text not visible during render = LCP delay

```bash
# Check if LCP image is in initial HTML
curl -sL https://example.com | grep -i '<img\|background\|fetchpriority'
# If hero image only loads after JS → LCP will be poor
```

**INP (Interaction to Next Paint) — React/Angular specific risks:**
- React re-renders on every state change blocking main thread
- Angular change detection running on every event
- Large event handlers with synchronous heavy computation

**Audit checklist for JS bundle:**
- [ ] Code splitting: each route loads only its own JS
- [ ] Tree shaking: unused code eliminated from bundles
- [ ] Lazy loading: non-critical components loaded on demand
- [ ] Critical CSS: above-fold CSS inlined, rest deferred
- [ ] Third-party scripts: async/defer on all non-critical scripts
- [ ] Image priority: `fetchpriority="high"` on LCP image, in initial HTML

---

#### 8.12 Framework-Specific SEO Audit Commands

```bash
# NEXT.JS — check SSR output
curl -sL https://example.com | grep '__NEXT_DATA__'  # confirms Next.js
curl -sL https://example.com | grep -c '<p\|<h[1-6]'  # content in raw HTML?
curl -sL https://example.com/non-existent | head -1  # should be 404

# ANGULAR — check Angular Universal (SSR)
curl -sL https://example.com | grep 'ng-version\|ng-server-context'
# ng-server-context present → Angular Universal (SSR) active
# ng-version only → client-side Angular, no SSR

# Check if meta tags are in raw HTML (works for all frameworks)
curl -sL https://example.com | grep -i 'meta name\|og:title\|canonical'

# Count words in raw HTML to gauge CSR vs SSR
WORDS=$(curl -sL https://example.com | grep -o '\b[a-zA-Z]\{3,\}\b' | wc -w)
echo "Word count in raw HTML: $WORDS"
# < 100 words → likely CSR only (SEO risk)
# > 500 words → likely SSR/SSG (good)

# Detect hash routing (critical issue)
curl -sL https://example.com | grep -o 'href="#[^"]*"' | head -10
```

---

#### 8.13 Open Graph & Social Sharing in SPAs

OG tags must be in server-rendered HTML — social scrapers (Facebook, Twitter/X, LinkedIn, WhatsApp) do NOT execute JavaScript.

```bash
# Test OG tags (simulating social scraper — no JS)
curl -sL https://example.com | grep -i 'og:\|twitter:'
# If empty → social sharing will show no preview
# Fix: OG tags must be in SSR output
```

**Tools to test social rendering:**
- Facebook Sharing Debugger
- Twitter Card Validator
- LinkedIn Post Inspector

---

#### 8.14 Tools for JS-Rendered Site Audits

| Tool | Purpose | React/Angular Specific |
|------|---------|----------------------|
| **Google Rich Results Test** | What Googlebot renders | Shows JS-rendered content |
| **Google URL Inspection Tool** | Live rendering by Googlebot | Compare with raw HTML |
| **Chrome DevTools → Disable JS** | See what Googlebot sees in Wave 1 | Network → JS disabled |
| **seo-crawler** (Cloudflare) | Full JS-rendered HTML per URL | Exact Googlebot view |
| **WebPageTest** | Waterfall with JS render timeline | Shows when content appears |
| **Lighthouse** | CWV + render-blocking resources | React/Angular bundle analysis |
| **React DevTools Profiler** | Component re-render analysis | INP debugging |
| **Angular DevTools** | Change detection analysis | INP debugging |
| **next build --analyze** | Bundle size visualization | Code splitting gaps |
| **source-map-explorer** | JS bundle composition | Identify heavy dependencies |

### 9. Rendering Visual Completo — CSS, JS y Lo Que Google Ve

Google WRS (Web Rendering Service) ejecuta Chrome headless con CSS completo. Ve la página **tal como la ve el usuario** — incluyendo layout, colores, posicionamiento y contenido generado por CSS. Pero tiene reglas específicas sobre qué prioriza.

#### Qué puede ver Google en CSS

| Elemento CSS | Google lo ve | Impacto en ranking |
|-------------|-------------|-------------------|
| `display: none` | ✅ Ve el contenido en DOM | ⚠️ Penaliza si se usa para ocultar keywords |
| `visibility: hidden` | ✅ Ve el contenido en DOM | ⚠️ Misma penalización si es intencionado |
| `opacity: 0` | ✅ Ve el contenido | ⚠️ Misma regla |
| `position: absolute` fuera de pantalla | ✅ Ve el contenido | ⚠️ Riesgo de cloaking |
| `::before` / `::after` (CSS content) | ✅ Ve el texto generado | ✅ Indexable si es contenido real |
| Background images (CSS) | ✅ Las ve pero NO las indexa como imágenes | ❌ No aparecen en Google Images |
| Texto blanco sobre fondo blanco | ✅ Lo detecta | ❌ Manual action por cloaking |
| Acordeones / tabs colapsados | ✅ Ve todo el contenido | ⚠️ Contenido colapsado indexado con menor peso |
| Lazy load de imágenes | ✅ Con scroll simulation | ✅ Lazy load estándar funciona bien |
| Animaciones CSS | ✅ Ve el estado final renderizado | ➖ Neutral |
| `font-size: 0` | ✅ Lo detecta | ❌ Señal de spam |

#### Regla crítica: contenido oculto con CSS
```bash
# Detectar texto con display:none o visibility:hidden en HTML raw
curl -sL [URL] | grep -i 'display.*none\|visibility.*hidden'

# Detectar texto de color igual al fondo (cloaking clásico)
# Requiere inspección visual o Cloudflare rendered HTML
```

**Google indexa el contenido CSS-oculto pero le da MENOS peso que el contenido visible.** Nunca esconder contenido con CSS para "optimizar" sin que el usuario lo vea — es cloaking.

#### Background images vs `<img>` — diferencia crítica SEO

```css
/* ❌ Esta imagen NO aparece en Google Images, NO tiene alt text */
.hero { background-image: url('hero-image.webp'); }

/* ✅ Esta imagen SÍ aparece en Google Images, tiene alt text */
```
```html
<img src="hero-image.webp" alt="Descripción descriptiva" fetchpriority="high">
```

**Regla:** Imágenes editoriales (fotos del producto, hero, blog) → siempre `<img>`. Background CSS → solo para decoración pura sin valor SEO.

#### Verificar rendering visual con Cloudflare

El skill `seo-crawler` captura el DOM renderizado post-CSS+JS. Para verificar el estado visual completo:

```bash
# Obtener HTML renderizado (incluye contenido generado por JS/CSS)
# Usar seo-crawler → guarda como .html
# Luego abrir en browser para ver exactamente qué ve Google
```

Para screenshot (comparar visual browser vs Googlebot):
- Chrome DevTools → `Ctrl+Shift+P` → "Capture full size screenshot"
- Con JS deshabilitado: `Ctrl+Shift+P` → "Disable JavaScript" → screenshot = Wave 1

#### Acordeones, Tabs y Contenido Colapsado

Google confirma (2025) que indexa contenido dentro de tabs y acordeones, pero con **menor peso** que el contenido visible por defecto.

| Patrón | Impacto | Recomendación |
|--------|---------|--------------|
| FAQ con acordeón JS | Indexado pero menor peso | Si el contenido es clave para ranking → ponerlo visible |
| Tabs de producto (specs, reviews) | Indexado con menor peso | Reviews: usar schema. Specs: considerar tabla visible |
| "Ver más" que expande con JS | Indexado | OK para contenido secundario |
| Contenido crítico (keyword principal) en acordeón | Menor peso en ranking | Mover a contenido visible |

### 9a. HTML5 Semantic Markup Audit

HTML5 semantic elements help Google understand page structure without needing to execute JavaScript. Critical for React/Angular sites where content may be JS-rendered.

#### Required Semantic Structure
```html
<!-- Correct semantic document structure -->
<header>           → Site header, logo, nav
  <nav>            → Primary navigation (one <nav> per major nav block)
</header>
<main>             → Main content — ONE per page, never repeated
  <article>        → Self-contained content (blog post, product)
    <h1>           → ONE per page, primary topic
    <section>      → Thematic grouping within article/main
      <h2>, <h3>   → Logical hierarchy, never skip levels
    </section>
  </article>
  <aside>          → Related/supplementary content (sidebar)
</main>
<footer>           → Site footer
```

#### HTML5 Audit Checklist

**Document structure:**
- [ ] `<main>` present and contains primary content (ONE per page)
- [ ] `<header>` wraps site header
- [ ] `<nav>` wraps primary navigation (not generic divs)
- [ ] `<footer>` wraps site footer
- [ ] `<article>` used for self-contained content (blog posts, products)
- [ ] `<section>` used for thematic groupings (not as generic div replacement)
- [ ] `<aside>` used for supplementary content only

**Heading hierarchy:**
- [ ] Exactly ONE `<h1>` per page (contains primary keyword)
- [ ] H2s used for major sections, H3s for subsections
- [ ] No skipped levels (H1 → H3 without H2)
- [ ] Headings describe content, not used for styling

**Common errors in React/Angular apps:**
- `<div>` soup — all structure via divs/spans, no semantic elements
- Multiple `<h1>` tags (component-based thinking without page-level awareness)
- Navigation in `<div class="nav">` instead of `<nav>`
- Missing `<main>` wrapper
- `<section>` used as generic wrapper instead of `<article>` for content

**Audit command:**
```bash
# Check for semantic elements in raw HTML
curl -sL https://example.com | grep -io '<main\|<header\|<footer\|<nav\|<article\|<aside\|<section' | sort | uniq -c

# Count H1 tags (should be exactly 1)
curl -sL https://example.com | grep -io '<h1' | wc -l

# Check heading hierarchy (H1 → H2 → H3 order)
curl -sL https://example.com | grep -io '<h[1-6][^>]*>' | head -20
```

**In React/Angular:** Semantic elements must be in the SSR/SSG output, not added client-side. If components render `<div>` wrappers, Google's Wave 1 crawl sees non-semantic HTML.

#### Accessibility-SEO Overlap
Elements that improve both accessibility and SEO:
- `lang` attribute on `<html>` tag (language signal to Google)
- `alt` text on all images
- `aria-label` on icon-only buttons (not crawled but affects usability signals)
- `<label>` on form inputs
- `title` attribute on iframes

### 10. Ver Lo Que Google Ve — Flujo de Diagnóstico por URL

Para cualquier URL de un sitio (incluyendo sitios en desarrollo), este flujo compara lo que Google ve en cada wave.

#### Paso 1 — Wave 1: HTML raw (lo que Googlebot ve al instante, sin ejecutar JS)

```bash
# 1a. Verificar que la URL es accesible (status code)
curl -o /dev/null -s -w "Status: %{http_code} | TTFB: %{time_starttransfer}s\n" [URL]

# 1b. Ver el HTML raw completo que Googlebot recibe en Wave 1
curl -sL [URL] | head -200

# 1c. Verificar meta tags críticos en HTML raw (title, canonical, robots, OG)
curl -sL [URL] | grep -i -E '(<title|meta name="description"|rel="canonical"|meta name="robots"|og:title|og:description)'

# 1d. Contar palabras en HTML raw (diagnóstico CSR vs SSR)
curl -sL [URL] | grep -o '\b[a-zA-Z]\{3,\}\b' | wc -w
# < 100 palabras → CSR puro (contenido invisible en Wave 1)
# > 500 palabras → SSR/SSG (contenido visible en Wave 1)

# 1e. Ver headings en HTML raw
curl -sL [URL] | grep -io '<h[1-6][^>]*>.*</h[1-6]>' | head -20

# 1f. Ver links en HTML raw (descubrimiento de URLs por Googlebot)
curl -sL [URL] | grep -o 'href="[^"#][^"]*"' | sort -u | head -30
```

#### Paso 2 — Wave 2: HTML renderizado (lo que Googlebot ve después de ejecutar JS)

Usar `seo-crawler` (Cloudflare Browser Rendering) para obtener el DOM completo post-JS.

```bash
# Con Cloudflare Browser Rendering API:
curl -X POST "https://api.cloudflare.com/client/v4/accounts/{account_id}/browser-rendering/content" \
  -H "Authorization: Bearer {api_token}" \
  -H "Content-Type: application/json" \
  -d '{"url": "[URL]", "rejectResourceTypes": ["image", "media", "font", "stylesheet"]}'
```

#### Paso 3 — Comparar Wave 1 vs Wave 2

| Elemento | En Wave 1 (raw) | En Wave 2 (rendered) | Diagnóstico |
|----------|----------------|---------------------|-------------|
| `<title>` | Presente / Ausente | Presente / Ausente | Si solo en Wave 2 → riesgo de delay o invisibilidad |
| `<h1>` | Presente / Ausente | Presente / Ausente | Si solo en Wave 2 → Googlebot indexa sin H1 en Wave 1 |
| Texto del cuerpo | N palabras | N palabras | Si Wave 1 << Wave 2 → CSR, contenido con delay |
| Links internos | N links | N links | Si Wave 1 << Wave 2 → URLs no descubiertas hasta Wave 2 |
| `canonical` | URL X | URL Y | Si difieren → conflicto, Google elige uno arbitrariamente |
| Schema JSON-LD | Presente / Ausente | Presente / Ausente | Si solo en Wave 2 → rich results con delay |

**Regla:** Todo lo que está en Wave 2 pero NO en Wave 1 = **riesgo de indexación tardía o fallida**.

---

### 11. Checklist Pre-Lanzamiento — Sitio en Desarrollo

Para sitios en desarrollo o staging que van a lanzarse a producción. Revisar ANTES de hacer el sitio público.

#### A. Configuración de Entorno (Crítico — Staging vs Producción)

```bash
# Verificar que robots.txt NO bloquea todo (error común en staging)
curl -sL [URL]/robots.txt
# NUNCA debe tener "Disallow: /" en producción

# Verificar noindex no está en todas las páginas (error de staging que pasa a prod)
curl -sL [URL] | grep -i 'noindex'
# Si aparece → revisar si es intencional o error del entorno de dev
```

| Check | Por qué es crítico | Comando |
|-------|------------------|---------|
| `robots.txt` no bloquea Googlebot | En dev suele tener `Disallow: /` — si pasa a prod = invisibilidad total | `curl [URL]/robots.txt` |
| Sin `noindex` global en producción | CMSs (WordPress, Drupal) tienen "Desalentar a los buscadores" activado en staging | Ver código fuente de cualquier página |
| Canonical apunta a dominio de producción | Si staging tiene canonical con URL de staging = confunde a Google | `curl -sL [URL] | grep canonical` |
| HTTPS funcionando en producción | En dev suele ser HTTP local | `curl -I [URL]` — verificar redirect HTTP→HTTPS |
| Sitemap disponible y accesible | `/sitemap.xml` debe retornar 200 | `curl -o /dev/null -s -w "%{http_code}" [URL]/sitemap.xml` |

#### B. URLs y Redirecciones

```bash
# Verificar que el dominio de producción hace redirect correcto (www vs non-www)
curl -I http://dominio.com       # debe redirigir a https://dominio.com o https://www.dominio.com
curl -I http://www.dominio.com   # debe redirigir al canonical elegido

# Verificar que no hay redirect chains
curl -sI -L [URL] | grep -E 'HTTP|Location'
# Solo debe haber 1 redirect, no 3+
```

#### C. Meta Tags en HTML Raw (no solo en JS)

Para sitios React/Angular/Vue — verificar ANTES del lanzamiento:

```bash
# ¿El title está en HTML raw?
curl -sL [URL] | grep -i '<title'
# Si vacío → el framework no tiene SSR → BLOQUER antes de launch

# ¿La meta description está en HTML raw?
curl -sL [URL] | grep -i 'meta name="description"'

# ¿El canonical está en HTML raw y apunta al dominio de producción correcto?
curl -sL [URL] | grep -i 'canonical'
# Verificar que NO sea la URL de staging/dev

# ¿Los OG tags están presentes? (para social sharing desde día 1)
curl -sL [URL] | grep -i 'og:'
```

#### D. Contenido Indexable en Wave 1

```bash
# Contar palabras en HTML raw de las páginas clave
# Homepage
curl -sL [URL] | grep -o '\b[a-zA-Záéíóúñ]\{3,\}\b' | wc -w

# Una página de producto/servicio/blog
curl -sL [URL]/pagina-clave | grep -o '\b[a-zA-Záéíóúñ]\{3,\}\b' | wc -w

# Umbral: < 100 palabras = CSR puro = contenido con indexación tardía o fallida
```

#### E. Checklist de Lanzamiento Completo

**Técnico:**
- [ ] `robots.txt` permite Googlebot (y otros bots relevantes)
- [ ] `robots.txt` AI crawler strategy definida (allow/block por bot — see AI Crawler Management above)
- [ ] Sin `noindex` en páginas públicas (verificar en settings del CMS)
- [ ] HTTPS funcionando con redirect desde HTTP
- [ ] www vs non-www: canonical elegido y redirect configurado
- [ ] Sitemap en `/sitemap.xml` o `/sitemap_index.xml`
- [ ] Google Analytics / GA4 instalado y recibiendo hits
- [ ] Google Search Console configurado con el sitio verificado
- [ ] Bing Webmaster Tools verificado
- [ ] Sitemap enviado en GSC y BWT
- [ ] IndexNow key file configurado (`/{key}.txt` accesible)
- [ ] Speculation Rules implementados para navegación clave (si aplica)

**Contenido:**
- [ ] Title tags en HTML raw (no solo JS) en todas las páginas
- [ ] Meta descriptions en HTML raw en todas las páginas
- [ ] Canonicals apuntan al dominio de producción (no staging/dev)
- [ ] H1 visible en HTML raw en cada página
- [ ] Links internos con `<a href>` real (no solo router.push() de JS)
- [ ] Schema JSON-LD en HTML raw (no solo JS-injected) en páginas clave
- [ ] OG tags en HTML raw (para social sharing)

**Performance:**
- [ ] LCP < 2.5s en PageSpeed Insights (móvil)
- [ ] No hay imágenes > 500KB en páginas principales
- [ ] Fuentes con `font-display: swap` o `optional`
- [ ] Hero image con `fetchpriority="high"` en HTML raw

**Post-lanzamiento (primeras 48h):**
- [ ] Request indexing de homepage en GSC URL Inspection
- [ ] Verificar que Googlebot puede acceder (GSC → Cobertura → Sin errores)
- [ ] Confirmar que el sitio aparece en `site:dominio.com` dentro de 3-7 días

#### F. Sitio Detrás de Autenticación / Staging Protegido

Si el sitio de desarrollo está detrás de contraseña o IP whitelist, Cloudflare Browser Rendering (y Googlebot real) **no pueden acceder**. Opciones:

| Opción | Cómo | Cuándo usar |
|--------|------|-------------|
| IP allowlist temporal para Cloudflare crawler | Agregar IPs de Cloudflare al whitelist de staging | Para auditar staging antes de lanzar |
| Bypass header en WAF | Agregar header secreto que bypassea la protección | Para sitios con WAF propio |
| Desplegar a producción con `noindex` temporal | Sitio live pero `noindex` en todas las páginas mientras termina el desarrollo | Para auditar en entorno real sin indexar |
| Chrome DevTools con JS deshabilitado | Ver Wave 1 manualmente en el browser | Para sitios locales sin acceso externo |

```bash
# Chrome DevTools → Wave 1 manual (para localhost o staging protegido):
# 1. Abrir DevTools (F12)
# 2. Command Palette (Ctrl+Shift+P)
# 3. "Disable JavaScript"
# 4. Recargar la página
# Lo que ves = Wave 1 (exactamente lo que Googlebot ve antes de renderizar JS)
```

### 12. IndexNow Protocol

IndexNow enables instant URL notification to search engines when content changes. As of 2026, it's a mainstream protocol — not experimental.

**Supported search engines:** Bing, Yandex, Naver, Seznam, Yep. **Google does NOT support IndexNow** (confirmed 2026 despite testing since 2021). For Google, use the Indexing API (for eligible content types) or request indexing via GSC.

**Scale (2026):** 5 billion+ daily URL submissions, 80M+ websites actively using it, 22% of clicked URLs in Bing results come from IndexNow.

**How it works:**
1. Generate an API key (any UUID)
2. Place key file at `https://example.com/{key}.txt`
3. Submit URLs via HTTP GET or POST to `https://api.indexnow.org/indexnow`

```bash
# Check if IndexNow key exists
curl -o /dev/null -s -w "%{http_code}" https://example.com/{key}.txt

# Submit a URL
curl "https://api.indexnow.org/indexnow?url=https://example.com/updated-page&key={your-key}"

# Bulk submit (POST, up to 10,000 URLs per batch)
curl -X POST "https://api.indexnow.org/indexnow" \
  -H "Content-Type: application/json" \
  -d '{"host":"example.com","key":"{key}","urlList":["https://example.com/page1","https://example.com/page2"]}'
```

**CMS integrations:**
- WordPress: Yoast SEO, Rank Math, Microsoft IndexNow plugin (10M+ active installs combined)
- Shopify: native integration (May 2025)
- Wix, Duda: built-in support

**Complementary with sitemaps:** Sitemaps for URL discovery (complete list), IndexNow for instant change notification. Use both.

**Audit checklist:**
- [ ] IndexNow key file accessible at root domain
- [ ] Key file returns 200 with valid key content
- [ ] CMS plugin or webhook triggers IndexNow on publish/update
- [ ] Bing Webmaster Tools shows IndexNow submissions received

## Output

### Technical Score: XX/100

### Category Breakdown
| Category | Status | Score |
|----------|--------|-------|
| Crawlability | pass/warn/fail | XX/100 |
| Indexability | pass/warn/fail | XX/100 |
| Security | pass/warn/fail | XX/100 |
| URL Structure | pass/warn/fail | XX/100 |
| Mobile | pass/warn/fail | XX/100 |
| Core Web Vitals | pass/warn/fail | XX/100 |
| Structured Data | pass/warn/fail | XX/100 |
| JS Rendering (React/Angular) | pass/warn/fail | XX/100 |
| HTML5 Semantic Markup | pass/warn/fail | XX/100 |
| IndexNow | pass/warn/fail | XX/100 |

### Critical Issues (fix immediately)
### High Priority (fix within 1 week)
### Medium Priority (fix within 1 month)
### Low Priority (backlog)

## DataForSEO Integration (Optional)

If DataForSEO MCP tools are available, use `on_page_instant_pages` for real page analysis (status codes, page timing, broken links, on-page checks), `on_page_lighthouse` for Lighthouse audits (performance, accessibility, SEO scores), and `domain_analytics_technologies_domain_technologies` for technology stack detection.

## Google API Integration (Optional)

If Google API credentials are configured, use `python scripts/pagespeed_check.py <url> --json` for real PSI + CrUX field data (replaces lab-only CWV estimates), `python scripts/crux_history.py <url> --json` for 40-week CWV trends (updated from 25 weeks), and `python scripts/gsc_inspect.py <url> --json` for real indexation status per URL.

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable | Report connection error with status code. Suggest verifying URL, checking DNS resolution, and confirming the site is publicly accessible. |
| robots.txt not found | Note that no robots.txt was detected at the root domain. Recommend creating one with appropriate directives. Continue audit on remaining categories. |
| HTTPS not configured | Flag as a critical issue. Report whether HTTP is served without redirect, mixed content exists, or SSL certificate is missing/expired. |
| Core Web Vitals data unavailable | Note that CrUX data is not available (common for low-traffic sites). Suggest using Lighthouse lab data as a proxy and recommend increasing traffic before re-testing. |
