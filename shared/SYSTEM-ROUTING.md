# SYSTEM-ROUTING — Quick Routing Reference

> For any AI agent reading this system: read this file first. It tells you which skill to invoke for each task and how to resolve ambiguous cases.

---

## Skill routing by family

### Entry & Orchestration

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo` | User types `/seo` with no subcommand; needs quick reference or skill index | User requests a full site audit | `seo-audit` |
| `seo-audit` | Full site audit or comprehensive diagnosis requested | Only routing/indexing needed | `seo` |

### Technical

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-technical` | Crawl errors, indexation, redirects, status codes, general technical triage | Deep CWV optimization (LCP/CLS/INP) | `seo-performance` |
| `seo-performance` | Core Web Vitals, page speed, LCP/CLS/INP deep diagnosis | General crawlability, indexation, redirects | `seo-technical` |
| `seo-server` | Server config: .htaccess, cache headers, HTTPS, HSTS, server-side redirects | CDN-specific issues only | `seo-cdn` |
| `seo-cdn` | **[utility]** CDN is the confirmed bottleneck (Cloudflare, Fastly, etc.) | Any non-CDN performance issue | `seo-performance` or `seo-server` |
| `seo-logs` | Log file analysis: crawl budget, Googlebot frequency, crawl patterns | Live crawl, real-time HTML capture | `seo-firecrawl` or `seo-crawler` |
| `seo-robots` | robots.txt rules, crawl directives, Disallow/Allow logic | Sitemap generation | `seo-sitemap` |
| `seo-sitemap` | XML sitemap creation, validation, index sitemaps | robots.txt, crawl directives | `seo-robots` |
| `seo-crawler` | **[utility]** Capture raw HTML of one specific URL (Cloudflare bypass needed) | Site-wide crawl | `seo-firecrawl` |
| `seo-firecrawl` | Full site crawl, multi-page discovery, structured data extraction at scale | Single-URL HTML capture | `seo-crawler` |

### Content

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-content` | Editorial strategy, E-E-A-T, content quality, topical authority, content gaps | Schema markup, rich result implementation | `seo-content-types` |
| `seo-content-types` | Schema types, rich results, JSON-LD, content technical requirements (HowTo, FAQ, Product) | Editorial quality, E-E-A-T, content strategy | `seo-content` |
| `seo-keywords` | Keyword research, intent classification, keyword mapping | On-page optimization, meta tags | `seo-page` |
| `seo-page` | Single-page audit: title, meta, H1, internal links, OG tags | Full site audit | `seo-audit` |
| `seo-schema` | JSON-LD structured data implementation and validation | General schema strategy, content types selection | `seo-content-types` |
| `seo-programmatic` | Programmatic SEO: templates, auto-generated pages at scale | One-off content optimization | `seo-content` |
| `seo-video` | YouTube optimization, video schema, video sitemaps | General content strategy | `seo-content` |
| `seo-ai-content-quality` | AI-generated content review for LLM citability and passage-level clarity | Human content E-E-A-T review | `seo-content` |

### International

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-international` | International SEO strategy, hreflang implementation, ccTLD/subdomain decisions | — | — |
| `seo-hreflang` | **[stub — do not invoke]** Redirects to `seo-international` section 11 | — | `seo-international` |

### Local

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-local` | Local SEO strategy: GBP, NAP consistency, citations, local content, reviews | Maps pack ranking, geo-grid | `seo-maps` |
| `seo-maps` | Google Maps ranking, GBP geo-grid, local pack position | General local SEO strategy | `seo-local` |

### Links

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-backlinks` | Backlink profile analysis, toxic links, DA/DR review, competitive gap analysis | Acquiring new links, outreach strategy | `seo-link-building` |
| `seo-link-building` | Link acquisition strategy, outreach, prospecting, anchor text planning | Analyzing existing backlink profile | `seo-backlinks` |
| `seo-internal-linking` | Internal link structure, siloing, anchor text distribution, orphan pages | External link analysis | `seo-backlinks` |

### Brand & Entity

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-brand` | Brand SERPs, branded queries, knowledge panel, ORM | Entity schema markup, KG disambiguation | `seo-entity` |
| `seo-entity` | Entity schema (Organization, Person), knowledge graph, semantic entity clarity | Brand SERP monitoring, reputation | `seo-brand` |

### Competitive

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-competitive` | Broad competitor landscape: traffic share, keyword gaps, strategy | Analyzing a single competitor page | `seo-competitor-pages` |
| `seo-competitor-pages` | Reverse-engineering a specific competitor page | Broad competitive analysis | `seo-competitive` |
| `seo-benchmark` | KPI benchmarking vs. industry or competitors | Deep competitive strategy | `seo-competitive` |

### Architecture

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-architecture` | IA, URL structure, taxonomy, site hierarchy, navigation | URL changes during a live migration | `seo-migrations` |
| `seo-migrations` | Site migrations: URL changes, CMS switches, domain moves, redirect mapping | General IA planning (no migration) | `seo-architecture` |

### UX & Conversion

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-cro` | Page-level conversion: CTAs, layout, above-fold, social proof, trust signals | Post-click friction in forms/onboarding | `seo-cx` |
| `seo-cx` | Post-click journey friction: forms, onboarding flows, 404 pages, microcopy, error states | On-page CTA / layout optimization | `seo-cro` |
| `seo-sxo` | SERP-to-page intent match, pogo-sticking, featured snippet positioning, first-impression UX | Page conversion optimization | `seo-cro` |
| `seo-ux-visual` | Visual UX audit: whitespace, hierarchy, readability, mobile layout | Conversion optimization | `seo-cro` |
| `seo-visual` | Screenshot capture, before/after visual diffs, Playwright rendering | UX analysis | `seo-ux-visual` |

### AI Search

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-ai-search-readiness` | Holistic AI Search assessment (start here for AI Search tasks) | Only one specific AI layer needed | `seo-geo`, `seo-aeo`, or `seo-llmo` |
| `seo-geo` | Visibility in AI Overviews, Perplexity, ChatGPT, Bing Copilot | Structuring QA pairs, passage-level content | `seo-aeo` |
| `seo-aeo` | Answer Engine Optimization: question-answer structure, featured snippets for AI, passage answerability | Technical LLM retrievability | `seo-llmo` |
| `seo-llmo` | LLM retrievability: structured passages, citation anchors, content chunking for model indexing | Generative surface visibility | `seo-geo` |

### Data & Reporting

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-reporting` | Client-facing reports, dashboards, KPI tracking, monthly reports | Raw data pull | `seo-dataforseo` or `seo-google` |
| `seo-dataforseo` | DataForSEO API: SERP data, keyword metrics, backlink data, rank tracking | Google-native data (GSC, GA4, PageSpeed) | `seo-google` |
| `seo-google` | Google APIs: GSC, GA4, PageSpeed/CrUX, Indexing API | Third-party SERP / backlink data | `seo-dataforseo` |

### Strategy

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-client-discovery` | New client intake; understand business model, stack, AI Search goals | Client already onboarded | `seo-plan` or `seo-audit` |
| `seo-plan` | Full SEO strategy roadmap (after audit or discovery) | Quick tactical action | relevant specific skill |
| `seo-growth-strategy` | Growth opportunities, content gaps, market expansion, 9-pillar growth audit | Tactical execution of known tasks | `seo-plan` |

### Utilities & Extensions

| Skill | Use when | Do NOT use when | Route to |
|-------|----------|-----------------|---------|
| `seo-images` | Audit existing images: alt text, file size, OG quality, lazy load | Generating new images | `seo-image-gen` |
| `seo-image-gen` | **[utility/extension]** Generate OG, hero, infographic, product images (requires nanobanana-mcp) | Analyzing existing images | `seo-images` |
| `seo-utm` | **[utility]** Generate UTM parameters for campaign tracking | Any SEO analysis | `seo-reporting` or `seo-link-building` |
| `seo-smo` | Social media optimization: OG tags, Twitter Cards, social content strategy | Email marketing | `seo-email` |
| `seo-email` | Email SEO, newsletters, drip campaigns with SEO intent | Social media | `seo-smo` |
| `seo-aso` | App Store Optimization (iOS / Android apps) | Web SEO | appropriate web skill |
| `seo-privacy` | Privacy compliance: Consent Mode v2, cookie banners, GDPR/CCPA impact on GA4/GTM | General technical SEO | `seo-technical` |

---

## Conflicting pairs — disambiguation

When two skills seem equally valid, use this table to decide.

| If you were about to use… | But the real task is… | Use this instead |
|--------------------------|----------------------|-----------------|
| `seo-content` | Schema markup, JSON-LD, rich results implementation | `seo-content-types` |
| `seo-content-types` | Editorial quality, E-E-A-T signals, content strategy | `seo-content` |
| `seo-brand` | Entity schema markup, knowledge graph disambiguation | `seo-entity` |
| `seo-entity` | Brand SERP monitoring, branded query management | `seo-brand` |
| `seo-backlinks` | Building new links, outreach, prospecting | `seo-link-building` |
| `seo-link-building` | Analyzing existing profile, toxic links, gap analysis | `seo-backlinks` |
| `seo-technical` | LCP, CLS, INP deep optimization, Core Web Vitals | `seo-performance` |
| `seo-performance` | Crawl errors, redirect chains, indexation issues | `seo-technical` |
| `seo-cro` | Post-click forms, onboarding friction, microcopy errors | `seo-cx` |
| `seo-cx` | Page-level CTAs, layout, above-fold conversion | `seo-cro` |
| `seo-sxo` | Fixing conversion on the landing page | `seo-cro` |
| `seo-cro` | Fixing SERP click-through, pogo-sticking, snippet positioning | `seo-sxo` |
| `seo-geo` | Structuring QA content, improving answerability | `seo-aeo` |
| `seo-aeo` | Technical passage chunking, LLM citation anchors | `seo-llmo` |
| `seo-geo` | Holistic AI Search assessment (don't know where to start) | `seo-ai-search-readiness` |
| `seo-hreflang` | Any hreflang implementation task | `seo-international` (section 11) |
| `seo-images` | Need to create a new OG or hero image | `seo-image-gen` |
| `seo-image-gen` | Need to audit existing images | `seo-images` |
| `seo-crawler` | Need to crawl the full site | `seo-firecrawl` |
| `seo-audit` | User just wants a skill index or quick reference | `seo` |

---

## Skill status legend

- **[utility]** — low-level helper; invoke from another skill, not directly
- **[extension]** — requires external tool/MCP (noted in SKILL.md)
- **[stub]** — non-invokable; routes to another skill
