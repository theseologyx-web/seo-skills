---
name: seo-google
description: >
  Google SEO APIs: Search Console (Search Analytics, URL Inspection, Sitemaps),
  PageSpeed Insights v5, CrUX field data with 25-week history, Indexing API v3,
  and GA4 organic traffic. Provides real Google field data for Core Web Vitals,
  indexation status, search performance, and organic traffic trends. Use when
  user says "search console", "GSC", "PageSpeed", "CrUX", "field data",
  "indexing API", "GA4 organic", "URL inspection", "google api setup",
  "real CWV data", "impressions", "clicks", "CTR", "position data",
  "LCP", "INP", "CLS", "FCP", "TTFB", or "Lighthouse scores". Not for third-party SERP data, keyword metrics, or backlink data — use seo-dataforseo.
user-invokable: true
argument-hint: "[command] [url|property]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Google SEO APIs

Direct access to Google's own SEO data. Bridges the gap between crawl-based
analysis (existing claude-seo skills) and Google's real-time field data: actual
Chrome user metrics, real indexation status, search performance, and organic traffic.

All APIs are free. Setup requires a Google Cloud project with API key and/or
service account -- run `/seo google setup` for step-by-step instructions.

## Prerequisites

Before executing any command, check credentials:
```bash
python scripts/google_auth.py --check --json
```

Config file: `~/.config/claude-seo/google-api.json`
```json
{
  "service_account_path": "/path/to/service_account.json",
  "api_key": "AIzaSy...",
  "default_property": "sc-domain:example.com",
  "ga4_property_id": "properties/123456789"
}
```

If missing, read `references/auth-setup.md` and walk the user through setup.

### Credential Tiers

| Tier | Detection | Available Commands |
|------|-----------|-------------------|
| **0** (API Key) | `api_key` present | `pagespeed`, `crux`, `crux-history`, `youtube`, `nlp` |
| **1** (OAuth/SA) | + OAuth token or service account | Tier 0 + `gsc`, `inspect`, `sitemaps`, `index` |
| **2** (Full) | + `ga4_property_id` configured | Tier 1 + `ga4`, `ga4-pages` |
| **3** (Ads) | + `ads_developer_token` + `ads_customer_id` | Tier 2 + `keywords`, `volume` |

Always communicate the detected tier before running commands.

## Quick Reference

| Command | What it does | Tier |
|---------|-------------|------|
| `/seo google setup` | Check/configure API credentials | -- |
| `/seo google pagespeed <url>` | PSI Lighthouse + CrUX field data | 0 |
| `/seo google crux <url>` | CrUX field data only (p75 metrics) | 0 |
| `/seo google crux-history <url>` | **40-week** CWV trend analysis | 0 |
| `/seo google gsc <property>` | Search Console: clicks, impressions, CTR, position | 1 |
| `/seo google inspect <url>` | URL Inspection: index status, canonical, crawl info | 1 |
| `/seo google inspect-batch <file>` | Batch URL Inspection from file | 1 |
| `/seo google sitemaps <property>` | GSC sitemap status | 1 |
| `/seo google index <url>` | Submit URL to Indexing API | 1 |
| `/seo google index-batch <file>` | Batch submit up to 200 URLs | 1 |
| `/seo google ga4 [property-id]` | GA4 organic traffic report | 2 |
| `/seo google ga4-pages [property-id]` | Top organic landing pages | 2 |
| `/seo google youtube <query>` | YouTube video search (views, likes, duration) | 0 |
| `/seo google youtube-video <id>` | YouTube video details + top comments | 0 |
| `/seo google nlp <url-or-text>` | NLP entity extraction + sentiment + classification | 0 |
| `/seo google entities <url-or-text>` | Entity analysis only (for E-E-A-T) | 0 |
| `/seo google keywords <seed>` | Keyword ideas from Google Ads Keyword Planner | 3 |
| `/seo google volume <keywords>` | Search volume lookup from Keyword Planner | 3 |
| `/seo google entity <query>` | Knowledge Graph entity check | 0 |
| `/seo google safety <url>` | Web Risk URL safety check | 0 |
| `/seo google quotas` | Show rate limits for all APIs | -- |

---

## PageSpeed + CrUX

### `/seo google pagespeed <url>`

Combined Lighthouse lab data + CrUX field data.

**Script:** `python scripts/pagespeed_check.py <url> --json`
**Reference:** `references/pagespeed-crux-api.md`
**Default:** Both mobile + desktop strategies, all Lighthouse categories.

Output merges lab scores (point-in-time Lighthouse) with field data (28-day
Chrome user metrics). CrUX tries URL-level first, falls back to origin-level.

### `/seo google crux <url>`

CrUX field data only (no Lighthouse run). Faster.

**Script:** `python scripts/pagespeed_check.py <url> --crux-only --json`

### `/seo google crux-history <url>`

**40-week** CrUX History trends (actualizado desde 25 semanas). Muestra si las métricas CWV mejoran, están estables o degradan. Permite correlacionar cambios con deploys, algorithm updates y cambios de contenido.

**Script:** `python scripts/crux_history.py <url> --json`
**Reference:** `references/pagespeed-crux-api.md`

Output includes per-metric trend direction, percentage change, weekly p75 values, and LCP image subparts.

#### LCP Image Subparts (disponibles en CrUX API desde 2025)

CrUX ahora expone los subcomponentes del LCP para diagnóstico más preciso:

| Subpart | Qué mide | Señal de problema |
|---------|---------|------------------|
| `time_to_first_byte` (TTFB) | Tiempo hasta primer byte del servidor | > 0.8s → problema de servidor/CDN |
| `resource_load_delay` | Delay entre TTFB y start de descarga del recurso LCP | > 0.2s → recurso no priorizado (falta preload) |
| `resource_load_time` | Tiempo de descarga del recurso LCP | > 0.5s → imagen demasiado grande o conexión lenta |
| `element_render_delay` | Delay entre descarga y render del elemento LCP | > 0.1s → JS bloqueando render |

```
LCP total = TTFB + resource_load_delay + resource_load_time + element_render_delay

Diagnóstico rápido:
- TTFB alto → fix: CDN, server response time, cache headers
- resource_load_delay alto → fix: <link rel="preload"> para imagen LCP
- resource_load_time alto → fix: comprimir imagen, WebP, CDN más cercano
- element_render_delay alto → fix: reducir JS que bloquea render
```

Extraer con: `crux_history` retorna `lcp_resource_type` (text/image) y subparts disponibles si el recurso LCP es una imagen.

---

## Search Console

### `/seo google gsc <property>`

Search Analytics: clicks, impressions, CTR, position for last 28 days.

**Script:** `python scripts/gsc_query.py --property <property> --json`
**Reference:** `references/search-console-api.md`
**Default:** 28 days, dimensions=query,page, type=web, limit=1000.

Includes quick-win detection: queries at position 4-10 with high impressions.

### `/seo google inspect <url>`

URL Inspection: real indexation status from Google.

**Script:** `python scripts/gsc_inspect.py <url> --json`

Returns: verdict (PASS/FAIL), coverage state, robots.txt status, indexing state,
page fetch state, canonical selection, mobile usability, rich results.

### `/seo google inspect-batch <file>`

Batch inspection from a file (one URL per line). Rate limited to 2,000/day per site.

**Script:** `python scripts/gsc_inspect.py --batch <file> --json`

### `/seo google sitemaps <property>`

List submitted sitemaps with status, errors, warnings.

**Script:** `python scripts/gsc_query.py sitemaps --property <property> --json`

---

## Indexing API

### `/seo google index <url>`

Notify Google of a URL update.

**Script:** `python scripts/indexing_notify.py <url> --json`
**Reference:** `references/indexing-api.md`

The Indexing API is officially for JobPosting and BroadcastEvent/VideoObject pages.
Always inform the user of this restriction. Daily quota: 200 publish requests.

### `/seo google index-batch <file>`

Batch submit URLs from a file. Tracks quota usage.

**Script:** `python scripts/indexing_notify.py --batch <file> --json`

---

## GA4 Traffic

### `/seo google ga4 [property-id]`

Organic traffic report: daily sessions, users, engagement rate, avg engagement time.

**Script:** `python scripts/ga4_report.py --property <id> --json`
**Reference:** `references/ga4-data-api.md`
**Default:** 28 days, filtered to Organic Search channel group.

> **Nota 2026:** GA4 usa Engagement Rate (no bounce rate). Bounce rate está deprecated. El report debe mostrar `engagementRate` y `averageSessionDuration`, no `bounceRate`.

### `/seo google ga4-pages [property-id]`

Top organic landing pages ranked by sessions.

**Script:** `python scripts/ga4_report.py --property <id> --report top-pages --json`

### `/seo google ga4-conversions [property-id]`

Conversiones orgánicas por tipo de evento y canal de atribución.

**Script:** `python scripts/ga4_report.py --property <id> --report conversions --json`

**Output:** Conversion event name, count, conversion rate por canal (organic, direct, paid), revenue si está configurado.
**Attribution model:** Data-driven (GA4 default desde oct 2023). Si tiene < 50 conversiones/mes, cae a Last-click — indicarlo en output.

### `/seo google ga4-engagement [property-id]`

Métricas de engagement por página orgánica.

**Script:** `python scripts/ga4_report.py --property <id> --report engagement --json`

**Output:** Landing page, engagement rate, avg engagement time, engaged sessions, scroll depth (si configurado), events/session.
**Uso:** Identificar páginas con tráfico pero baja interacción → candidatas a CRO o actualización de contenido.

### `/seo google ga4-attribution [property-id]`

Reporte de atribución data-driven: cómo cada canal contribuye al funnel.

**Script:** `python scripts/ga4_report.py --property <id> --report attribution --json`

**Output:** Canal, assisted conversions, direct conversions, revenue atribuido (data-driven vs last-click comparison).
**Uso:** Mostrar al cliente el valor real de SEO en el funnel completo, no solo como último canal.

### `/seo google ga4-realtime [property-id]`

Tráfico orgánico en tiempo real. Útil post-deploy o post-publicación de contenido.

**Script:** `python scripts/ga4_report.py --property <id> --report realtime --json`

**Output:** Usuarios activos ahora, páginas activas, fuente de tráfico (organic/direct/etc.), eventos en tiempo real.

#### Consent Mode v2 — Impacto en Datos GA4

**Contexto:** Obligatorio en EU desde marzo 2024. Afecta la calidad de todos los datos GA4.

```
Sin Consent Mode v2 configurado:
→ GA4 no recibe datos de usuarios EU que rechazan cookies
→ Los datos aparecen como "Direct" o desaparecen completamente

Con Consent Mode v2 + Behavioral Modeling activo:
→ GA4 modela estadísticamente las sesiones no consentidas
→ Los datos modelados están marcados internamente (no visibles directamente)
→ Más preciso que sin modelado, pero menos preciso que datos reales

Cómo verificar si Consent Mode está activo:
GA4 → Admin → Data collection → Consent mode → Status
→ "Active" con checkmarks en analytics_storage y ad_storage = correcto

Activar behavioral modeling:
GA4 → Admin → Reporting → Reporting identity → Blended
(requiere mínimo ~1,000 conversiones/mes para ser estadísticamente válido)
```

**Cómo reportar al cliente:**
```
Si Consent Mode activo con modelado:
"Los datos incluyen estimaciones para usuarios EU que no aceptaron cookies 
(~X% del tráfico total). Los datos son orientativos para ese segmento."

Si Consent Mode NO activo:
"Importante: los datos de tráfico EU están incompletos. Recomendamos 
implementar Consent Mode v2 para recuperar datos modelados."
```

#### GA4 2026 Features (nuevas)

| Feature | Qué hace | Cómo acceder |
|---------|---------|-------------|
| **Analytics Advisor** | AI help contextual dentro de GA4 | Icono sparkle en cualquier reporte |
| **Generated Insights** | Resumen automático de cambios significativos | GA4 Homepage → Insights |
| **Cross-channel budgeting** (beta) | Planificación de presupuesto multi-canal | Admin → Advertising → Budget planning |
| **Conversion attribution analysis** (beta) | Compara modelos de atribución lado a lado | Advertising → Attribution → Model comparison |

---

#### `/seo google gsc-aio [property]`

AI Overviews performance en Search Console. Muestra clics e impresiones desde el placement de AI Overviews.

**Script:** `python scripts/gsc_query.py --property <property> --type web --search-type ai_overviews --json`

**Cómo acceder en GSC manual:**
```
GSC → Performance → Search type → AI Overviews
→ Filtrar por queries y páginas para ver qué contenido aparece en AIO
→ Comparar CTR desde AIO vs. posición orgánica clásica
```

**Output:** Queries que triggean AIO para el sitio, impresiones, clics desde AIO, CTR.

> **Interpretación clave:** CTR desde AIO suele ser < 5% (los usuarios leen la respuesta sin hacer clic). Priorizar queries donde el sitio aparece en AIO pero NO en la posición orgánica 1 — doble oportunidad.

#### `/seo google gsc-discover [property]`

Google Discover performance — para sitios con contenido editorial/publisher.

**Script:** `python scripts/gsc_query.py --property <property> --type discover --json`

**Aplica cuando:** Publisher, media, blog con >10K sesiones/mes de Discover.
**Output:** Páginas con impresiones en Discover, CTR, clics. Identificar qué tipo de contenido funciona en Discover vs. Search.

#### `/seo google gsc-compare [property] [--period mom|yoy]`

Comparación automática MoM o YoY de métricas GSC.

**Script:** `python scripts/gsc_query.py --property <property> --compare <period> --json`

**Output:** Tabla de clicks / impressions / CTR / position: período actual vs. período anterior, diferencia absoluta y %.

#### GSC Bulk Data Export a BigQuery

Para sitios grandes (>10K páginas o >10K queries/día), la API de Search Analytics tiene límite de 1,000 filas. La exportación a BigQuery elimina este límite.

**Setup (una vez):**
```
1. GSC → Settings → Bulk data export → Enable
2. Seleccionar BigQuery project donde exportar
3. Los datos se actualizan diariamente (lag de 2-3 días igual que API)
4. Dataset incluye: searchdata_site_impression, searchdata_url_impression
```

**Ventajas vs. API estándar:**
```
✅ Sin límite de filas (todos los datos, no solo top 1,000)
✅ Incluye queries anonimizadas que la API excluye por privacidad
✅ Permite SQL avanzado (aggregate, join, window functions)
✅ Histórico más largo disponible (desde cuando se activó)
✅ Integración con Looker Studio via BigQuery connector
```

**Queries SQL útiles:**
```sql
-- Top queries no-brand con CTR bajo (oportunidades de mejora de title/meta)
SELECT query, SUM(clicks) as clicks, SUM(impressions) as impressions,
       SUM(clicks)/SUM(impressions) as ctr,
       AVG(sum_position)/SUM(impressions) as avg_position
FROM `project.dataset.searchdata_site_impression`
WHERE data_date >= DATE_SUB(CURRENT_DATE(), INTERVAL 28 DAY)
  AND query NOT LIKE '%brand%'
GROUP BY query
HAVING avg_position < 10 AND ctr < 0.05
ORDER BY impressions DESC
LIMIT 100;
```

**Cuándo recomendar:** Clientes SaaS con >50K queries/mes en GSC o publishers con cientos de miles de páginas.

---

## YouTube (Video SEO)

YouTube mentions have the strongest AI visibility correlation (0.737). Free, API key only.

### `/seo google youtube <query>`

Search YouTube for videos. Returns title, channel, views, likes, duration.

**Script:** `python scripts/youtube_search.py search "<query>" --json`
**Reference:** `references/youtube-api.md`
**Quota:** 100 units per search (10,000 units/day free).

### `/seo google youtube-video <video_id>`

Detailed video info + tags + top 10 comments.

**Script:** `python scripts/youtube_search.py video <video_id> --json`
**Quota:** 2 units (video details + comments).

---

## NLP Content Analysis

Google's own entity/sentiment analysis. Enhances E-E-A-T scoring.

### `/seo google nlp <url-or-text>`

Full NLP analysis: entities, sentiment, content classification.

**Script:** `python scripts/nlp_analyze.py --url <url> --json` or `--text "..."`
**Reference:** `references/nlp-api.md`
**Free tier:** 5,000 units/month. Requires billing enabled on GCP project.

### `/seo google entities <url-or-text>`

Entity extraction only (faster, less quota).

**Script:** `python scripts/nlp_analyze.py --url <url> --features entities --json`

---

## Keyword Research (Google Ads)

Gold-standard keyword volume data. Requires Google Ads account.

### `/seo google keywords <seed>`

Generate keyword ideas from seed terms.

**Script:** `python scripts/keyword_planner.py ideas "<seed>" --json`
**Reference:** `references/keyword-planner-api.md`
**Requires:** Ads developer token + customer ID in config (Tier 3).

### `/seo google volume <keywords>`

Search volume for specific keywords (comma-separated).

**Script:** `python scripts/keyword_planner.py volume "<kw1>,<kw2>" --json`

---

## Supplementary

### `/seo google entity <query>`

Knowledge Graph entity check. Verifies brand presence.

**Reference:** `references/supplementary-apis.md`
Uses Knowledge Graph Search API with API key.

### `/seo google safety <url>`

Web Risk API check for malware/social engineering flags.

**Reference:** `references/supplementary-apis.md`

### `/seo google quotas`

Display rate limits table. Read `references/rate-limits-quotas.md`.

---

## Reports

After any analysis command, offer to generate a PDF/HTML report.

### `/seo google report <type>`

Generate a professional PDF report with charts and analytics.

**Script:** `python scripts/google_report.py --type <type> --data <json> --domain <domain> --format pdf`

| Type | Input | Output |
|------|-------|--------|
| `cwv-audit` | PSI + CrUX + CrUX History data | Core Web Vitals audit with gauges, timelines, distributions |
| `gsc-performance` | GSC query data | Search Console report with query tables, quick wins |
| `indexation` | Batch inspection data | Indexation status with coverage donut chart |
| `full` | All data combined | Comprehensive Google SEO report (all sections) |

**Workflow:**
1. Run data collection commands (pagespeed, gsc, inspect-batch, etc.)
2. Save JSON output to file: `python scripts/pagespeed_check.py <url> --json > data.json`
3. Generate report: `python scripts/google_report.py --type cwv-audit --data data.json --domain <domain>`

**Convention:** After completing analysis, suggest: "Generate a report? Use `/seo google report <type>`"

---

## Rate Limits

| API | Per-Minute | Per-Day | Auth |
|-----|-----------|---------|------|
| PSI v5 | 240 QPM | 25,000 QPD | API Key |
| CrUX + History | 150 QPM (shared) | Unlimited | API Key |
| GSC Search Analytics | 1,200 QPM/site | 30M QPD | Service Account |
| GSC URL Inspection | 600 QPM | 2,000 QPD/site | Service Account |
| Indexing API | 380 RPM | 200 publish/day | Service Account |
| GA4 Data API | 10 concurrent | ~25K tokens/day | Service Account |

## Cross-Skill Integration

- **seo-audit**: Spawns `seo-google` agent for live CWV + indexation data (conditional)
- **seo-technical**: Uses pagespeed_check.py for real CWV field data
- **seo-performance**: CrUX field data supplements Lighthouse lab data
- **seo-sitemap**: GSC sitemap status shows real crawl/index coverage
- **seo-content**: GSC query data informs keyword targeting
- **seo-geo**: GSC search appearance data includes AI Overview references
- **seo-reporting**: GSC + GA4 data is the foundation of monthly SEO reports and ROI calculations
- **seo-keywords**: GSC query data (positions 4-20, high impressions) reveals quick-win keyword opportunities

## Output Format

- CWV metrics: traffic-light rating (Good / Needs Improvement / Poor)
- Performance reports: tables with sortable columns
- Always include data freshness note
- Save reports as `GOOGLE-API-REPORT-{domain}.md`
- Use templates from `assets/templates/` for structured output

## Technical Notes

- INP replaced FID on March 12, 2024. Never reference FID.
- CLS values from CrUX are string-encoded (e.g., "0.05"). Scripts handle parsing.
- CrUX 404 = insufficient traffic, not an auth error.
- Search Analytics data has 2-3 day lag.
- `round_trip_time` replaced `effectiveConnectionType` in CrUX (Feb 2025).
- Custom Search JSON API is closed to new customers (2025).

## Error Handling

| Scenario | Action |
|----------|--------|
| No credentials configured | Run `/seo google setup`. List Tier 0 commands that work with just an API key. |
| Service account lacks GSC access | Report error. Instruct: add `client_email` to GSC > Settings > Users > Add. |
| CrUX data unavailable (404) | Report insufficient Chrome traffic. Suggest PSI lab data as fallback. |
| GA4 property not found | Report error. Show how to find property ID in GA4 Admin > Property Details. |
| Indexing API quota exceeded | Report 200/day limit. Suggest prioritizing most important URLs. |
| Rate limit (429) | Wait and retry with exponential backoff. Report which API hit the limit. |
