---
name: seo-keywords
description: >
  Keyword research, clustering, search intent segmentation, and keyword-to-page
  mapping. Covers query discovery, competitive keyword analysis, topical authority
  mapping, and content gap identification. Use when user says "keyword research",
  "keyword strategy", "search intent", "keyword clusters", "keyword mapping",
  "find keywords", "content gaps", or "topical authority".
user-invokable: true
argument-hint: "[topic, domain, or niche]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Keyword Research & Strategy

Full keyword intelligence workflow: from discovery to clustering to page mapping to content calendar. Used to build topical authority and capture demand at every stage of the funnel.

---

## 1. Search Intent Framework

**Before researching keywords, understand intent.** Google matches pages to queries based on intent — getting this wrong is the #1 cause of ranking failure.

### The 4 Intent Types

| Intent | User Goal | Query Examples | Best Content Format |
|--------|-----------|---------------|---------------------|
| **Informational** | Learn something | "how does SEO work", "what is LCP", "best time to post on Instagram" | Blog post, guide, video, FAQ |
| **Navigational** | Find a specific site/page | "Ahrefs login", "Google Search Console", "SEMrush pricing page" | Brand page (don't try to rank for competitor nav) |
| **Commercial** | Research before buying | "best SEO tools", "Ahrefs vs SEMrush", "top local SEO agencies" | Comparison page, listicle, reviews |
| **Transactional** | Ready to buy/act | "buy SEO audit", "hire SEO consultant", "sign up Moz Pro" | Product/service page, landing page |

### Intent Signals in SERPs (How to Detect)
Look at what Google currently ranks for the keyword:
- **10 blue links with blog posts** → informational
- **Product pages / e-commerce** → transactional
- **Listicles / "best X" articles** → commercial
- **YouTube videos in top 3** → video intent (create video first)
- **Featured snippet** → informational, structure content to win it
- **People Also Ask box** → informational cluster, answer each PAA
- **Local pack** → local intent, needs local SEO treatment
- **Knowledge Panel** → navigational / entity query

**Rule:** Match your content format to what Google already rewards for that query.

---

## 2. Keyword Discovery Methods

### Seed Keyword Expansion
Start with 3-5 core seed keywords and expand in all directions:

```
Seed: "seo audit"

Expand:
├── Broader: "seo", "website audit", "digital marketing audit"
├── Narrower: "technical seo audit", "on-page seo audit", "local seo audit"
├── Questions: "how to do an seo audit", "what is an seo audit"
├── Comparisons: "seo audit vs seo analysis", "best seo audit tools"
├── Modifiers: "free seo audit", "seo audit checklist", "seo audit template"
└── Related: "seo health check", "website seo score", "seo site analysis"
```

### Discovery Sources

**Google itself (free, high-signal):**
- **Autocomplete**: Type keyword + space, note suggestions
- **People Also Ask (PAA)**: Expand all PAA boxes for a query
- **Related searches**: Bottom of SERP (10 more queries)
- **"Searches related to"**: Semantic neighbors Google confirms as related
- **Google Trends**: Rising queries, seasonal patterns, geographic interest

**Search Console (your own data):**
```
GSC → Performance → Queries
Filter: Position > 10 (not yet ranking well = opportunity)
Filter: Impressions > 100 (enough search volume to matter)
Sort by: Impressions DESC
```
These are keywords Google already associates with your site but where you're not ranking well yet — easiest wins.

**Competitor keyword gaps:**
- Take competitor URL → paste into keyword research tool
- Export their ranking keywords
- Filter for keywords where YOU don't rank in top 20
- These are gaps in your coverage

**Reddit & forums:**
- Search `site:reddit.com "[your topic]"` for real user language
- Find sub-reddits in your niche — sort by "top" posts for popular questions
- These reveal how real users phrase their problems (vs. how tools phrase keywords)

**YouTube autocomplete:**
- YouTube search bar → type keyword → see suggestions
- YouTube-specific vocabulary often differs from web search

---

## 3. Keyword Metrics to Evaluate

### Core Metrics

| Metric | What It Means | How to Use |
|--------|--------------|------------|
| **Search Volume** | Monthly searches (avg 12 months) | Demand signal — but not the only one |
| **Keyword Difficulty (KD)** | How hard to rank (0-100) | Calibrate to your domain authority |
| **CPC (Cost Per Click)** | What advertisers pay per click | High CPC = high commercial value |
| **CTR Curve** | % of clicks position 1, 2, 3 gets | Estimate traffic from ranking |
| **SERP Features** | Snippets, PAA, videos, maps present | Affects organic CTR |
| **Trend** | Rising, stable, or declining | Avoid declining keywords |

### Volume Context by Site Size

| Site Authority | Target Volume Range | Why |
|---------------|--------------------|----|
| New site (DA <20) | 0-500/month | Low competition, win small |
| Growing site (DA 20-40) | 500-5,000/month | Building traffic base |
| Established (DA 40-60) | 1,000-20,000/month | Competing for mid-tier |
| Authority site (DA 60+) | 5,000-100,000+/month | Can target high competition |

**Don't ignore low-volume keywords.** A 50/month keyword with high commercial intent often converts better than a 10,000/month informational keyword.

### Keyword Difficulty Calibration

| KD Score | Competition Level | When to Target |
|----------|-----------------|---------------|
| 0-20 | Very low | Always — quick wins for any site |
| 20-40 | Low | Good for sites DA 20+ |
| 40-60 | Medium | DA 40+ sites, with good content |
| 60-80 | High | DA 50+ with strong content + links |
| 80-100 | Very high | DA 70+, strong topical authority |

---

## 4. Keyword Clustering

**Cluster first, then create pages.** One page can rank for hundreds of related keywords.

### How to Build Clusters

**SERP-based clustering (most accurate):**
If two keywords return ≥ 3 of the same top-10 URLs → same cluster, same page.

```
"seo audit" → ranks same 5 pages as "website seo analysis" → SAME PAGE
"seo audit checklist" → ranks same 4 pages → ADD to same page
"how to do seo audit" → different top 10 → SEPARATE PAGE (informational vs. tool)
```

**Semantic clustering:**
Group by topic similarity and parent/child relationships:

```
Cluster: Email Marketing
├── Pillar (head): "email marketing" (50K/mo)
├── Sub-topics:
│   ├── "email marketing strategy" (8K/mo)
│   ├── "email marketing tools" (12K/mo)
│   ├── "email marketing best practices" (5K/mo)
│   ├── "email marketing metrics" (3K/mo)
│   └── "email marketing automation" (15K/mo)
└── Long-tail:
    ├── "how to write email subject lines" (2K/mo)
    ├── "email open rate benchmarks" (1.5K/mo)
    └── "cold email templates" (4K/mo)
```

### Cluster Types

| Type | Description | Example |
|------|-------------|---------|
| **Pillar** | Broad, high-volume parent topic | "email marketing" |
| **Supporting** | Sub-topic that links to pillar | "email marketing strategy" |
| **Long-tail** | Specific, low-competition, high-intent | "how to increase email open rate for SaaS" |
| **Comparison** | Commercial intent cluster | "Mailchimp vs Klaviyo" |
| **FAQ/Question** | Informational cluster from PAA | "what is a good email open rate" |

---

## 5. Keyword-to-Page Mapping

Every target keyword needs exactly ONE canonical page. No cannibalization.

### Mapping Framework

```
STEP 1: Assign each cluster a "primary keyword" (highest volume + clearest intent)
STEP 2: List supporting keywords (synonyms, related phrases, PAA questions)
STEP 3: Map to existing page OR flag as "content needed"
STEP 4: Check for cannibalization (two pages targeting same keyword)
```

### Mapping Template

| Primary Keyword | Volume | Intent | Target Page | Status | Supporting Keywords |
|----------------|--------|--------|------------|--------|---------------------|
| email marketing | 50,000 | Info/Nav | /email-marketing-guide | Exists — needs update | email marketing definition, what is email marketing |
| email marketing tools | 12,000 | Commercial | /best-email-marketing-tools | Create new | best email platforms, email marketing software |
| Mailchimp vs Klaviyo | 6,000 | Commercial | /mailchimp-vs-klaviyo | Create new | Mailchimp alternative, Klaviyo review |
| how to write subject lines | 2,000 | Informational | /email-subject-lines | Create new | email subject line examples, good subject lines |

---

### Cannibalization — Framework Completo

#### Definición
Canibalización ocurre cuando dos o más páginas del mismo sitio compiten por la **misma keyword primaria con la misma intención de búsqueda**, confundiendo a Google sobre cuál debe posicionar. Resultado: ninguna rankea bien, la autoridad se divide, el CTR cae.

---

#### ADVERTENCIA CRÍTICA: La URL no es diagnóstico de canibalización

**Error frecuente de herramientas y SEOs:** detectar canibalización basándose únicamente en similitud de URL (alias final). Esto genera falsos positivos constantemente.

**Ejemplo del error:**
```
dominio.com/digital-inspections-software/        ← página de servicio
dominio.com/blog/digital-inspections-software/   ← artículo de blog
```
Una herramienta o un análisis superficial señala esto como canibalización porque el slug final `/digital-inspections-software/` es el mismo. **Esto puede ser completamente incorrecto.**

**Por qué es un falso positivo potencial:**
- La página de servicio puede tener title: *"Digital Inspections Software | VLX"* → intención transaccional
- El blog puede tener title: *"How Digital Inspections Software Improves Fleet Safety"* → intención informacional
- H1 de la página de servicio: *"The Digital Inspections Software Built for Fleet Teams"*
- H1 del blog: *"5 Ways Digital Inspections Software Transforms Your Operations"*
- Son páginas distintas dirigidas a usuarios en momentos distintos del funnel

**La URL es solo el punto de partida para identificar candidatas. El diagnóstico real requiere 4 capas.**

---

#### Las 4 Capas del Diagnóstico de Canibalización

Analiza siempre en este orden. Si una capa descarta la canibalización, no necesitas continuar.

**Capa 1 — URL (identificación de candidatas, no diagnóstico)**
- Busca páginas con slugs similares o idénticos en distintos directorios
- Busca páginas en el mismo directorio con slugs semánticamente similares
- Resultado: lista de pares candidatos a revisar. **No es canibalización todavía.**

**Capa 2 — Keyword primaria asignada**
- ¿Cuál es la keyword primaria que tú has asignado a cada página?
- Si las keywords primarias son distintas → revisa intención antes de concluir nada
- Si la misma keyword primaria está asignada a dos páginas → sigue a Capa 3
- Sin keyword asignada en ninguna → usa GSC para ver por qué queries aparece cada una

**Capa 3 — Title y H1**
- Lee el title tag completo de ambas páginas (no solo la keyword)
- Lee el H1 completo de ambas páginas
- Pregunta: ¿El contexto del title/H1 responde a la misma intención de búsqueda?
  - *"Digital Inspections Software | Brand"* → transaccional (quiero comprar/contratar)
  - *"How Digital Inspections Software Works"* → informacional (quiero aprender)
  - Distintas intenciones → **no es canibalización real**. Google puede mostrar ambas.

**Capa 4 — Contexto del contenido**
- ¿Qué responde cada página? ¿A qué pregunta del usuario responde?
- ¿El contenido es para el mismo momento del funnel (TOFU/MOFU/BOFU)?
- ¿El CTA de ambas páginas es el mismo? (sign up, comprar, leer más)
- Si el contenido responde preguntas distintas para usuarios distintos → **no es canibalización**

**Confirmación final — GSC:**
- Filtra GSC por la query en cuestión → ¿aparecen 2+ URLs con impresiones para esa misma query?
- Si sí → ¿cuál recibe más clics? ¿Es la que debería ganar según intención?
- Esto confirma si Google también las está confundiendo o si las está usando correctamente para intenciones distintas

---

#### Veredicto: ¿Es canibalización real o no?

| Escenario | Veredicto | Acción |
|-----------|-----------|--------|
| Mismo slug en distintos directorios + titles/H1 con diferente intención | **No es canibalización** | Opcional: asegurarse de que los titles sean suficientemente distintos |
| Mismo slug en distintos directorios + titles/H1 con **misma intención** | **Sí es canibalización** | Consolidar o diferenciar |
| Keywords distintas asignadas + contexto distinto + ambas rankeando | **No es canibalización** | Puede ser co-ranking deseable |
| Keywords distintas pero Google las posiciona para el mismo query con 0 diferencia de intención | **Canibalización semántica** | Diferenciar contenido más claramente |
| Dos landing pages de servicio con mismo H1 y mismo CTA | **Canibalización explícita** | Consolidar con 301 |
| Pillar page + cluster page compitiendo por la misma head term | **Canibalización de jerarquía** | Redefinir asignación de keywords entre ambas |

---

#### REGLA CRÍTICA: Homepage vs. Páginas de Producto/Servicio

**La homepage NUNCA debe ser el target primario de una keyword de producto o servicio.**

| Página | Keywords que le corresponden | Nunca debe targetear |
|--------|------------------------------|----------------------|
| Homepage | Brand (nombre empresa), umbrella terms ("inspection software"), navigational | Keywords de producto específico como primary KW |
| Página de producto/servicio | Keyword de producto ("digital inspections software") | Competir con homepage por brand terms |
| Blog post | Keywords informacionales ("how to digitize inspections") | Keywords transaccionales de producto |

**Ejemplo de error:**
- ❌ Homepage primary KW: "digital inspections software" → también asignada a /digital-inspections-software/
- ✅ Homepage primary KW: "VLX" + "inspection software platform" (umbrella/brand)
- ✅ /digital-inspections-software/ primary KW: "digital inspections software"

---

#### Tipos de Canibalización (solo aplican si las 4 capas la confirman)

| Tipo | Descripción | Ejemplo | Gravedad |
|------|-------------|---------|----------|
| **Explícita** | Dos páginas con mismo H1/Title/keyword primaria e intención idéntica | Home y /producto/ con mismo H1 transaccional | Crítica |
| **Implícita** | Google elige rankear la página "equivocada" — la que tú no quieres | Google prefiere el blog sobre la landing de producto para query transaccional | Alta |
| **Semántica** | Keywords distintas pero Google las trata como equivalentes | "digital inspection software" y "digital inspections software" en páginas con mismo contexto | Media |
| **De jerarquía** | Pillar y cluster compiten por la misma head term | /recetas-de-coco/ y /recetas/de-coco/ con mismo H1 | Media |
| **Co-ranking aceptable** | Dos URLs para el mismo query pero intenciones distintas — Google las muestra a usuarios distintos | Landing transaccional + guía informacional del mismo tema en el mismo SERP | **No es problema** |

---

#### Detección desde GSC

```
1. Exportar GSC Performance → páginas → filtrar por query objetivo
2. Si 2+ URLs reciben impresiones para la misma query → CANDIDATAS (no confirmado todavía)
3. Aplicar las 4 capas: keyword asignada → title → H1 → contexto
4. Si las 4 capas confirman misma intención → CANIBALIZACIÓN REAL
5. Ver cuál tiene más clics → esa es la "página ganadora" que Google prefiere actualmente
6. Verificar si la ganadora es la que debería ganar según intención de negocio
```

**Señal en GSC de canibalización activa (real):**
- Posición promedio oscila bruscamente (7 → 14 → 9 → 12) para la misma query
- CTR bajo a pesar de impresiones altas
- Dos URLs alternando en SERP para el mismo query con la misma intención de búsqueda

**Método con `site:` operator:**
```
site:dominio.com "keyword objetivo"
→ Si aparecen 2+ páginas relevantes → aplicar las 4 capas antes de diagnosticar
→ No concluir canibalización solo porque aparecen dos páginas
```

---

#### Árbol de Decisión — Resolución

```
¿Dos páginas candidatas comparten URL similar o misma keyword en GSC?
│
├── Capa 3: ¿El title y H1 de ambas tienen la misma intención de búsqueda?
│   ├── NO → No es canibalización real. Verificar que los titles sean suficientemente
│   │         distintos para que Google los diferencie. Fin.
│   └── SÍ → Continúa.
│
├── Capa 4: ¿El contenido de ambas responde la misma pregunta para el mismo
│   tipo de usuario en el mismo momento del funnel?
│   ├── NO → Co-ranking aceptable o diferenciación suficiente. Monitorizar en GSC.
│   └── SÍ → Es canibalización. Continúa.
│
├── ¿Una página claramente gana más clics e impresiones en GSC?
│   ├── SÍ → Esa es la página "ganadora". Fortalécela. Reconvierte la otra.
│   └── NO → Evalúa cuál tiene más autoridad (backlinks, antigüedad, contenido).
│
├── ¿Las páginas tienen contenido muy similar (>70% overlap)?
│   ├── SÍ → CONSOLIDA: redirige la más débil (301) a la más fuerte.
│   └── NO → DIFERENCIA: asigna keywords distintas, reescribe H1/Title/meta.
│
└── ¿Es una página sin tráfico ni backlinks?
    ├── SÍ → ELIMINA o REDIRIGE con 301 a la página principal del tema.
    └── NO → DIFERENCIA o CONSOLIDA según contenido.
```

---

#### Acciones de Resolución

| Situación | Acción | Cómo |
|-----------|--------|------|
| Mismo slug, distintas intenciones confirmadas (title/H1 distinto) | Ninguna — no hay problema | Asegurarse de que titles sean suficientemente distintos |
| Contenido muy similar, misma intención, una página más fuerte | Consolidar | 301 de débil → fuerte. Fusionar contenido único. |
| Contenido diferente, misma intención detectada | Diferenciar intención | Reescribir H1/Title de la menos importante hacia una intención distinta |
| Homepage compite con página de producto | Corregir asignación | Homepage: brand/umbrella KW. Producto: KW de producto como primary. |
| Blog post rankea en lugar de landing para query transaccional | Señalizar a Google | Internal link fuerte de blog → landing con anchor exact match. Si el blog es duplicado: 301 o canonical blog → landing. |
| Dos landings de servicio con mismo H1 e intención | Fusionar o especializar | Si mercados distintos: diferencia por industria/use case. Si son iguales: 301 + consolida. |

---

#### Qué NO es canibalización

- ✅ Dos URLs con el mismo slug final en distintos directorios (`/producto/` y `/blog/producto/`) si tienen title/H1/contexto distintos
- ✅ Homepage rankeando para brand terms Y página de producto rankeando para su keyword de producto → normal y deseable
- ✅ Un blog post informacional y una landing transaccional apareciendo en el mismo SERP → intents distintos, Google los usa para usuarios distintos
- ✅ Múltiples páginas de industria rankeando para variantes específicas de la keyword principal → cobertura topical, no canibalización
- ✅ Página de categoría + página de producto rankeando juntas en SERP → Google muestra profundidad del sitio, no confusión

---

## 6. Topical Authority Mapping

Google rewards sites that cover a topic comprehensively. Topical authority = ranking without needing many backlinks. In the AI search era, topical authority is also the primary signal for AI systems when deciding which sources to cite.

### Topical Map Creation Methodology

**Hub-and-Spoke model:**
```
PILLAR (hub): "Email Marketing" — broad, comprehensive (3,000-5,000 words)
  ├── SPOKE: "Email marketing for SaaS" (use case)
  ├── SPOKE: "Welcome email best practices" (subtopic)
  ├── SPOKE: "Email open rate benchmarks 2026" (data)
  ├── SPOKE: "Email marketing vs SMS marketing" (comparison)
  └── SPOKE: "Best email marketing tools" (commercial)
```

**Alternative: Pillar-Cluster model** (for e-commerce and broader sites)
- Pillar page = category page (commercial intent)
- Clusters = product/comparison/review pages around that category

### Topic Coverage Gap Analysis

**Method:**
1. Pick your core topic (e.g., "email marketing")
2. List all sub-topics your site covers
3. Search Google for 20-30 related queries in that topic — note which you DON'T rank for
4. Use Ahrefs Content Gap or SEMrush Keyword Gap vs. top 3 competitors
5. Identify which sub-topics have NO page on your site
6. Prioritize by volume + relevance + entity connection

**Topical authority checklist:**
- [ ] Pillar page covers the core topic comprehensively (covers all subtopics at a high level)
- [ ] Supporting pages cover every major sub-topic with depth
- [ ] Internal links connect all related pages (hub-and-spoke)
- [ ] No important sub-topic is missing (gaps = authority leak)
- [ ] Pages link back up to the pillar
- [ ] Entity coverage: the topic's main entities are named and defined across your content cluster

### Topical Authority Measurement
| Metric | Tool | Benchmark |
|--------|------|-----------|
| Featured snippets in topic cluster | Ahrefs / GSC | Growing quarter-over-quarter |
| Indexing velocity | GSC Coverage report | New content indexed within 24-48 hours (signals high trust) |
| Topic coverage breadth | Ahrefs / SEMrush | >80% of subtopics have a page |
| AI citation rate | Semrush AI Toolkit, manual sampling | Track monthly |

**Timeline for results:**
- 6-12 months: Noticeable ranking improvements for cluster keywords
- 18-24 months: Full topical authority established for competitive topics

### Competitive Topical Coverage
Compare your topic coverage vs. the top-ranking competitor:
- Take their sitemap or crawl their blog
- Identify topics they cover that you don't
- These are your "mandatory" gaps to fill

### Entity-Based Keyword Strategy (AI Era)
In 2026, topical authority is measured at the **entity level**, not just the keyword level:
- Identify the key entities in your topic (people, brands, products, concepts)
- Ensure each entity is mentioned, defined, and connected to other entities in your content
- Entity gaps (entities your competitors connect that you don't) are often more impactful than keyword gaps
- Use Google's Natural Language API to identify which entities your top-ranking pages associate with your topic

---

## 7. Featured Snippet Optimization

Featured snippets (Position 0) get ~8% CTR. More importantly in 2026: **pages selected as featured snippets are also the most likely to be cited in AI Overviews** — snippet optimization IS AI citation optimization.

> **Context:** AI Overviews appear on **58% of queries** (2026), displacing traditional snippets on many queries. However, pages that already rank for featured snippets are the primary candidates for AI Overview citation. Optimizing for snippets = dual benefit.

### Snippet Types & How to Win

| Snippet Type | How to Trigger | Optimal format | AI Overview rate |
|-------------|---------------|---------------|-----------------|
| **Paragraph** | Directly answer "what is X" in 40-60 words | Short direct paragraph after H2 | High (~80%) |
| **List (numbered)** | "How to X", "steps to X" | `<ol>` with 5-8 steps, imperative tense | Low (~30%) — AI Overviews less common for how-to |
| **List (bulleted)** | "Types of X", "examples of X" | `<ul>` with 5-8 parallel items | Medium |
| **Table** | Comparison queries, data | 3 columns × 5-6 rows max | Medium |
| **Video** | "How to" visual tasks | YouTube video with chapters | Low |

**Snippet-optimized format:**
```markdown
## What is Keyword Clustering?

Keyword clustering is the process of grouping related search queries
that share the same search intent into a single topic, allowing one
page to rank for multiple related keywords simultaneously.

### Types of Keyword Clusters:
- Semantic clusters: grouped by topic similarity
- SERP-based clusters: grouped by shared ranking URLs
- Intent clusters: grouped by user goal (informational, commercial)
```

---

## 8. Long-Tail Keyword Strategy

Long-tail keywords (3+ words, low volume) drive 70% of all searches and convert better.

### Why Long-Tail Matters

| Metric | Head Keyword | Long-Tail Keyword |
|--------|-------------|------------------|
| Example | "email marketing" | "email marketing for ecommerce stores 2025" |
| Volume | 50,000/mo | 200/mo |
| Competition | Very high | Very low |
| Conversion rate | ~1% | ~5-10% |
| Ranking difficulty | Hard | Easy |

### Long-Tail Discovery Tactics
1. **PAA expansion**: Each PAA answer spawns more PAA questions — click every one
2. **Answer The Public**: Visualizes all question variants for a topic
3. **AlsoAsked**: Maps PAA question trees
4. **GSC low-impression queries**: Volume < 100 but perfectly relevant
5. **Reddit/Quora mining**: Real user phrasing = natural long-tail keywords
6. **"Site:" your competitors' blog**: Find specific articles they rank for

---

## 9. Seasonal & Trend Analysis

### Google Trends Usage
```
1. Enter keyword → check interest over time (past 5 years)
2. Check geographic distribution → which regions have highest interest?
3. Compare keywords → "email marketing" vs "SMS marketing" — which is growing?
4. Check related queries → "rising" tab shows emerging keywords
5. Export CSV for seasonal planning
```

### Seasonal Content Calendar
Plan content publication 2-3 months before seasonal peak:
- Holiday content: publish October for December peak
- Tax content: publish January for April peak
- Summer content: publish March for June peak

Google typically takes 2-6 weeks to index and rank new content.

> **Google Discover timing is different:** Discover traffic peaks **within 48-72 hours** of publication then declines rapidly. Seasonal content targeting Discover needs to be published much closer to the peak — not 2 months before. Strategy: publish the article for organic SEO months early, then do a "freshness update" (update the article with new data) right before the seasonal peak to trigger Discover recirculation.

---

## 10. AI-Era Keyword Dimensions

The AI search landscape fundamentally changes keyword strategy in 2026. Not all keywords behave the same way anymore.

### AI Overviews CTR Impact
AI Overviews appear on 58% of queries. Their impact on organic CTR varies by query type:

| Query type | AI Overview rate | Remaining organic CTR | Strategy |
|------------|-----------------|----------------------|---------|
| Definitional "what is X" | Very high (~80%) | Low — most clicks absorbed | Optimize for AI citation (be the source) |
| Procedural "how to X" | Lower (~30%) | Higher | Rank #1-3 for clicks + optimize for citation |
| Comparison "X vs Y" | Medium (~50%) | Medium | Featured snippet + comparison table for citation |
| Transactional "buy X", "price X" | Low (~15%) | High — users want to buy | Focus on click optimization |
| Navigational "brand name" | Very low | High | Focus on brand protection |
| Local "near me" | Low | High | Focus on Local Pack + GBP |

### AI Citation Keywords vs. Click Keywords
**A recalibrated approach to keyword selection:**

**Citation keywords** (optimize for AI citation, not clicks):
- Definitional, educational, high-AI-Overview-rate
- Still valuable for brand visibility even at near-zero CTR
- Target these to be cited in AI Overviews = brand awareness at scale
- KD doesn't matter much for these — what matters is E-E-A-T quality

**Click keywords** (optimize for organic clicks):
- Transactional, commercial, comparison-with-specific-action
- Lower AI Overview presence = higher organic CTR survival
- Traditional KD/volume analysis still applies

### AI Keyword Difficulty Recalibration
Traditional KD scores don't account for AI Overviews stealing clicks. A practical adjustment:

```
Effective Click Opportunity = Search Volume × (1 - AI Overview Saturation Rate) × Traditional CTR Curve

Example:
- KW: "what is bounce rate" — Vol: 8,000 — KD: 35 — AI Overview: Yes (80%)
- Effective clicks: 8,000 × 0.20 (remaining CTR) × 0.10 (position 3) = ~160 clicks
- Previously would have been 8,000 × 0.10 = ~800 clicks

- KW: "bounce rate reduction tools" — Vol: 800 — KD: 28 — AI Overview: No
- Effective clicks: 800 × 0.10 = ~80 clicks (nearly same as "easy" keyword above)
```

**Practical rule:** A KD 40 keyword without AI Overview is often more valuable than a KD 20 keyword with AI Overview.

### Search Everywhere Optimization (SEvO)
Keywords don't only live on Google. In 2026:

| Platform | Keyword research source | Notes |
|----------|------------------------|-------|
| YouTube | YouTube search bar autocomplete + TubeBuddy/vidIQ | Different vocabulary than Google — more conversational |
| Reddit | Reddit search + subreddit hot posts | 97% of Google results include Reddit — mine for phrasing |
| TikTok | TikTok search autocomplete | Gen Z-heavy, increasingly indexed by Google |
| Amazon | Amazon autocomplete + Helium10 | E-commerce: keyword cannibalization risk |
| App Store | AppFollow, Sensor Tower | Mobile apps: keyword crossover with Google |

**Reddit keyword mining workflow:**
1. Search `site:reddit.com "[your topic]"` in Google
2. Find threads with high engagement (100+ upvotes)
3. Extract the vocabulary users use — often different from formal keyword tools
4. High-upvote Reddit threads = validated content angles

---

## 11. Tools by Use Case

### Keyword Research Platforms
| Tool | Best For | Pricing |
|------|----------|---------|
| Ahrefs Keywords Explorer | Most accurate volume, KD, and SERP analysis | Paid |
| SEMrush Keyword Magic Tool | Largest database, intent classification | Paid |
| Moz Keyword Explorer | SERP analysis, priority scoring | Paid |
| Mangools KWFinder | Beginner-friendly, accurate local volume | Paid |
| Ubersuggest | Budget option, basic research | Free + Paid |
| Google Keyword Planner | Official Google data (broad ranges) | Free (Google Ads account) |
| Keyword Surfer (Chrome ext.) | Volume overlay on Google results | Free |

### Question & Long-Tail Discovery
| Tool | Purpose | Pricing |
|------|---------|---------|
| Answer The Public | Visual question map — **acquired by Neil Patel/Ubersuggest, free tier very limited (3 searches/day)**. Use AlsoAsked or Google PAA expansion instead for most research | Free (very limited) + Paid |
| AlsoAsked | PAA question trees | Free (limited) + Paid |
| SparkToro | Audience research + what they search | Paid |
| Exploding Topics | Emerging keyword trends early detection | Free + Paid |

### Clustering Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| Keyword Insights | AI-powered clustering + intent | Paid |
| Cluster AI | SERP-based clustering | Paid |
| Screaming Frog (manual) | Export + cluster in spreadsheet | Free + Paid |
| KeyClusters | Bulk SERP clustering | Paid |

### Topical Authority & Gap Analysis
| Tool | Purpose | Pricing |
|------|---------|---------|
| Ahrefs Content Gap | Competitor keyword gaps | Paid |
| SEMrush Keyword Gap | Multi-competitor gap analysis | Paid |
| MarketMuse | Topical coverage scoring | Paid |
| Surfer SEO | Content brief + topical coverage | Paid |
| Frase | Topic research + brief generation | Paid |

---

## Output Format

### Keyword Research Report

**Topic/Niche:** [Topic]
**Date:** [Date]
**Domain:** [URL if analyzing a specific site]

#### Executive Summary
[3-4 sentences: total keyword opportunity, top cluster, quickest wins, topical gaps]

#### Keyword Universe Summary
| Category | # Keywords | Est. Traffic Potential | Avg KD |
|---------|-----------|----------------------|--------|
| Head terms (>10K/mo) | X | X | X |
| Mid-tail (1K-10K/mo) | X | X | X |
| Long-tail (<1K/mo) | X | X | X |
| Quick wins (KD <30, intent match) | X | X | X |

#### Top Clusters (Priority Order)
| Cluster | Primary KW | Volume | KD | Intent | Page Status | Priority |
|---------|-----------|--------|-----|--------|------------|---------|
| [Cluster 1] | [KW] | X | X | [Type] | Exists/Create | 🔴 High |
| [Cluster 2] | [KW] | X | X | [Type] | Exists/Create | 🟡 Med |

#### Quick Wins (Rank in 30-60 days)
Keywords where you're in positions 4-20 with high impression count in GSC — optimize existing pages for these first.

| Keyword | Current Position | Impressions/mo | Target Page | Recommended Action |
|---------|-----------------|---------------|------------|-------------------|
| [KW] | [pos] | [X] | [URL] | [Add to H2, expand content, etc.] |

#### Cannibalization Issues
| Keyword | Competing Pages | Recommendation |
|---------|----------------|----------------|
| [KW] | [URL1] vs [URL2] | Consolidate into [URL] |

#### Content Calendar (Next 90 days)
| Month | Topic | Cluster | Volume | Intent | Format |
|-------|-------|---------|--------|--------|--------|
| Month 1 | [Topic] | [Cluster] | X | [Type] | Blog/Video/Landing page |

---

## 9. B2B vs B2C Keyword Strategy

| Dimensión | B2B | B2C |
|-----------|-----|-----|
| **Volumen típico** | Bajo (10-500/mo) — normal y aceptable | Alto (1K-100K+/mo) |
| **Longitud** | Long-tail, muy específico, jargon técnico | Mix: head + long-tail, lenguaje cotidiano |
| **Intención dominante** | Informacional + Comparativa (research largo) | Transaccional + Comercial (decisión rápida) |
| **Keywords de conversión** | "mejor software X para empresas", "X vs Y enterprise", "X precio", "X integración con Y" | "comprar X", "X oferta", "X barato", "X opiniones" |
| **Keywords TOFU** | "qué es X", "cómo hacer X en empresas", "guía X para [industria]" | "qué es X" (menor volumen), branded, tendencias |
| **Fuente de seeds** | LinkedIn, G2, Capterra, trade publications, conferencias del sector | Amazon, Reddit, TikTok, Google Shopping, influencers |
| **Prioridad de cluster** | TOFU educativo → MOFU comparativa → BOFU demo/pricing | Directo a comercial/transaccional si el producto lo permite |
| **KD aceptable** | KD bajo con volumen muy bajo sigue siendo valioso (cada lead puede valer miles) | KD debe correlacionar con volumen — bajo KD + bajo volumen = poco ROI |
| **Estacionalidad** | Poca o nula — ciclo de compra constante | Alta en muchos sectores (retail, travel, etc.) |

### B2B: Keywords que no aparecen en herramientas pero convierten

En B2B, muchas keywords de alta conversión tienen volumen tan bajo que las herramientas las muestran como "0" o no las detectan. Estrategia:
- **GSC mining**: queries reales con 0-10 impresiones que están convirtiendo
- **Sales team**: qué preguntan los prospects antes de comprar — esas son keywords BOFU
- **Competitor reviews en G2/Capterra**: el lenguaje exacto que usan buyers reales
- **LinkedIn posts virales del sector**: detectar terminología emergente antes que las herramientas

### Métricas de éxito distintas

| Métrica | B2B | B2C |
|---------|-----|-----|
| Volumen mínimo aceptable | 10/mo puede ser suficiente si convierte | < 100/mo raramente justifica esfuerzo |
| Conversión esperada | MQL / demo request | Compra / signup |
| Valor por conversión | Alto (€€€) | Bajo-medio (€) |
| Justificación de ROI | 1 lead desde una keyword de 20 búsquedas/mes puede valer miles | Necesitas volumen para que tenga sentido |

---

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Keyword gap vs. competitors | `/seo competitive [domain]` | Maps which competitors rank for keywords you don't cover |
| Live search volume and difficulty | `/seo dataforseo volume <keywords>` | Real-time metrics vs. tool estimates |
| Search intent classification | `/seo dataforseo intent <keywords>` | Automated informational/commercial/transactional detection |
| Google Ads volume data | `/seo google keywords <seed>` | Gold-standard volume from Google's own Keyword Planner |
| GSC quick-win keywords (positions 4-20) | `/seo google gsc <property>` | Finds keywords already ranking but underperforming — easiest wins |
| Turn clusters into optimized content | `/seo content <url>` | E-E-A-T signals, readability, and content depth for target keywords |
| Full SEO plan from keyword strategy | `/seo plan [business-type]` | Integrates clusters into phased roadmap with content calendar |
