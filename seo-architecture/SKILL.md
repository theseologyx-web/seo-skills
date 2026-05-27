---
name: seo-architecture
description: >
  Expert information architecture for SEO. Two modes: CREATE — design site
  architecture from scratch given keyword research and brand brief (silos,
  URL taxonomy, content hierarchy, internal linking blueprint); AUDIT — compare
  old vs new architecture for migrations or redesigns, detect unauthorized
  changes, validate silo integrity, and flag SEO risks with a traffic-weighted
  semaphore report. Use when user says "arquitectura de información", "site
  architecture", "content silos", "URL hierarchy", "silo structure", "pillar
  and cluster", "information architecture audit", "architecture review",
  "rediseño arquitectura", "cambios de estructura sin aprobación", or any
  request to design or validate how a website organizes its content and URLs.
user-invokable: true
argument-hint: "create [keyword-file or topic] | audit [old-sitemap] [new-sitemap]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  version: "2.0.0"
  category: seo
---

# SEO Information Architecture

Information architecture is the foundation of SEO. Before content quality, before link building, before technical optimizations — the structure of the site determines which pages can rank, how authority flows, and whether search engines can understand what the site is about.

This skill has two expert modes:

- **CREATE** — design architecture from scratch given keyword research + brand clarity
- **AUDIT** — analyze existing architecture against a proposed or deployed one (migrations, redesigns, unauthorized changes)

---

## MODE SELECTION

**Use CREATE when:**
- Building a new site
- Launching a new content section
- Restructuring an existing site from strategy (not from an existing structure)

**Use AUDIT when:**
- A migration or redesign is planned or has already happened
- You need to validate a proposed URL structure before implementation
- You suspect unauthorized changes were made to the architecture
- A developer or designer changed the structure without SEO approval

If the user doesn't specify, ask: *"¿Estás diseñando una arquitectura nueva o auditando una existente?"*

---

## MODE 1: CREATE — Architecture Design from Scratch

### Required Inputs

Before starting, collect:

1. **Keyword research** — clusters with search volume and intent (file, spreadsheet, or list)
2. **Brand brief** — what the brand sells/offers, target audience, unique value, competitors
3. **Site type** — e-commerce, SaaS, publisher, local service, agency, informational, marketplace
4. **Business priorities** — which products/services/topics must appear in the primary navigation

If any of these is missing, ask for it. Architecture designed without keyword data is guesswork.

---

### Step 1: Brand & Intent Mapping

Before grouping URLs, map what the brand covers and what users need:

**Brand territories** — core topic areas the brand has authority or ambition to own:
- Identify from: brand brief + top-traffic pages (if site exists) + competitive analysis
- A brand territory = a topic cluster large enough to justify a silo

**Intent layers** — classify every keyword cluster by search intent:

| Intent | Description | Typical Page Type |
|--------|-------------|-------------------|
| Navigational | User looking for a specific brand/page | Homepage, brand pages |
| Informational | User seeking knowledge | Blog, guides, how-to, glossary |
| Commercial investigation | User comparing options | Comparison pages, reviews, best-of |
| Transactional | User ready to buy/act | Product, service, pricing, landing pages |

**Rule:** Each silo should serve primarily one intent level. Mixing transactional and informational content in the same silo dilutes topical signals.

---

### Step 2: Silo Design

A silo is a group of pages about the same topic, interlinked tightly with each other and pointing up to a pillar page. Silos tell search engines: *"all of this content belongs to this topic cluster."*

#### Silo hierarchy:
```
Root / Homepage
├── Silo A (Pillar Page) — broad topic, high volume
│   ├── Cluster Page A1 — subtopic, medium volume
│   ├── Cluster Page A2 — subtopic, medium volume
│   │   └── Supporting Page A2a — narrow, long-tail
│   └── Cluster Page A3
├── Silo B (Pillar Page)
│   ├── ...
```

#### Silo design rules:

1. **One pillar per silo** — the pillar covers the broad topic comprehensively; cluster pages go deep on subtopics
2. **Topical proximity** — pages within a silo must be semantically related; when in doubt, keep them separate
3. **Intent consistency** — avoid mixing commercial pages with pure informational pages in the same silo unless the pillar naturally bridges both
4. **Silo depth** — ideal: 3 levels maximum (pillar → cluster → supporting). Deeper hierarchies hurt crawlability.
5. **Silo size** — ideal range: 8–15 cluster pages per pillar (minimum 5, maximum 20–25). Sites with only 3 cluster pages per pillar rarely achieve meaningful topical authority. Exception: silos just launched can start at 3 and grow.
6. **Flexible silos** — 2025 consensus has moved away from rigid silo isolation. Contextual cross-silo linking is recommended when topics genuinely overlap. The old "never link between pillar pages of unrelated silos" rule stands, but cross-silo linking at cluster level is encouraged — not just tolerated. Rigid isolation with zero cross-silo links can actually reduce topical depth signals.

**When flat architecture is appropriate (no silos):**
- Sites with fewer than 50 pages and low topic diversity
- Single-service local businesses where blog content is secondary
- Landing page–only sites without content strategy
- Hybrid: flat for transactional/service pages, silos for editorial/blog content

#### From keyword clusters to silos:

```
Keyword cluster → Silo?
- Volume > [threshold defined by site size]? → candidate for pillar
- Multiple supporting keywords? → confirmed silo
- Only 1-2 supporting keywords? → cluster page within an adjacent silo
- Single keyword, no supporting? → supporting page
```

Deliver: **Silo Map** — visual tree showing silo name, pillar URL slug, cluster pages, and supporting pages.

---

### Step 2b: Content Classification Framework

Antes de asignar URLs, clasifica cada pieza de contenido. Contenido mal clasificado = silo contaminado = señales confusas para Google.

#### Clasificación por función en el sitio:

| Tipo | Función SEO | Posición en jerarquía | Ejemplos |
|------|-------------|----------------------|---------|
| **Pillar / Hub** | Captura tráfico de head term, distribuye autoridad al cluster | Nivel 1 (silo root) | /recetas/de-coco/, /servicios/seo/ |
| **Cluster** | Captura tráfico de mid-tail, apunta al pillar | Nivel 2 | /recetas/de-coco/flan/, /servicios/seo/local/ |
| **Supporting** | Long-tail, profundidad temática, apoya al cluster | Nivel 3 | /recetas/de-coco/flan/sin-horno/ |
| **Transaccional** | Conversión directa | Fuera de blog/silo editorial | /contratar/, /precio/, /demo/ |
| **Utilidad** | Requerida legalmente o por UX, sin valor SEO directo | Footer / standalone | /privacidad/, /terminos/, /contacto/ |
| **Programática** | Páginas generadas a escala (ubicaciones, productos) | Nivel 2-3 según tipo | /recetas/de-coco/[ingrediente]/ |

#### Clasificación por intención de búsqueda:

| Intención | Señales en la keyword | Tipo de página | En URL |
|-----------|----------------------|----------------|--------|
| Informacional | cómo, qué es, guía, tutorial, aprende | Blog, guía, how-to | `/blog/` o `/[silo]/` |
| Comercial/investigación | mejor, comparar, review, vs, alternativa | Comparativa, ranking | `/comparativas/` o dentro del silo |
| Transaccional | comprar, contratar, precio, cotizar | Página de producto/servicio | `/[producto]/`, `/servicios/` |
| Navegacional | nombre de marca + función | Branded pages | `/[marca]/` |
| Local | [servicio] + ciudad/zona | Location pages | `/[servicio]/[ciudad]/` |

#### Detección de contenido mal clasificado:

Señales de que una página está en el silo equivocado o al nivel incorrecto:
- La keyword principal de la página no coincide con la keyword del silo donde está
- La página compite en rankings contra su propia página padre para la misma keyword con la misma intención (posible canibalización — verificar con title/H1/contexto antes de diagnosticar)
- La profundidad de URL no refleja la especificidad del tema (un artículo muy específico está a nivel 1)
- El tipo de contenido no corresponde al tipo esperado en esa posición de la jerarquía
- CTR bajo + posición decente = intención mal resuelta = contenido en silo equivocado

#### Agrupación de tipos de contenido — reglas:

- **Mismo formato ≠ mismo silo.** Una receta de coco y un artículo sobre historia del coco no van en el mismo cluster aunque ambos sean artículos de blog.
- **Misma keyword ≈ mismo silo.** Agrupa por tema semántico, no por formato ni fecha.
- **Silos editoriales y silos transaccionales no se mezclan.** `/blog/seo/` y `/servicios/seo/` son silos separados que se enlazan, no uno solo.
- **Un contenido = una URL canónica = un silo.** Si el mismo contenido podría estar en dos silos, elige el más específico y enlaza desde el otro.

---

### Step 3: URL Taxonomy

URL structure is the physical manifestation of the architecture. It must reflect the silo hierarchy.

#### Hierarchy patterns by site type:

**Informational / Publisher:**
```
/[silo]/[cluster]/[supporting]
/marketing/email-marketing/email-subject-lines-guide
```

**E-commerce:**
```
/[category]/[subcategory]/[product]
/ropa/camisetas/camiseta-polo-blanca
```

**SaaS:**
```
/[product-area]/[feature or use-case]
/features/integrations/zapier
/blog/[topic]/[article]
```

**Local service:**
```
/servicios/[service]
/[city]/[service]     ← only if multi-location
```

**Agency / Consultancy:**
```
/servicios/[service-line]
/casos-de-estudio/[industry or client-type]
/blog/[topic]/[article]
```

#### URL naming rules:

- Use hyphens, never underscores
- Lowercase only, ASCII only — no accented characters (á→a, é→e, í→i, ó→o, ú→u, ü→u, ñ→n)
- No special characters: `& % = ? # + @ ! ( ) [ ] { } ' " , ; : . * ~ ^ |`
- No spaces (use hyphen instead)
- Target keyword at the start of the slug — not the end
- Max 3–5 words per slug segment (each folder level)
- No dates in URLs for evergreen content (`/blog/2024/01/how-to` → `/blog/how-to`)
- No parameters in canonical URLs for content pages
- No trailing slash inconsistency — define one convention and enforce it site-wide. Google treats `/page` and `/page/` as two different URLs. Implement 301 from non-preferred to preferred variant at server/CDN level (nginx: `rewrite ^/(.*)/$ /$1 permanent;` or Cloudflare redirect rule)
- Total URL length: ideally under 75 characters (domain included). Hard limit: 115 characters.

#### Stop words in URLs — remove unless semantically necessary:

Spanish stop words to strip: `el, la, los, las, un, una, unos, unas, y, o, a, al, del, en, con, por, para, que, se, es, su, lo`

**"de" is a partial exception:** remove when possible, but keep when it's part of the keyword structure:
- ❌ `/blog/como-elegir-el-mejor-colchon` → ✅ `/blog/como-elegir-mejor-colchon`
- ✅ `/recetas/de-coco/` — "de" es parte del término de búsqueda "recetas de coco"
- ✅ `/recetas/de-coco/flan/` — el slug final no necesita repetir "de coco", la jerarquía ya lo dice

#### Hierarchical keyword inheritance (concepto clave):

Los segmentos padre transmiten contexto keyword a los segmentos hijo. No es necesario repetir keywords ya presentes en la ruta.

```
Keyword objetivo: "receta de flan de coco"
URL correcta:     /recetas/de-coco/flan/
                   ↑         ↑      ↑
                 "recetas"  "coco" "flan"
→ Google lee la ruta completa y entiende "receta de coco + flan"

Keyword objetivo: "receta de arroz de coco"
URL correcta:     /recetas/de-coco/arroz/
→ Misma lógica: /arroz/ hereda "de coco" del padre

❌ Incorrecto: /recetas/receta-de-flan-de-coco/  (redundante, largo)
❌ Incorrecto: /recetas/coco/flan-de-coco/        (repite "coco")
❌ Incorrecto: /recetas/de-coco/flan-de-coco/     (repite toda la cadena)
✅ Correcto:   /recetas/de-coco/flan/             (herencia limpia)
```

**Ejemplos por tipo de sitio:**

| Keyword objetivo | URL optimizada | URL incorrecta |
|------------------|----------------|----------------|
| receta arroz de coco | `/recetas/de-coco/arroz/` | `/recetas/receta-de-arroz-de-coco/` |
| zapatos de mujer rojos | `/zapatos/mujer/rojos/` | `/zapatos/zapatos-para-mujer-color-rojo/` |
| abogado divorcios Madrid | `/abogados/divorcios/madrid/` | `/abogados/abogado-de-divorcios-en-madrid/` |
| software gestión restaurantes | `/software/restaurantes/` | `/software/software-de-gestion-para-restaurantes/` |

#### Architecture depth decision:

| Páginas del sitio | Profundidad recomendada | Niveles de URL | Razón |
|-------------------|------------------------|----------------|-------|
| < 100 | 2 niveles | `/silo/pagina/` | Presupuesto de rastreo concentrado |
| 100–1,000 | 3 niveles | `/silo/cluster/pagina/` | Silo + cluster + supporting |
| 1,000–10,000 | 3–4 niveles | `/silo/sub/cluster/pagina/` | Añadir subcategoría |
| > 10,000 | 4 niveles máx | Arquitectura programática | Usar `seo-programmatic` |

**Regla crítica:** Ninguna página de contenido a más de 4 niveles de profundidad desde la raíz. Cada nivel adicional reduce la autoridad transferida y dificulta el rastreo.

**Regla de los 3 clics:** Toda página debe ser alcanzable desde la homepage en máximo 3 clics de navegación, independientemente de la profundidad de URL. Nota: Google nunca ha confirmado esta regla como factor de ranking. Su valor real es garantizar descubrimiento por crawl — la densidad de enlazado interno importa más que el conteo de clics. Una página a 5 clics pero bien enlazada internamente rankea mejor que una a 2 clics sin enlaces.

**Señal de problema:** Si el 80%+ de las URLs están al mismo nivel de profundidad (ej: todas en `/blog/[slug]/` sin subcategorías), el sitio no está comunicando jerarquía temática a Google. Ver sección "Orphan URL & Depth Equity Analysis" en AUDIT mode.

---

### Step 4: Navigation Architecture

Navigation IS the architecture made visible. It signals to Google which pages are most important.

#### Navigation layers:

**Primary nav** — main silos only (5-8 items max). What appears here receives the most internal link equity.

**Secondary nav / mega menu** — pillar + top cluster pages. For large sites, group by silo within the menu.

**Footer nav** — utility pages (Privacy, Terms, Contact, Sitemap) + possibly secondary silos not in primary nav.

**Breadcrumbs** — mandatory for 3+ level hierarchies. Format: `Home > Silo > Cluster > Page`. Implement with BreadcrumbList schema in JSON-LD (Google's preferred format):

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "name": "Home", "item": "https://example.com/"},
    {"@type": "ListItem", "position": 2, "name": "Blog", "item": "https://example.com/blog/"},
    {"@type": "ListItem", "position": 3, "name": "Email Marketing", "item": "https://example.com/blog/email-marketing/"},
    {"@type": "ListItem", "position": 4, "name": "Subject Lines Guide"}
  ]
}
```

Rules: schema breadcrumbs must match visible navigation exactly. Recommended depth: 3–5 levels. Note: as of January 2025, Google removed breadcrumb display from mobile search results, but continues to use breadcrumb structure for understanding site hierarchy — implement regardless.

**In-content nav** — pillar pages must have a table of contents linking to all cluster articles within the silo.

#### Navigation rules:

- Never put all pages in the nav — only the entry point per silo
- Orphan pages (not reachable via nav or internal links) = invisible to crawlers
- Every silo must be reachable from the homepage in 1 click (via nav)
- Every cluster page must be reachable from its pillar in 1 click

---

### Step 5: Content Priority Matrix

Map every page/template to a priority tier based on volume, intent, and business value:

| Tier | Criteria | Action |
|------|----------|--------|
| P1 — Foundation | Core pillar pages, highest-volume keywords, money pages | Build first, link from everywhere |
| P2 — Growth | Cluster pages for P1 silos, commercial investigation | Build in first 3 months |
| P3 — Authority | Supporting pages, long-tail, informational depth | Build over 6-12 months |
| P4 — Opportunistic | Trending topics, seasonal, programmatic | Build when resources allow |

---

### Step 6: Internal Linking Blueprint

Define the linking rules before building content:

**Mandatory links:**
- Pillar → all cluster pages in its silo (contextual link + dedicated section)
- Each cluster page → its pillar (1 contextual link in intro or first paragraph)
- Supporting pages → their cluster page and pillar

**Cross-silo links:**
- Allowed at cluster level when topics are genuinely complementary
- Never link between pillar pages of unrelated silos
- Max 2-3 cross-silo links per page to avoid diluting topical signals

**Anchor text rules:**
- Pillar pages: anchor = primary keyword for the pillar
- Cluster pages: anchor = the specific subtopic keyword
- Never generic ("click here", "read more")
- Vary anchor text across multiple linking pages — exact match on 20-30%, variations on the rest

*For deep internal linking analysis, use `/seo internal-linking`.*

---

### Step 7: Architecture Document Output

Generate `ARCHITECTURE.md` with:

```markdown
# Site Architecture — [Brand Name]

## Brand Territories
[List of approved topic areas]

## Silo Map
[Visual tree of silos, pillars, clusters, supporting pages]

## URL Taxonomy
[Pattern by page type with examples]

## Navigation Structure
[Primary / secondary / footer breakdown]

## Content Priority Matrix
[P1/P2/P3/P4 page list]

## Internal Linking Rules
[Mandatory links + cross-silo policy + anchor text rules]

## Pages Count Estimate
| Tier | Count | Est. Build Time |
|------|-------|-----------------|

## Pending Decisions
[Any structural decision that requires stakeholder input before building]
```

---

## MODE 2: AUDIT — Architecture Comparison & Change Detection

Use this mode when a migration or redesign has happened (or is proposed), and you need to validate the new architecture is SEO-sound — or detect unauthorized changes.

### Required Inputs

| Input | Required | Source |
|-------|----------|--------|
| Old architecture (URL list or sitemap) | Yes | Old sitemap XML, Screaming Frog export, GSC crawl |
| New architecture (URL list or sitemap) | Yes | New sitemap XML, staging site crawl, URL mapping document |
| Original SEO architecture document | Recommended | ARCHITECTURE.md, SEO brief, or migration plan |
| GSC performance data (old URLs) | Recommended | Traffic, impressions, rankings per URL |
| Redirect map | Recommended | 301 mapping document from dev team |

If inputs are missing, state what can and cannot be assessed, then proceed with available data.

---

### Step 1: URL Inventory Comparison

Build a comparison table of old vs new URLs:

```
Old URL | New URL | Status | Change Type | Traffic (GSC) | Risk
/blog/email-marketing-guide | /recursos/email-marketing | 301 | URL moved | 2,400/mo | MEDIUM
/servicios/seo | /servicios/posicionamiento-web | 301 | URL renamed | 5,100/mo | HIGH
/sobre-nosotros | /about | 301 | Language change | 80/mo | LOW
/herramientas/calculadora | [NOT FOUND] | 404 | Page removed | 320/mo | CRITICAL
```

**Change type taxonomy:**

| Change Type | Definition |
|-------------|------------|
| URL moved | Same page, different path |
| URL renamed | Same position in hierarchy, different slug |
| Page removed | Old URL has no equivalent in new architecture |
| Page added | New URL with no equivalent in old architecture |
| Silo restructured | Category/parent changed |
| Hierarchy changed | Depth level changed (e.g., 2-level → 3-level) |
| Slug modified | Minor change in last path segment |
| Language changed | URL language/locale modified |
| Parameter added | Clean URL → parameterized URL |

---

### Step 2: Silo Integrity Check

Compare old silo structure vs new silo structure:

**Questions to answer:**
- Were any silos merged? (Risk: keyword cannibalization or loss of topical authority)
- Were any silos split? (Risk: dilution of pillar authority, orphaned cluster pages)
- Were pillar pages moved? (Risk: all internal links pointing to old URL, link equity lost)
- Were cluster pages assigned to a different silo? (Risk: topical signal dilution)
- Were cross-silo relationships preserved?

For each change, classify:

| Severity | Condition |
|----------|-----------|
| CRITICAL | Pillar page removed, high-traffic page 404'd, silo destroyed |
| HIGH | Pillar page URL changed without proper redirect, silo merged without redirect consolidation |
| MEDIUM | Cluster pages reassigned to wrong silo, deep page moved to root |
| LOW | Minor slug change with 301, new pages added in correct position |
| OK | No SEO-relevant change |

---

### Step 3: Orphan URL & Depth Equity Analysis

Esta es una de las auditorías más ignoradas y más reveladoras. Detecta dos problemas distintos:

#### A) URLs huérfanas

Una URL huérfana es aquella que existe en el sitio pero que **ninguna otra página enlaza internamente**. Google puede no descubrirla nunca, o no otorgarle autoridad aunque la rastree.

**Cómo detectarlas:**
- Sitemap XML: URLs presentes en sitemap pero sin ningún enlace interno entrante (Screaming Frog: compare Sitemap list vs Inlinks count = 0)
- Screaming Frog: crawl completo → filtrar por "Inlinks = 0" en páginas indexables
- GSC: páginas con impresiones pero sin enlace interno que las referencie = huérfanas de facto
- Bases de datos: páginas generadas automáticamente (CMS, programática) sin enlace en ninguna categoría

**Clasificación de huérfanas:**

| Tipo | Descripción | Acción |
|------|-------------|--------|
| Huérfana total | Sin enlace interno, sin en sitemap | Revisar si tiene tráfico GSC. Si no: auditar para eliminar o incorporar al silo |
| Huérfana de navegación | En sitemap, sin enlace interno | Añadir enlace desde su pillar o cluster correspondiente |
| Huérfana profunda | Enlazada solo desde otra huérfana | Resolver cadena completa |
| Huérfana estacional | Correctamente desactivada fuera de temporada | Verificar que tiene noindex o redirect temporal |

**Señales de urgencia en una huérfana:**
- Tiene tráfico orgánico histórico en GSC → crítica: está rankeando sin apoyo, puede caer
- Tiene backlinks externos → crítica: se está perdiendo autoridad que podría fluir al silo
- Es una página de producto o servicio → crítica: ni usuarios ni Google pueden encontrarla de forma natural

---

#### B) Depth Equity Analysis — URLs todas al mismo nivel

Problema: cuando la mayoría de URLs están a la misma profundidad, el sitio no comunica jerarquía temática a Google.

**Señales del problema:**
- 90%+ de URLs en nivel 2 (`/blog/[slug]`) sin subcategorías
- Pillar pages y supporting pages conviven al mismo nivel de profundidad
- La URL de una guía de 5.000 palabras está al mismo nivel que un artículo de 400 palabras

**Análisis de distribución de profundidad:**

Clasificar todas las URLs por nivel:
```
Nivel 0: /                    → Homepage
Nivel 1: /recetas/            → Silo / Pillar
Nivel 2: /recetas/de-coco/   → Cluster / Subcategoría
Nivel 3: /recetas/de-coco/flan/  → Supporting / Artículo
Nivel 4: /recetas/de-coco/flan/sin-horno/  → Variante (solo si tiene volumen propio)
```

Calcular distribución actual y comparar con distribución recomendada:

| Nivel | Distribución problemática | Distribución saludable |
|-------|--------------------------|----------------------|
| Nivel 1 | 5% (correcto) | 3–8% |
| Nivel 2 | 5% (demasiado bajo) | 15–25% |
| Nivel 3 | 90% (todo al mismo nivel) | 55–70% |
| Nivel 4+ | 0% | 0–10% (solo programática o muy Long-tail) |

**Cuándo el "mismo nivel" está justificado:**
- Sitio pequeño (<50 páginas): profundidad 2 es suficiente
- Blog sin categorías por decisión de marca (válido si hay baja diversidad temática)
- Arquitectura flat intencional con fuerte enlazado interno alternativo

**Cuándo el mismo nivel es un problema:**
- El sitio tiene > 100 páginas todas en `/blog/[slug]/`
- Hay keywords de distinto peso semántico tratadas estructuralmente igual
- GSC muestra que las páginas long-tail roban tráfico a las pillar porque Google no sabe cuál es más importante

**Recomendación de estructura basada en keywords:**

Para cada URL mal posicionada en la jerarquía, proponer su posición correcta:

```
URL actual:    /blog/receta-de-flan-de-coco/         (nivel 2, sin jerarquía)
URL propuesta: /recetas/de-coco/flan/                (nivel 3, dentro del silo correcto)
Redirect:      301 de la antigua a la nueva
Justificación: la keyword "recetas de coco" tiene volumen propio que justifica
               un pillar a nivel 2. El flan es un subtema dentro de ese cluster.
```

---

### Step 4: Unauthorized Change Detection

This is the forensic layer — identifying what changed **without SEO approval**.

**Process:**
1. Compare new architecture against the approved SEO architecture document (if available)
2. Flag every divergence as: Planned / Unplanned
3. For each unplanned change: assess if it's SEO-neutral, SEO-positive, or SEO-damaging

**Unauthorized change patterns to flag:**

- Developer renamed slugs "for cleaner code" without redirect
- Designer removed a category tier to simplify navigation
- CMS migration changed URL pattern automatically (`/blog/post` → `/blog/2024/01/post`)
- Marketing renamed service pages to match new brand language without 301s
- E-commerce platform changed category URLs (`/ropa-hombre` → `/categoria/hombre/ropa`)
- A/B test permanently changed the URL structure on test variant

**Report format per unauthorized change:**

```
CHANGE DETECTED (Unplanned)
URL Old:     /servicios/seo-local
URL New:     /servicios/local
Redirect:    Missing (404)
Traffic at risk: 890/mo (GSC)
Severity: CRITICAL
Recommendation: Implement 301 redirect immediately from old to new URL.
                Notify dev team. Update internal links to canonical URL.
```

---

### Step 5: Redirect Map Validation

Validate the redirect map the dev team provided (if available):

- Every 301 maps to a semantically equivalent page (not homepage by default)
- No redirect chains (A→B→C — must be A→C)
- No redirect loops
- No redirect to a 404 destination
- High-traffic URLs have their most relevant page as destination (not a generic category)

**Red flags:**
- Blanket redirect of deleted pages to homepage → Google sees as soft 404
- Redirect to a page with no-index
- Redirect to a page with canonical pointing elsewhere
- Temporary (302) redirect used instead of permanent (301) for content moves

---

### Step 6: Internal Link Breakage Assessment

When URLs change, internal links become broken or misaligned:

- Pages that linked to the old URL now need updating
- Use `seo-internal-linking` for deep anchor text and link graph analysis
- Priority: fix internal links on high-authority pages first (homepage, pillar pages, nav)

---

### Step 7: Competitive Architecture Benchmarking (Optional)

If requested, compare the audited architecture against top 3 competitors:

- How many silo levels do they use?
- How are their main categories structured?
- Do they have content types (blog, resources, tools) the audited site is missing?
- URL pattern differences that could indicate different strategic choices

Use WebFetch + robots.txt/sitemap.xml to map competitor structure without crawling.

---

### Step 8: Audit Report Output

Generate `ARCHITECTURE-AUDIT.md`:

```markdown
# Architecture Audit Report — [Brand Name]
Date: [date]
Auditor: [name or "SEO Specialist"]

## Executive Summary
[2-3 sentences: what changed, overall risk level, immediate actions required]

## Change Log
[Full comparison table: old URL | new URL | status | change type | traffic | severity]

## Silo Integrity Assessment
[Old silo map vs new silo map, differences highlighted]

## Unauthorized Changes
[List of unplanned changes with severity and recommendation]

## Redirect Map Issues
[Chains, loops, missing redirects, wrong destinations]

## Critical Actions (do before go-live or immediately if already live)
1. [CRITICAL item]
2. [CRITICAL item]

## High Priority Actions (within 7 days)
1. [HIGH item]

## Medium Priority Actions (within 30 days)
1. [MEDIUM item]

## Architecture Health Score
| Dimension | Score | Notes |
|-----------|-------|-------|
| Silo integrity | X/10 | |
| URL taxonomy consistency | X/10 | |
| Redirect coverage | X/10 | |
| Internal link preservation | X/10 | |
| Unauthorized changes | X/10 | |
| **Overall** | **X/10** | |

## Approved Changes (No Action Needed)
[List of planned changes that are SEO-sound]
```

---

## Content Pruning & Redirect Strategy

El contenido que no aporta valor SEO, de negocio ni de usuario daña la arquitectura. Google penaliza implícitamente sitios con alto porcentaje de contenido thin, duplicado o sin engagement. Esta sección define cuándo y cómo eliminar, consolidar o redirigir.

### Árbol de decisión: ¿qué hago con esta página?

```
¿La página tiene tráfico orgánico en los últimos 12 meses? (GSC > 0 clics)
├── SÍ → ¿El tráfico es > 10 clics/mes?
│   ├── SÍ → ¿La página cumple intención de búsqueda?
│   │   ├── SÍ → MANTENER. Mejorar si CTR < 3% o posición > 20
│   │   └── NO → CONSOLIDAR con la página correcta + 301
│   └── NO (1-10 clics/mes) → ¿Tiene backlinks externos con DA relevante?
│       ├── SÍ → MANTENER + mejorar contenido o redirigir a mejor página
│       └── NO → Evaluar ELIMINAR (noindex temporal → 6 meses → 410 o 301)
└── NO (0 clics orgánicos) → ¿Tiene función de negocio o UX crítica?
    ├── SÍ (ej: /contacto, /privacidad, /carrito) → MANTENER (excluir de análisis SEO)
    └── NO → ¿Tiene backlinks externos?
        ├── SÍ → REDIRIGIR a página más relevante del silo (301)
        └── NO → ELIMINAR (301 a categoría padre o 410 si no hay equivalente)
```

### Señales para priorizar la poda (por orden de urgencia):

| Señal | Descripción | Acción |
|-------|-------------|--------|
| Thin content | < 300 palabras, sin imagen, sin respuesta real a la KW | Mejorar o eliminar |
| Canibalización (confirmada) | Misma keyword primaria + mismo intent + mismo contexto de contenido en dos páginas. **No diagnosticar solo por URL similar.** Ver framework completo en `/seo keywords`. | Consolidar: una gana, otra redirige |
| Contenido duplicado interno | Mismo contenido en 2+ URLs sin canonical | Canonical a la URL correcta o 301 |
| 0 clics + 0 impresiones 12 meses | Ni siquiera aparece en SERPs | Evaluar eliminación |
| Bounce rate > 90% + tiempo < 15 seg | Usuarios entran y salen inmediatamente | Revisar intención + mejorar o eliminar |
| Sin enlace interno entrante (huérfana) | No hay ruta interna hacia ella | Conectar al silo o eliminar |
| Datos de Analytics: 0 sesiones orgánicas + 0 conversiones | Sin valor para negocio ni usuario | Candidata a eliminación |
| Contenido temporal sin actualizar | Evento pasado, promo expirada, noticia vieja | 301 a categoría o 410 |
| URL mal clasificada (fuera del silo correcto) | La KW no coincide con el silo donde está | Mover + 301 |

### Opciones de acción y cuándo aplicarlas:

#### MANTENER + Mejorar
- Tiene tráfico pero bajo rendimiento (posición 11-20, CTR < 2%)
- La intención está bien resuelta pero el contenido es escaso o desactualizado
- Acción: ampliar, actualizar, añadir schema, mejorar meta title/description

#### CONSOLIDAR (Merge + 301)
- Dos páginas compiten por la misma keyword (canibalización)
- Ambas tienen contenido parcial que juntas formarían una página completa
- Acción: combinar contenido en la URL más fuerte → 301 de la débil a la fuerte
- Regla: la URL que recibe la redirección debe tener el slug más keyword-optimizado

#### REUBICAR (Mover + 301)
- La página es buena pero está en el silo incorrecto
- La URL no refleja la jerarquía correcta (contenido de nivel 3 en nivel 1)
- Acción: nueva URL en posición correcta → 301 de la anterior

#### NOINDEX (temporal)
- Página en revisión, no se sabe si eliminar
- Contenido estacional que no está en temporada
- Acción: `<meta name="robots" content="noindex">` + revisión a los 3-6 meses
- No usar noindex como solución permanente — Google puede dejar de rastrear y no ver el cambio

#### ELIMINAR (410 Gone)
- Página sin tráfico, sin backlinks, sin función de negocio, sin equivalente útil
- Usa 410 (no 404) para señalar a Google que la eliminación es intencional y permanente
- 404 = "no encontrado" (puede ser temporal); 410 = "eliminado intencionalmente"

#### REDIRIGIR (301 Permanent)
- La página tiene backlinks o tráfico que no quieres perder
- Existe una página equivalente más relevante hacia donde enviar
- Regla: redirigir siempre a la página semánticamente más cercana, nunca a la homepage por defecto

### Protocolo de poda por lotes:

**Fase 1 — Inventario completo con contexto de página**

No basta con exportar URLs. Cada URL necesita su contexto completo antes de tomar cualquier decisión. Sin contexto, la poda es ciega.

Para cada URL del sitio, recopilar:

| Campo | Fuente | Por qué importa |
|-------|--------|----------------|
| **URL canónica** (limpia, sin UTMs ni parámetros) | Screaming Frog / sitemap | Base de comparación — ver seo-plan Paso 1b Paso 0 |
| **Title tag** | Screaming Frog / crawl | Indica intención declarada de la página |
| **H1** | Screaming Frog / crawl | Puede diferir del title — revela desalineación |
| **Keyword primaria asignada** | KW mapping / GSC query principal | ¿Hay keyword asignada? ¿Es la correcta? |
| **Keywords secundarias** | GSC top queries para esa URL | Contexto semántico real de la página |
| **Meta description** | Screaming Frog | ¿Existe? ¿Está optimizada? |
| **Profundidad de URL** (nivel 1/2/3/4) | Screaming Frog | Indica si está bien posicionada en la jerarquía |
| **Inlinks internos** (cuántas páginas enlazan a ella) | Screaming Frog | 0 = huérfana |
| **URLs de páginas que la enlazan** | Screaming Frog | Contexto de dónde fluye autoridad hacia ella |

**Fase 2 — Cruce con datos de rendimiento**

Una vez que tienes el inventario con contexto, cruzar con todas las fuentes de datos:

| Fuente | Datos a cruzar | Nota crítica |
|--------|---------------|-------------|
| **GSC** | Clics + impresiones + posición media + CTR — últimos 12 meses | Filtrar por URL exacta (todas sus versiones si hubo redirects) |
| **GA4** | Sesiones totales por canal (orgánico, directo, referral, social, paid) + conversiones | **Una página puede tener 0 clics orgánicos en GSC pero tráfico directo o referral significativo en GA4 — ese tráfico también cuenta para la decisión** |
| **Ahrefs WMT** | Backlinks externos + referring domains | Backlinks valiosos = proteger siempre |
| **Screaming Frog / crawl** | Inlinks internos (cuántas páginas enlazan a esta URL) | 0 inlinks = huérfana aunque tenga tráfico |

> **Regla de oro:** si la página tiene tráfico en GA4 aunque no aparezca en GSC, **no se elimina sin investigar**. Puede estar recibiendo tráfico directo, referral o de email que vale la pena conservar. La decisión de eliminar requiere que tanto GSC como GA4 muestren 0 actividad útil.

**Fase 3 — Árbol de decisión ampliado (con GA4)**

```
¿La página tiene tráfico orgánico en GSC últimos 12 meses? (> 0 clics)
├── SÍ → ¿El tráfico es > 10 clics/mes?
│   ├── SÍ → ¿La página cumple intención de búsqueda para su KW primaria?
│   │   ├── SÍ → MANTENER. Mejorar si CTR < 3% o posición > 20
│   │   └── NO → CONSOLIDAR con la página correcta + 301
│   └── NO (1-10 clics/mes) → ¿Tiene backlinks externos con DA relevante?
│       ├── SÍ → MANTENER + mejorar contenido o redirigir a mejor página
│       └── NO → Revisar GA4 antes de decidir (ver abajo)
└── NO (0 clics orgánicos en GSC) → ¿Tiene tráfico en GA4 por cualquier canal?
    ├── SÍ → ¿El tráfico GA4 tiene valor de negocio? (conversiones, engagement)
    │   ├── SÍ → MANTENER (página con función no-orgánica — email, referral, directo)
    │   └── NO → ¿Tiene backlinks externos?
    │       ├── SÍ → REDIRIGIR a página más relevante del silo (301)
    │       └── NO → ¿Tiene función de negocio o UX crítica?
    │           ├── SÍ (/contacto, /privacidad, /carrito) → MANTENER
    │           └── NO → ELIMINAR (301 a categoría padre o 410)
    └── NO (0 GA4 + 0 GSC) → ¿Tiene función de negocio o UX crítica?
        ├── SÍ → MANTENER (excluir del análisis SEO)
        └── NO → ¿Tiene backlinks externos?
            ├── SÍ → REDIRIGIR (301 a página más relevante del silo)
            └── NO → ELIMINAR (301 o 410)
```

**Fase 4 — Clasificar y priorizar**

Con todos los datos: clasificar cada URL en MANTENER / MEJORAR / CONSOLIDAR / REUBICAR / NOINDEX / ELIMINAR / REDIRIGIR y priorizar por impacto en crawl budget y arquitectura.

**Fase 5 — Implementar en lotes**

- Máximo 50 URLs por lote para monitorizar efecto en GSC
- Esperar 2-4 semanas entre lotes para leer el impacto
- Documentar cada cambio en el Content Pruning Log

### Output esperado: Content Pruning Log

```markdown
| URL canónica | Title | H1 | KW primaria | KW secundarias | Clics GSC | Sesiones GA4 | Backlinks | Inlinks | Acción | Destino 301 | Justificación |
|--------------|-------|----|-------------|----------------|-----------|-------------|-----------|---------|--------|-------------|---------------|
| /blog/receta-flan-coco | Receta de Flan de Coco | Cómo hacer flan de coco | receta flan coco | flan coco sin horno, flan fácil | 0 | 0 | 0 | 0 | 301 | /recetas/de-coco/flan/ | Mal clasificada, sin silo, sin tráfico |
| /blog/tipos-de-flan | Tipos de Flan | Los 8 tipos de flan más populares | tipos de flan | variantes de flan, clases de flan | 12 | 45 | 1 | 3 | MEJORAR | — | Tráfico GA4 > GSC (directo), ampliar contenido |
| /blog/2019/promo-navidad | Promo Navidad 2019 | Oferta Navidad | — | — | 0 | 2 | 0 | 0 | 410 | — | Temporal expirado, sin equivalente, tráfico residual |
| /blog/receta-coco | Receta de Coco | Todo sobre las recetas con coco | recetas de coco | postres de coco, dulces de coco | 45 | 120 | 3 | 8 | CONSOLIDAR | /recetas/de-coco/ | Canibaliza pillar, mover tráfico y backlinks |
```

> El log es el documento de trabajo y de historial. Cada decisión debe quedar registrada con su justificación para poder revertir si hay efectos negativos inesperados en GSC.

### Crawl Budget — cuándo es prioritario

**Actualización crítica (febrero 2026):** Google redujo el límite máximo de rastreo por recurso HTML de 15MB a **2MB** (reducción del 86.7%). Los archivos PDF mantienen el límite de 64MB. Esto tiene implicaciones importantes para páginas con contenido inline pesado (JS embebido, CSS inline, tablas grandes). Páginas que excedan 2MB serán truncadas por Googlebot — el contenido más allá del corte no será indexado.

El crawl budget solo es crítico cuando:
- El sitio tiene **>10.000 URLs indexables**
- Hay **muchas páginas thin o duplicadas** consumiendo presupuesto sin rankear
- GSC muestra que páginas importantes **no se rastrean frecuentemente**
- El sitio tiene **ecommerce con facetas/filtros** que generan miles de URLs

Señales de problema de crawl budget en GSC:
- Páginas importantes con fecha de último rastreo muy antigua (>30 días)
- Alto porcentaje de páginas en "Descubiertas, no rastreadas actualmente"
- Spike de URLs con errores 404 consumiendo rastreo

Acciones de mejora de crawl budget que aplican a este protocolo:
- Eliminar o redirigir páginas sin valor → reduce URLs que Googlebot debe procesar
- Corregir redirect chains → cada salto adicional consume presupuesto
- Noindex en páginas de administración, filtros, paginación sin valor
- Bloquear en robots.txt recursos que no necesitan rastreo (imágenes de backend, PDFs internos)

---

## Faceted Navigation SEO

Faceted navigation (e-commerce filters: color, size, price range, brand) is "by far the most common source of overcrawl issues" (Google). Each filter combination generates a new URL, creating thousands of near-duplicate pages.

### Decision matrix: index or not

| Facet type | Index? | Rationale |
|-----------|--------|-----------|
| Facet with dedicated search volume (e.g., `/zapatos/mujer/rojos/`) | **YES** — self-canonicalize | Real keyword demand justifies indexing |
| Facet combination with no search volume (e.g., `/zapatos/mujer/rojos/talla-38/`) | **NO** — noindex or block | No demand, dilutes crawl budget |
| Price range filters (`?precio=50-100`) | **NO** — parameter exclusion | Pure UX, no keyword value |
| Sort order parameters (`?orden=precio-asc`) | **NO** — canonical to base URL | Duplicate content |
| Pagination of filtered results | Conditional — see Pagination section | Only if meaningful unique content |

### Layered control system (recommended)

Google recommends combining controls rather than relying on one:

1. **Canonical** — self-referencing canonical on pages worth indexing; canonical to base category on pages that shouldn't be indexed
2. **Noindex** — for filter combinations with zero search demand
3. **robots.txt** — block crawling of obvious waste patterns (URL parameters via `Disallow: /*?*`)
4. **JavaScript filtering** — use client-side filtering (no URL change) for pure UX filters (sort, price slider, secondary attributes)
5. **GSC parameter handling** — configure URL parameters in GSC for remaining problematic patterns

### Implementation patterns

```
/zapatos/mujer/          → index (category pillar)
/zapatos/mujer/rojos/    → index if volume exists (facet with demand)
/zapatos/mujer/?color=rojo  → canonical to /zapatos/mujer/rojos/ (or noindex)
/zapatos/mujer/rojos/talla-38/  → noindex (no volume for this combo)
```

---

## Pagination Strategy

### Google's current guidance (2026)

- **rel=prev/next deprecated by Google** (2019) — do NOT implement these tags for Google. Bing still uses them.
- Each paginated page should **self-canonicalize** — `<link rel="canonical" href="https://example.com/blog/page/2/">`. Do NOT canonical page 2+ to page 1 or a view-all page (this was correct before 2019, now it's wrong).
- Google treats each paginated page as a standalone page and uses internal links + content signals to understand the series.

### Pagination approaches by SEO impact

| Approach | SEO signal | Implementation notes |
|----------|-----------|----------------------|
| **Traditional pagination** (`/page/2/`, `?page=2`) | Good — each page indexable, self-canonical | Add `?page=` to canonical URLs |
| **Load more button** (History API) | Best — preserves page URLs + UX | Use `history.pushState()` to update URL per loaded batch |
| **Infinite scroll** | Problematic unless fallback exists | Requires crawlable paginated URLs as fallback for Googlebot |
| **View all page** | Use only if < 50 items total | Large view-all pages can hurt performance and 2MB crawl limit |

### When to noindex paginated pages

- Paginated pages beyond page 3-4 with no meaningful unique content
- Category filter + pagination combinations (e.g., `/ropa/mujer/page/5/?color=azul`)
- Use: `<meta name="robots" content="noindex, follow">` — keep `follow` to pass equity

---

## JavaScript Framework Architecture for SEO

Architecture decisions for modern JS-heavy sites are inseparable from rendering strategy. The wrong choice means Google cannot index the site.

### Rendering strategy decision matrix

| Strategy | How it works | SEO impact | Best for |
|----------|-------------|-----------|---------|
| **SSR** (Server-Side Rendering) | HTML generated per request on server | Excellent — full HTML in first response | Dynamic content, user-specific pages |
| **SSG** (Static Site Generation) | HTML pre-built at build time | Excellent — fastest TTFB | Marketing sites, docs, blogs |
| **ISR** (Incremental Static Regeneration) | SSG + background revalidation | Excellent — fresh static pages | E-commerce, news, programmatic SEO |
| **CSR** (Client-Side Rendering) | HTML built in browser via JS | Poor for SEO — Googlebot sees empty shell until second wave rendering | SPAs, dashboards, authenticated areas only |
| **RSC** (React Server Components) | Components render on server, no JS hydration | Excellent + smaller JS bundle | Next.js 13+ app router |
| **Islands Architecture** | Mostly static HTML, isolated JS "islands" for interactive parts | Excellent — minimal JS, fast INP | Content sites with some interactivity (Astro, Eleventy) |
| **Streaming SSR** | HTML sent in chunks as components resolve | Excellent — faster TTFB for slow data | Pages with multiple data sources |

### Framework recommendations by site type

| Site type | Recommended framework | Rendering strategy |
|-----------|----------------------|-------------------|
| Marketing / blog | Astro, Next.js, Nuxt | SSG or ISR |
| E-commerce | Next.js, Nuxt, Shopify Hydrogen | ISR (product pages) + SSG (categories) |
| SaaS marketing site | Astro, Next.js | SSG |
| SaaS app | Next.js, Nuxt | SSR for auth pages, SSG for public |
| Programmatic SEO | Next.js, Nuxt | ISR — critical for scale |
| Local business | Astro, Next.js | SSG |

### Architecture-level SEO rules for JS frameworks

- **Never CSR for indexable content pages** — use SSR or SSG minimum
- **Inline critical CSS** in `<head>` — eliminates render-blocking for above-fold content
- **Partial hydration / islands** where possible — reduces JS bundle, improves INP (43% failure rate in 2026)
- **Edge rendering** (Cloudflare Workers, Vercel Edge) — reduces TTFB from 200-800ms to 20-50ms for globally distributed audiences; direct CWV improvement
- **React Server Components** in Next.js app router eliminate client-side JS for static components entirely — significant INP improvement
- **Internal link validation** — in SPA/PWA architectures, Next.js `<Link>` or Nuxt `<NuxtLink>` must be used instead of `<a href>` to ensure proper crawlability

---

## Architecture for AI Search Visibility

AI Overviews appear in 25.8% of US searches (Jan 2026). AI-referred sessions grew 527% YoY in early 2025. Architecture decisions now directly affect AI citation rates.

### How topic clusters affect AI citations

Research shows organized hub-spoke content increases AI citation probability from 12% to 41%. The mechanism: AI systems use link relationships to infer topical authority and content depth.

| Architecture factor | AI citation impact |
|--------------------|--------------------|
| Hub-spoke cluster with 8+ cluster pages | +3.4x citation probability vs flat blog |
| Bi-directional internal linking (pillar ↔ cluster) | +2.7x citation probability |
| Question-based headings (H2/H3 as actual questions) | +1.9x extractability score |
| Structured answer blocks immediately after question headings | +2.1x citation rate |
| Unique statistics/data per page | High — AI systems prefer citeable facts |
| Flat architecture with no clustering | -65% AI citation rate vs clustered sites |

### Content structure for AI extractability

Design every page in a cluster with AI extraction in mind:

```markdown
## What is [primary concept]? ← question-based H2

[Direct answer in 2-3 sentences — no preamble] ← answer block

[Supporting detail, examples, data] ← supporting content
```

**Required signals per page for AI visibility:**
- At least one direct definition or answer block per primary keyword
- Statistics with source and year (AI systems cite quantified claims at higher rates)
- Named entities (brands, people, organizations) with clear relationships
- Structured data (FAQ, HowTo, Article schema) for machine-readable extraction
- Clear authorship attribution (Profile schema)

### Internal linking for AI citation

- **Bi-directional links are mandatory** — pillar links to all clusters, every cluster links back to pillar. One-directional clusters are invisible to AI reasoning systems.
- **Entity co-mention** — when linking from cluster A to cluster B, include both primary entities in the surrounding text (reinforces semantic relationship)
- **Cross-silo contextual links** — AI systems build entity graphs; linking related entities across silos improves overall site citation rate

---

## DataForSEO Integration

When DataForSEO MCP tools are available:

| Task | Tool |
|------|------|
| Validate keyword volumes for proposed URL slugs | `kw_data_google_ads_search_volume` |
| Check competitor URL patterns and site structure | `dataforseo_labs_google_competitors_domain` |
| Estimate traffic at risk per URL | `dataforseo_labs_bulk_traffic_estimation` |
| Verify current rankings for URLs before restructuring | `dataforseo_serp_google_organic_live` |

---

## GSC Integration

When Google Search Console data is available (via `seo-google`):

- Pull click/impression/ranking data per URL before architecture changes
- Identify which URLs have ranking history worth protecting
- Post-migration: compare performance before/after for changed URLs

---

## Multilingual Architecture

When the site serves multiple languages or regions:

- Architecture must be mirrored per locale (same silo structure across languages)
- URL pattern options: subdirectory `/es/`, subdomain `es.domain.com`, ccTLD `domain.es`
- For the technical implementation of hreflang tags, use `seo-hreflang`
- For market prioritization and localization strategy, use `seo-international`
- **Rule:** Never mix languages within a silo — each silo must be fully in one language/locale

---

## Related Skills

| Need | Skill |
|------|-------|
| Deep internal link analysis (link graph, orphan pages, anchor text) | `/seo internal-linking` |
| Keyword research to feed into silo design | `/seo keywords [topic]` |
| Competitive architecture benchmarking | `/seo competitive [domain]` |
| Migration redirect mapping and post-migration recovery | `/seo migrations` |
| Technical URL validation (canonicals, indexation, crawlability) | `/seo technical [url]` |
| Programmatic architecture (thousands of template pages) | `/seo programmatic [url]` |
| Hreflang implementation for multilingual architecture | `/seo hreflang [url]` |
| International market and locale strategy | `/seo international [domain]` |
| Full SEO plan integrating architecture as phase | `/seo plan [business-type]` |
| Live keyword volumes to validate URL slugs | `/seo dataforseo` |
| GSC traffic data to prioritize high-risk URLs | `/seo google gsc <property>` |
