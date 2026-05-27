---
name: seo-firecrawl
description: >
  Full-site crawling, scraping, and site mapping via Firecrawl MCP.
  Use when user says "crawl site", "map site", "full crawl",
  "find all pages", "broken links", "site structure",
  "discover pages", "JS rendering", or needs site-wide analysis.
user-invokable: true
argument-hint: "[command] <url>"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
compatibility: "Requires Firecrawl MCP server"
metadata:
  author: AgriciDaniel
  version: "1.7.2"
  category: seo
---

# Firecrawl Extension for Claude SEO

This skill requires the Firecrawl extension to be installed:
```bash
./extensions/firecrawl/install.sh
```

**Check availability:** Before using any Firecrawl tool, verify the MCP server
is connected by checking if `firecrawl_scrape` or any Firecrawl tool
is available. If tools are not available, inform the user the extension is not
installed and provide install instructions.

## Quick Reference

| Command | Purpose |
|---------|---------|
| `/seo firecrawl crawl <url>` | Full-site crawl with content extraction |
| `/seo firecrawl map <url>` | Discover site structure (URLs only, fast) |
| `/seo firecrawl scrape <url>` | Single-page scrape with JS rendering |
| `/seo firecrawl search <query> <url>` | Search within a crawled site |
| `/seo firecrawl extract <url> <schema>` | Structured data extraction con schema definition |
| `/seo firecrawl agent <task>` | AI agent autónomo para tareas web complejas |
| `/seo firecrawl interact <url>` | Browser session interactivo (click, scroll, login) |
| `/seo firecrawl batch <urls>` | Procesamiento en paralelo de múltiples URLs |

## Commands

### crawl -- Full-Site Crawl

Crawl an entire website starting from the given URL. Returns page content,
metadata, and links for all discovered pages.

**MCP Tool:** `firecrawl_crawl`

**Parameters:**
- `url` (required): Starting URL to crawl
- `limit`: Max pages to crawl (default: 100, max: 500)
- `maxDepth`: Max link depth from start URL (default: 3)
- `includePaths`: Array of glob patterns to include (e.g., `["/blog/*"]`)
- `excludePaths`: Array of glob patterns to exclude (e.g., `["/admin/*", "/api/*"]`)
- `scrapeOptions.formats`: Output formats -- `["markdown", "html", "links"]`

**SEO Usage Patterns:**
1. **Comprehensive audit crawl**: Crawl full site, extract all pages for subagent analysis
2. **Section-focused crawl**: Use `includePaths` to audit only `/blog/*` or `/products/*`
3. **Broken link detection**: Crawl with `["links"]` format, check all hrefs for 404s
4. **Content inventory**: Extract all page titles, meta descriptions, H1s at scale
5. **SPA/JS-rendered sites**: Firecrawl renders JavaScript, solving the Issue #11 problem

**Example orchestration for `/seo audit`:**
```
1. firecrawl_map(url) -> get all URLs (fast, no content)
2. Filter to top 50 most important pages (homepage, key sections)
3. firecrawl_crawl(url, limit=50) -> get full content
4. Feed content to seo-technical, seo-content, seo-schema agents
```

**Cost awareness:**
- Free tier: 500 credits/month
- 1 credit = 1 page crawled or scraped
- Map operations are cheaper (0.5 credits per URL discovered)
- Always inform user of estimated credit usage before large crawls

### map -- Site Structure Discovery

Discover all URLs on a website without fetching content. Fast and credit-efficient.

**MCP Tool:** `firecrawl_map`

**Parameters:**
- `url` (required): Website URL to map
- `limit`: Max URLs to discover (default: 5000)
- `search`: Optional search term to filter URLs

**SEO Usage Patterns:**
1. **Sitemap comparison**: Map site, compare discovered URLs vs XML sitemap
2. **Orphan page detection**: URLs in sitemap but not linked from any page
3. **Crawl budget analysis**: Total indexable pages vs pages linked from homepage
4. **URL pattern analysis**: Identify URL structure patterns, duplicates, parameter bloat
5. **Pre-audit discovery**: Run map first, then targeted crawl on key sections

**Output:** Array of URLs. Present as:
```
Site: example.com
Pages discovered: 342

URL Pattern Breakdown:
  /blog/*          - 128 pages (37%)
  /products/*      - 89 pages (26%)
  /category/*      - 45 pages (13%)
  /pages/*         - 32 pages (9%)
  / (root pages)   - 48 pages (14%)
```

### scrape -- Single-Page Deep Scrape

Scrape a single page with full JavaScript rendering. More thorough than
`fetch_page.py` because it executes JS and waits for dynamic content.

**MCP Tool:** `firecrawl_scrape`

**Parameters:**
- `url` (required): Page URL to scrape
- `formats`: Output formats -- `["markdown", "html", "links", "screenshot"]`
- `onlyMainContent`: Strip nav/footer/sidebar (default: true)
- `waitFor`: CSS selector or milliseconds to wait for content
- `timeout`: Request timeout in ms (default: 30000)
- `actions`: Browser actions before scraping (click, scroll, wait)

**SEO Usage Patterns:**
1. **SPA content extraction**: Scrape JS-rendered React/Vue/Angular pages
2. **Dynamic content audit**: Pages with lazy-loaded content below the fold
3. **Paywall/login detection**: Identify content behind authentication walls
4. **Main content extraction**: Use `onlyMainContent` for clean E-E-A-T analysis
5. **Screenshot capture**: Use `screenshot` format for visual analysis

**When to use scrape vs fetch_page.py:**
| Scenario | Use |
|----------|-----|
| Static HTML page | `fetch_page.py` (no API cost) |
| JS-rendered SPA | `firecrawl_scrape` (renders JS) |
| Need response headers | `fetch_page.py` (returns headers) |
| Need clean markdown | `firecrawl_scrape` (better extraction) |
| Rate-limited/blocked | `firecrawl_scrape` (handles anti-bot) |

### search -- Site-Scoped Search

Search within a website for specific content. Useful for finding pages
related to a topic without crawling everything.

**MCP Tool:** `firecrawl_search`

**Parameters:**
- `query` (required): Search query
- `url` (required): Website to search within
- `limit`: Max results (default: 10)
- `scrapeOptions.formats`: Output format for matched pages

**SEO Usage Patterns:**
1. **Content gap validation**: Search for a keyword on the site to check if content exists
2. **Internal linking opportunities**: Find pages mentioning a topic that could link to each other
3. **Duplicate content detection**: Search for key phrases to find near-duplicates
4. **Competitor content research**: Search competitor site for specific topics

### extract -- Structured Data Extraction

Extrae datos estructurados de una página o sitio usando un schema definido. Más eficiente que scrape cuando sabes exactamente qué datos necesitas.

**MCP Tool:** `firecrawl_extract`

**Parameters:**
- `urls` (required): URL o array de URLs a extraer
- `prompt`: Descripción en lenguaje natural de qué extraer
- `schema`: JSON schema de los datos a extraer (opcional pero recomendado para consistencia)

**SEO Usage Patterns:**

1. **Pricing de competidores en formato estructurado:**
```json
schema: {
  "product_name": "string",
  "pricing_tiers": [{"name": "string", "price": "number", "features": ["string"]}],
  "free_tier_available": "boolean",
  "pricing_url": "string"
}
```

2. **Features de competidores para comparison pages:**
```json
schema: {
  "features": [{"name": "string", "available": "boolean", "notes": "string"}],
  "integrations": ["string"],
  "last_updated": "string"
}
```

3. **Testimonials/reviews para análisis:**
```json
schema: {
  "reviews": [{"rating": "number", "text": "string", "source": "string", "date": "string"}]
}
```

4. **Job postings para competitive intelligence:**
```json
schema: {
  "open_roles": [{"title": "string", "department": "string", "location": "string"}]
}
```

**Costo:** ~2-5 créditos por URL dependiendo de complejidad.

---

### agent -- AI Web Agent Autónomo

Agente AI que navega la web autónomamente para completar tareas complejas de múltiples pasos. Elimina la necesidad de cadenas manuales de scrape.

**MCP Tool:** `firecrawl_agent` (usa `firecrawl_agent_status` para verificar progreso)

**Modelos disponibles:**
- `spark-1-fast` — Rápido, tareas simples
- `spark-1-mini` — Equilibrio velocidad/capacidad
- `spark-1-pro` — Tareas complejas, máxima capacidad

**SEO Usage Patterns:**

1. **Competitor pricing research completo:**
```
Task: "Find all pricing plans for [competitor], including any hidden fees, 
annual vs monthly discounts, and enterprise pricing. Return structured data."
→ El agent navega el sitio, encuentra pricing, FAQs de pricing, páginas de contacto
```

2. **SERP feature tracking:**
```
Task: "Search Google for '[keyword]' and tell me: 
Is there an AI Overview? What sources does it cite? 
Is there a featured snippet? Who owns it?"
```

3. **Competitor content audit batch:**
```
Task: "Find all blog posts published in the last 3 months on [competitor blog URL]. 
List titles, dates, estimated word count, and main topic."
```

4. **GEO verification:**
```
Task: "Search Perplexity.ai for '[keyword]' and identify 
which domains are cited in the response."
```

**Workflow con status check:**
```
1. firecrawl_agent(task, model="spark-1-pro") → retorna agent_id
2. firecrawl_agent_status(agent_id) → verificar completado
3. Procesar resultado cuando status = "completed"
```

**Costo estimado:** 10-50 créditos por tarea dependiendo de complejidad y pasos.

---

### interact -- Browser Session Interactivo

Convierte cualquier scrape en una sesión de browser activa donde el agente puede hacer clic, escribir y navegar en lenguaje natural. Permite acceder a contenido detrás de login.

**MCP Tools:** `firecrawl_browser_create`, `firecrawl_browser_execute`, `firecrawl_browser_list`, `firecrawl_browser_delete`

**SEO Usage Patterns:**

1. **Login + scrape de herramienta protegida:**
```
browser_create() → session_id
browser_execute(session_id, "Navigate to [url]/login")
browser_execute(session_id, "Fill email field with [email]")
browser_execute(session_id, "Fill password field with [password]")
browser_execute(session_id, "Click login button")
browser_execute(session_id, "Extract all data from dashboard")
browser_delete(session_id)
```

2. **SPA navigation paso a paso:**
```
browser_execute(session_id, "Click on 'Pricing' tab")
browser_execute(session_id, "Select 'Annual billing' toggle")
browser_execute(session_id, "Extract all plan names and prices")
```

3. **Testing de conversion flows (para seo-cro):**
```
browser_execute(session_id, "Navigate to [landing page]")
browser_execute(session_id, "Fill contact form with test data")
browser_execute(session_id, "Click submit and capture confirmation page")
```

**Session Persistence (Named Profiles):**
```
# Login una vez, reusar el estado de sesión en futuros scrapes:
browser_create(profile_name="client-dashboard") → guarda cookies/localStorage
# En scrapes futuros:
browser_create(profile_name="client-dashboard") → restaura sesión anterior
# Útil para: monitoreo recurrente de páginas detrás de login
```

**Costo:** ~5-15 créditos por sesión dependiendo de pasos ejecutados.

---

### PDF Parsing

Firecrawl parsea PDFs automáticamente cuando se pasa una URL de PDF al comando `scrape`. El parser (Rust-based, 3x más rápido que versiones anteriores) extrae texto searchable.

**Cuándo usar:**

| Caso de uso SEO | Acción |
|----------------|--------|
| Auditar whitepapers y case studies indexados | `scrape <pdf_url>` → verificar que tiene texto (no solo imágenes) |
| Extraer contenido de PDFs para content audit | `scrape <pdf_url>` → analizar E-E-A-T del contenido |
| PDFs de competidores para competitive intel | `scrape <pdf_url>` → extraer insights estratégicos |
| Verificar que PDFs son accesibles a Googlebot | Si scrape falla → el PDF puede ser solo imagen → señal de indexación problemática |

```bash
# Ejemplo: auditar si un PDF corporativo es indexable
/seo firecrawl scrape https://ejemplo.com/whitepaper.pdf
→ Si retorna texto → PDF indexable ✅
→ Si retorna vacío o error → PDF es imagen escaneada → no indexable ❌
```

---

### batch -- Procesamiento en Paralelo

Procesa múltiples URLs simultáneamente. Failure handling automático — si una URL falla, las demás continúan.

**MCP Tool:** `firecrawl_crawl` con array de URLs, o múltiples llamadas a `firecrawl_scrape` en paralelo.

**SEO Usage Patterns:**
1. **Audit de top páginas del sitio:** batch scrape de top 50 páginas por tráfico (desde GSC)
2. **Competitive content audit:** scrape en paralelo de páginas equivalentes en 5 competidores
3. **Freshness check:** scrape batch de todas las URLs de un sitemap para verificar status codes

**Tabla de costos por operación:**

| Operación | Créditos por unidad | Ejemplo |
|-----------|-------------------|---------|
| `map` (URL discovery) | 0.5/URL | 500 URLs → 250 créditos |
| `scrape` (single page) | 1/página | 100 páginas → 100 créditos |
| `crawl` (full site) | 1/página | 200 páginas → 200 créditos |
| `extract` (structured) | 2-5/URL | 20 competidores → 40-100 créditos |
| `agent` (autonomous) | 10-50/tarea | 5 tareas → 50-250 créditos |
| `interact` (browser) | 5-15/sesión | 3 sesiones → 15-45 créditos |
| `search` | 1/búsqueda | 10 búsquedas → 10 créditos |

**Free tier:** 500 créditos/mes = ~500 páginas scrapeadas o ~1 auditoría mediana.
**Paid plans:** Verificar en firecrawl.dev para límites actuales (cambió en 2025).

> **Regla de eficiencia:** Siempre usar `map` antes de `crawl`. Map descubre URLs sin extraer contenido, permite seleccionar qué páginas crawlear. Ahorra hasta 80% de créditos en auditorías grandes.

---

## Cross-Skill Integration

### With seo-audit (full audit)
When Firecrawl is available during `/seo audit`:
1. Use `firecrawl_map` to discover all site URLs
2. Compare with XML sitemap (seo-sitemap) to find orphan/missing pages
3. Select top pages for deep analysis
4. Feed crawled content to all subagents (technical, content, schema, geo)
5. Report total crawlable pages, URL patterns, and crawl depth

### With seo-technical
- Broken link detection: crawl all internal links, check for 404s
- Redirect chain mapping: follow all redirects, flag chains > 2 hops
- Mixed content detection: check HTTP resources on HTTPS pages
- Canonical verification: compare canonical URLs with actual URLs

### With seo-sitemap
- Sitemap coverage: % of crawled pages present in sitemap
- Orphan pages: pages found by crawl but missing from sitemap
- Stale sitemap entries: URLs in sitemap that return 404/410

### With seo-content
- Content extraction: feed clean markdown to E-E-A-T analysis
- Thin content detection: identify pages with < 300 words at scale
- Duplicate content: compare content across pages for near-duplicates

### With seo-schema
- Schema extraction: pull JSON-LD from all crawled pages
- Schema coverage: % of pages with structured data
- Schema validation: batch-validate extracted schemas

## Error Handling

| Error | Cause | Resolution |
|-------|-------|-----------|
| `FIRECRAWL_API_KEY not set` | MCP not configured | Run `./extensions/firecrawl/install.sh` |
| `402 Payment Required` | Credits exhausted | Check usage at firecrawl.dev/app, upgrade plan |
| `429 Too Many Requests` | Rate limited | Wait 60s, reduce crawl concurrency |
| `408 Timeout` | Page too slow to render | Increase `timeout`, try without JS rendering |
| `403 Forbidden` | Site blocks crawling | Check robots.txt, may need to skip this site |

**Graceful fallback:** If Firecrawl is unavailable, inform the user and suggest:
1. Use `fetch_page.py` for single-page analysis (no API cost)
2. Use `WebFetch` tool for basic HTML retrieval
3. Install Firecrawl: `./extensions/firecrawl/install.sh`
