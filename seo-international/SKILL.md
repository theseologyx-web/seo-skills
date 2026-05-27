---
name: seo-international
description: >
  International SEO strategy and technical implementation. Covers URL structure
  (ccTLD vs subdomain vs subdirectory), geo-targeting signals, market prioritization,
  localization strategy, keyword research per market, VPN-based SERP testing, GSC
  International Targeting, and full hreflang implementation (validation, generation,
  HTML/HTTP header/XML sitemap methods). Use when user says "international SEO",
  "global SEO", "expand to new country", "multilingual site", "geo-targeting",
  "ccTLD", "subdirectory vs subdomain", "hreflang", "i18n SEO", "multi-language",
  "multi-region", or "language tags".
user-invokable: true
argument-hint: "[domain or target markets]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# International SEO Strategy

Full international SEO goes far beyond hreflang tags. It encompasses site architecture decisions, geo-targeting signals, content strategy per market, and ongoing monitoring. (For hreflang implementation details, see the `seo-hreflang` skill.)

---

## 1. URL Structure Decision: ccTLD vs. Subdomain vs. Subdirectory

This is the most consequential international SEO decision — it's hard to change later.

### Comparison Matrix

| Structure | Example | Geo Signal | Link Equity | Effort | Best For |
|-----------|---------|-----------|-------------|--------|---------|
| **ccTLD** | `example.de` | ⭐⭐⭐ Strongest | Split across domains | High | Committed, well-funded global expansion |
| **Subdomain** | `de.example.com` | ⭐⭐ Medium | Mostly shared | Medium | Large sites, separate tech stacks per region |
| **Subdirectory** | `example.com/de/` | ⭐ Weakest alone | Fully shared | Low | Most sites — recommended default |

### ccTLD (Country Code Top-Level Domain)
```
example.de  ← Germany
example.fr  ← France
example.co.uk ← UK
example.com.br ← Brazil
```
**Pros:**
- Strongest local trust signal with users AND Google
- Local hosting preference easier to justify
- Each domain treated independently (bad news can't hurt the other)

**Cons:**
- You must build domain authority for EACH new TLD from zero
- More expensive (domain costs × number of markets)
- More complex to manage (DNS, SSL, CMS for each)
- Link building effort multiplied

**When to use:** You're committing to a specific market for 5+ years with dedicated local team and budget.

### Subdomain
```
de.example.com
fr.example.com
```
**Pros:**
- Separate hosting/infrastructure per region
- Google treats them as somewhat independent (less so than ccTLD)
- Easier to manage than ccTLD at scale

**Cons:**
- Weakest option in practice — not as local as ccTLD, doesn't share authority as well as subdirectory
- Users less familiar with convention (vs. /de/)

**When to use:** You need separate tech stacks (e.g., different CMS per region) but can't justify full ccTLD investment.

### Subdirectory (Recommended for Most Sites)
```
example.com/de/
example.com/fr/
example.com/en-gb/
```
**Pros:**
- All pages share the main domain's authority
- Single domain to build — easier link building
- Simpler to manage technically
- Google's John Mueller has confirmed subdirectories work just as well as ccTLDs long-term

**Cons:**
- Weaker country-specific trust signal (mitigated by hreflang + GSC geo-targeting + content)
- Must use GSC International Targeting to compensate

**When to use:** Default choice unless you have specific reasons for ccTLD.

### URL Pattern Conventions
```
# Language only (no region)
/fr/ → French speakers everywhere
/es/ → Spanish speakers everywhere

# Language + Region (when the same language differs by market)
/en-us/ → US English
/en-gb/ → UK English
/pt-br/ → Brazilian Portuguese
/pt-pt/ → Portugal Portuguese
/zh-hans/ → Simplified Chinese (mainland)
/zh-hant/ → Traditional Chinese (Taiwan/HK)
```

---

## 2. Geo-Targeting Signals

Google uses multiple signals to determine which country a page targets. More signals = stronger geo-targeting:

### Signal Strength Hierarchy

| Signal | Weight | How to Set |
|--------|--------|-----------|
| ccTLD | Strongest | Purchase `.de`, `.fr`, etc. |
| GSC International Targeting | Strong | Search Console → Legacy tools → International Targeting |
| Server hosting location | Medium | Host in target country or use CDN with geo-rules |
| Content language | Medium | `<html lang="de">`, content in German |
| Hreflang tags | Medium | See `seo-hreflang` skill |
| Backlinks from local domains | Medium | `.de` links for German pages |
| Business address on page | Moderate | Footer address, schema `PostalAddress` |
| Local phone number | Moderate | Country code in `<head>` schema |
| Local currency | Moderate | Pricing in local currency |
| Domain TLD extension | Variable | `.com` is international, not country-specific |

### GSC International Targeting Setup

> **Note (2025-2026):** Google has reduced emphasis on the International Targeting tool in favor of hreflang tags and content signals as primary geo-targeting mechanisms. The tool still exists but Google recommends relying primarily on correct hreflang implementation and URL structure.

```
Google Search Console → Settings → International Targeting
(Previously under "Legacy tools & reports → International Targeting" — path may vary by account)

For subdirectory structure:
- Create a GSC property for each subdirectory (or use prefix property)
- Set the country target for each: example.com/de/ → Germany

For subdomain:
- Add each subdomain as separate GSC property
- Set country for each property

For ccTLD:
- Google auto-detects ccTLD geo-targeting
- No need to set manually (but verify it's correct)
```

### `<html lang="">` Tag
```html
<!-- German page -->
<html lang="de">

<!-- UK English page -->
<html lang="en-GB">

<!-- Brazilian Portuguese page -->
<html lang="pt-BR">
```
This signals to browsers AND crawlers the content language.

---

## 3. Market Prioritization Framework

Before expanding to any market, evaluate with this framework:

### Market Selection Criteria

| Criterion | Data Source | Weight |
|-----------|------------|--------|
| Search volume for your keywords in that country | Ahrefs/SEMrush country filter | High |
| Competition level in that market | SERP analysis per country | High |
| Revenue potential (GDP, e-commerce adoption) | World Bank, Statista | High |
| Language + translation cost | Internal | Medium |
| Legal/regulatory complexity | Research per country | Medium |
| Existing organic traffic from that country | GA4 + GSC | Medium |
| Existing brand awareness | Brand search volume | Low |

### Priority Score Formula
```
Market Score = (Search Volume × 0.3) + (Revenue Potential × 0.3) +
               (Low Competition × 0.2) + (Existing Traffic × 0.2)

Divide by: Translation Cost × Legal Complexity Factor

Top 3 markets by score = start here
```

### Quick Market Validation with GSC
```
GSC → Performance → Countries tab
→ Sort by Clicks DESC
→ Identify countries already sending traffic without targeted content
→ These are lowest-effort international expansion opportunities
(you're already ranking without optimization — imagine with it)
```

---

## 4. Content Localization Strategy

### Translation vs. Transcreation vs. Native Content

| Approach | Description | Cost | SEO Value | When to Use |
|----------|-------------|------|-----------|------------|
| **Machine translation** | DeepL/Google Translate | Very low | Low (often penalized) | Never for important pages without human review |
| **AI-assisted translation** | Claude, GPT-4o + human review | Low | Medium-High | Good balance of cost + quality; AI maintains keyword intent better than pure MT |
| **Human translation** | Direct 1:1 translation | Low-Medium | Medium | Factual/technical content |
| **Transcreation** | Adapted for local culture | Medium | High | Marketing pages, CTAs |
| **Native creation** | Local writer creates original | High | Highest | Priority markets, blog |

**Google's stance:** Machine-translated content at scale can trigger spam policies. Always human-review at minimum.

### Local Keyword Research (Per Market)

Same intent, different phrasing per country — never assume keywords translate directly:

```
US English: "running shoes"        → 27,100/mo in US
UK English: "trainers"             → 40,500/mo in UK (not "running shoes"!)
Germany: "Laufschuhe"             → keyword research needed in German
France: "chaussures de running"   → not "running shoes" in French

Process:
1. Take your top 20 keywords
2. Translate concept (not keyword) into target language
3. Research local volume + variants in local language
4. Build country-specific keyword list (may differ significantly)
```

### Content Differences Beyond Language

| Element | Localization Needed? |
|---------|---------------------|
| Date format | DD/MM/YYYY (EU) vs. MM/DD/YYYY (US) |
| Currency | Local currency + format ($, €, £, ¥) |
| Phone number format | Country code + local format |
| Address format | Country-specific postal conventions |
| Images | Avoid US-centric imagery for non-US markets |
| Case studies / testimonials | Local customers preferred |
| Legal pages | Privacy policy must comply with local law (GDPR for EU) |
| Pricing | May vary by market |

### Multilingual Keyword Cannibalization

When two language versions target the same bilingual audience, pages can compete against each other in the same SERP.

**Most common scenario:** US Hispanic market — an English page and a Spanish page both appear in US results for a bilingual keyword.

**Detection:**
```
GSC → Performance → Queries
→ Filter by country: United States
→ For each high-impression query, click "Pages" tab
→ If both /en/ and /es/ URLs appear for the same query = cannibalization

Ahrefs → Site Explorer → Organic Keywords
→ Filter by country: US
→ Look for keywords where both language versions rank simultaneously
```

**Prevention:**
- Implement hreflang correctly — Google serves the right version based on user language preference, which reduces overlap
- Target different keyword intents per language: Spanish page targets native Spanish speakers with native-language queries; English page targets general US audience — intents should differ, not just language
- For bilingual-targeted content, pick one language per page — don't create parallel duplicates
- Never cross-canonicalize between language versions — each locale page must self-canonicalize

**When cannibalization has low impact:**
- Hreflang is correctly implemented (Google typically surfaces the language-matched version)
- Different CTAs, pricing, or offers per version make them functionally distinct
- If cannibalization persists despite correct hreflang, audit keyword intent: the two pages may need to target genuinely different queries

---

## 5. Schema for International Sites

### Organization Schema with Multi-Market Addresses
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Example Corp",
  "url": "https://example.com",
  "address": [
    {
      "@type": "PostalAddress",
      "streetAddress": "123 Main St",
      "addressLocality": "New York",
      "addressRegion": "NY",
      "postalCode": "10001",
      "addressCountry": "US"
    },
    {
      "@type": "PostalAddress",
      "streetAddress": "456 Oxford St",
      "addressLocality": "London",
      "postalCode": "W1C 1AP",
      "addressCountry": "GB"
    }
  ]
}
```

### Offer Schema with Multi-Currency Pricing
```json
{
  "@type": "Offer",
  "price": "99.00",
  "priceCurrency": "USD",
  "availableAtOrFrom": {
    "@type": "Country",
    "name": "US"
  }
}
```

### WebSite Schema per Language Version
Each language version should have its own WebSite schema:
```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "Example (Deutsch)",
  "url": "https://example.com/de/",
  "inLanguage": "de"
}
```

---

## 6. Technical International SEO Checklist

```bash
# Verify hreflang is present and correct
curl -sL https://example.com/de/ | grep -i "hreflang"

# Verify lang attribute
curl -sL https://example.com/de/ | grep -i '<html'

# Verify canonical points to the correct locale URL
curl -sL https://example.com/de/ | grep -i 'rel="canonical"'

# Check that correct language content is served
curl -sL https://example.com/de/ | grep -i '<title'

# Verify no accidental noindex on locale pages
curl -sL https://example.com/de/ | grep -i 'robots'
```

### Common Technical Issues
| Issue | Symptom | Fix |
|-------|---------|-----|
| Wrong canonical across locales | `/de/` page has canonical to `/en/` | Set canonical to same-locale URL |
| Missing self-referencing hreflang | Google ignores hreflang set | Add self-reference to every locale page |
| Serving same content in multiple locales | Duplicate content | Localize content, not just translate |
| Redirect loops on locale detection | Bot gets stuck in loop | Disable geo-redirect for bots based on User-Agent |
| Geo-IP redirect blocks Googlebot | Googlebot (from US) only indexes US version | Allow Googlebot to crawl all locales |

---

## 7. Geo-IP Redirects (Handle with Care)

Automatic redirects based on visitor IP are risky for SEO:

### The Problem
```
Googlebot crawls from US IPs.
If example.com/de/ redirects US IPs → example.com/en/,
Googlebot can't index the German pages.
```

### The Solution
```javascript
// Check user-agent before redirecting
// Never redirect known bots based on geo-IP

const botUserAgents = ['Googlebot', 'Bingbot', 'Slurp', 'DuckDuckBot'];
const isBot = botUserAgents.some(bot => req.headers['user-agent'].includes(bot));

if (!isBot && userCountry === 'DE' && !req.path.startsWith('/de/')) {
  // Show a banner suggesting /de/ rather than hard redirect
  // Or: only redirect on first visit (set cookie to prevent loop)
  res.redirect(302, '/de/' + req.path); // Use 302, not 301, for geo redirects
}
```

**Best practice:** Use a language/region suggestion banner rather than automatic redirect. Let users choose.

---

## 8. VPN-Based SERP Testing

To verify rankings and SERP appearance in target markets:

### Tools for Local SERP Testing
| Tool | Method | Accuracy |
|------|--------|---------|
| **BrightLocal Search Grid** | Geo-grid rank checking | High |
| **Advanced Web Ranking** | Country + language filter | High |
| **SEMrush Position Tracking** | Country targeting | High |
| **SERPRobot** | Country-specific rank check | Medium |
| **Ahrefs Rank Tracker** | Country + location | High |
| **Manual VPN** | Connect to target country VPN, search manually | Exact (but not scalable) |
| **Google Search with `&gl=` param** | `google.com/search?q=keyword&gl=de&hl=de` | Medium |

### Google Search Parameters for Local Testing
```
# Test German search results (no VPN needed)
https://www.google.com/search?q=keyword&gl=de&hl=de

# Parameters:
# gl=  → country (ISO 3166-1 alpha-2: de, fr, gb, us, br, etc.)
# hl=  → interface language (ISO 639-1: de, fr, en, pt, etc.)
# cr=  → country restrict (countryDE, countryFR, etc.)

# Test UK results:
https://www.google.com/search?q=keyword&gl=gb&hl=en

# Test French results:
https://www.google.com/search?q=keyword&gl=fr&hl=fr
```

### VPN for Accurate Local Search Testing
VPNs give you the most accurate local SERP experience:
1. Connect to VPN server in target country
2. Open browser in incognito mode
3. Search on `google.de` (Germany), `google.fr` (France), etc.
4. Results reflect what local users see

**Recommended VPN services for SEO testing:** ExpressVPN, NordVPN, Mullvad (server selection per city possible).

---

## 9. International SEO Monitoring

### GSC Reports to Monitor Per Market
```
GSC → Performance → Filter by Country
- Clicks, impressions, CTR per country
- Compare: this month vs. same month last year (YoY)
- Identify: high impressions / low CTR countries (opportunity to improve meta data)

GSC → Performance → Filter by Country → Filter by Query
- What are users searching for in each market?
- Are queries in local language (good) or English (may need more localization)?
```

### GA4 International Reporting
```
GA4 → Reports → User → User Attributes → Country
Filter: Session default channel = Organic Search
→ Organic traffic by country

Compare:
- Conversion rate by country (may need local landing pages if very different)
- Bounce rate by country (high bounce in a country = content not resonating)
- Revenue by country (prioritize investment where ROI is highest)
```

---

## 10. Tools for International SEO

| Tool | Purpose | Pricing |
|------|---------|---------|
| **Ahrefs** | Keyword research per country, rank tracking by country | Paid |
| **SEMrush** | International keyword + competitor analysis | Paid |
| **Sistrix** | Dominant for European market analysis (DE, FR, ES, IT, UK) | Paid |
| **Searchmetrics** | Global SERP visibility by country | Paid |
| **hreflang.org** | Free hreflang tag generator and validator | Free |
| **Merkle hreflang tester** | Validate hreflang implementation | Free |
| **DeepL** | Best machine translation (human review still required) | Free + Paid |
| **Google Translate** | Quick translation reference only | Free |
| **TranslatePress** | WordPress plugin for multilingual sites | Paid |
| **WPML** | WordPress multilingual plugin | Paid |
| **Weglot** | Multilingual plugin — server-side rendering available since 2024; verify SSR mode is active (client-side/JS-only mode still causes crawlability issues) | Paid |
| **Crowdin** | Translation management platform | Paid |

---

## Output Format

### International SEO Strategy Report

**Domain:** [domain]
**Current Markets:** [list]
**Target Markets:** [list]
**URL Structure:** [ccTLD/Subdomain/Subdirectory]

#### Current International Performance (GSC Data)
| Country | Clicks | Impressions | CTR | Avg Position |
|---------|--------|-------------|-----|-------------|
| [Country] | X | X | X% | X |

#### Market Prioritization Matrix
| Market | Search Vol. | Competition | Revenue Potential | Priority Score |
|--------|------------|-------------|------------------|---------------|
| [Country] | X | Low/Med/High | High/Med/Low | X |

#### URL Structure Assessment
**Current:** [structure]
**Recommended:** [structure]
**Rationale:** [reason]

#### Localization Gaps
| Language | Content Status | Hreflang | GSC Targeting | Gaps |
|----------|---------------|---------|--------------|------|
| [lang] | Translated/Machine/Missing | ✅/❌ | ✅/❌ | [list] |

#### Technical Issues Found
| Issue | Affected URLs | Priority | Fix |
|-------|--------------|---------|-----|
| [issue] | [URLs] | 🔴/🟡/🟢 | [action] |

#### 90-Day Action Plan
1. [Priority market + action]
2. [Technical fix]
3. [Content localization task]

---

## 11. Hreflang: Technical Implementation

> **Critical expectation-setting:** Google treats hreflang as a **hint, not a directive** (reiterated May 2025). Canonical tags, content similarity, site structure, and indexation status can all override hreflang annotations. Hreflang does not guarantee correct locale serving — it improves the probability. Set client expectations accordingly.

### Validation Checks

#### Self-Referencing Tags
- Every page must include an hreflang tag pointing to **itself**
- The self-referencing URL must exactly match the page's canonical URL
- Missing self-referencing tags cause Google to ignore the entire hreflang set

#### Return Tags (Bidirectional Requirement)
- If page A links to page B with hreflang, page B must link back to page A
- Every hreflang relationship must be bidirectional (A→B **and** B→A)
- Missing return tags invalidate the hreflang signal for both pages

#### x-default Tag
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

#### Language Code Validation
Must use ISO 639-1 two-letter codes:
```
✅ en, fr, de, ja, zh-Hans, zh-Hant, pt-BR, pt-PT
❌ eng (ISO 639-2, invalid)
❌ jp (should be ja for Japanese)
❌ zh (ambiguous — specify zh-Hans or zh-Hant)
```

#### Region Code Validation
Optional region qualifier uses ISO 3166-1 Alpha-2:
```
✅ en-US, en-GB, pt-BR, es-MX, es-ES
❌ en-uk (should be en-GB — UK is not a valid ISO code)
❌ es-LA (Latin America is not a country)
```

#### Canonical URL Alignment
- Hreflang tags must only appear on **canonical** URLs
- If a page has `rel=canonical` pointing elsewhere, hreflang on that page is ignored
- Canonical URL and hreflang URL must match exactly (including trailing slashes)

#### Protocol Consistency
- All URLs in an hreflang set must use the same protocol (HTTPS)
- Mixed HTTP/HTTPS causes validation failures

### Implementation Methods

#### Method 1: HTML Link Tags
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

#### Method 2: HTTP Headers
Best for non-HTML files (PDFs, documents):
```
Link: <https://example.com/doc.pdf>; rel="alternate"; hreflang="en-US",
      <https://example.com/fr/doc.pdf>; rel="alternate"; hreflang="fr",
      <https://example.com/doc.pdf>; rel="alternate"; hreflang="x-default"
```
Set via Nginx/Apache config or CDN rules.

#### Method 3: XML Sitemap (Recommended for large sites)
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

#### Method Comparison
| Method | Best For | Pros | Cons |
|--------|----------|------|------|
| HTML link tags | Small sites | Easy, visible in source | Bloats `<head>`, hard at scale |
| HTTP headers | Non-HTML files | Works for PDFs | Complex server config |
| XML sitemap | Large/cross-domain | Scalable, centralized | Not visible on page |

### Common Hreflang Mistakes

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

### Quick Validation
```bash
# Check if hreflang tags exist on a page
curl -sL https://example.com/page | grep -i "hreflang"

# Check lang attribute
curl -sL https://example.com/page | grep -i '<html'

# Verify canonical matches hreflang self-reference
curl -sL https://example.com/page | grep -i -E '(canonical|hreflang)'
```

**Tools:** hreflang.org validator (free), Merkle hreflang tester (free), Ahrefs Site Audit hreflang report (paid).

### Debugging Hreflang Issues

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

### CMS-Specific Notes

| Platform | Approach |
|----------|---------|
| **WordPress + WPML** | WPML auto-generates hreflang; verify in Siteground/hreflang validator post-install |
| **WordPress + Polylang** | Free tier handles hreflang; PRO adds sitemap integration |
| **Shopify** | No native hreflang — requires Shopify Markets (2022+) or Langify/Weglot app |
| **Webflow** | Localization feature (2024) generates hreflang automatically |
| **Next.js** | `next/head` + `i18n` config in `next.config.js`; use `next-sitemap` for multilingual sitemap |
| **Nuxt** | `@nuxtjs/i18n` module handles hreflang + sitemap automatically |
