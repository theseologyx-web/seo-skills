---
name: seo-brand
description: >
  Brand SEO and reputation management. Analyzes branded SERPs, knowledge panels,
  Google Business Profile, third-party mentions, review signals, and brand entity
  optimization. Use when user says "brand SERP", "knowledge panel", "branded search",
  "reputation management", "brand mentions", or "entity SEO".
  Not for entity schema markup or knowledge graph disambiguation — use seo-entity.
user-invokable: true
argument-hint: "[brand name or url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Brand SEO & Reputation Management

Analyze and optimize a brand's presence in search results beyond rankings — covering knowledge panels, branded SERPs, entity authority, and reputation signals.

> **Skill relacionado:** `seo-entity` cubre la capa técnica de entidades (Knowledge Graph, Wikipedia/Wikidata, relaciones semánticas, schema para clarificación de entidad). Este skill (`seo-brand`) se enfoca en la capa de reputación y presencia en SERPs (branded SERPs, menciones, reviews, GBP). Úsalos juntos para una estrategia de entidad completa.

## CRITICAL: Data Extraction

Use `curl` for accurate meta tag extraction (WebFetch misses `<head>`):
```bash
curl -sL [URL] | grep -i -E '(<title|meta name="description"|property="og:|rel="canonical")'
```

---

## 1. Branded SERP Audit

### What to Check
Perform a `site:` and brand name search and analyze what appears:

**SERP elements to identify:**
- Homepage and sitelinks (are they accurate and helpful?)
- Knowledge Panel (present? accurate? verified?)
- Reviews/ratings in SERP
- Social media profiles (correct order/presence?)
- News results (positive, neutral, or negative?)
- Third-party review sites (Trustpilot, G2, Yelp, etc.)
- Competitor ads on branded terms
- Image carousel

**Scoring:**
| Element | Status | Priority |
|---------|--------|----------|
| Knowledge Panel present | ✅/❌ | High |
| Sitelinks showing | ✅/❌ | High |
| No negative results on page 1 | ✅/❌ | Critical |
| Social profiles present | ✅/❌ | Medium |
| Reviews visible | ✅/❌ | Medium |

---

## 2. Knowledge Panel Optimization

### Verification Status
- Is the panel claimed/verified by the brand?
- Does it show accurate name, description, founding date, founders?
- Are social profiles linked correctly?
- Is the logo/image correct?

### How to Improve
- Claim via Google Business Profile or Google's entity verification
- Ensure consistent NAP (Name, Address, Phone) across web
- Add structured data (`Organization`, `LocalBusiness`, `Person` schema)
- Get mentions from authoritative sources (Wikipedia, Wikidata, news outlets)

### Schema for Entity Authority
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Brand Name",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "sameAs": [
    "https://twitter.com/brand",
    "https://linkedin.com/company/brand",
    "https://en.wikipedia.org/wiki/Brand"
  ],
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-800-000-0000",
    "contactType": "customer service"
  }
}
```

**`sameAs` is critical** — links the brand entity to its profiles across the web, strengthening entity disambiguation in Google's Knowledge Graph.

---

## 3. Brand Entity Strength

### Entity Signals to Evaluate
- **Wikipedia presence**: Does the brand have a Wikipedia page? Is it accurate?
- **Wikidata entry**: Is the brand in Wikidata with correct properties?
- **News mentions**: Coverage in authoritative publications (BBC, Forbes, industry press)
- **Academic/institutional citations**: Mentions in .edu, .gov, or research sources
- **Brand search volume**: Branded queries in GSC — growing or declining?

### Entity Authority Score
| Signal | Present | Weight |
|--------|---------|--------|
| Wikipedia page | ✅/❌ | High |
| Wikidata entry | ✅/❌ | High |
| Google Knowledge Panel | ✅/❌ | High |
| 10+ authoritative mentions | ✅/❌ | Medium |
| Consistent NAP across 20+ sources | ✅/❌ | Medium |
| Verified social profiles | ✅/❌ | Medium |

---

## 4. Reputation Management

### Review Signals
Check across platforms:
- **Google**: Rating, number of reviews, recency, response rate
- **Trustpilot / G2 / Capterra** (SaaS)
- **Yelp / TripAdvisor** (local/hospitality)
- **Amazon** (e-commerce)
- **Glassdoor** (employer brand)

**Health indicators:**
| Metric | Good | Warning | Critical |
|--------|------|---------|---------|
| Average rating | ≥ 4.0 | 3.5-3.9 | < 3.5 |
| Review recency | Last 30 days | Last 90 days | > 6 months |
| Response rate | > 80% | 50-80% | < 50% |
| Negative review % | < 10% | 10-25% | > 25% |

### Negative SERP Management
If negative results appear on page 1 for brand queries:
1. Identify the source and content
2. If inaccurate: contact publisher for correction or submit legal removal
3. If accurate: address root cause, respond publicly
4. Create/optimize positive content to displace: blog posts, press releases, social profiles, interviews
5. Build citations and links to positive pages

---

## 5. Brand Mentions & Unlinked Citations

### Finding Unlinked Mentions
Search for brand mentions without links:
```
"brand name" -site:yourdomain.com
```

These are **link building opportunities** — reach out to convert unlinked mentions to links.

### Brand Mention Monitoring
Recommended tools:
- Google Alerts (free)
- Mention.com
- Brand24
- Ahrefs Alerts

---

## 6. Social Profile Optimization for SEO

### Profiles to Audit
| Platform | Priority | Why |
|----------|----------|-----|
| Google Business Profile | Critical | Direct SERP influence |
| LinkedIn Company Page | High | Often ranks for brand searches |
| Twitter/X | Medium | Appears in knowledge panels (DAU declining vs Threads) |
| Threads (Meta) | High | 141.5M DAU Jan 2026 — surpassed X. Indexed by Google. |
| Bluesky | Medium | 30M+ users, growing 372% YoY. Profiles indexed by Google. |
| YouTube Channel | High | Second largest search engine |
| Facebook Page | Medium | Appears in local searches |
| Instagram | Medium | Visual brand presence |

**Each profile should have:**
- Consistent brand name (exact match)
- Complete bio with brand keywords
- Website link
- Verified status where available
- Regular activity (signals entity is active)

---

## 7. Tools by Function

### Brand Mention Monitoring
| Tool | Strengths | Pricing |
|------|-----------|---------|
| Google Alerts | Free, covers news + web | Free |
| Brand24 | Real-time, sentiment analysis, influencer reach | Paid |
| Mention | Multi-source, team collaboration | Paid |
| Ahrefs Alerts | Backlink + mention tracking combined | Paid (with Ahrefs) |
| Talkwalker | Enterprise, social listening + news | Paid |
| BuzzSumo | Content + mention research, brand tracking | Paid |

### Review Management
| Tool | Platform Coverage | Best For |
|------|-----------------|---------|
| Birdeye | Google, Facebook, 200+ sites | Multi-location businesses |
| Podium | Google, Facebook, reviews + messaging | Local businesses |
| Trustpilot | Own platform + widget for site | E-commerce, SaaS |
| ReviewTrackers | Aggregates 100+ review sites | Enterprise |
| Yext | Reviews + listings consistency | Multi-location + enterprise |
| Grade.us | White-label, agency-friendly | Agencies |

### Reputation SERP Management
| Tool | Purpose | Pricing |
|------|---------|---------|
| BrightLocal | Local SERP tracking + reputation | Paid |
| Semrush Brand Monitoring | Branded SERP tracking | Paid (with Semrush) |
| Moz | Domain authority + brand signals | Paid |
| Ahrefs | Backlinks + brand mentions | Paid |

### Knowledge Panel & Entity
| Tool | Purpose | Pricing |
|------|---------|---------|
| Google Search Console | Branded query performance | Free |
| Wikidata | Entity management | Free |
| Schema Markup Validator | Validate Organization schema | Free |
| Google Knowledge Panel claim | Direct entity verification | Free |

---

## 8. NavBoost, Share of Search & AI Brand Visibility

### NavBoost & Brand Signals

NavBoost is a confirmed Google ranking system (leaked 2023) that uses 13 months of click data — Chrome visits, dwell time, good/bad clicks — to re-rank results. Brand signals feed NavBoost directly:

- **Branded search volume** = users already know and seek your brand → strong positive signal
- **Social engagement that drives site visits** with good dwell time → NavBoost credit
- **Brand mentions on Reddit/forums** → users click through → NavBoost signal
- **Email campaigns** driving qualified traffic with good engagement → NavBoost credit

**Practical implication:** Growing branded search volume is not vanity — it's a direct ranking input. Invest in brand awareness as an SEO tactic.

### Share of Search

Developed by James Hankins & Les Binet (IPA). Correlates directly with market share (leading indicator — predicts future performance before it shows in revenue).

```
Share of Search = brand search volume / total category search volume

Example:
  Your brand: 10,000 searches/month
  Category total (all brands): 100,000 searches/month
  Share of Search: 10%

Track monthly with Google Trends (relative) or GSC + keyword tool (absolute).
Growing Share of Search = growing market share, typically 6-12 months ahead.
```

**Benchmark against competitors:**
```
GSC → Performance → Queries
→ Filter by branded terms
→ Compare monthly trend

Google Trends → Compare 4-5 brand names in your category
→ Relative search interest over time
```

### AI Brand Visibility

In 2026, AI Overviews appear in 25%+ of Google searches. 80% of branded searches end without a click. Your brand now needs visibility *inside* the SERP, not just rankings.

**Where your brand can appear:**
- Google AI Overviews (Gemini-powered)
- ChatGPT web browsing (700M weekly users)
- Perplexity citations
- Bing Copilot

**How to optimize for AI brand citations:**
1. **Wikipedia + Wikidata presence** — AI systems heavily cite Wikipedia for entity context
2. **Reviews on G2/Capterra/Trustpilot** — 28% of AI citations for B2B software come from review aggregators
3. **Reddit threads and forums** — AI Overviews cite Reddit for "real user experience" queries
4. **Structured data (Organization schema)** — complete `sameAs`, `description`, `foundingDate`
5. **Consistent brand messaging** — AI extracts your positioning from multiple sources; inconsistency = confusion

**Monitoring tools:**
| Tool | Purpose |
|------|---------|
| Otterly.ai | Track brand mentions in AI search responses |
| Profound | AI visibility analytics |
| Manual testing | Search brand + product terms in ChatGPT/Perplexity monthly |

**Add to Branded SERP Audit checklist:**
- Does an AI Overview appear for your brand name?
- Is the information accurate?
- Are competitor brands cited instead of yours?

---

## Output Format

### Brand SEO Health Report

**Brand:** [Name]
**Audit Date:** [Date]

#### Executive Summary
[2-3 sentences: Overall brand SERP health, biggest opportunity, biggest risk]

#### Branded SERP Snapshot
| Result Type | Present | Quality | Action |
|-------------|---------|---------|--------|
| Knowledge Panel | ✅/❌ | Good/Fair/Poor | ... |
| Sitelinks | ✅/❌ | Good/Fair/Poor | ... |
| Negative results (page 1) | ✅/❌ | - | ... |
| Social profiles | ✅/❌ | Good/Fair/Poor | ... |
| Review ratings | ✅/❌ | X.X/5 | ... |

#### Priority Actions
| Priority | Action | Expected Impact | Effort |
|----------|--------|-----------------|--------|
| 🔴 | [Critical action] | [Impact] | [Low/Med/High] |
| 🟡 | [High action] | [Impact] | [Low/Med/High] |
| 🟢 | [Quick win] | [Impact] | [Low/Med/High] |
