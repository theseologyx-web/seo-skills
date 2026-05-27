---
name: seo
description: >
  Comprehensive SEO analysis for any website or business type. Performs full site
  audits, single-page deep analysis, technical SEO checks (crawlability, indexability,
  Core Web Vitals with INP), schema markup detection/validation/generation, content
  quality assessment (E-E-A-T framework per Dec 2025 update extending to all
  competitive queries), image optimization, sitemap analysis, and Generative Engine
  Optimization (GEO) for AI Overviews, ChatGPT, and Perplexity citations. Analyzes
  AI crawler accessibility (GPTBot, ClaudeBot, PerplexityBot), llms.txt compliance,
  brand mention signals, and passage-level citability. Industry detection for SaaS,
  e-commerce, local business, publishers, agencies. Triggers on: "SEO", "audit",
  "schema", "Core Web Vitals", "sitemap", "E-E-A-T", "AI Overviews", "GEO",
  "technical SEO", "content quality", "page speed", "structured data".
user-invokable: true
argument-hint: "[command] [url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Agent
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# SEO: Universal SEO Analysis Skill

> **Rol de este skill:** Router/index y punto de entrada rápido. Para auditorías completas con scoring detallado y subagentes, usar `seo-audit` (10 categorías, lógica de orquestación completa). Este skill hace checks rápidos y enruta a los skills especializados según la necesidad.

**Invocation:** `/seo $1 $2` where `$1` is the command and `$2` is the URL or argument.

**Scripts:** Located at the plugin root `scripts/` directory.

Comprehensive SEO analysis across all industries (SaaS, local services,
e-commerce, publishers, agencies). Orchestrates 46 specialized sub-skills.
Extensions (MCP-dependent): seo-dataforseo, seo-firecrawl, seo-image-gen.

## Quick Reference

| Command | What it does |
|---------|-------------|
| **— Auditorías —** | |
| `/seo audit <url>` | Full website audit with parallel subagent delegation |
| `/seo page <url>` | Deep single-page analysis |
| **— Técnico —** | |
| `/seo technical <url>` | Technical SEO audit (9 categories) |
| `/seo performance <url>` | Core Web Vitals diagnosis and optimization |
| `/seo sitemap <url or generate>` | Analyze or generate XML sitemaps |
| `/seo schema <url>` | Detect, validate, and generate Schema.org markup |
| `/seo images <url>` | Image optimization analysis |
| `/seo visual <url>` | Screenshots, mobile rendering, HTML vs visual comparison |
| `/seo server <url>` | Server infrastructure audit (hosting, security headers, response codes) |
| `/seo cdn [url or CDN provider]` | CDN configuration and SEO impact analysis |
| `/seo logs [log file path or URL]` | Server log analysis for crawl budget and bot behavior |
| `/seo migrations [current-url] [new-url]` | SEO site migration planning and risk analysis |
| **— Contenido —** | |
| `/seo content <url>` | E-E-A-T and content quality analysis |
| `/seo content-types [tipo o url]` | Page-type-specific SEO (recipe, how-to, event, course, video...) |
| `/seo internal-linking [url or sitemap]` | Internal link structure analysis and optimization |
| `/seo architecture create [topic] \| audit [old] [new]` | Information architecture planning for SEO |
| `/seo keywords [topic, domain, or niche]` | Keyword research, clustering, and intent mapping |
| **— AI y GEO —** | |
| `/seo geo <url>` | AI Overviews / Generative Engine Optimization |
| `/seo entity [brand or url]` | Entity SEO and Knowledge Graph optimization |
| **— Competencia y Benchmarks —** | |
| `/seo competitive [domain or niche]` | Competitor analysis, content gaps, SERP landscape |
| `/seo benchmark <url> [--industry saas\|ecommerce\|local\|finance\|health\|media\|travel\|education\|realestate\|b2b]` | Compare site metrics against industry averages |
| `/seo competitor-pages [url\|generate]` | Competitor comparison page generation |
| **— Autoridad y Links —** | |
| `/seo backlinks <url>` | Backlink profile analysis (requires DataForSEO extension) |
| `/seo link-building [dominio] [--tactic guest-post\|digital-pr\|broken-link\|skyscraper]` | Proactive link acquisition strategy and execution |
| `/seo brand [brand name or url]` | Brand SEO, SERP reputation, and knowledge panel |
| **— Local —** | |
| `/seo local <url>` | Local SEO analysis (GBP, citations, reviews, map pack) |
| `/seo maps [command] [args]` | Maps intelligence (geo-grid, GBP audit, reviews, competitors) |
| **— UX / CX / CRO —** | |
| `/seo ux-visual <url>` | Visual hierarchy, accessibility (WCAG 2.2), typography audit |
| `/seo sxo <url>` | Search Experience Optimization: intent, pogo-sticking, SERP features |
| `/seo cx <url>` | Customer Experience: user journey, trust signals, form UX, CTAs |
| `/seo cro <url>` | Conversion Rate Optimization for SEO traffic |
| **— Internacional —** | |
| `/seo hreflang [url]` | Hreflang/i18n SEO audit and generation |
| `/seo international [domain or target markets]` | International SEO strategy and technical implementation |
| **— Social y Multi-canal —** | |
| `/seo smo [url o handle]` | Social Media Optimization for SEO and brand signals |
| `/seo aso [app name or store URL]` | App Store Optimization (iOS + Google Play) |
| `/seo video [url]` | Video SEO (YouTube, TikTok, Instagram, LinkedIn) |
| `/seo email [domain or email tool]` | Email marketing → traffic → behavioral SEO signals |
| **— Estrategia y Reportes —** | |
| `/seo plan <business-type>` | Strategic SEO planning |
| `/seo programmatic [url\|plan]` | Programmatic SEO analysis and planning |
| `/seo reporting [domain or client]` | SEO reporting, dashboards, and client-ready deliverables |
| `/seo privacy [url]` | Privacy-first SEO, GDPR/CCPA compliance, Consent Mode v2 |
| **— Datos y APIs —** | |
| `/seo google [command] [url]` | Google SEO APIs (GSC, PageSpeed, CrUX, Indexing, GA4) |
| `/seo dataforseo [command]` | Live SEO data via DataForSEO (extension) |
| `/seo firecrawl [command] <url>` | Full-site crawling and site mapping (extension) |
| `/seo utm [url] [--source X] [--medium Y] [--campaign Z]` | UTM parameter builder, naming conventions, GA4 integration |
| `/seo image-gen [use-case] <description>` | AI image generation for SEO assets (extension) |

## Orchestration Logic

When the user invokes `/seo audit`, delegate to subagents in parallel:
0. **Google Algorithm Update Check (MANDATORY — before spawning any subagent):**
   Use WebSearch to check for Google algorithm updates in the last 90 days.
   Sources (in order): **Google Search Status Dashboard** (`https://status.search.google.com/products/rGHU1u87FJnkP6W2GwMi/history?hl=es`) — fuente oficial primaria; luego Google Search Central Blog, SE Roundtable, Search Engine Land.
   Capture: update name, rollout dates, what it targets (content, spam, links, helpful content, core, reviews, etc.).
   Cross-reference update dates with GSC data if available: traffic drops/spikes that align with rollout dates must be flagged.
   Add a "Google Algorithm Context" block at the top of the Executive Summary — before any findings.
   Never skip this step. Algorithm context determines whether findings are root causes or symptoms.
1. Detect business type (SaaS, local, ecommerce, publisher, agency, other)
2. Spawn subagents: seo-technical, seo-content, seo-schema, seo-sitemap, seo-performance, seo-visual, seo-geo
3. If Google API credentials detected (`python scripts/google_auth.py --check`), also spawn seo-google agent
4. If local business detected, also spawn seo-local agent
5. If local business detected AND DataForSEO MCP available, also spawn seo-maps agent
6. If Firecrawl MCP available, use `firecrawl_map` to discover all site URLs before analysis
6. Collect results and generate unified report with SEO Health Score (0-100)
7. Create prioritized action plan (Critical -> High -> Medium -> Low)
8. **Offer PDF report**: "Generate a professional PDF report? Use `/seo google report full`"

For individual commands, load the relevant sub-skill directly.
After any analysis command completes, offer to generate a PDF report via `scripts/google_report.py`.

## Industry Detection

Detect business type from homepage signals:
- **SaaS / Tech**: pricing page, /features, /integrations, /docs, "free trial", "sign up", "book a demo"
- **Local Service**: phone number, address, service area, "serving [city]", Google Maps embed → auto-suggest `/seo local` for deeper analysis
- **E-commerce**: /products, /collections, /cart, "add to cart", product schema
- **Publisher / Media**: /blog, /articles, /topics, article schema, author pages, publication dates, "/noticias"
- **Agency / Consultancy**: /case-studies, /portfolio, /industries, "our work", client logos
- **Finance / Fintech**: /cotizar, /inversión, /préstamo, "tasa", "rendimiento", "crédito", regulatory disclaimers
- **Healthcare / Medical**: /pacientes, /tratamientos, /médicos, "consulta", /clinica, "appointment booking"
- **Education**: /cursos, /lecciones, /certificaciones, "enroll", "learning management", "syllabus"
- **Real Estate**: /propiedades, /listings, /rent, /buy, property schema, MLS references
- **B2B / Enterprise**: /enterprise, /solutions, /industries, "request a quote", "talk to sales", long sales cycle signals
- **Travel / Hospitality**: /hoteles, /vuelos, /destinos, "reservar", "check availability", "tarifa"
- **Non-profit**: /donate, /volunteer, /mission, .org TLD, 501(c)(3) references

## Quality Gates

Read `references/quality-gates.md` for thin content thresholds per page type.
Hard rules:
- WARNING at 30+ location pages (enforce 60%+ unique content)
- HARD STOP at 50+ location pages (require user justification)
- Never recommend HowTo schema (deprecated Sept 2023)
- FAQ schema for Google rich results: only government and healthcare sites (Aug 2023 restriction); existing FAQPage on commercial sites -> flag Info priority (not Critical), noting AI/LLM citation benefit; adding new FAQPage -> not recommended for Google benefit
- All Core Web Vitals references use INP, never FID

## Reference Files

Load these on-demand as needed (do NOT load all at startup):
- `references/cwv-thresholds.md`: Current Core Web Vitals thresholds and measurement details
- `references/schema-types.md`: All supported schema types with deprecation status
- `references/eeat-framework.md`: E-E-A-T evaluation criteria (Sept 2025 QRG update)
- `references/quality-gates.md`: Content length minimums, uniqueness thresholds
- `references/local-seo-signals.md`: Local ranking factors, review benchmarks, citation tiers, GBP status
- `references/local-schema-types.md`: LocalBusiness subtypes, industry-specific schema and citation sources

Maps-specific references (loaded by seo-maps skill, not at startup):
- `references/maps-geo-grid.md`, `references/maps-gbp-checklist.md`, `references/maps-api-endpoints.md`, `references/maps-free-apis.md`

## Scoring Methodology

### SEO Health Score (0-100)
Weighted aggregate of all categories:

| Category | Weight | Notas |
|----------|--------|-------|
| Technical SEO | 22% | Crawlability, indexability, security, redirects |
| Content Quality | 22% | E-E-A-T, thin content, freshness, passage citability |
| On-Page SEO | 18% | Titles, metas, H1-H6, URL structure, internal links |
| AI Search Readiness | 14% | AI crawler access, llms.txt, SAIV potential, schema for AI, citability passages |
| Schema / Structured Data | 10% | Tipos relevantes, validez, cobertura |
| Performance (CWV) | 9% | LCP, INP, CLS — field data preferido sobre lab data |
| Images | 5% | Alt text, formats, lazy loading, compression |

> **Por qué AI Search subió de 10% a 14%:** AI Overviews presentes en >13% de queries en 2026 y creciendo. Sitios invisibles para AI están perdiendo visibilidad estructuralmente. Content Quality bajó 1% y On-Page 2% para acomodar el cambio — refleja el peso real en 2026.

### Priority Levels
- **Critical**: Blocks indexing or causes penalties (immediate fix required)
- **High**: Significantly impacts rankings (fix within 1 week)
- **Medium**: Optimization opportunity (fix within 1 month)
- **Low**: Nice to have (backlog)

## Agentic Web Readiness

En 2026, AI agents (ChatGPT, Claude, Perplexity, SearchGPT) no solo citan contenido — también compran, reservan y completan transacciones autónomamente. Evaluar como dimensión adicional en `/seo audit`:

| Check | Qué evaluar | Señal positiva |
|-------|------------|---------------|
| Machine readability | ¿Puede un AI agent parsear el contenido? | Contenido en texto plano (no solo imágenes), estructura semántica clara |
| API endpoints expuestos | ¿Hay APIs públicas que agents pueden consumir? | OpenAPI/REST docs, /api/ público, structured data feeds |
| Agentic commerce readiness | ¿Puede un agent completar una transacción? | Checkout funcional, precios en schema, disponibilidad structured |
| llms.txt | ¿Declara preferencias para AI consumption? | Archivo presente y bien estructurado |
| AI crawler access | ¿GPTBot, ClaudeBot, PerplexityBot están permitidos? | robots.txt ALLOW para todos |
| Schema completeness | ¿Entities, Products, FAQs en JSON-LD? | ≥ 3 tipos de schema relevantes implementados |

**Herramienta de referencia:** WordLift AI Audit (free) evalúa machine readability.

**Cuándo añadir al reporte:** Para clientes SaaS o e-commerce con objetivo de AI visibility. No es crítico para sitios puramente informativos o locales pequeños.

---

## Sub-Skills

This skill orchestrates 46 specialized sub-skills:

**Auditorías**
1. **seo-audit** -- Full website audit with parallel delegation
2. **seo-page** -- Deep single-page analysis

**Técnico**
3. **seo-technical** -- Technical SEO (9 categories)
4. **seo-performance** -- Core Web Vitals measurement, diagnosis, and optimization
5. **seo-sitemap** -- Sitemap analysis and generation
6. **seo-schema** -- Schema markup detection and generation
7. **seo-images** -- Image optimization
8. **seo-visual** -- Screenshots, mobile rendering, HTML vs visual comparison
9. **seo-server** -- Server infrastructure audit (hosting, TLS, response codes, security headers)
10. **seo-cdn** -- CDN configuration and SEO impact analysis
11. **seo-logs** -- Server log analysis for crawl budget and bot behavior
12. **seo-migrations** -- SEO site migration planning and risk analysis
13. **seo-robots** -- robots.txt validation, audit, and generation

**Contenido**
14. **seo-content** -- E-E-A-T and content quality
15. **seo-content-types** -- Page-type-specific SEO (recipe, how-to, event, course, video, job posting...)
16. **seo-internal-linking** -- Internal link structure analysis and optimization
17. **seo-architecture** -- Information architecture planning for SEO (keyword-to-URL mapping, site structure)
18. **seo-keywords** -- Keyword research, clustering, and intent mapping

**AI y GEO**
19. **seo-geo** -- AI Overviews / GEO optimization
20. **seo-entity** -- Entity SEO and Knowledge Graph optimization

**Competencia y Benchmarks**
21. **seo-competitive** -- Competitor analysis, content gaps, SERP landscape
22. **seo-benchmark** -- Site metrics vs. industry averages (CWV, CTR, authority, content)
23. **seo-competitor-pages** -- Competitor comparison page generation

**Autoridad y Links**
24. **seo-backlinks** -- Backlink profile analysis (requires DataForSEO extension)
25. **seo-link-building** -- Proactive link acquisition: guest posts, digital PR, broken links, skyscraper
26. **seo-brand** -- Brand SEO, SERP reputation, and knowledge panel management

**Local**
27. **seo-local** -- Local SEO (GBP, NAP, citations, reviews, local schema, multi-location)
28. **seo-maps** -- Maps intelligence (geo-grid, GBP audit, reviews, competitor radius)

**UX / CX / CRO**
29. **seo-ux-visual** -- Visual hierarchy, accessibility (WCAG 2.2), typography, above-fold
30. **seo-sxo** -- Search Experience Optimization: intent match, SERP features, pogo-sticking
31. **seo-cx** -- Customer Experience: user journey, trust signals, form UX, 404s, CTAs
32. **seo-cro** -- Conversion Rate Optimization for SEO traffic

**Internacional**
33. **seo-hreflang** -- Hreflang/i18n SEO audit and generation
34. **seo-international** -- International SEO strategy and technical implementation

**Social y Multi-canal**
35. **seo-smo** -- Social Media Optimization for SEO and brand signals
36. **seo-aso** -- App Store Optimization (iOS App Store + Google Play)
37. **seo-video** -- Video SEO (YouTube, TikTok, Instagram, LinkedIn)
38. **seo-email** -- Email marketing → traffic → behavioral SEO signals

**Estrategia y Reportes**
39. **seo-plan** -- Strategic planning with templates
40. **seo-programmatic** -- Programmatic SEO analysis and planning
41. **seo-reporting** -- SEO reporting, dashboards, and client-ready deliverables
42. **seo-privacy** -- Privacy-first SEO, GDPR/CCPA compliance, Consent Mode v2

**Datos y APIs**
43. **seo-google** -- Google SEO APIs (GSC, PageSpeed, CrUX, Indexing API, GA4)
44. **seo-dataforseo** -- Live SEO data via DataForSEO MCP (extension)
45. **seo-firecrawl** -- Full-site crawling and site mapping via Firecrawl MCP (extension)
46. **seo-utm** -- UTM parameter builder, naming conventions, GA4 integration, campaign tracking
47. **seo-image-gen** -- AI image generation for SEO assets via Gemini (extension)

## Subagents

For parallel analysis during audits:
- `seo-technical` -- Crawlability, indexability, security, CWV
- `seo-content` -- E-E-A-T, readability, thin content
- `seo-schema` -- Detection, validation, generation
- `seo-sitemap` -- Structure, coverage, quality gates
- `seo-performance` -- Core Web Vitals measurement
- `seo-visual` -- Screenshots, mobile testing, above-fold
- `seo-geo` -- AI crawler access, llms.txt, citability, brand mention signals
- `seo-local` -- GBP signals, NAP consistency, reviews, local schema, industry-specific local factors (conditional: spawned when Local Service detected)
- `seo-maps` -- Geo-grid rank tracking, GBP audit, review intelligence, competitor radius mapping (conditional: spawned when Local Service detected AND DataForSEO MCP available)
- `seo-google` -- CWV field data, URL indexation status, organic traffic trends (conditional: spawned when Google API credentials detected)
- `seo-dataforseo` -- Live SERP, keyword, backlink, local SEO data (extension, optional)
- `seo-image-gen` -- SEO image audit and generation plan (extension, optional)
- `seo-firecrawl` -- Full-site crawl and site mapping (extension, optional; used by audit for URL discovery)

## Error Handling

| Scenario | Action |
|----------|--------|
| Unrecognized command | List available commands from the Quick Reference table. Suggest the closest matching command. |
| URL unreachable | Report the error and suggest the user verify the URL. Do not attempt to guess site content. |
| Sub-skill fails during audit | Report partial results from successful sub-skills. Clearly note which sub-skill failed and why. Suggest re-running the failed sub-skill individually. |
| Ambiguous business type detection | Present the top two detected types with supporting signals. Ask the user to confirm before proceeding with industry-specific recommendations. |
| API rate limit hit (DataForSEO, Google) | Report which API hit the limit. Wait 60s before retry for per-minute limits. For per-day limits (e.g., Indexing API 200/day), stop and report remaining quota. |
| Crawl timeout on large site | Reduce `limit` parameter. Use `firecrawl_map` first (fast, no content) then targeted crawl on top 50 pages. Report which pages were analyzed and which were skipped. |
| Site behind authentication | Flag as "authenticated content — cannot crawl automatically". Suggest manual export via GSC Bulk Data Export or Firecrawl browser session with login. Proceed with publicly accessible pages only. |
| JavaScript SPA — content not rendering | Switch to `firecrawl_scrape` (renders JS) instead of raw fetch. If Firecrawl unavailable, flag as "JS-rendered — analysis based on server-side HTML only; may miss content". |
| No industry match found | Default to "General" benchmarks. Flag that industry-specific recommendations may not apply. Ask user to specify their business type manually. |
