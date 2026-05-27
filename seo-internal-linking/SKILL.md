---
name: internal-linking
description: >
  Analyze and optimize a site's internal link structure for SEO. Use whenever
  the user mentions internal linking, link structure, orphan pages, topic
  clusters, anchor text optimization, link equity distribution, or site
  architecture for SEO. Also trigger when someone asks "how do I link my pages
  better?" or "which pages need more internal links?" or shares a sitemap and
  wants linking advice. This skill diagnoses linking issues and recommends
  specific link additions—it does not modify the site unless explicitly asked.
user-invokable: true
argument-hint: "[url or sitemap]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  version: "1.1.0"
  category: seo
---

# Internal Linking Optimizer

You help users improve SEO through smarter internal linking.

Internal links are one of the few ranking levers site owners fully control. They tell search engines which pages matter, how topics relate, and where authority should flow. A well-linked site helps both crawlers and users find content — a poorly-linked site hides its own best pages.

---

## CRITICAL: Data Extraction Method

**WebFetch strips the HTML `<head>` section** when converting to markdown. This means it CANNOT see canonical tags, hreflang, or meta robots directives — all of which affect internal linking analysis (e.g., detecting canonicalized duplicates or noindex pages that shouldn't receive links).

### Correct approach for each page analyzed:

**Use `curl` via Bash for `<head>` tags:**
```bash
curl -sL [URL] | grep -i -E '(rel="canonical"|meta name="robots"|hreflang|<title)'
```

**Use WebFetch for body content:** internal links, anchor text, navigation, footer, and visible page structure.

**NEVER report canonical tags or meta robots as "not found" based solely on WebFetch.** Always verify with `curl`.

---

## Before You Start

Understand the scope before diving in:

1. **What's the goal?** Full link audit, fix specific pages, or plan links for new content?
2. **Site size?** A 30-page site needs different treatment than a 5,000-page site.
3. **What data is available?** Sitemap, crawl data, analytics, or just a list of URLs?

If the user provides a URL or sitemap, work with that. If data is limited, state assumptions and proceed — partial analysis is better than no analysis.

### Getting Data

Adapt to what's available:

- **With crawl data or sitemap**: Map the full link graph, identify orphans, calculate link distribution.
- **With analytics**: Prioritize by traffic — fix linking issues on pages that already get visitors first.
- **With just URLs or content**: Analyze topical relevance and recommend contextual links.
- **Manual only**: Ask for key page URLs, content categories, and topic clusters. Note which findings come from analysis vs. inference.

---

## Core Analysis Areas

Work through whichever areas are relevant to the user's request. Not every audit needs all sections.

### 1. Link Distribution Overview

Map how internal links are spread across the site. The goal: important pages should have more internal links pointing to them, and no valuable page should be an orphan.

Look for:
- **Orphan pages** (zero internal links) — these are invisible to crawlers navigating the site
- **Under-linked important pages** — high-value pages with few internal links
- **Over-concentrated linking** — homepage or nav pages hoarding most links while deeper content starves
- **Average links per page** — context for whether the site links too little or too much

Report the distribution shape: is linking top-heavy (all links go to a few pages), flat (evenly spread), or scattered?

### 2. Orphan Page Detection

Orphan pages are the highest-priority fix because they're essentially hidden from search engine crawlers following internal links.

**Detection methods (use in combination):**

- **Screaming Frog** — crawl full site → filter "Inlinks = 0" in the Pages tab on indexable pages. Cross-reference against Sitemap list to find pages in sitemap with no internal links.
- **GSC + crawl cross-reference** — export all URLs with impressions from GSC → compare against Screaming Frog inlinks data → pages with GSC impressions but 0 inlinks = "hidden rankers" (ranking without link support, vulnerable to ranking drops)
- **GA4 orphan detection** — pages with sessions (traffic) but 0 crawl-discovered inlinks = pages users are finding but crawlers might miss
- **JetOctopus / Botify** — ML-based orphan detection for large sites (10K+ pages); surfaces orphan clusters by page template type

For each orphan found, classify by priority:
- **High priority**: Has traffic or rankings despite being orphaned — linking to it will amplify existing performance
- **Medium priority**: Relevant content that should be discoverable — link from related pages
- **Low priority**: Outdated or thin content — consider redirecting or removing instead of linking

Always recommend *specific source pages* to link from, not just "add links."

### 3. Topic Cluster Linking

Topic clusters work when the pillar page and cluster articles are tightly interlinked. Check for:

- Does the pillar link to all its cluster articles?
- Does each cluster article link back to the pillar?
- Do related cluster articles cross-link to each other?
- Are there cluster articles that exist but aren't connected to any pillar?

Map the current state (what's linked, what's missing) and recommend the specific links to add. Think of it as closing gaps in a web — every piece of the cluster should be reachable from every other piece within 1-2 clicks.

### 4. Anchor Text Analysis

Anchor text tells search engines what the target page is about. Problems to look for:

- **Generic anchors** ("click here", "read more", "this article") — wasted signals
- **Over-optimized anchors** — same exact-match keyword used repeatedly, which looks manipulative
- **Mismatched anchors** — anchor text doesn't describe the target page's content
- **No variety** — all links to a page use identical text

A healthy anchor text profile for any target page should roughly follow:
- Exact match keyword: 10–20%
- Partial match / variations: 30–40%
- Branded or natural phrases: 20–30%
- Generic / contextual: 10–20%

**Important:** These ratios are derived from backlink profile analysis. For internal links specifically, Google treats anchor text signals differently than external backlinks — exact-match internal anchors are generally safe at higher ratios (up to 40-50%) without manipulation concerns. The distribution guidance above is most relevant when auditing external backlink profiles.

When recommending anchor text changes, suggest 3-4 variations for each target page.

### 5. Contextual Link Opportunities

Find places in existing content where a relevant internal link could naturally be added. This is where the biggest quick wins usually are.

For each opportunity, provide:
- **Source page** and approximate location (which section or paragraph)
- **Target page** to link to
- **Suggested anchor text**
- **Why this link makes sense** (topical match, user journey, authority distribution)

Prioritize opportunities where:
- The source page has high authority or traffic (link passes more value)
- The target page needs a ranking boost
- The topical connection is strong and natural

---

## Prioritization

Not all link changes are equal. Rank recommendations by impact:

1. **Fix orphan pages with existing traffic/rankings** — immediate ROI
2. **Link to under-linked money pages** from high-authority pages — direct ranking benefit
3. **Complete topic cluster connections** — strengthens topical authority
4. **Replace generic anchor text** on high-value links — improves signal quality
5. **Add contextual links in high-traffic content** — leverages existing audience
6. **Navigation and footer adjustments** — site-wide impact but lower per-link value

---

## Output Guidelines

Adapt the format to the request:

- **Full audit**: Start with an overview (total pages, total links, average links/page, orphan count), then findings by area, then a prioritized action plan.
- **Specific page optimization**: Focus on links to add and links to request from other pages.
- **New content planning**: Recommend which existing pages to link from and to, with anchor text.

For every link recommendation, always include: source page, target page, and suggested anchor text. Vague advice like "add more internal links" is not useful — be specific.

### Validation Checks

Before finalizing output, verify:
- Every recommendation cites a specific source page, target page, and anchor text
- Orphan page lists include URLs and clear next steps
- Data source is stated (crawl data, analytics, user-provided, or inferred)
- Recommendations are prioritized, not just listed

---

## Example

**User**: "Find internal linking opportunities for my blog post about email marketing best practices"

**Good output structure**:

1. Current state: The post at `/blog/email-marketing-best-practices/` has only 2 internal links.

2. Links to add from this post:
   - "building your email list" → `/blog/grow-email-list/` (paragraph 2, topical match)
   - "write compelling subject lines" → `/blog/email-subject-lines/` (paragraph 5)
   - "segment your audience" → `/blog/email-segmentation-guide/` (segmentation section)

3. Pages that should link TO this post:
   - `/blog/digital-marketing-guide/` — email section, anchor: "email marketing best practices"
   - `/services/marketing-services/` — related content area, anchor: "email marketing strategies"

4. Priority: Add the 3 outbound links first (you control the page), then request inbound links from the related pages.

---

## Quantitative Guidelines

Use these benchmarks to assess link density:

| Metric | Guideline | Notes |
|--------|-----------|-------|
| Contextual links per 1,000 words | 2–5 | More is fine if topically natural; fewer = under-exploited |
| Maximum total links per page | ~150 | Beyond this, individual link equity dilutes significantly |
| Pillar page inlinks | 20+ | High-authority page needs proportional internal support |
| Cluster page inlinks | 5–15 | Pillar + neighboring cluster pages |
| Supporting page inlinks | 2–5 | Cluster parent + 1-2 related pages |
| Link audit frequency | Monthly for pages >10K visits | Quarterly for smaller sites |
| Freshness signal | Add internal links to updated content | Signals re-crawl priority to Google |

---

## Internal Linking for AI Visibility

Bi-directional internal linking increases AI citation probability by 2.7x. AI systems (Google AI Overviews, Perplexity, ChatGPT) use link relationships to build entity graphs and assess topical authority.

**Bi-directional linking is mandatory for AI visibility:**
- Pillar → cluster: exists in most sites ✓
- Cluster → pillar: often missing — every cluster page must link back to its pillar
- Cluster ↔ cluster: sibling pages should cross-link when content genuinely overlaps

**Entity relationship reinforcement:** when linking between pages, include both primary entity names in the surrounding sentence. This reinforces semantic relationships that AI systems use for citation decisions.

**Link freshness for AI re-indexing:** add new internal links to existing pages when publishing new cluster content. This triggers re-crawl of the linked page, updating the AI system's understanding of the cluster's depth.

---

## E-commerce Internal Linking Patterns

E-commerce sites have unique linking requirements that differ from editorial sites:

| Pattern | Implementation | SEO purpose |
|---------|---------------|------------|
| Product → category (upward) | "See all [category]" link in product page | Passes authority up; confirms taxonomy |
| Category → subcategory | Faceted navigation or section nav | Distributes authority through hierarchy |
| Product → related products | "You might also like" module | Builds topical depth; reduces bounce |
| Product → complementary products | "Complete the look" / "Frequently bought together" | Cross-silo contextual links |
| Recently viewed | Personalized internal linking | Crawled by Googlebot; passes equity |
| Breadcrumbs | `Home > Category > Subcategory > Product` + BreadcrumbList schema | Critical for deep e-commerce hierarchies |
| Category landing → editorial | "Learn more about [category]" → blog guide | Bridges transactional + informational silos |

**Faceted navigation links:** do not let filter combinations create crawlable URLs without value. Use JS filtering for secondary facets; only create indexable URLs for facets with real search volume. See `seo-architecture` Faceted Navigation section.

---

## Automated Internal Linking Tools

For sites with 100+ pages, manual internal linking is insufficient:

| Tool | Best for | Approach |
|------|---------|----------|
| **LinkWhisper** (~$77/yr, WordPress) | WordPress sites | NLP-based suggestions as you write; bulk linking dashboard |
| **InLinks** | Entity-based sites | Semantic entity detection; topic model-based suggestions |
| **Yoast SEO Premium** | WordPress content hubs | Cornerstone content + orphan detector + related post suggestions |
| **Screaming Frog + Python** | Custom automation | Export link data → identify gaps → bulk update via CMS API |
| **Programmatic related-content modules** | Template-based sites | Algorithm: TF-IDF similarity or shared taxonomy tags — auto-generate "related" sections |

**Custom automation pattern:**
```python
# Find pages with same primary tag/category
related_pages = pages.filter(tags__overlap=current_page.tags)
# Rank by authority (inlinks count)
related_pages = related_pages.order_by('-inlink_count')[:5]
# Inject as "Related reading" module
```

---

## Link Graph Visualization

For complex sites, visual link graph analysis reveals authority distribution patterns that tables cannot:

- **Screaming Frog** — "Force Directed" crawl diagram (Settings > Crawl Visualisation). Shows link relationships as a visual network; authority hotspots visible as dense clusters.
- **Gephi** (free, open-source) — export Screaming Frog link data as CSV → import as graph → run PageRank algorithm → visualize authority flow through the site
- **Sitebulb** — built-in visual link maps + orphan detection with visual context
- **Screaming Frog + Gephi workflow:**
  1. Export "All Inlinks" as CSV from Screaming Frog
  2. Import as edge list in Gephi (source = linking page, target = linked page)
  3. Run HITS algorithm → authority score per node
  4. Color by authority score → identify under-linked high-value pages

---

## Related Skills

| Need | Skill |
|------|-------|
| Design silo structure and URL hierarchy from scratch | `/seo architecture create [topic]` |
| Audit architecture changes in migration or redesign | `/seo architecture audit [old] [new]` |
| Keyword clusters to define topic groupings | `/seo keywords [topic]` |
| Programmatic internal linking at scale | `/seo programmatic [url]` |
