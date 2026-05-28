---
name: seo-dataforseo
description: >
  Live SEO data via DataForSEO MCP server. SERP analysis (Google, Bing, Yahoo,
  YouTube), keyword research (volume, difficulty, intent, trends), backlink
  profiles, on-page analysis (Lighthouse, content parsing), competitor analysis,
  content analysis, business listings, AI visibility (ChatGPT scraper, LLM
  mention tracking), and domain analytics. Requires DataForSEO extension
  installed. Use when user says "dataforseo", "live SERP", "keyword volume",
  "backlink data", "competitor data", "AI visibility check", "LLM mentions",
  or "real search data". Not for Google-native data (GSC, GA4, PageSpeed) — use seo-google.
user-invokable: true
argument-hint: "[command] [query]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
compatibility: "Requires DataForSEO MCP server"
metadata:
  author: AgriciDaniel
  version: "1.6.1"
  category: seo
---

# DataForSEO: Live SEO Data (Extension)

Live search data via the DataForSEO MCP server. Provides real-time SERP results,
keyword metrics, backlink profiles, on-page analysis, content analysis, business
listings, AI visibility checking, and LLM mention tracking across
9 API modules with 79 MCP tools.

## Prerequisites

This skill requires the DataForSEO extension to be installed:
```bash
./extensions/dataforseo/install.sh
```

**Check availability:** Before using any DataForSEO tool, verify the MCP server
is connected by checking if `serp_organic_live_advanced` or any DataForSEO tool
is available. If tools are not available, inform the user the extension is not
installed and provide install instructions.

## API Credit Awareness

DataForSEO charges per API call. Be efficient:
- Prefer bulk endpoints over multiple single calls
- Use default parameters (US, English) unless user specifies otherwise
- Cache results mentally within a session; don't re-fetch the same data
- Warn user before running expensive operations (full backlink crawls, large keyword lists)

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/seo dataforseo serp <keyword>` | Google organic SERP results |
| `/seo dataforseo serp-youtube <keyword>` | YouTube search results |
| `/seo dataforseo youtube <video_id>` | YouTube video deep analysis |
| `/seo dataforseo keywords <seed>` | Keyword ideas and suggestions |
| `/seo dataforseo volume <keywords>` | Search volume for keywords |
| `/seo dataforseo difficulty <keywords>` | Keyword difficulty scores |
| `/seo dataforseo intent <keywords>` | Search intent classification |
| `/seo dataforseo trends <keyword>` | Google Trends data |
| `/seo dataforseo backlinks <domain>` | Full backlink profile |
| `/seo dataforseo competitors <domain>` | Competitor domain analysis |
| `/seo dataforseo ranked <domain>` | Ranked keywords for domain |
| `/seo dataforseo intersection <domains>` | Keyword/backlink overlap |
| `/seo dataforseo traffic <domains>` | Bulk traffic estimation |
| `/seo dataforseo subdomains <domain>` | Subdomains with ranking data |
| `/seo dataforseo top-searches <domain>` | Top queries mentioning domain |
| `/seo dataforseo onpage <url>` | On-page analysis (Lighthouse + parsing) |
| `/seo dataforseo tech <domain>` | Technology stack detection |
| `/seo dataforseo whois <domain>` | WHOIS registration data |
| `/seo dataforseo content <keyword/url>` | Content analysis and trends |
| `/seo dataforseo listings <keyword>` | Business listings search |
| `/seo dataforseo ai-scrape <query> [--platform chatgpt\|gemini]` | ChatGPT/Gemini scraper for GEO |
| `/seo dataforseo ai-mentions <keyword>` | LLM mention tracking for GEO |
| `/seo dataforseo ai-keyword-data <keywords>` | Search volume en AI tools (no solo Google) |
| `/seo dataforseo historical-serp <keyword>` | Evolución histórica de SERP + AIO timeline |
| `/seo dataforseo bulk <operation> <data>` | Operaciones bulk eficientes (keywords, backlinks, traffic) |

---

## SERP Analysis

### `/seo dataforseo serp <keyword>`

Fetch live Google organic search results.

**MCP tools:** `serp_organic_live_advanced`

**Default parameters:** location_code=2840 (US), language_code=en, device=desktop, depth=100

**Also supports:** The `serp_organic_live_advanced` tool supports Google, Bing, and Yahoo via the `se` parameter. Specify "bing" or "yahoo" to switch search engines.

**Output:** Rank, URL, title, description, domain, featured snippets, AI overview references, People Also Ask.

### `/seo dataforseo serp-youtube <keyword>`

Fetch YouTube search results. Valuable for GEO. YouTube mentions correlate most strongly with AI citations.

**MCP tools:** `serp_youtube_organic_live_advanced`

**Output:** Video title, channel, views, upload date, description, URL.

### `/seo dataforseo youtube <video_id>`

Deep analysis of a specific YouTube video: info, comments, and subtitles. YouTube mentions have the strongest correlation (0.737) with AI visibility, making this critical for GEO analysis.

**MCP tools:** `serp_youtube_video_info_live_advanced`, `serp_youtube_video_comments_live_advanced`, `serp_youtube_video_subtitles_live_advanced`

**Parameters:** video_id (the YouTube video ID, e.g., "dQw4w9WgXcQ")

**Output:** Video metadata (title, channel, views, likes, description), top comments with engagement, subtitle/transcript text.

---

## Keyword Research

### `/seo dataforseo keywords <seed>`

Generate keyword ideas, suggestions, and related terms from a seed keyword.

**MCP tools:** `dataforseo_labs_google_keyword_ideas`, `dataforseo_labs_google_keyword_suggestions`, `dataforseo_labs_google_related_keywords`

**Default parameters:** location_code=2840 (US), language_code=en, limit=50

**Output:** Keyword, search volume, CPC, competition level, keyword difficulty, trend.

### `/seo dataforseo volume <keywords>`

Get search volume and metrics for a list of keywords.

**MCP tools:** `kw_data_google_ads_search_volume`

**Parameters:** keywords (array, comma-separated), location_code, language_code

**Output:** Keyword, monthly search volume, CPC, competition, monthly trend data.

### `/seo dataforseo difficulty <keywords>`

Calculate keyword difficulty scores for ranking competitiveness.

**MCP tools:** `dataforseo_labs_bulk_keyword_difficulty`

**Parameters:** keywords (array), location_code, language_code

**Output:** Keyword, difficulty score (0-100), interpretation (Easy/Medium/Hard/Very Hard).

### `/seo dataforseo intent <keywords>`

Classify keywords by user search intent.

**MCP tools:** `dataforseo_labs_search_intent`

**Parameters:** keywords (array), location_code, language_code

**Output:** Keyword, intent type (informational, navigational, commercial, transactional), confidence score.

### `/seo dataforseo trends <keyword>`

Analyze keyword trends over time using Google Trends data.

**MCP tools:** `kw_data_google_trends_explore`

**Parameters:** keywords (array), location_code, date_from, date_to, language_code

**Output:** Keyword, time series data, trend direction, seasonality signals.

---

## Domain & Competitor Analysis

### `/seo dataforseo backlinks <domain>`

Comprehensive backlink profile analysis.

**MCP tools:** `backlinks_summary`, `backlinks_backlinks`, `backlinks_anchors`, `backlinks_referring_domains`, `backlinks_bulk_spam_score`, `backlinks_timeseries_summary`

**Default parameters:** limit=100 per sub-call

**Output:** Total backlinks, referring domains, domain rank, spam score, top anchors, new/lost backlinks over time, dofollow ratio, top referring domains.

### `/seo dataforseo competitors <domain>`

Identify competing domains and estimate traffic.

**MCP tools:** `dataforseo_labs_google_competitors_domain`, `dataforseo_labs_google_domain_rank_overview`, `dataforseo_labs_bulk_traffic_estimation`

**Output:** Competitor domains, keyword overlap %, estimated traffic, domain rank, common keywords.

### `/seo dataforseo ranked <domain>`

List keywords a domain ranks for with positions and page data.

**MCP tools:** `dataforseo_labs_google_ranked_keywords`, `dataforseo_labs_google_relevant_pages`

**Default parameters:** limit=100, location_code=2840

**Output:** Keyword, position, URL, search volume, traffic share, SERP features.

### `/seo dataforseo intersection <domain1> <domain2> [...]`

Find shared keywords and backlink sources across 2-20 domains.

**MCP tools:** `dataforseo_labs_google_domain_intersection`, `backlinks_domain_intersection`

**Parameters:** domains (2-20 array)

**Output:** Shared keywords with positions per domain, shared backlink sources, unique keywords per domain.

### `/seo dataforseo traffic <domains>`

Estimate organic search traffic for one or more domains.

**MCP tools:** `dataforseo_labs_bulk_traffic_estimation`

**Parameters:** domains (array)

**Output:** Domain, estimated organic traffic, estimated traffic cost, top keywords.

### `/seo dataforseo subdomains <domain>`

Enumerate subdomains with their ranking data and traffic estimates.

**MCP tools:** `dataforseo_labs_google_subdomains`

**Parameters:** target (domain), location_code, language_code

**Output:** Subdomain, ranked keywords count, estimated traffic, organic cost.

### `/seo dataforseo top-searches <domain>`

Find the most popular search queries that mention a specific domain in results.

**MCP tools:** `dataforseo_labs_google_top_searches`

**Parameters:** target (domain), location_code, language_code

**Output:** Query, search volume, domain position, SERP features, traffic share.

---

## Technical / On-Page

### `/seo dataforseo onpage <url>`

Run on-page analysis including Lighthouse audit and content parsing.

**MCP tools:** `on_page_instant_pages`, `on_page_content_parsing`, `on_page_lighthouse`

**Usage:**
- `on_page_instant_pages`:Quick page analysis (status codes, meta tags, content size, page timing, broken links, on-page checks)
- `on_page_content_parsing`:Extract and parse page content (plain text, word count, structure)
- `on_page_lighthouse`:Full Lighthouse audit (performance score, accessibility, best practices, SEO, Core Web Vitals)

**Output:** Pages crawled, status codes, meta tags, titles, content size, load times, Lighthouse scores, broken links, resource analysis.

### `/seo dataforseo tech <domain>`

Detect technologies used on a domain.

**MCP tools:** `domain_analytics_technologies_domain_technologies`

**Output:** Technology name, version, category (CMS, analytics, CDN, framework, etc.).

### `/seo dataforseo whois <domain>`

Retrieve WHOIS registration data.

**MCP tools:** `domain_analytics_whois_overview`

**Output:** Registrar, creation date, expiration date, nameservers, registrant info (if public).

---

## Content & Business Data

### `/seo dataforseo content <keyword/url>`

Analyze content quality, search for content by topic, and track phrase trends.

**MCP tools:** `content_analysis_search`, `content_analysis_summary`, `content_analysis_phrase_trends`

**Parameters:** keyword (for search/trends) or URL (for summary)

**Output:** Content matches with quality scores, sentiment analysis, readability metrics, phrase trend data over time.

### `/seo dataforseo listings <keyword>`

Search business listings for local SEO competitive analysis.

**MCP tools:** `business_data_business_listings_search`

**Parameters:** keyword, location (optional)

**Output:** Business name, description, category, address, phone, domain, rating, review count, claimed status.

---

## AI Visibility / GEO

### `/seo dataforseo ai-scrape <query> [--platform chatgpt|gemini]`

Scrape what AI search platforms return for a query. Real GEO visibility check: see which sources ChatGPT or Gemini cites for your target keywords.

**MCP tools:**
- ChatGPT: `ai_optimization_chat_gpt_scraper`
- Gemini: `ai_optimization_gemini_scraper` *(disponible desde 2025 — DataForSEO AI Optimization API)*

**Parameters:** query, location_code (optional), language_code (optional).
- Para ChatGPT: `ai_optimization_chat_gpt_scraper_locations` para ubicaciones disponibles
- Para Gemini: verificar ubicaciones disponibles en el endpoint equivalente de Gemini

**Workflow multi-platform (recomendado):**
```
1. Lanzar ChatGPT scraper para la query objetivo
2. Lanzar Gemini scraper para la misma query
3. Comparar: ¿aparece el dominio del cliente en ambos, uno, o ninguno?
4. Identificar qué dominios sí aparecen → análisis de por qué (contenido, autoridad, formato)
5. Reportar como "ChatGPT Visibility: ✅/❌ | Gemini Visibility: ✅/❌"
```

**Output:** Respuesta del AI, fuentes citadas/URLs, dominios referenciados.

> **Nota:** Perplexity scraper — verificar disponibilidad en DataForSEO. Si no está disponible como endpoint, usar Firecrawl `/interact` para simular búsquedas en perplexity.ai.

### `/seo dataforseo ai-mentions <keyword>`

Track how LLMs mention brands, domains, and topics. Critical for GEO. Measures actual AI visibility across multiple LLM platforms.

**MCP tools:** `ai_opt_llm_ment_search`, `ai_opt_llm_ment_top_domains`, `ai_opt_llm_ment_top_pages`, `ai_opt_llm_ment_agg_metrics`

**Parameters:** keyword, location_code (optional), language_code (optional). Use `ai_opt_llm_ment_loc_and_lang` for available locations/languages and `ai_optimization_llm_models` for supported LLM models.

**Workflow:**
1. Search LLM mentions with `ai_opt_llm_ment_search` (find mentions of a brand/keyword across LLM responses)
2. Get top cited domains with `ai_opt_llm_ment_top_domains` (which domains are most cited for this topic)
3. Get top cited pages with `ai_opt_llm_ment_top_pages` (which specific pages are most cited)
4. Get aggregate metrics with `ai_opt_llm_ment_agg_metrics` (overall mention volume, trends)

**Output:** LLM mention count, top cited domains with frequency, top cited pages, mention trends over time, cross-platform visibility scores.

**Advanced:** Use `ai_opt_llm_ment_cross_agg_metrics` for cross-model comparison (how mentions differ across ChatGPT, Claude, Perplexity, etc.).

---

### `/seo dataforseo ai-keyword-data <keywords>`

Obtiene métricas de keywords basadas en uso en AI tools — complementa el volumen de búsqueda clásico de Google con datos de popularidad en AI search.

**MCP tools:** `ai_optimization_keyword_data_search_volume`

**Por qué usar esto vs. volumen clásico:**
```
Volumen Google Keyword Planner → búsquedas en Google Search
AI Keyword Data → popularidad en herramientas AI (ChatGPT, Gemini, etc.)
Diferencia clave: hay keywords populares en AI que no tienen volumen Google significativo
→ útil para content strategy orientada a AI citations, no solo ranking orgánico clásico
```

**Output:** Keyword, AI search volume estimate, tendencia en AI platforms, comparativa vs. volumen Google.

---

### `/seo dataforseo historical-serp <keyword> [--date YYYY-MM]`

Trackea la evolución histórica de una SERP para detectar cuándo aparecieron/desaparecieron AI Overviews, featured snippets u otros SERP features.

**MCP tools:** `dataforseo_labs_google_historical_serp`, `dataforseo_labs_google_historical_keyword_data`, `dataforseo_labs_google_historical_rank_overview`

**Workflows:**

*1. Timeline de AI Overviews para un keyword:*
```
historical_serp(keyword, date_range=últimos 12 meses)
→ Identificar en qué mes apareció por primera vez AIO para ese keyword
→ Correlacionar con caída de CTR (si hay datos GSC)
→ Recomendar: optimizar para AIO si ya está presente, preparar si no
```

*2. Evolución del dominio en el tiempo:*
```
historical_rank_overview(domain, date_range)
→ Ver cómo cambió el tráfico estimado, las keywords en top 10, el domain rank
→ Útil para análisis de impacto de algorithm updates (HCU, spam updates, etc.)
```

*3. Competitive SERP evolution:*
```
Para cada competidor en top 5:
  historical_serp(keyword) → posición por mes
→ Detectar quién subió/bajó y cuándo → inferir causas
```

**Output:** Tabla cronológica de posiciones, SERP features presentes por mes, cambios de AIO coverage.

---

### `/seo dataforseo bulk <operation> <data>`

Operaciones en bulk para análisis a escala. Siempre preferir bulk sobre llamadas individuales.

**MCP tools disponibles para bulk:**

| Operación | MCP tool | Límite típico | Costo estimado |
|-----------|---------|--------------|---------------|
| Keyword difficulty (500+ keywords) | `dataforseo_labs_bulk_keyword_difficulty` | 1,000/llamada | ~$0.002/keyword |
| Traffic estimation (múltiples dominios) | `dataforseo_labs_bulk_traffic_estimation` | 1,000/llamada | ~$0.001/dominio |
| Backlink count (lista de páginas) | `backlinks_bulk_backlinks` | 1,000/llamada | ~$0.001/URL |
| Referring domains bulk | `backlinks_bulk_referring_domains` | 1,000/llamada | ~$0.001/dominio |
| New/lost backlinks | `backlinks_bulk_new_lost_referring_domains` | 1,000/llamada | ~$0.002/dominio |
| Domain ranks | `backlinks_bulk_ranks` | 1,000/llamada | ~$0.001/dominio |

**Regla de eficiencia:**
```
❌ Llamar keyword_difficulty 100 veces con 1 keyword cada vez = 100 créditos
✅ Llamar dataforseo_labs_bulk_keyword_difficulty 1 vez con 100 keywords = ~10 créditos
```

**Workflow bulk típico (KW research completo):**
```
1. kw_ideas(seed) → lista de 200 keywords candidatas
2. bulk_keyword_difficulty(200 keywords) → filtrar KD < 40
3. bulk search_volume(keywords_filtradas) → ordenar por volumen
4. search_intent(top 50) → clasificar por intent
→ Output: shortlist priorizada para content plan
```

---

## Pipeline Workflows End-to-End

### Pipeline 1: KW Research Completo

```
Input: seed keyword + dominio cliente

Paso 1: keyword_ideas(seed) → 100-200 keywords candidatas
Paso 2: bulk_keyword_difficulty(todas) → filtrar según KD objetivo
Paso 3: search_volume(filtradas) → datos de volumen real
Paso 4: search_intent(top candidatas) → clasificar informational/commercial/transactional
Paso 5: ranked_keywords(dominio) → eliminar keywords donde ya rankea en top 5

Output: tabla priorizada → volumen / KD / intent / gap vs. posición actual
Siguiente paso: seo-keywords para content brief
```

### Pipeline 2: Competitive Gap Analysis

```
Input: dominio cliente + 3-5 competidores

Paso 1: competitors(dominio) → identificar top competidores si no se conocen
Paso 2: intersection(cliente, comp1, comp2, comp3) → keywords compartidas
Paso 3: ranked_keywords(cada competidor) → keywords donde cliente NO rankea
Paso 4: bulk_traffic_estimation(competidores) → cuánto tráfico tienen ellos
Paso 5: backlinks_domain_intersection → fuentes de links compartidas

Output: gap analysis → qué keywords tienen ellos que no tiene el cliente
Siguiente paso: seo-content para crear el contenido que cierra el gap
```

### Pipeline 3: GEO / AI Visibility Report

```
Input: dominio cliente + lista de queries objetivo

Paso 1: ai_scrape_chatgpt(queries) → ¿aparece el dominio?
Paso 2: ai_scrape_gemini(queries) → ¿aparece en Gemini?
Paso 3: ai_mentions(marca/dominio) → menciones en LLMs + dominios más citados
Paso 4: ai_opt_llm_ment_agg_metrics(marca) → volumen de menciones + tendencia
Paso 5: top_pages_mentioned → qué páginas son las más citadas por AI

Output: GEO-VISIBILITY-BASELINE.md → estado actual de visibilidad AI
Siguiente paso: seo-geo para optimización
```

---

## Available Utility Tools

These DataForSEO tools are available for internal use by the agent but do not have dedicated commands:

- `serp_locations`:Location code lookups for SERP queries
- `serp_youtube_locations`:Location code lookups for YouTube queries
- `kw_data_google_ads_locations`:Location lookups for keyword data
- `kw_data_dfs_trends_demography`:Demographic data for trend analysis
- `kw_data_dfs_trends_subregion_interests`:Subregion interest data for trends
- `kw_data_dfs_trends_explore`:DFS proprietary trends data
- `kw_data_google_trends_categories`:Google Trends category lookups
- `dataforseo_labs_google_keyword_overview`:Quick keyword metrics overview
- `dataforseo_labs_google_historical_serp`:Historical SERP results for a keyword
- `dataforseo_labs_google_serp_competitors`:Competitors for a specific SERP
- `dataforseo_labs_google_keywords_for_site`:Keywords a site ranks for (alternative to ranked)
- `dataforseo_labs_google_page_intersection`:Page-level intersection analysis
- `dataforseo_labs_google_historical_rank_overview`:Historical domain rank data
- `dataforseo_labs_google_historical_keyword_data`:Historical keyword metrics
- `dataforseo_labs_available_filters`:Available filter options for Labs endpoints
- `backlinks_competitors`:Find domains with similar backlink profiles
- `backlinks_bulk_backlinks`:Bulk backlink counts for multiple targets
- `backlinks_bulk_new_lost_referring_domains`:Bulk new/lost referring domains
- `backlinks_bulk_new_lost_backlinks`:Bulk new/lost backlinks
- `backlinks_bulk_ranks`:Bulk rank overview for multiple targets
- `backlinks_bulk_referring_domains`:Bulk referring domain counts
- `backlinks_domain_pages_summary`:Summary of pages on a domain
- `backlinks_domain_pages`:List pages on a domain with backlink data
- `backlinks_page_intersection`:Shared backlink sources at page level
- `backlinks_referring_networks`:Referring network analysis
- `backlinks_timeseries_new_lost_summary`:Track new/lost backlinks over time
- `backlinks_bulk_pages_summary`:Bulk page summaries
- `backlinks_available_filters`:Available filter options for Backlinks endpoints
- `domain_analytics_whois_available_filters`:WHOIS filter options
- `domain_analytics_technologies_available_filters`:Technology detection filter options
- `ai_opt_kw_data_loc_and_lang`:AI optimization keyword data locations/languages
- `ai_optimization_keyword_data_search_volume`:AI-specific keyword volume data
- `ai_optimization_llm_response`:Direct LLM response analysis
- `ai_optimization_llm_mentions_filters`:Available filters for LLM mentions
- `ai_optimization_chat_gpt_scraper_locations`:Available locations for ChatGPT scraper

## Cross-Skill Integration

When DataForSEO MCP tools are available, other claude-seo skills can leverage live data:

- **seo-audit**:Spawn `seo-dataforseo` agent for real SERP, backlink, on-page, and listings data
- **seo-technical**:Use `on_page_instant_pages` / `on_page_lighthouse` for real crawl data, `domain_analytics_technologies_domain_technologies` for stack detection
- **seo-content**:Use `kw_data_google_ads_search_volume`, `dataforseo_labs_bulk_keyword_difficulty`, `dataforseo_labs_search_intent` for real keyword metrics, `content_analysis_summary` for content quality
- **seo-page**:Use `serp_organic_live_advanced` for real SERP positions, `backlinks_summary` for link data
- **seo-geo**:Use `ai_optimization_chat_gpt_scraper` for real ChatGPT visibility, `ai_opt_llm_ment_search` for LLM mention tracking
- **seo-plan**:Use `dataforseo_labs_google_competitors_domain`, `dataforseo_labs_google_domain_intersection`, `dataforseo_labs_bulk_traffic_estimation` for real competitive intelligence
- **seo-keywords**:Use `kw_data_google_ads_search_volume`, `dataforseo_labs_bulk_keyword_difficulty`, `dataforseo_labs_search_intent` for live volume, difficulty, and intent data
- **seo-competitive**:Use `dataforseo_labs_google_competitors_domain`, `dataforseo_labs_google_domain_intersection`, `backlinks_domain_intersection` for live competitor analysis
- **seo-reporting**:Use `backlinks_timeseries_summary`, `dataforseo_labs_google_ranked_keywords` for backlink and ranking data in monthly reports
- **seo-local**:Use `business_data_business_listings_search` for local competitive landscape and citation auditing
- **seo-maps**:Use `business_data_business_listings_search` and Maps SERP endpoints for geo-grid tracking and GBP audits

## Error Handling

- **MCP server not connected**: Report that DataForSEO extension is not installed or MCP server is unreachable. Suggest running `./extensions/dataforseo/install.sh`
- **API authentication failed**: Report invalid credentials. Suggest checking DataForSEO API login/password in MCP config
- **Rate limit exceeded**: Report the limit hit and suggest waiting before retrying
- **No results returned**: Report "no data found" for the query rather than guessing. Suggest broadening the query or checking location/language codes
- **Invalid location code**: Report the error and suggest using the locations lookup tool to find the correct code

## Output Formatting

Match existing claude-seo output patterns:
- Use tables for comparative data
- Prioritize issues as Critical > High > Medium > Low
- Include specific, actionable recommendations
- Show scores as XX/100 where applicable
- Note data source as "DataForSEO (live)" to distinguish from static analysis
