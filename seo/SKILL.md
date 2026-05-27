---
name: seo
description: >
  Router/index y punto de entrada para el sistema SEO. Enruta a los 46 skills
  especializados según la tarea. Para auditorías completas con scoring y subagentes
  usar seo-audit. Cubre: quick reference de comandos, detección de industria,
  quality gates, referencias a archivos de soporte. Triggers: "SEO", "audit",
  "qué skill uso", "por dónde empiezo", "seo de", o cualquier tarea SEO sin
  skill específico claro.
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

## Orchestration

`/seo audit <url>` delega a **`seo-audit`** — orquestador único con scoring detallado (SEO Health Score 0-100), subagentes en paralelo, plan de acción priorizado y contexto de algoritmos.

Para comandos individuales, cargar el sub-skill correspondiente directamente desde la tabla Quick Reference.

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

