---
name: seo-backlinks
description: >
  Backlink profile analysis: referring domains, anchor text distribution,
  toxic link detection, competitor gap analysis. Requires DataForSEO extension.
  Use when user says "backlinks", "link profile", "referring domains",
  "anchor text", "toxic links", "link gap", "link building",
  "disavow", or "backlink audit".
  Not for building new links or outreach — use seo-link-building.
user-invokable: true
argument-hint: "<url>"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
compatibility: "Requires DataForSEO MCP server (extension)"
metadata:
  author: AgriciDaniel
  version: "1.7.2"
  category: seo
---

# Backlink Profile Analysis

> **Skill relacionado:** `seo-link-building` cubre la adquisición proactiva de backlinks (outreach, guest post, digital PR, broken link building). Este skill (`seo-backlinks`) se enfoca en el análisis del perfil existente — radiografía actual, tóxicos, anchor text, comparación con competidores. Para identificar oportunidades de adquisición y ejecutar campañas, usar `seo-link-building`.

This skill requires the DataForSEO extension:
```bash
./extensions/dataforseo/install.sh
```

**Check availability:** Before using backlink tools, verify the DataForSEO MCP server
is connected by checking if `dataforseo_backlinks_summary` or similar tools are available.
If unavailable, inform the user and provide install instructions.

## Quick Reference

| Command | Purpose |
|---------|---------|
| `/seo backlinks <url>` | Full backlink profile analysis |
| `/seo backlinks gap <url1> <url2>` | Competitor backlink gap analysis |
| `/seo backlinks toxic <url>` | Toxic link detection and disavow recommendations |
| `/seo backlinks new <url>` | New and lost backlinks (recent changes) |

## Analysis Framework

When analyzing a backlink profile, produce all 7 sections below.

### 1. Profile Overview

Use `dataforseo_backlinks_summary` to get:
- Total backlinks and referring domains
- Domain rank / authority score
- Follow vs nofollow ratio
- Historical trend (growing, stable, or declining)

**Scoring:**

| Metric | Good | Warning | Critical |
|--------|------|---------|----------|
| Referring domains | >100 | 20-100 | <20 |
| Follow ratio | >60% | 40-60% | <40% |
| Domain diversity | No single domain >5% | 1 domain >10% | 1 domain >25% |
| Trend | Growing or stable | Slow decline | Rapid decline (>20%/quarter) |

### 2. Anchor Text Distribution

Use `dataforseo_backlinks_anchors` to analyze anchor text patterns.

**Healthy distribution benchmarks:**

| Anchor Type | Target Range | Over-Optimization Signal |
|-------------|-------------|-------------------------|
| Branded (company/domain name) | 30-50% | <15% |
| URL/naked link | 15-25% | N/A |
| Generic ("click here", "learn more") | 10-20% | N/A |
| Exact match keyword | 3-10% | >15% |
| Partial match keyword | 5-15% | >25% |
| Long-tail / natural | 5-15% | N/A |

Flag if exact-match anchors exceed 15% -- this is a Google Penguin risk signal.

### 3. Referring Domain Quality

Use `dataforseo_backlinks_referring_domains` to assess link quality.

Analyze:
- **TLD distribution**: .edu, .gov, .org = high authority. Excessive .xyz, .info = low quality
- **Country distribution**: Match target market. 80%+ from irrelevant countries = PBN signal
- **Language distribution**: Should match site language. Foreign-language links can be spam
- **Domain rank distribution**: Healthy profiles have links from all authority tiers
- **Follow/nofollow per domain**: Sites that only nofollow = limited SEO value

### 4. Toxic Link Detection

Identify potentially harmful backlinks using these patterns:

**High-risk indicators (flag immediately):**
- Links from known PBN (Private Blog Network) domains
- Unnatural anchor text patterns (100% exact match from a domain)
- Links from penalized or deindexed domains
- Mass directory submissions (50+ directory links)
- Comment spam links (forum/blog comment backlinks at scale)
- Link farms (sites with 10K+ outbound links per page)
- Foreign-language links to English sites (unless intentional)
- Paid link patterns (footer/sidebar links across all pages of a domain)
- **AI-generated guest post farms** (SpamBrain 2025+): high volume of thin, AI-generated guest posts embedding paid backlinks, no editorial oversight — targeted specifically in October 2025 spam update
- **Site Reputation Abuse / Parasite SEO**: links from high-authority domains where the content section operates independently (e.g. /blog run by a third party). Google "decouples" authority algorithmically since August 2025 — these links carry minimal or zero value

**Medium-risk indicators (review manually):**
- Links from unrelated niches
- Reciprocal link patterns (A links to B, B links to A)
- Links from thin content pages (<100 words)
- Excessive links from a single domain (>50 backlinks from 1 domain)

**Output:**
```
Toxic Link Report
=================
Total backlinks analyzed: X
Potentially toxic: Y (Z%)

High Risk (recommend disavow):
  [list with domain, reason, anchor text]

Medium Risk (review manually):
  [list with domain, reason, anchor text]
```

### 5. Top Pages by Backlinks

Use `dataforseo_backlinks_backlinks` with target type "page" to find:
- Which pages attract the most backlinks
- Pages with high-authority links (link magnets)
- Pages with zero backlinks (internal linking opportunities)
- 404 pages with backlinks (redirect opportunities to reclaim link equity)

### 6. Competitor Gap Analysis

For `/seo backlinks gap <url1> <url2>`:

Use `dataforseo_backlinks_referring_domains` for both domains and compare:
- Domains linking to competitor but NOT to target = link building opportunities
- Domains linking to both = validate existing relationships
- Domains linking only to target = competitive advantage

**Output:**
```
Competitor Gap: example.com vs competitor.com
=============================================
Competitor has links from X domains you don't
You have links from Y domains competitor doesn't

Top 20 Link Building Opportunities:
  [domain, authority, anchor used for competitor, contact suggestion]
```

### 7. New and Lost Backlinks

Use `dataforseo_backlinks_backlinks` with date filters to track:
- New backlinks in last 30/60/90 days
- Lost backlinks in last 30/60/90 days
- Link velocity (new per month trend)

**Red flags:**
- Sudden spike in new links (possible negative SEO attack)
- Sudden loss of many links (site penalty or content removal)
- Declining velocity over 3+ months (content not attracting links)

## 8. rel Attributes — Nofollow, Sponsored, UGC

| Attribute | Purpose | Google Treatment |
|-----------|---------|-----------------|
| `rel="nofollow"` | "I don't vouch for this link" | Hint — Google may still pass PageRank |
| `rel="sponsored"` | Paid/affiliate links | Hint — identifies commercial relationship |
| `rel="ugc"` | User-generated content | Hint — lower trust weight |

**All three are hints, not directives (since 2019).** Google may follow nofollow links if deemed valuable.

**Apply on outbound links:**
- Affiliate/paid → `rel="sponsored"`
- Comment section → `rel="ugc nofollow"`
- Links you don't endorse → `rel="nofollow"`
- Internal links → never add nofollow

**In backlink audit context:**
- High % nofollow inbound → reduced PageRank benefit
- All inbound nofollow → no ranking signal regardless of quantity
- Inbound `sponsored` from legitimate sites → still brand value
- Dofollow links without `sponsored` on paid placements → manual action risk for the linking site

**Link schemes that trigger SpamBrain:**
- Buying dofollow links without `rel="sponsored"`
- Excessive link exchanges without disclosure
- PBN links (Private Blog Networks)
- Site-wide footer/widget links without nofollow

## 8.5 Spam Update Timeline (Backlink-Relevant)

| Update | Date | Impact |
|--------|------|--------|
| Link Spam Update | Dec 2022 | First major SpamBrain link devaluation |
| SpamBrain expansion | Oct 2023 | Scaled to more languages and link patterns |
| Site Reputation Abuse (manual) | Mar 2024 | Parasite SEO manual actions begin |
| **Site Reputation Abuse (algorithmic)** | **Aug 2025** | Full algorithmic enforcement — authority decoupling |
| **AI Guest Post Farms** | **Oct 2025** | SpamBrain targeting AI-generated guest posts at scale |
| **March 2026 Spam Update** | **Mar 2026** | Fastest ever (<20 hours). Excludes link spam and site reputation abuse from scope — those have separate enforcement. |

**Key implication (2026):** Backlinks from sites running independent third-party content sections (Forbes Advisor model) carry significantly reduced or zero value. Focus link building on sites with genuine editorial control.

## 8.6 Brand Mentions as Off-Page Signal

In 2026, the off-page signal balance has shifted:
- **~45%** — traditional backlinks (link equity, PageRank)
- **~55%** — entity/brand signals (unlinked mentions, co-citation, brand search volume)

**Practical implication:** Brand mention campaigns now outperform pure link building for authority in competitive niches. Always include:
1. **Monitor unlinked mentions** (Google Alerts, Brand24, Ahrefs Alerts)
2. **Convert to links** where possible (reach out for attribution)
3. **Accept unlinked mentions as value** — Google's NLP/entity recognition processes them regardless of a link

## 9. Link Building Strategy & Guest Posting

After the audit, always provide a proactive link acquisition plan:

### Guest Posting
- Identify relevant blogs/publications in the niche accepting guest contributors
- Qualify targets: Domain Authority >30, real traffic, editorial standards, topic relevance
- Outreach process: find editor contact, pitch angle aligned to their audience, include writing samples
- Anchor text strategy: branded or partial-match anchors only — never exact-match in guest posts
- Track placements: record URL, DA, anchor used, date published

### Digital PR & Link Bait
- Original research, data studies, surveys → earns editorial links from journalists
- Tools: Featured.com (HARO relaunched April 2025), Qwoted, Source of Sources (SOS — free, by HARO founder Peter Shankman), #JournoRequest on X, SourceBottle — respond to journalist queries
  - ~~Connectively~~ closed December 9, 2024 — remove from any active workflows
- Infographics, tools, calculators → naturally attract links

### Broken Link Building
```bash
# 1. Find competitor's top linked pages (use dataforseo_backlinks_backlinks)
# 2. Check if those URLs are still live
curl -o /dev/null -s -w "%{http_code}" [URL]
# 3. If 404 → offer your content as replacement to linking sites
```

### Resource Page Link Building
- Search: `site:example.com "resources"` or `intitle:"useful links" [niche]`
- Identify resource pages in your niche and pitch your best content as addition

### Reclaiming Lost & Unlinked Mentions
- Use `dataforseo_backlinks_backlinks` with date filters to find lost links → contact site to restore
- Search for brand mentions without links → request attribution link

### Outreach Template
```
Subject: Resource suggestion for [their page title]

Hi [Name],

I came across your [page] and noticed you link to [competitor resource].
We published [your content] which covers [specific angle they don't].

Would you consider adding it as an additional resource?

[Your name]
```

## Backlink Health Score

Calculate a 0-100 score based on:

| Factor | Weight | Scoring |
|--------|--------|---------|
| Referring domain count | 20% | Scale based on industry benchmarks |
| Domain quality distribution | 20% | % from authority domains (DR 40+) |
| Anchor text naturalness | 15% | Penalty for over-optimization |
| Toxic link ratio | 20% | <2% = full score, >10% = 0 |
| Link velocity trend | 10% | Growing = full, declining = partial |
| Follow/nofollow ratio | 5% | >60% follow = full score |
| Geographic relevance | 10% | % from target market countries |

## Output Format

### Backlink Health Score: XX/100

| Section | Status | Score |
|---------|--------|-------|
| Profile Overview | pass/warn/fail | XX/100 |
| Anchor Distribution | pass/warn/fail | XX/100 |
| Referring Domain Quality | pass/warn/fail | XX/100 |
| Toxic Links | pass/warn/fail | XX/100 |
| Top Pages | info | N/A |
| Link Velocity | pass/warn/fail | XX/100 |

### Critical Issues (fix immediately)
### High Priority (fix within 1 month)
### Medium Priority (ongoing improvement)
### Link Building Opportunities (top 10)

## Error Handling

| Error | Cause | Resolution |
|-------|-------|-----------|
| DataForSEO tools unavailable | Extension not installed | Run `./extensions/dataforseo/install.sh` |
| No backlink data returned | Domain too new or very small | Note: small sites may have <10 backlinks |
| API credits insufficient | DataForSEO balance low | Check balance at app.dataforseo.com |

**Graceful fallback:** If DataForSEO is unavailable:
1. Inform user that backlink analysis requires the DataForSEO extension
2. Offer to check robots.txt and sitemap for basic link signals instead
3. Suggest manual tools: Ahrefs Webmaster Tools (free), Google Search Console Links report

## Reference Documentation

Load on demand (do NOT load at startup):
- `references/backlink-quality.md` -- Detailed toxic link patterns and scoring methodology
