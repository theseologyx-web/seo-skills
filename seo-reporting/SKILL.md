---
name: seo-reporting
description: >
  SEO reporting, dashboards, and ROI measurement. Covers KPI selection, data
  visualization, client reporting, executive dashboards, traffic attribution,
  ROI calculation, and automated reporting workflows. Use when user says "SEO
  report", "SEO dashboard", "report for client", "SEO ROI", "KPIs", "SEO metrics",
  "monthly report", "executive report", or "how to measure SEO success".
user-invokable: true
argument-hint: "[domain, client name, or reporting period]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# SEO Reporting, Dashboards & ROI Measurement

Build reports that communicate SEO value clearly, attribute results correctly, and drive informed decisions — for yourself, your team, or your clients.

> **Principio 2026 — Revenue First:** Los reportes SEO deben empezar siempre por impacto en negocio (ingresos, leads, pipeline). Las métricas técnicas y de ranking son herramientas internas, no KPIs de cliente. Nunca abrir un reporte con "posición media" o "Domain Authority" — abrir con "ingresos orgánicos" o "leads orgánicos".

---

## Métricas a Retirar de Reportes (2026)

Las siguientes métricas eran útiles pero ya no son KPIs principales — usarlas solo como diagnóstico interno:

| Métrica obsoleta | Por qué retirar | Reemplazar por |
|-----------------|----------------|---------------|
| **Bounce Rate** | GA4 ya no lo calcula igual; no correlaciona con éxito SEO | **Engagement Rate** (GA4 nativo) |
| **Domain Authority (DA) / Domain Rating (DR)** como KPI | Son métricas de terceros, no de Google; varían según herramienta | Organic traffic growth + Revenue |
| **Average Position** como KPI primario | Media global de posición esconde más de lo que revela; fácil de manipular | Keywords en Top 3 / Top 10 (conteo) |
| **Páginas vistas por sesión** | Contexto-dependiente; alta ≠ bueno (puede ser navegación perdida) | Engagement Rate + Session duration |
| **Bounce Rate en reportes cliente** | Confunde; clientes no entienden si alto es bueno o malo | Engagement Rate con explicación |

> **Cómo comunicarlo al cliente:** "Hemos actualizado el dashboard para reflejar cómo Google y GA4 miden el rendimiento hoy. Ahora priorizamos métricas conectadas directamente a ingresos."

---

## 1. SEO Metrics Framework

Not all metrics matter equally. Organize them in 3 tiers:

### Tier 1: Business Metrics (What clients/executives care about)

| Metric | Definition | Source | Frequency |
|--------|-----------|--------|-----------|
| Organic Revenue | Revenue attributed to organic search | GA4 | Monthly |
| Organic Conversions | Goal completions from organic traffic | GA4 | Monthly |
| Organic Conversion Rate | Organic conversions / organic sessions | GA4 | Monthly |
| Cost Savings vs. PPC | Organic traffic × avg CPC = avoided ad spend | GA4 + GKP | Monthly |
| Leads from Organic | Lead form submissions from organic | GA4 | Monthly |
| Revenue Per Organic Visit | Organic Revenue / Organic Sessions | GA4 | Monthly |

### Tier 2: SEO Performance Metrics (What SEOs track)

| Metric | Definition | Source | Frequency |
|--------|-----------|--------|-----------|
| Organic Sessions | Sessions from organic search | GA4 | Monthly |
| Organic Traffic Growth | % change vs. prior period and YoY | GA4 | Monthly |
| Keyword Rankings | Position for target keyword set | Rank tracker | Weekly |
| Keywords in Top 3 / Top 10 | Count of keywords in positions 1-3 and 1-10 | Rank tracker | Weekly |
| Click-Through Rate (CTR) | Clicks / Impressions (SERP level) | GSC | Monthly |
| Average Position | Mean ranking across all tracked keywords | GSC | Monthly |
| Impressions | How often pages appear in SERPs | GSC | Monthly |
| New Pages Indexed | Pages added to Google index | GSC | Monthly |

### Tier 2b: AI Visibility Metrics (nuevo en 2025-2026)

Reportar solo si el cliente tiene objetivo de visibilidad en AI Overviews / ChatGPT / Perplexity:

| Metric | Definition | Source | Frequency |
|--------|-----------|--------|-----------|
| SAIV (Share of AI Visibility) | % de queries objetivo donde aparece el dominio en respuestas AI | Otterly.ai / Peec AI / SE Ranking | Monthly |
| AI Citations count | Nº de veces citado en AI Overviews ese mes | BrightEdge / Semrush / manual | Monthly |
| AI Overviews impressions | Impresiones donde el sitio aparece en AIO | GSC (sección "AI Overviews") | Monthly |
| AI Overviews clicks | Clics desde AI Overviews | GSC | Monthly |
| Brand mentions in AI | Menciones de marca en respuestas AI sin link | Otterly.ai / Brand24 | Monthly |

> **GSC y AI Overviews:** Desde mediados de 2024, GSC muestra una sección específica de "AI Overviews" en Performance. Activar el filtro "Search type: AI Overviews" para ver clics e impresiones desde este placement.

### Tier 3: Technical / Diagnostic Metrics (What SEOs diagnose)

| Metric | Definition | Source | Frequency |
|--------|-----------|--------|-----------|
| Core Web Vitals | LCP, INP, CLS for key pages | GSC / PageSpeed | Monthly |
| Crawl Errors | 404s, server errors found by Google | GSC | Weekly |
| Index Coverage | % of submitted URLs indexed | GSC | Monthly |
| Backlinks Acquired | New referring domains | Ahrefs/SEMrush | Monthly |
| Domain Rating / Authority | DR o DA — solo como contexto, no KPI primario | Ahrefs/Moz | Monthly |
| Page Speed Score | Lighthouse score key pages | PageSpeed | Monthly |
| Engagement Rate (GA4) | % sesiones con engagement ≥10s, ≥2 páginas, o conversión | GA4 | Monthly |

---

## 2. SEO ROI Calculation

### Method 1: Traffic Value (Most Common)

```
SEO Traffic Value = Organic Sessions × Avg. CPC for your keywords

Example:
- Monthly organic sessions: 45,000
- Average CPC for your keyword set: $2.80

Traffic Value = 45,000 × $2.80 = $126,000/month
Annual Traffic Value = $1,512,000

SEO Investment (agency + tools): $3,500/month = $42,000/year
ROI = ($1,512,000 - $42,000) / $42,000 × 100 = 3,500% ROI
```

**Get average CPC from:**
- Google Keyword Planner (free, requires Google Ads account)
- Ahrefs / SEMrush keyword data

### Method 2: Revenue Attribution (Most Accurate)

```
Organic Revenue (from GA4 e-commerce or conversion value)
÷ SEO Investment
× 100 = ROI %

Example:
- Monthly organic revenue: $85,000
- Monthly SEO cost: $4,500
- Monthly ROI: ($85,000 / $4,500) × 100 = 1,889%
```

**GA4 Setup for Revenue Attribution:**
```javascript
// E-commerce purchase event
gtag('event', 'purchase', {
  transaction_id: 'T12345',
  value: 250.00,
  currency: 'USD',
  items: [...]
});

// Lead conversion value
gtag('event', 'generate_lead', {
  value: 150.00,  // avg lead value
  currency: 'USD'
});
```

### Method 3: Conversion Value for Lead Gen Sites

```
SEO Leads × Avg. Lead Value × Close Rate = Attributed Revenue

Example:
- Monthly organic leads: 120
- Avg. lead value (sale price): $2,500
- Close rate: 12%

Attributed Revenue = 120 × $2,500 × 0.12 = $36,000/month
```

### Method 4: Cost Savings vs. PPC

```
Cost to buy equivalent traffic via PPC:
= (Organic Clicks for each keyword × CPC for that keyword)
Sum across all keywords

This shows: "Without SEO, you'd pay X/month to get this same traffic via Google Ads"
```

---

## 3. Standard Report Structures

### Monthly SEO Report Structure (Client-Facing)

**Page 1: Executive Summary**
- Overall performance vs. last month (traffic, rankings, conversions)
- Top 3 wins this month
- Top 3 priorities for next month
- Traffic value / ROI snapshot

**Page 2: Traffic & Conversions**
- Organic sessions: this month vs. last month vs. same month last year (YoY)
- Organic conversion rate trend
- Organic revenue or leads
- GA4 channel attribution (organic vs. paid vs. social)

**Page 3: Rankings Dashboard**
- Keywords in top 3 / 5 / 10 / 20 (counts and trends)
- Featured snippets owned
- Top movers (biggest rank improvements and drops this month)
- New keywords entered top 10

**Page 4: Technical Health**
- Core Web Vitals status (mobile + desktop)
- Index coverage: indexed / excluded / errors
- Crawl errors resolved this month
- New backlinks acquired

**Page 5: Work Completed This Month**
- Content published
- Technical fixes implemented
- Link building activity
- On-page optimizations

**Page 6: Next Month Plan**
- Priorities and expected impact
- Content calendar
- Technical work planned

**Page 7: AI Visibility (si aplica)**
> Incluir solo si el cliente tiene objetivo de visibilidad en AI / si el sector tiene alta cobertura AIO

- SAIV este mes vs. mes anterior (y baseline cuando se empezó a medir)
- Impresiones y clics desde AI Overviews (GSC → "Search type: AI Overviews")
- Top 3 queries donde aparece en AI Overviews
- Páginas más citadas por AI
- Acciones tomadas para mejorar citabilidad (schema, formato, longitud)
- Comparación: ¿el tráfico desde AIO canibiliza o complementa el orgánico clásico?

```
Bloque de cliente para AI:
"Este mes aparecemos en respuestas de IA para X búsquedas, 
con X impresiones en AI Overviews (Google). 
Esto representa una nueva capa de visibilidad que los 
rankings tradicionales no capturan."
```

---

## 4. Report Segments & Attribution

### Segmenting Organic Traffic Correctly in GA4

**Brand vs. Non-Brand Split:**
```
Brand traffic = organic sessions where query contains brand name
Non-brand = everything else

Why it matters: Brand traffic can grow independently of SEO work
(PR, word of mouth, ads). Non-brand growth = SEO contribution.
```

**Set up in GSC:**
```
Performance → Queries → Filter: "Query does not contain [BrandName]"
= Non-brand clicks and impressions
```

**Traffic by Device:**
```
GA4 → Reports → User → Tech → Device category
Compare: Mobile vs. Desktop organic performance
If mobile conversion rate << desktop: CRO opportunity
```

**Traffic by Landing Page:**
```
GA4 → Reports → Engagement → Landing page
Filter: Session default channel = Organic Search
= Shows which pages drive organic traffic
```

**Traffic by Location:**
```
GA4 → Reports → User → User attributes → Region
Filter: Session default channel = Organic Search
= Shows geographic distribution of organic visitors
```

### Attribution Models for SEO

| Model | How It Works | When to Use |
|-------|-------------|------------|
| Last-click | Credit to last touchpoint before conversion | Solo para referencia histórica; no recomendado |
| First-click | Credit to first channel that drove visit | Mostrar SEO como canal de descubrimiento |
| **Data-driven** ✅ | ML distribuye crédito entre todos los touchpoints según impacto real | **Default GA4 desde oct 2023 — usar siempre que sea posible** |
| Linear | Equal credit to all touchpoints | Cuando todos los canales contribuyen igualmente |
| Position-based | 40% first, 40% last, 20% middle | Balance discovery + conversión |

#### Data-Driven Attribution — Guía práctica (GA4)

**¿Por qué es el default correcto?**
- GA4 cambió el modelo default de "Last click" a "Data-driven" en octubre 2023
- Usa ML para asignar crédito basado en cómo cada touchpoint realmente impactó la conversión
- No requiere configuración manual — si tienes suficientes conversiones (mínimo ~50/mes), se activa solo

**Limitaciones a conocer:**
```
Requiere mínimo ~50 conversiones/mes para que el modelo sea estadísticamente válido
Si hay < 50 conversiones → GA4 cae back a Last-click → indicarlo en reporte
No disponible en GA4 Free para todas las propiedades → verificar configuración
```

**Cómo verificar que está activo:**
```
GA4 → Admin → Attribution settings → Reporting attribution model
→ Debe decir "Data-driven"
Si dice "Last click" → cambiarlo manualmente
```

**Impacto en reportes SEO:**
```
Data-driven generalmente:
- Aumenta el crédito atribuido a SEO (captura su rol en early touchpoints)
- Reduce el crédito de Direct / Last-click
- Muestra SEO como canal de pipeline, no solo de cierre

En reportes: mencionar "usamos data-driven attribution (GA4 default)" 
para credibilidad metodológica con clientes sofisticados.
```

**Para clientes con Consent Mode + pérdida de datos (EU):**
```
Consent Mode v2 + GA4 → hasta 40%+ de sesiones no consentidas en EU/LATAM
→ GA4 usa modelado estadístico para estimar conversiones no consentidas
→ Activar "Behavioral modeling" en GA4 Admin para recuperar datos estimados
→ Indicar en reporte: "X% de datos son modelados por Consent Mode"
```

**For SEO:** Data-driven attribution da la imagen más precisa del rol de SEO en el customer journey — especialmente en SaaS/B2B donde el ciclo de compra es largo y SEO actúa como canal de descubrimiento.

---

## 5. Dashboard Templates

### Executive Dashboard (1 Page, KPI Focus)
```
┌─────────────────────────────────────────────────┐
│  SEO Performance Dashboard — [Month] [Year]      │
├──────────────┬──────────────┬───────────────────┤
│ Organic      │ Conversions  │ Traffic Value      │
│ Sessions     │              │                    │
│ 45,231       │ 892          │ $126,646/mo        │
│ ▲ 12% MoM   │ ▲ 8% MoM    │ ▲ 12% MoM         │
├──────────────┴──────────────┴───────────────────┤
│ Rankings: [●●●●○] 68% keywords in top 10        │
│ Keywords top 3: 142  |  Top 10: 384             │
├─────────────────────────────────────────────────┤
│ Technical Health: ✅ CWV Pass | 3 issues pending │
│ Backlinks: +24 new domains this month            │
└─────────────────────────────────────────────────┘
```

### Weekly Internal Dashboard (SEO Team)
```
Week of [Date]

RANKINGS CHANGE:
- Improved: [Keyword] +4 → position 6
- Declined: [Keyword] -2 → position 14
- New top 10: [Keyword] now at position 8

INDEXATION:
- New pages indexed: 12
- Crawl errors: 3 (resolved: 2, pending: 1)

CONTENT:
- Published: [Article title]
- In progress: [Article title]

LINKS:
- New: [domain.com] DR 45 — [context]
```

---

## 6. Year-over-Year (YoY) Reporting

YoY comparison removes seasonality distortion — always include it alongside MoM.

### Why YoY Matters
```
November traffic: 45,000 sessions
October traffic: 62,000 sessions
MoM change: -27% ← looks bad

November last year: 38,000 sessions
YoY change: +18.4% ← actually strong growth
```

Seasonality affects almost every industry. Without YoY, clients panic at normal seasonal dips.

### Setting Up YoY Comparison in GA4
```
Reports → Date range → Compare → Previous year
(always compare same date range: e.g., Nov 1-30 this year vs. Nov 1-30 last year)
```

---

## 7. Communicating SEO to Non-Technical Stakeholders

### Translation Guide

| Technical Metric | Business Translation |
|----------------|---------------------|
| "We improved 3 keywords from position 15 to position 8" | "We moved from page 2 to page 1 on 3 searches, which increases the traffic we get from those topics by ~4-5x" |
| "We fixed 47 crawl errors" | "We removed 47 technical barriers that were preventing Google from properly reading our website" |
| "Our LCP improved from 3.8s to 2.1s" | "Our pages load 45% faster, which reduces the % of users who leave before seeing anything" |
| "We acquired 12 new backlinks" | "12 other websites now link to us, which increases our credibility in Google's eyes" |
| "Organic traffic grew 22% YoY" | "We're getting 22% more free visitors from search than we were a year ago — avoiding X dollars in paid advertising costs" |
| "We rank #1 for [keyword]" | "When people search for [keyword], we're the first result they see" |

### Executive Reporting Principles
1. **Lead with business outcomes, not SEO jargon**
2. **Always show trend, not just snapshot** (direction matters)
3. **Connect SEO to revenue** wherever possible
4. **Explain "why"** not just "what" changed
5. **Be honest about drops** — explain the cause and the plan
6. **Show work in progress** — what was done this month

---

## 8. Automated Reporting Workflows

### GA4 → Looker Studio Dashboard
```
1. Go to lookerstudio.google.com
2. Create new report → Add data source → Google Analytics
3. Key metrics to visualize:
   - Line chart: Organic sessions over 13 months (shows YoY)
   - Scorecard: Organic conversions (vs. previous period)
   - Table: Top landing pages by organic sessions
   - Pie chart: Device breakdown for organic traffic
4. Schedule: Email delivery to client on 1st of each month
```

### GSC → Looker Studio Dashboard
```
1. Add data source → Search Console
2. Key charts:
   - Clicks and impressions (line chart, 12 months)
   - CTR by landing page (table)
   - Top queries by impressions (bar chart)
   - Average position trend (line chart)
3. Filter: Web results only (exclude Discover, Image)
```

### Automated Rank Tracking Reports
Most rank trackers offer scheduled email delivery:
- **Ahrefs**: Position tracking → Email digests (weekly/monthly)
- **SEMrush**: Position tracking → Scheduled reports
- **Mangools SERPWatcher**: Automated weekly report email

### Custom Reporting Automation with Python
```python
# Pull GSC data via API
from googleapiclient.discovery import build
from oauth2client.service_account import ServiceAccountCredentials

def get_search_analytics(site_url, start_date, end_date):
    service = build('searchconsole', 'v1', credentials=credentials)
    request = {
        'startDate': start_date,
        'endDate': end_date,
        'dimensions': ['query', 'page', 'device'],
        'rowLimit': 1000
    }
    response = service.searchanalytics().query(siteUrl=site_url, body=request).execute()
    return response.get('rows', [])
```

---

## 9. Reporting for Agencies vs. In-House

### Agency Client Reports
**Focus:** Proving value, justifying retainer, building trust
- Clear before/after comparisons
- Work completed section (shows where the money went)
- Traffic value calculation (ROI framing)
- Competitive positioning (are we gaining vs. competitors?)
- Clear next steps (shows strategic thinking)

**Frequency:** Monthly report + brief weekly Slack/email update for active clients

### In-House SEO Reports
**Focus:** Prioritization, resource allocation, internal buy-in
- Report to: Marketing Director, CMO, CEO
- Language: Business metrics first
- Include: Resource requests (headcount, tool budget, dev time)
- Benchmarks: Industry and competitor comparison
- Trend: 12-month view (decisions need context)

**Frequency:** Monthly executive summary + ongoing live dashboard

---

## 10. Tools for SEO Reporting

### Dashboard & Visualization
| Tool | Best For | Pricing |
|------|----------|---------|
| Looker Studio (Google) | Free, GA4/GSC integration, shareable | Free |
| SEMrush Reports | Auto-generated from SEMrush data | Paid (with SEMrush) |
| Ahrefs Dashboard | Built-in reporting from Ahrefs data | Paid (with Ahrefs) |
| AgencyAnalytics | White-label client dashboards | Paid |
| Databox | Multi-source KPI dashboards | Free + Paid |
| Klipfolio | Custom dashboard builder | Paid |
| Whatagraph | Visual reports for agencies | Paid |

### Rank Tracking
| Tool | Best For | Pricing |
|------|----------|---------|
| Ahrefs Rank Tracker | Accurate, local + global | Paid |
| SEMrush Position Tracking | SERP feature tracking | Paid |
| Mangools SERPWatcher | Budget-friendly, visual | Paid |
| Advanced Web Ranking | Enterprise, market share | Paid |
| BrightLocal | Local rank tracking | Paid |

### Data Aggregation / Automation
| Tool | Purpose | Pricing |
|------|---------|---------|
| Looker Studio | Connect GA4 + GSC + sheets in one dashboard | Free |
| Supermetrics | Pull data from 100+ sources into Sheets/Looker | Paid |
| Porter Metrics | Marketing data → Looker Studio templates | Paid |
| Google Sheets + API | Custom automated pulls from GA4/GSC | Free (dev time) |
| Zapier | Automate report delivery + notifications | Free + Paid |

---

## 11. QBR — Quarterly Business Review Template

El QBR es el reporte más estratégico del año. No es un reporte mensual × 3 — es una revisión de si la estrategia SEO está funcionando y si hay que pivotar.

**Frecuencia:** Cada trimestre (Q1: ene-mar, Q2: abr-jun, Q3: jul-sep, Q4: oct-dic)
**Audiencia:** Cliente + stakeholders con poder de decisión (CEO, CMO, Director de Marketing)
**Duración reunión:** 45-60 minutos

### Estructura del QBR SEO

**Sección 1: Performance del Trimestre (10 min)**
- Organic Revenue / Leads: Q actual vs. Q anterior vs. mismo Q año pasado
- Organic sessions growth (YoY — elimina estacionalidad)
- Keywords en Top 3 / Top 10: evolución en 3 meses
- Traffic Value total del trimestre
- CWV status: mejoras implementadas y resultado

**Sección 2: Logros del Trimestre (5 min)**
- Top 3 victorias con impacto medible
- Ejemplo: "Publicamos X artículos → X nuevas keywords en top 10 → +X% tráfico orgánico"
- Evitar: listar tareas completadas sin conectar a resultado

**Sección 3: Revisión de Objetivos (10 min)**
- ¿Cumplimos los OKRs/KPIs del trimestre?
- Para cada objetivo: ✅ cumplido / ⚠️ parcial / ❌ no cumplido + razón
- Si no se cumplió: causa honesta (algoritmo, competencia, recursos, timeline)

**Sección 4: Análisis de Mercado (5 min)**
- ¿Cambios de algoritmo que afectaron el sitio?
- ¿Movimientos de competidores relevantes?
- ¿Tendencias de búsqueda emergentes en el sector?
- ¿Impacto de AI Overviews en el tráfico del sector?

**Sección 5: Estrategia Próximo Trimestre (15 min)**
- Top 3 prioridades estratégicas (no tácticas)
- OKRs propuestos para el próximo trimestre con métricas específicas
- Recursos necesarios (budget adicional, acceso dev, contenido)
- Riesgos identificados y plan de mitigación

**Sección 6: ROI del Período (5 min)**
- Investment total (retainer + herramientas + contenido)
- Traffic Value generado
- Revenue atribuido (si está disponible)
- ROI del trimestre + acumulado anual

### QBR Dashboard (formato tabla)

| Área | Q actual | Q anterior | Mismo Q año pasado | Objetivo Q | Estado |
|------|---------|-----------|-------------------|-----------|--------|
| Organic Sessions | X | X | X | X | ✅/⚠️/❌ |
| Organic Revenue / Leads | $X / X | $X / X | $X / X | $X / X | ✅/⚠️/❌ |
| Keywords Top 10 | X | X | X | X | ✅/⚠️/❌ |
| Traffic Value | $X | $X | $X | — | ✅/⚠️/❌ |
| CWV Pass Rate | X% | X% | X% | >75% | ✅/⚠️/❌ |
| Backlinks nuevos | X | X | X | X | ✅/⚠️/❌ |
| SAIV (AI Visibility) | X% | X% | baseline | — | track |

### OKR Framework para SEO (por trimestre)

```
Objective: Aumentar el pipeline orgánico en [sector/mercado objetivo]

KR1: Incrementar leads orgánicos de X a Y (Q actual)
KR2: Alcanzar X keywords en top 3 para queries comerciales clave
KR3: Reducir LCP mobile de Xs a Xs en páginas de mayor tráfico
KR4: Publicar X piezas de contenido orientadas a queries de conversión
```

### Señales de que el QBR necesita ajuste estratégico

```
Traffic sube pero conversiones no → Problema de relevancia de tráfico o UX
Rankings mejoran pero CTR baja → SERP con más AIO/features → explorar GEO
Tráfico estable pero revenue baja → Cambio en conversión o en valor por cliente
Pérdida de tráfico en queries informacionales → AIO cannibalization → pivotar a GEO
YoY tráfico baja pero tráfico branded sube → SEO funcionando como brand awareness
```

---

## Output Format

### Monthly SEO Report

**Client/Project:** [Name]
**Reporting Period:** [Month Year] vs. [Previous Month] and [Same Month Last Year]
**Prepared by:** [Name]
**Date:** [Date]

---

#### Executive Summary
[3-4 sentences: performance vs. goals, top win, key challenge, next priority]

#### KPI Dashboard

| KPI | This Month | vs. Last Month | vs. Last Year | Goal | Status |
|-----|-----------|---------------|--------------|------|--------|
| Organic Sessions | X | ▲/▼ X% | ▲/▼ X% | X | ✅/⚠️/❌ |
| Organic Conversions | X | ▲/▼ X% | ▲/▼ X% | X | ✅/⚠️/❌ |
| Organic Revenue | $X | ▲/▼ X% | ▲/▼ X% | $X | ✅/⚠️/❌ |
| Keywords Top 10 | X | ▲/▼ X | ▲/▼ X | X | ✅/⚠️/❌ |
| Traffic Value | $X | ▲/▼ X% | ▲/▼ X% | - | ✅/⚠️/❌ |

#### Top Ranking Movements
**Biggest Gains:**
| Keyword | Previous Position | Current Position | Change |
|---------|-----------------|-----------------|--------|
| [KW] | X | X | ▲ X |

**Biggest Drops:**
| Keyword | Previous Position | Current Position | Change | Reason |
|---------|-----------------|-----------------|--------|--------|
| [KW] | X | X | ▼ X | [Algorithm/competitor/technical] |

#### Work Completed This Month
- [ ] [Task 1] — [Expected impact]
- [ ] [Task 2] — [Expected impact]

#### Next Month Priorities
1. [Priority 1] — [Rationale]
2. [Priority 2] — [Rationale]
3. [Priority 3] — [Rationale]

#### SEO ROI Summary
- Traffic Value This Month: $X
- SEO Investment: $X
- ROI: X%
- Annualized Traffic Value: $X

---

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| GSC clicks, impressions, CTR, position data | `/seo google gsc <property>` | Foundation of ranking and traffic sections in any SEO report |
| GA4 organic traffic and conversions | `/seo google ga4 [property-id]` | Revenue attribution and ROI calculation |
| CWV field data for technical health section | `/seo google crux <url>` | Real Chrome user p75 metrics vs. Lighthouse estimates |
| Backlink profile and new/lost referring domains | `/seo dataforseo backlinks <domain>` | Monthly backlink acquisition section |
| Keyword rankings and traffic estimates | `/seo dataforseo ranked <domain>` | Ranking movements table and market share |
| KPI targets and phase benchmarks | `/seo plan [business-type]` | KPI framework and success criteria to measure against |
