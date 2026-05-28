---
name: seo-page
description: >
  Deep single-page SEO analysis covering on-page elements, content quality,
  technical meta tags, schema, images, and performance. Use when user says
  "analyze this page", "check page SEO", "single URL", "check this page",
  "page analysis", or provides a single URL for review.
  Not for full site audit — use seo-audit.
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Single Page Analysis

## What to Analyze

### On-Page SEO — Rank Math Style Checklist

Evalúa cada punto como PASS ✅ / WARN ⚠️ / FAIL ❌. El objetivo es verde en todos los puntos básicos antes de publicar.

---

#### A. Title Tag
| Check | Criterio | Estado |
|-------|---------|--------|
| Longitud | 50-60 caracteres (≈580px). < 30 = muy corto. > 60 = recortado por Google | — |
| Keyword a la izquierda | La keyword principal debe estar en los primeros 3-4 palabras del title | — |
| Keyword presente | Keyword exacta o variante muy cercana (no sinónimos lejanos) | — |
| Único | No repetido en otras páginas del mismo sitio | — |
| Sin keyword stuffing | No repetir la keyword ni rellenar con variantes forzadas | — |
| Marca al final | Formato recomendado: `[Keyword + descriptor] — Marca` | — |

**Estructura óptima de title:**
```
[Keyword principal al inicio] [descriptor o beneficio] — [Marca]
Ejemplo: "SEO para SaaS: Guía Completa de Estrategia — NombreMarca"
```

---

#### B. Meta Description
| Check | Criterio | Estado |
|-------|---------|--------|
| Longitud | 145-155 caracteres (mobile) / hasta 160 (desktop). No hay límite técnico, pero Google recorta | — |
| Keyword al inicio | La keyword principal debe aparecer en las primeras palabras | — |
| Texto descriptivo | Explica qué encontrará el usuario después del click | — |
| CTA al final | Terminar con llamada a la acción: "Aprende cómo", "Descúbrelo", "Empieza hoy" | — |
| Única | No usar la misma meta description en múltiples páginas | — |
| Sin comillas dobles | Las comillas dobles truncan la meta description en SERP | — |

**Estructura obligatoria de meta description:**
```
[Keyword principal] + [texto descriptivo con beneficio] + [CTA]

❌ MAL: "Información sobre SEO para empresas SaaS en esta página."
✅ BIEN: "SEO para SaaS: aprende a escalar el tráfico orgánico con estrategias probadas para B2B. Guía paso a paso."
```

---

#### C. H1
| Check | Criterio | Estado |
|-------|---------|--------|
| Único | Solo un H1 por página (nunca 0, nunca 2+) | — |
| Keyword presente | Keyword principal exacta o variante directa | — |
| Keyword a la izquierda | Idealmente en las primeras palabras del H1 | — |
| Longitud | 20-70 caracteres recomendado (no hay límite técnico) | — |
| No igual al title | Puede ser similar pero no copia exacta — son elementos distintos | — |
| Coincide con intención | Refleja el tema de la página, no clickbait | — |

---

#### D. Headings H2–H6 y Keyword en Estructura
| Check | Criterio | Estado |
|-------|---------|--------|
| Keyword en al menos 1 H2 | La keyword principal (o variante semántica cercana) debe aparecer en un H2 | — |
| Keywords secundarias en H2s | Cada H2 cubre un subtema con su propia keyword/concepto | — |
| Sin saltos de jerarquía | Nunca H1→H3 sin H2 intermedio | — |
| Sin múltiples H1 | Revisar que no haya H1 escondidos en CSS o JS | — |
| H2s descriptivos | No "Introducción", "Conclusión" — describir el beneficio o el tema | — |
| H3s dentro de su H2 | Los H3 son subsecciones del H2 que los contiene | — |

**Ejemplo de jerarquía correcta:**
```
H1: Guía de Email Marketing para SaaS
  H2: Qué es el email marketing para SaaS (keyword secundaria)
    H3: Diferencias con email marketing B2C
    H3: KPIs clave en SaaS
  H2: Cómo construir una lista de email para SaaS
    H3: Lead magnets que funcionan en B2B
  H2: Herramientas de email marketing para SaaS (keyword de herramienta)
```

---

#### E. Keyword en el Contenido
| Check | Criterio | Estado |
|-------|---------|--------|
| Keyword en primer 10% del texto | Debe aparecer en el primer párrafo visible, idealmente en las primeras 100-150 palabras | — |
| Keyword en introducción | Si el artículo tiene introducción, la keyword aparece antes del primer H2 | — |
| Densidad natural (1-2%) | No forzar. En texto de 1500 words: 15-30 menciones máximo de la keyword exacta | — |
| Variantes semánticas | Usar sinónimos, variantes LSI, entidades relacionadas para no repetir la misma frase | — |
| Sin over-optimization | Nunca repetir la keyword exacta más de 1 vez por párrafo | — |
| Keyword en conclusión | Si hay sección de cierre, incluir keyword o variante principal | — |

**Over-optimization: señales de alerta**
```
❌ "El email marketing para SaaS es importante. Con email marketing para SaaS puedes..."
✅ "El email marketing para SaaS es una de las estrategias con mejor ROI. Con esta táctica puedes..."
```

---

#### F. URL
| Check | Criterio | Estado |
|-------|---------|--------|
| Keyword en URL | La keyword principal (o versión condensada) debe estar en el slug | — |
| Sin stop words innecesarias | Eliminar: de, la, el, los, the, a, an, and, or, for, in, on | — |
| Hyphens como separadores | Usar `-` (nunca `_`, espacios, o caracteres especiales) | — |
| Todo en minúsculas | `/email-marketing-saas` no `/Email-Marketing-SaaS` | — |
| Longitud de slug | Máximo 5-6 palabras en el slug. Más = dilución de relevancia | — |
| Sin parámetros innecesarios | `?id=123&ref=home` en URLs públicas = mala práctica | — |
| Sin fechas en el slug | `/2024/01/15/articulo` dificulta actualización sin perder historial | — |

**Estructura de URLs para blogs (taxonomía SEO):**
```
Estructura correcta:
dominio.com/[categoría]/[slug]

Ejemplos:
dominio.com/email-marketing/automatizacion-saas/
dominio.com/seo-tecnico/velocidad-de-carga/
dominio.com/blog/email-marketing-saas/  ← si no hay categorías temáticas

EVITAR:
dominio.com/blog/2024/01/15/como-hacer-email-marketing-para-empresas-saas-en-2024/
dominio.com/post?id=4821
dominio.com/categoria/subcategoria/sub-subcategoria/otro-nivel/slug/
```

**Agrupación de contenido y selección de categorías para blogs:**
- Máximo 2 niveles de profundidad: `/categoria/slug/`
- Categorías deben ser términos que la gente busca (keyword research)
- Una categoría = un cluster temático (no crear categorías de 1-2 artículos)
- No mezclar temas en una categoría para mantener topical authority
- Las categorías mismas son páginas que deben tener contenido propio (no solo listas de posts)

---

#### G. Imágenes — On-Page SEO
| Check | Criterio | Estado |
|-------|---------|--------|
| Al menos 1 alt con keyword | La keyword principal debe estar en el alt text de al menos una imagen | — |
| Alt text descriptivo | Describe la imagen + incluye keyword de forma natural | — |
| Sin keyword stuffing en alts | No listar keywords: `alt="SEO email marketing herramientas estrategia"` | — |
| Nombre de archivo con keyword | El archivo debe nombrarse con la keyword y hyphens antes de subir | — |
| Sin texto dentro de imágenes | Google no lee texto embebido en imágenes — siempre incluir ese texto en HTML | — |

**Redacción correcta de alt text:**
```
❌ alt=""                                      → missing
❌ alt="imagen"                                → genérico sin valor
❌ alt="email marketing saas herramientas"     → keyword stuffing
❌ alt="Captura de pantalla 2024-01-15"        → nombre de archivo como alt
✅ alt="Dashboard de métricas de email marketing para una empresa SaaS"
✅ alt="Gráfica de tasa de apertura de emails en campañas B2B"
✅ alt="Comparativa de herramientas de email marketing para SaaS 2025"
```

**Nomenclatura correcta de archivos de imagen:**
```
❌ IMG_4821.jpg
❌ captura-de-pantalla.png
❌ email marketing saas.jpg   (con espacios)
❌ emailMarketingSaaS.jpg      (camelCase)
✅ email-marketing-saas-dashboard.webp
✅ tasa-apertura-emails-b2b-saas.webp
✅ herramientas-email-marketing-comparativa.webp
```

**Texto dentro de imágenes — regla absoluta:**
- Nunca incluir texto importante (estadísticas, datos clave, CTAs, instrucciones) SOLO dentro de una imagen
- Google no lo indexa, los lectores de pantalla no lo leen, no es seleccionable
- Si la infografía tiene texto clave → añadir el mismo contenido en HTML debajo o en el alt

---

#### H. Internal Linking (Interlinking)
| Check | Criterio | Estado |
|-------|---------|--------|
| Cantidad adecuada | 3-5 internal links por cada 1000 palabras | — |
| Anchor text con keyword | El anchor text debe contener la keyword del artículo DESTINO | — |
| Contextual, no genérico | El link aparece dentro del flujo del texto, no solo en sidebar | — |
| No "clic aquí" ni "más info" | Anchors genéricos desperdician relevancia semántica | — |
| Diversidad de anchor | No siempre el mismo anchor para la misma URL de destino | — |
| Sin orphan pages | Cada página del sitio tiene al menos 1 internal link apuntándole | — |
| Contenidos relacionados al final | Sección "Artículos relacionados" o "Seguir leyendo" al final del post | — |

**Anchor text correcto para interlinking:**
```
❌ "Para más información, haz clic aquí"
❌ "Ver este artículo"
✅ "aprende cómo funciona la automatización de email marketing"
✅ "guía completa de email marketing para SaaS"
✅ "herramientas de email marketing B2B comparadas"
```

**Sección de contenidos relacionados:**
- Al menos 3-4 links a artículos del mismo cluster temático
- Incluir imagen thumbnail + título + breve descripción (mejora CTR interno)
- Colocar antes del footer, no dentro del contenido

---

### Keyword Assignment & Cannibalization Check

Before confirming the keyword strategy for this page, identify the content type and verify intent alignment:

**Step 1 — Identify content type:**
Homepage / Product-Service Page / Use Case Page / Case Study / Blog Article / About Us / Pricing Page / Comparison Page / Category Page / FAQ / Landing Page / Ecommerce Product Page

**Step 2 — Verify intent alignment:**

| Content Type | Intent | Correct Primary KW | NEVER |
|-------------|--------|-------------------|-------|
| Homepage | Navegacional | Brand + umbrella terms | Specific product KW, long tails |
| Product/Service Page | Transaccional | "[product] + [benefit/industry]" | Brand only, "qué es" queries |
| Use Case Page | Comercial | "[product] para [segment/role]" | Exact same KW as main product page |
| Case Study | Comercial | "[client] + [result]", "caso de éxito [industry]" | Transaccional direct |
| Blog Article | Informacional | "qué es X", "cómo X", "guía de X" | Transaccional KWs (those go to landing/product) |
| About Us | Navegacional | "[Brand] + quiénes somos / historia" | Any non-branded KW |
| Pricing Page | Transaccional | "[product] precio / planes / tarifas" | Informational or brand generic |
| Comparison Page | Comercial | "[product A] vs [product B]", "alternativas a X" | Direct product KW (goes to product page) |
| Category Page | Navegacional/Info | Category umbrella term (must be searchable) | Specific article-level KW |
| FAQ | Informacional | Exact question the user types | Transaccional intent KWs |
| Landing Page | Transaccional | Specific offer KW — must match ad/source copy | Multiple CTAs or topics |
| Ecommerce Product | Transaccional | "[brand] [model] [variant]" | Category-level KW (goes to category page) |

**Step 3 — Cannibalization check:**
- Is another page targeting the same primary keyword? → Flag as cannibalization risk
- Use `site:domain.com "keyword"` or check GSC for multiple URLs getting impressions for the same query
- Two pages of different types can target related but distinct keywords — not the same keyword

**Step 4 — SERP validation:**
Before assigning a keyword to a page type, check what Google shows in top 10 for that query:
- Top 10 = blogs → keyword belongs in a blog article
- Top 10 = product pages → keyword belongs in a product page
- Top 10 = comparison pages → keyword needs a comparison page
- Forcing the wrong content type against SERP signals → low rankings regardless of optimization

### Content Quality
- Word count vs page type minimums (see quality-gates.md)
- Readability: Flesch Reading Ease score, grade level
- Keyword density: natural (1-3%), semantic variations present
- E-E-A-T signals: author bio, credentials, first-hand experience markers
- Content freshness: publication date, last updated date

### Technical Elements
- Canonical tag: present, self-referencing or correct
- Meta robots: index/follow unless intentionally blocked
- Hreflang: if multi-language, correct implementation (see errors section below)

**Open Graph / Social Preview:**
- `og:title` — matches intent, not just the title tag
- `og:description` — 150-200 chars, compelling (this is what shows on social/Slack/WhatsApp)
- `og:image` — recommended 1,200×630px (minimum), 1,600px wide for Google Discover cards
- `og:image:alt` — required for accessibility; screen readers use this
- `og:url` — canonical URL (not the current URL if different)
- `og:type` — `article` for blog posts, `website` for homepage/product pages
- **Connection to Google Discover:** Google Discover uses `og:image` for card display. Without `max-image-preview:large` AND a 1,200px+ og:image, Discover shows a small thumbnail instead of a full card

**Twitter Card:**
- `twitter:card` — `summary_large_image` for articles/blog posts (shows big image), `summary` for product/homepage
- `twitter:title`, `twitter:description`, `twitter:image` — if og: tags are set, Twitter uses those as fallback

**Hreflang Common Errors:**
- Missing `x-default` tag → Google doesn't know which page to show for unmatched locales
- Language-only code (`en`) instead of language-region (`en-US`, `en-GB`) — acceptable but less precise
- Canonical/hreflang conflict: canonical pointing to a different URL than the hreflang self-reference
- Missing reciprocal hreflang: every page in the hreflang set must reference all other pages

### Schema Markup
- Detect all types (JSON-LD preferred)
- Validate required properties
- Identify missing opportunities
- NEVER recommend HowTo (deprecated) or FAQ (restricted to gov/health)

### Images
- Alt text: present, descriptive, includes keywords where natural
- File size: flag >200KB (warning), >500KB (critical)
- Format: recommend WebP/AVIF over JPEG/PNG
- Dimensions: width/height set for CLS prevention
- Lazy loading: loading="lazy" on below-fold images

### Core Web Vitals (reference only, not measurable from HTML alone)
- Flag potential LCP issues (huge hero images, render-blocking resources)
- Flag potential CLS issues (missing image dimensions, injected content)

**INP (Interaction to Next Paint) — replaced FID as Core Web Vital March 2024:**
- Good: ≤200ms | Needs improvement: ≤500ms | Poor: >500ms
- Common causes from HTML/JS analysis:
  - Heavy synchronous JS event handlers
  - Long tasks blocking the main thread (scripts with no `async`/`defer`)
  - Complex DOM size (>1,500 elements) — increases rendering cost per interaction
  - Third-party scripts (chat widgets, analytics, ad platforms) without lazy loading
  - No use of `scheduler.yield()` for long tasks (modern mitigation pattern)

### AI Citation Readiness Check (page-level)
For article/blog pages, check whether the page has elements that make it citable by AI systems:
- [ ] TL;DR summary box in first 200 words (50-70 words, contains primary answer)
- [ ] Visible author byline with link to author bio
- [ ] At least 1 statistic/data point with source in first 500 words
- [ ] H2/H3 headings phrased as questions (PAA-optimized)
- [ ] Direct answer paragraph within 50 words after each question heading
- [ ] Tables or lists for comparative/enumerable data

### Passage Ranking Readiness
Google's Passage Ranking affects ~7% of queries by ranking page sections independently. Check:
- [ ] Each H2 section is **self-contained** (answers one specific question without needing surrounding context)
- [ ] H2 headings include target subtopic keywords
- [ ] Sections are 150-400 words (not too short to lack context, not too long to dilute)
- [ ] No sections that only make sense if read sequentially (defeats passage ranking)

### Visual Search Readiness (product/image-heavy pages)
Google Lens processes 12B+ queries/month. For product and image-rich pages:
- [ ] Product images have clean backgrounds (white/neutral) for Lens recognition
- [ ] Images are high-resolution (minimum 800px on longest dimension)
- [ ] Multiple angles available for product images
- [ ] Product structured data (Product schema) present for visual commerce eligibility
- [ ] Image distinctiveness — generic stock photos don't rank in Google Lens

### Author Entity Verification (article pages)
Per Feb 2026 Google Authors guidance:
- [ ] Visible author byline with full name (not "Staff" or "Admin")
- [ ] Byline links to author bio page
- [ ] Author bio page has: photo, credentials, sameAs links to LinkedIn/publications
- [ ] Person schema on author bio page (or ProfilePage schema)
- [ ] Author name spelling consistent across all articles on the site

## Output

### Page Score Card
```
Overall Score: XX/100

On-Page SEO:     XX/100  ████████░░
Content Quality: XX/100  ██████████
Technical:       XX/100  ███████░░░
Schema:          XX/100  █████░░░░░
Images:          XX/100  ████████░░
```

### Rank Math-Style Content Checklist

Marcar cada item como ✅ PASS / ⚠️ WARN / ❌ FAIL

**Title Tag**
- [ ] Longitud: XX caracteres (objetivo 50-60)
- [ ] Keyword a la izquierda (primeras 3-4 palabras)
- [ ] Keyword presente
- [ ] Único en el sitio
- [ ] Marca al final

**Meta Description**
- [ ] Longitud: XX caracteres (objetivo 145-155)
- [ ] Keyword al inicio
- [ ] Tiene texto descriptivo + CTA al final
- [ ] Única en el sitio
- [ ] Sin comillas dobles

**H1**
- [ ] Solo uno en la página
- [ ] Keyword presente
- [ ] Keyword a la izquierda
- [ ] No igual al title tag

**Headings**
- [ ] Keyword en al menos 1 H2
- [ ] Sin saltos de jerarquía
- [ ] H2s descriptivos (no "Introducción")

**Keyword en Contenido**
- [ ] Keyword en el primer párrafo / primer 10% del texto
- [ ] Densidad natural (1-2%)
- [ ] Variantes semánticas presentes
- [ ] Sin over-optimization (no repetir exacta más de 1x por párrafo)

**URL**
- [ ] Keyword en slug
- [ ] Sin stop words
- [ ] Hyphens como separadores
- [ ] Máximo 5-6 palabras en el slug
- [ ] Estructura de categorías coherente (si es blog)

**Imágenes**
- [ ] Al menos 1 alt con la keyword
- [ ] Alts descriptivos (no genéricos ni stuffed)
- [ ] Nombres de archivo con keyword y hyphens
- [ ] Sin texto crítico dentro de imágenes

**Internal Linking**
- [ ] 3-5 links por 1000 words
- [ ] Anchor text contiene keyword del destino
- [ ] Sin "clic aquí" ni anchors genéricos
- [ ] Sección de contenidos relacionados al final

**Elementos de Contenido**
- [ ] Bullets / listas presentes
- [ ] Negritas en conceptos clave (no decorativas)
- [ ] Imágenes o visual cada 400-600 words
- [ ] Sin bloques de texto de 200+ words sin break visual

**Semáforo global:**
- 🟢 Verde: 0-2 FAILs → Listo para publicar
- 🟡 Amarillo: 3-5 FAILs → Corregir antes de publicar
- 🔴 Rojo: 6+ FAILs → Necesita reescritura/restructuración

### Issues Found
Organized by priority: Critical -> High -> Medium -> Low

### Recommendations
Specific, actionable improvements with expected impact

### Schema Suggestions
Ready-to-use JSON-LD code for detected opportunities

## DataForSEO Integration (Optional)

If DataForSEO MCP tools are available, use `serp_organic_live_advanced` for real SERP positions and `backlinks_summary` for backlink data and spam scores.

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Full schema validation and JSON-LD generation | `/seo schema <url>` | Rich result eligibility, required properties, error detection |
| Real CWV field data | `/seo google pagespeed <url>` | Chrome user metrics (p75) vs. Lighthouse lab estimates |
| Live Lighthouse and crawl analysis | `/seo dataforseo onpage <url>` | Real-time on-page data beyond static HTML parsing |
| Deeper E-E-A-T and content quality | `/seo content <url>` | AI citation readiness, readability scoring, thin content |
| Site-wide technical issues | `/seo technical <url>` | Canonicals, hreflang, security headers, mobile — beyond single page |
| Expand to full site audit | `/seo audit <url>` | All pages, all dimensions, with scoring and action plan |

---

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable (DNS failure, connection refused) | Report the error clearly. Do not guess page content. Suggest the user verify the URL and try again. |
| Page requires authentication (401/403) | Report that the page is behind authentication. Suggest the user provide the rendered HTML directly or a publicly accessible URL. |
| JavaScript-rendered content (empty body in HTML) | Note that key content may be rendered client-side. Analyze the available HTML and flag that results may be incomplete. Suggest using a browser-rendered snapshot if available. |
