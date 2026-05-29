# SEO Competitor Audit

Use this skill when the task is to audit named competitors deeply, one by one, in order to understand why they win in search and where the target site can beat them.

This skill is not for generic benchmarking or broad market summaries. It performs advanced audits of actual competing domains, subfolders, or page sets.

## When to use

Use this skill when the user asks for:
- a deep SEO audit of competitors,
- a competitor teardown,
- a domain-by-domain comparison,
- reasons specific competitors outrank the site,
- competitive gap analysis grounded in real pages and real structures.

Route to other skills when needed:
- `seo-benchmark` for market averages and sector norms.
- `seo-competitive` for high-level multi-rival synthesis.
- `seo-backlinks` for analysis of the target site's current backlink profile.
- `seo-link-building` for acquisition planning.
- `seo-technical` for deeper technical validation.
- `seo-page` for single-page page-level comparison.
- `seo-aeo`, `seo-llmo`, `seo-ai-search-readiness` for deeper AI Search decomposition.

## Core objective

Audit each competitor as if it were a client site, but interpret findings comparatively.
The goal is not only to describe what the competitor has, but to explain:
- why it may be outperforming,
- which advantages are real vs superficial,
- which gaps are attackable,
- which differences matter strategically.

## Required audit dimensions per competitor

### 1. Search positioning
- Main keyword territories.
- Estimated visibility patterns.
- SERP presence by intent and by page type.
- Branded vs non-branded footprint.

### 2. Architecture and crawl logic
- URL taxonomy and directory logic.
- Information architecture.
- Category depth.
- Pagination, faceting, filters, indexation strategy.
- International/local structure where relevant.

### 3. Content system
- Content types covered.
- Template quality.
- Depth, freshness, helpfulness, and specificity.
- Topical authority and cluster strategy.
- Commercial vs informational balance.
- Comparison, alternative, use-case, FAQ, and problem-solving content.

### 4. On-page and search appearance
- Titles, descriptions, headings, entity reinforcement.
- Schema use.
- Snippet shaping.
- Rich result eligibility signals.
- Answerability and citation friendliness.

### 5. Internal linking
- Hub pages.
- Silo logic.
- Contextual internal links.
- Cross-template reinforcement.
- Orphan-risk patterns where visible.

### 6. Technical and rendered quality
- Crawlability and indexability clues.
- Rendering dependence.
- Canonicalization clues.
- Duplication patterns.
- Broken template patterns.
- Mobile/rendered discoverability.

### 7. Performance and UX quality
- Core Web Vitals clues where available.
- Layout stability clues.
- Visual clarity.
- CTA and conversion friction.
- Trust elements and page usability.

### 8. Backlinks and authority posture
- Referring domain quality patterns.
- Link type mix.
- Velocity patterns if visible.
- Linkable asset footprint.
- Authority concentration by section or page type.

### 9. Brand and entity footprint
- Knowledge Panel / brand SERP clues.
- Entity consistency.
- About, authorship, trust, editorial transparency.
- Reputation signals visible in search.

### 10. AI Search and machine readiness
- GEO / citation potential.
- AEO / answer extraction quality.
- LLMO / passage clarity.
- Agentic Browsing / machine interaction readiness where relevant.
- `llms.txt`, machine-readable summaries, structured interaction surfaces where applicable.

## Required output structure

```yaml
competitor_audit_output:
  audited_competitors:
    - competitor: ""
      competitor_type: direct | indirect | serp_competitor | marketplace | publisher
      search_positioning: []
      architecture_findings: []
      content_findings: []
      onpage_findings: []
      schema_findings: []
      internal_linking_findings: []
      technical_findings: []
      performance_ux_findings: []
      backlink_findings: []
      brand_entity_findings: []
      ai_search_findings: []
      strongest_advantages: []
      weakest_areas: []
      attackable_gaps: []
      strategic_threat_level: low | medium | high
  cross_competitor_patterns:
    repeated_wins: []
    repeated_weaknesses: []
    whitespace_opportunities: []
    strategic_implications: []
  recommended_handoffs:
    - seo-competitive
    - seo-benchmark
    - seo-link-building
    - seo-technical
    - seo-aeo
    - seo-llmo
```

## Interpretation rules

- Do not confuse visibility with quality; explain which advantages are structural vs brand-driven vs authority-driven.
- Do not stop at "they have more content" or "they have more links". Explain why their system compounds performance.
- Distinguish between competitors that are true business rivals and competitors that only overlap in SERPs.
- Separate attackable gaps from non-attackable advantages.
- Identify opportunities that can realistically be captured in 30, 60, and 90+ day horizons.
