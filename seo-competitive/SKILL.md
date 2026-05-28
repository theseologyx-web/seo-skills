---
name: seo-competitive
description: >
  SEO competitive analysis and market intelligence. Covers competitor identification,
  keyword gap analysis, content gap analysis, backlink gap, SERP market share,
  competitor content strategy reverse-engineering, and differentiation opportunities.
  Use when user says "competitive analysis", "competitor SEO", "keyword gap",
  "content gap", "SERP market share", "who are my competitors", or "how to beat competitors".
  Not for analyzing a single competitor page — use seo-competitor-pages. Not for industry benchmark (averages) — use seo-benchmark.
user-invokable: true
argument-hint: "[your domain or niche]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# SEO Competitive Analysis & Market Intelligence

Systematically analyze your SEO competitive landscape to identify gaps, opportunities, and differentiation strategies.

---

## 1. Competitor Identification

Not all competitors are the same. Distinguish between:

### Types of SEO Competitors

| Type | Definition | Example |
|------|-----------|---------|
| **Direct competitors** | Same product/service, same audience | Two plumbers in same city |
| **SERP competitors** | Rank for same keywords, different business | Plumber vs. home services directory |
| **Content competitors** | Compete for informational traffic | Blog ranking for "how to fix a pipe" |
| **Indirect competitors** | Different solution to same problem | Plumber vs. DIY plumbing guides |

**For SEO, SERP competitors matter most** — even if they don't sell what you sell.

### How to Identify SERP Competitors

**Method 1: Manual SERP audit**
Search your top 5-10 keywords and note which domains appear most frequently across results. The domains that appear in 6+ of your top 10 queries are your primary SERP competitors.

**Method 2: Tool-based**
- Ahrefs: Site Explorer → Competing Domains
- SEMrush: Domain Analytics → Organic Research → Competitors
- Moz: Link Explorer → Ranking Keywords → filter for competitor domains

**Method 3: Google autocomplete competitors**
Search `[your brand] vs` → see who users compare you to.

### Competitor Prioritization Matrix
| Competitor | Overlap Score | DA/DR | Traffic Est. | Threat Level |
|-----------|--------------|-------|-------------|-------------|
| competitor1.com | 65% | 72 | 450K/mo | 🔴 High |
| competitor2.com | 48% | 58 | 280K/mo | 🟡 Medium |
| competitor3.com | 31% | 45 | 120K/mo | 🟢 Low |

---

## 2. SERP Market Share Analysis

### Keyword Overlap Analysis
For your top 50-100 keywords, measure how often each competitor appears vs. you:

```
Your visibility score: 23%  (appears in top 10 for 23% of target keywords)
Competitor A:         41%
Competitor B:         38%
Competitor C:         17%

Gap to close: +18 points to match Competitor A
```

### Share of Voice (SOV) Calculation
```
Your SOV = (Sum of your CTR-weighted rankings) / (Total possible clicks in your keyword set)

Simple version:
- For each keyword you rank for: multiply search volume × CTR for your position
- Sum all values
- Divide by total search volume of your entire target keyword set
```

### Tracking Market Share Over Time
Track monthly changes in:
- Number of keywords in top 3 / top 10 / top 20
- Estimated organic traffic vs. competitors
- Featured snippet ownership
- PAA ownership for target topics

---

## 3. Keyword Gap Analysis

### Finding Keywords Competitors Rank For (You Don't)

**Process:**
1. Take 3-5 competitor domains
2. Export all their ranking keywords (top 100 positions)
3. Filter: remove keywords YOU already rank in top 10 for
4. Remaining = your keyword gaps

**Priority filters to apply:**
- Volume > 500/month (enough traffic to matter)
- KD < 60 (achievable for your site)
- Intent matches your content capability
- Not brand keywords (competitor brand = not useful to you)

### Gap Types

| Gap Type | Description | Action |
|----------|-------------|--------|
| **Missing keyword** | They rank, you don't even target it | Create new content |
| **Ranking gap** | You both target it, they rank higher | Improve existing page |
| **Intent gap** | You have a page but wrong format | Reformat/restructure page |
| **Coverage gap** | They cover sub-topics you skip | Expand existing page |

### Keyword Gap Visualization
```
Competitor ranks for 2,400 keywords in your niche
You rank for 800 of the same keywords

Shared keywords (both rank): 600
Only competitor ranks: 1,800 → YOUR GAPS
Only you rank: 200 → YOUR ADVANTAGES
```

---

## 4. Content Gap Analysis

### Identifying Missing Content Formats
For each high-value keyword cluster:
- What format does the SERP reward? (video, listicle, tool, guide)
- Does your competitor use that format?
- Do you have that format?

| Topic | SERP Rewards | Competitor Has | You Have | Gap? |
|-------|-------------|---------------|---------|------|
| "seo audit checklist" | Downloadable checklist + blog | ✅ | ❌ | Create checklist |
| "best seo tools" | Detailed comparison + table | ✅ | ✅ | Improve table |
| "seo audit tutorial" | YouTube video | ✅ | ❌ | Create video |

### Content Depth Gap
Use WebFetch to compare content depth:
```bash
# Compare word count / content sections
curl -sL [your-url] | wc -w
curl -sL [competitor-url] | wc -w

# Check headings
curl -sL [competitor-url] | grep -i -E '<h[1-3]'
```

**What to analyze:**
- Word count (are they 3x longer than you?)
- Number of H2/H3 sections (sub-topic coverage)
- Presence of: tables, images, videos, tools, downloads, FAQs
- Internal links (topic cluster depth)
- Schema markup (rich results they get that you don't)

### Content Freshness Gap
- When was their top content last updated?
- Do they have a "last updated" signal on the page?
- Are they publishing faster/more frequently than you?

**Publication frequency benchmark:**
```
Competitor A: 12 posts/month
Competitor B: 8 posts/month
Your site: 3 posts/month

Gap: Need 2-4x more content to compete on volume
```

---

## 5. Backlink Gap Analysis

### Link Profile Comparison
| Metric | You | Comp A | Comp B |
|--------|-----|--------|--------|
| Referring domains | 320 | 1,240 | 890 |
| DR/DA | 38 | 67 | 58 |
| Dofollow links | 280 | 1,100 | 780 |
| Gov/Edu links | 2 | 18 | 12 |
| Avg link DR | 32 | 51 | 44 |

### Finding Link Opportunities from Gaps
**Competitor-specific links you don't have:**
1. Export competitor's backlinks (Ahrefs / Semrush)
2. Filter: DR > 40, dofollow, not your domain
3. Identify patterns: directories, resource pages, press, partnerships
4. Pursue the same sources

**"Skyscraper" link targets:**
- Find competitor's top linked-to pages
- Create a better version of that content
- Reach out to sites linking to the inferior version

### Anchor Text Analysis
Compare your anchor text distribution vs. competitors:

| Anchor Type | Your % | Competitor % | Assessment |
|-------------|--------|-------------|-----------|
| Branded | 45% | 38% | ✅ Healthy |
| Naked URL | 20% | 15% | ✅ Normal |
| Exact match keyword | 8% | 12% | ⚠️ Slightly under-optimized |
| Partial match | 15% | 22% | ⚠️ Gap |
| Generic ("click here") | 12% | 13% | ✅ Normal |

---

## 6. Competitor Content Strategy Reverse-Engineering

### Identifying Their Top-Performing Content
```bash
# Via Ahrefs: Site Explorer → Top Pages → sort by organic traffic
# Via SEMrush: Domain Analytics → Organic Research → Pages
# Manual: site:competitor.com + check most shared content
```

**What to analyze for each top page:**
- What keyword cluster does it target?
- What makes it rank? (content quality, links, schema, UX)
- Can you create something better / more comprehensive?
- Is the keyword relevant to YOUR business?

### Editorial Calendar Intelligence
By analyzing competitor publication patterns:
```
Month 1: [Topic A] — 4 posts
Month 2: [Topic B] — 3 posts
Month 3: [Topic A revisited] — 2 posts

Pattern: They update old content quarterly, publish new weekly
Strategy: Match their publication frequency, then exceed their depth
```

### Topic Prioritization (Competitor Data)
Rank topics by:
1. **Competitor traffic to that topic** (high traffic = high opportunity)
2. **Your current position** (positions 4-20 = quick wins)
3. **Content gap** (no competing page = easier to rank)
4. **Intent alignment** (commercial topics = higher ROI)

---

## 7. Technical Competitive Analysis

### Site Speed Comparison
```bash
# Compare TTFB and performance scores
curl -o /dev/null -s -w "TTFB: %{time_starttransfer}s\n" https://competitor.com
# Also use: PageSpeed Insights API for lab data comparison
```

### Crawlability Comparison
- How many pages do they have indexed?
- `site:competitor.com` → estimated index size
- Do they have better site architecture?
- Faster crawl budget usage?

### Schema Advantage Audit
Which rich results do competitors get that you don't?
- Star ratings in SERP? → they have `aggregateRating` schema
- FAQ accordion in SERP? → they have `FAQPage` schema
- Sitelinks? → their brand architecture signals trust
- Video carousel? → `VideoObject` schema on their blog

### Core Web Vitals Comparison
| Metric | You | Comp A | Comp B |
|--------|-----|--------|--------|
| LCP (mobile) | 3.2s | 1.8s | 2.4s |
| INP (mobile) | 280ms | 180ms | 220ms |
| CLS | 0.12 | 0.04 | 0.08 |

**If competitors have significantly better CWV**, this explains ranking gaps even when your content is better.

---

## 8. SERP Feature Competitive Analysis

Map which SERP features competitors own for your target keywords:

| SERP Feature | Your Ownership | Competitor A | Competitor B |
|-------------|---------------|-------------|-------------|
| Featured Snippets | 12 | 47 | 31 |
| People Also Ask | 28 | 89 | 64 |
| Image Pack | 3 | 22 | 15 |
| Video Carousel | 0 | 8 | 5 |
| Local Pack | 15 | 15 | 12 |
| Knowledge Panel | 1 | 3 | 2 |
| Sitelinks | 1 | 1 | 1 |

**High-gap features = highest ROI to pursue first.**

---

## 9. Differentiation Opportunities

After mapping the competitive landscape, identify where to differentiate:

### The "Blue Ocean" Keyword Strategy
Find keyword clusters where:
- Search volume is real (>500/month)
- Competitor coverage is thin or low quality
- Your expertise is genuinely stronger

These are your "blue ocean" opportunities — less competition, more differentiation.

### Content Differentiation Angles
If competitors have covered a topic, still win by:
- **More recent data**: "2025 study" vs. their "2023 data"
- **Original research**: Conduct a survey, publish unique data
- **Better format**: Video when they have text; interactive tool when they have a list
- **Specific audience**: "Email marketing for nonprofits" vs. generic "email marketing"
- **Deeper expertise**: Go 10x deeper on one aspect they cover shallowly

### Competitor Vulnerability Signals
Look for competitors that:
- Have stale content (last updated 2+ years ago)
- Have weak on-page SEO (missing H1, thin content)
- Have poor user experience (slow, bad mobile)
- Have few backlinks relative to their age
- Rely on one or two high-traffic pages (concentrate → target those topics)

---

## 10. Tools by Function

### All-in-One Competitive Intelligence
| Tool | Best For | Pricing |
|------|----------|---------|
| Ahrefs | Backlinks, keywords, content gaps, rank tracking | Paid |
| SEMrush | Full competitive suite + PPC + social | Paid |
| Moz Pro | Domain authority, keyword gaps, SERP analysis | Paid |
| SpyFu | Historical PPC + SEO competitor data | Paid |
| SimilarWeb | Traffic estimation, audience overlap, channels | Free + Paid |
| Sistrix | European market specialization | Paid |

### SERP Analysis
| Tool | Purpose | Pricing |
|------|---------|---------|
| SERPstat | SERP features, market share | Paid |
| Authoritas | Enterprise SERP visibility tracking | Paid |
| Advanced Web Ranking | Rank tracking + market share | Paid |
| SERP Robot | Rank checker | Free + Paid |

### Content Intelligence
| Tool | Purpose | Pricing |
|------|---------|---------|
| BuzzSumo | Top content by shares + engagement | Paid |
| MarketMuse | Topic authority + content gap | Paid |
| Clearscope | Content optimization vs. top competitors | Paid |
| Surfer SEO | Content brief from competitor analysis | Paid |

### Backlink Analysis
| Tool | Purpose | Pricing |
|------|---------|---------|
| Ahrefs | Largest backlink database | Paid |
| Majestic | Trust Flow, Citation Flow | Paid |
| Moz Link Explorer | DA + spam score | Paid |
| SEMrush Backlink Gap | Multi-competitor link gap | Paid |

### AI Visibility Competitive Intelligence (2026)
| Tool | Purpose | Pricing |
|------|---------|---------|
| Otterly.ai | Share of AI Voice (SAIV) monitoring | Paid |
| Peec AI | Citation tracking across AI platforms | Paid |
| Rank Prompt | AEO (Answer Engine Optimization) tracking | Paid |
| SE Ranking AI Overviews Tracker | AI Overviews citation tracking | Paid |
| HubSpot AEO Grader | Free AI visibility check | Free |

**AI Competitive Intelligence Framework:**
1. **Share of AI Voice (SAIV):** % de queries donde tu marca aparece en AI responses vs competidores
2. **Consensus Layer:** ¿Qué competidores aparecen en 15+ fuentes distintas (foros, reviews, press, social)? Eso > un backlink DR90
3. **Entity Authority:** Comparar Knowledge Graph presence, Wikipedia entry, structured data coverage — más relevante que DA/DR
4. **Multi-platform:** Analizar competencia no solo en Google sino YouTube, Reddit (1,900% growth en Google), TikTok (49% USA users)

**Nota sobre DA/DR:** siguen siendo útiles como referencia rápida, pero en 2026 entity authority, brand mentions y AI citations son señales más relevantes. No usar DA como métrica decisiva.

---

## Output Format

### Competitive Analysis Report

**Your Domain:** [domain]
**Competitors Analyzed:** [list]
**Analysis Date:** [date]

#### Competitive Landscape Summary
[3-4 sentences: market position, biggest threat, biggest opportunity]

#### Market Share Dashboard
| Domain | Keyword Overlap | Est. Traffic | DA/DR | SOV % |
|--------|----------------|-------------|-------|-------|
| Your site | - | X | X | X% |
| Comp A | X% | X | X | X% |
| Comp B | X% | X | X | X% |

#### Top 10 Keyword Gaps (Priority Order)
| Keyword | Volume | KD | Who Ranks | Action |
|---------|--------|-----|----------|--------|
| [KW] | X | X | [Comp] | Create/Improve |

#### Content Gaps
| Topic | Format Needed | Who Has It | Priority |
|-------|-------------|-----------|---------|
| [Topic] | [Format] | [Comp] | 🔴/🟡/🟢 |

#### Backlink Opportunities
| Source | DR | Link Type | Competitor Has It | How to Get |
|--------|-----|----------|-----------------|-----------|
| [Domain] | X | [Type] | ✅ | [Outreach/Submit/Partner] |

#### 90-Day Action Plan
| Priority | Action | Expected Impact | Effort |
|----------|--------|----------------|--------|
| 🔴 | [Action] | [Impact] | [Low/Med/High] |

---

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Keyword gap clustering and content calendar | `/seo keywords [topic]` | Turns competitor keyword gaps into clusters and priorities |
| Live competitor domain and traffic data | `/seo dataforseo competitors <domain>` | Real-time traffic estimates, keyword overlap, domain rank |
| Backlink gap analysis | `/seo backlinks [domain]` | Link profile comparison and outreach opportunities |
| Technical competitive comparison (CWV, schema) | `/seo technical <url>` | Side-by-side technical gap analysis vs. competitors |
| Comparison and alternatives pages | `/seo competitor-pages [url]` | Turn competitive intelligence into high-converting pages |
| Full strategic plan from competitive data | `/seo plan [business-type]` | Integrates competitive analysis into phased SEO roadmap |
