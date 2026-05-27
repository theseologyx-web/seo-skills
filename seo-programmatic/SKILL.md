---
name: seo-programmatic
description: >
  Programmatic SEO planning and analysis for pages generated at scale from data
  sources. Covers template engines, URL patterns, internal linking automation,
  thin content safeguards, and index bloat prevention. Use when user says
  "programmatic SEO", "pages at scale", "dynamic pages", "template pages",
  "generated pages", or "data-driven SEO".
user-invokable: true
argument-hint: "[url or plan]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Programmatic SEO Analysis & Planning

Build and audit SEO pages generated at scale from structured data sources.
Enforces quality gates to prevent thin content penalties and index bloat.

## Data Source Assessment

Evaluate the data powering programmatic pages:
- **CSV/JSON files**: Row count, column uniqueness, missing values
- **API endpoints**: Response structure, data freshness, rate limits
- **Database queries**: Record count, field completeness, update frequency
- Data quality checks:
  - Each record must have enough unique attributes to generate distinct content
  - Flag duplicate or near-duplicate records (>80% field overlap)
  - Verify data freshness; stale data produces stale pages

## Template Engine Planning

Design templates that produce unique, valuable pages:
- **Variable injection points**: Title, H1, body sections, meta description, schema
- **Content blocks**: Static (shared across pages) vs dynamic (unique per page)
- **Conditional logic**: Show/hide sections based on data availability
- **Supplementary content**: Related items, contextual tips, user-generated content
- Template review checklist:
  - Each page must read as a standalone, valuable resource
  - No "mad-libs" patterns (just swapping city/product names in identical text)
  - Dynamic sections must add genuine information, not just keyword variations

## URL Pattern Strategy

### Common Patterns
- `/tools/[tool-name]`: Tool/product directory pages
- `/[city]/[service]`: Location + service pages
- `/integrations/[platform]`: Integration landing pages
- `/glossary/[term]`: Definition/reference pages
- `/templates/[template-name]`: Downloadable template pages

### URL Rules
- Lowercase, hyphenated slugs derived from data
- Logical hierarchy reflecting site architecture
- No duplicate slugs; enforce uniqueness at generation time
- Keep URLs under 100 characters
- No query parameters for primary content URLs
- Consistent trailing slash usage (match existing site pattern)

## Internal Linking Automation

- **Hub/spoke model**: Category hub pages linking to individual programmatic pages
- **Related items**: Auto-link to 3-5 related pages based on data attributes
- **Breadcrumbs**: Generate BreadcrumbList schema from URL hierarchy
- **Cross-linking**: Link between programmatic pages sharing attributes (same category, same city, same feature)
- **Anchor text**: Use descriptive, varied anchor text. Avoid exact-match keyword repetition
- Link density: 3-5 internal links per 1000 words (match seo-content guidelines)

## Thin Content Safeguards

### Quality Gates

| Metric | Threshold | Action |
|--------|-----------|--------|
| Pages without content review | 100+ | ⚠️ WARNING: require content audit before publishing |
| Pages without justification | 500+ | 🛑 HARD STOP: require explicit user approval and thin content audit |
| Unique content per page | <50% | ⚠️ WARNING: approaching thin content threshold (50% minimum post-2026 enforcement) |
| Unique content per page | <40% | ❌ HARD STOP: high penalty risk — require human review and approval before publishing |
| Word count per page | <300 | ⚠️ Flag for review (may lack sufficient value) |

### Scaled Content Abuse: Enforcement Context (2025-2026)

Google's Scaled Content Abuse policy (introduced March 2024) saw major enforcement escalation in 2025:

- **June 2025:** Wave of manual actions targeting websites with AI-generated content at scale
- **August 2025:** SpamBrain spam update enhanced pattern detection for AI-generated link schemes and content farms
- **March 2026:** Core update explicitly targeted scaled content abuse as primary enforcement action. Sites with 30-40% unique content increasingly penalized; industry threshold shifted to 50% minimum safe threshold.
- **Result:** Google reported 45% reduction in low-quality, unoriginal content in search results post-March 2024 enforcement

**Enhanced quality gates for programmatic pages:**
- **Content differentiation:** ≥50% of content must be genuinely unique between any two programmatic pages (not just city/keyword string replacement); 30-40% is no longer a safe buffer post-March 2026
- **Human review:** Minimum 5-10% sample review of generated pages before publishing
- **Progressive rollout:** Publish in batches of 50-100 pages. Monitor indexing and rankings for 2-4 weeks before expanding. Never publish 500+ programmatic pages simultaneously without explicit quality review.
- **Standalone value test:** Each page should pass: "Would this page be worth publishing even if no other similar pages existed?"
- **Site reputation abuse:** If publishing programmatic content under a high-authority domain (not your own), this may trigger site reputation abuse penalties. Google began enforcing this aggressively in November 2024.

> **Recommendation (updated post-March 2026):** Industry standard has shifted. Set WARNING at `<50%` unique content and HARD STOP at `<40%`. The previous 30-40% range is no longer a safe buffer.

### Safe Programmatic Pages (OK at scale)
✅ Integration pages (with real setup docs, API details, screenshots)
✅ Template/tool pages (with downloadable content, usage instructions)
✅ Glossary pages (200+ word definitions with examples, related terms)
✅ Product pages (unique specs, reviews, comparison data)
✅ Data-driven pages (unique statistics, charts, analysis per record)

### Penalty Risk (avoid at scale)
❌ Location pages with only city name swapped in identical text
❌ "Best [tool] for [industry]" without industry-specific value
❌ "[Competitor] alternative" without real comparison data
❌ AI-generated pages without human review and unique value-add
❌ Pages where >60% of content is shared template boilerplate

### Uniqueness Calculation
Unique content % = (words unique to this page) / (total words on page) × 100

Measure against all other pages in the programmatic set. Shared headers, footers, and navigation are excluded from the calculation. Template boilerplate text IS included.

## AI-Assisted Programmatic Content

AI can be a force multiplier for programmatic SEO — but only when used as a data enrichment layer, not as a template filler.

### The Core Distinction

| Usage Type | Example | Google's View | SEO Outcome |
|------------|---------|---------------|-------------|
| **AI as template filler** | AI generates the same boilerplate text for 5,000 city pages, swapping city name | Scaled content abuse | Penalty risk |
| **AI as data enrichment** | AI analyzes unique data per record and writes a 2-sentence insight specific to that record | Adds unique value | Safe + effective |

### Responsible AI Framework

**What AI can do per programmatic page:**
- Summarize unique data attributes into a 1-2 sentence insight
- Generate a market-specific commentary based on data differences between records
- Write a unique FAQ answer per record based on actual record data
- Score or classify records and explain the classification in natural language
- Generate a "why this matters" paragraph from numeric data (e.g. stats, reviews, pricing)

**What AI should NOT do:**
- Write the main body content using the same template prompt across all pages
- Paraphrase shared boilerplate to create artificial "uniqueness"
- Generate content without access to the actual unique data for that record

### Quality Scoring Automation

Build a uniqueness check into your generation pipeline:

```python
# Pseudocode: uniqueness check per page before publishing
def check_uniqueness(page_content, all_pages_content):
    unique_words = set(page_content.split()) - shared_boilerplate_words
    uniqueness_ratio = len(unique_words) / len(page_content.split())

    if uniqueness_ratio < 0.40:
        return "HARD STOP — do not publish"
    elif uniqueness_ratio < 0.50:
        return "WARNING — review before publishing"
    else:
        return "PASS"
```

### Human Review Sampling

Even with AI enrichment, human review remains mandatory:
- **Minimum:** Review 5-10% of generated pages before publishing any batch
- **Sampling method:** Random sample + lowest-scoring pages by uniqueness metric
- **Check for:** Hallucinated data, factual errors, AI tells ("As an AI language model..."), generic phrasing that doesn't reflect the actual record data
- **Approval gate:** One human must sign off per batch before publishing

### Progressive Rollout with AI Content

```
Batch 1: 50 pages → publish → monitor 2 weeks
  └─ Check: indexing rate, rankings, CTR, bounce rate
Batch 2: 100 pages → publish → monitor 2 weeks
  └─ Check: same metrics + crawl stats in GSC
Batch 3+: Scale up if no quality signals trigger
  └─ Stop if: GSC shows manual action, deindexing spike, or rankings drop
```

---

## Canonical Strategy

- Every programmatic page must have a self-referencing canonical tag
- Parameter variations (sort, filter, pagination) canonical to the base URL
- Paginated series: canonical to page 1 or use rel=next/prev
- If programmatic pages overlap with manual pages, the manual page is canonical
- No canonical to a different domain unless intentional cross-domain setup

## Sitemap Integration

- Auto-generate sitemap entries for all programmatic pages
- Split at 50,000 URLs per sitemap file (protocol limit)
- Use sitemap index if multiple sitemap files needed
- `<lastmod>` reflects actual data update timestamp (not generation time)
- Exclude noindexed programmatic pages from sitemap
- Register sitemap in robots.txt
- Update sitemap dynamically as new records are added to data source

## Index Bloat Prevention

- **Noindex low-value pages**: Pages that don't meet quality gates
- **Pagination**: Noindex paginated results beyond page 1 (or use rel=next/prev)
- **Faceted navigation**: Noindex filtered views, canonical to base category
- **Crawl budget**: For sites with >10k programmatic pages, monitor crawl stats in Search Console
- **Thin page consolidation**: Merge records with insufficient data into aggregated pages
- **Regular audits**: Monthly review of indexed page count vs intended count

## Output

### Programmatic SEO Score: XX/100

### Assessment Summary
| Category | Status | Score |
|----------|--------|-------|
| Data Quality | ✅/⚠️/❌ | XX/100 |
| Template Uniqueness | ✅/⚠️/❌ | XX/100 |
| URL Structure | ✅/⚠️/❌ | XX/100 |
| Internal Linking | ✅/⚠️/❌ | XX/100 |
| Thin Content Risk | ✅/⚠️/❌ | XX/100 |
| Index Management | ✅/⚠️/❌ | XX/100 |

### Critical Issues (fix immediately)
### High Priority (fix within 1 week)
### Medium Priority (fix within 1 month)
### Low Priority (backlog)

### Recommendations
- Data source improvements
- Template modifications
- URL pattern adjustments
- Quality gate compliance actions

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Content quality and thin content audit | `/seo content <url>` | E-E-A-T signals, uniqueness check, doorway page risk per page |
| Keyword strategy for URL patterns | `/seo keywords [topic]` | Cluster-based URL structure, intent mapping per template type |
| Sitemap generation for large programmatic sites | `/seo sitemap <url>` | Handles 50K-URL split, `lastmod`, index/noindex enforcement |
| Crawl budget and index bloat management | `/seo technical <url>` | Crawl stats, noindex audit, faceted navigation controls |
| Internal linking automation | `/seo internal-linking` | Hub/spoke architecture and anchor text strategy at scale |
| Full SEO plan incorporating programmatic content | `/seo plan [business-type]` | Places programmatic SEO in Phase 3 (Scale) of the roadmap |

---

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable | Report connection error with status code. Suggest verifying URL accessibility and checking for authentication requirements. |
| No programmatic pages detected | Inform user that no template-generated or data-driven page patterns were found. Suggest checking if pages use client-side rendering or if the URL points to the correct section. |
| Thin content threshold exceeded | Trigger quality gate warning. Report the unique content percentage and flag pages below 40% uniqueness. Require user acknowledgment before proceeding. |
| Quality gate violation | Halt analysis at the HARD STOP threshold (500+ pages without justification or <40% unique content). Present findings and require explicit user approval to continue. |
