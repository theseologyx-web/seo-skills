---
name: seo-competitor-pages
description: >
  Generate SEO-optimized competitor comparison and alternatives pages. Covers
  "X vs Y" layouts, "alternatives to X" pages, feature matrices, schema markup,
  and conversion optimization. Use when user says "comparison page", "vs page",
  "alternatives page", "competitor comparison", "X vs Y", "versus",
  "compare competitors", or "alternative to".
  Not for broad competitive landscape or strategy — use seo-competitive.
user-invokable: true
argument-hint: "[url or generate] [competitor]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Competitor Comparison & Alternatives Pages

Create high-converting comparison and alternatives pages that target
competitive intent keywords with accurate, structured content.

## Page Types

### 1. "X vs Y" Comparison Pages
- Direct head-to-head comparison between two products/services
- Balanced feature-by-feature analysis
- Clear verdict or recommendation with justification
- Target keyword: `[Product A] vs [Product B]`

### 2. "Alternatives to X" Pages
- List of alternatives to a specific product/service
- Each alternative with brief summary, pros/cons, best-for use case
- Target keyword: `[Product] alternatives`, `best alternatives to [Product]`

### 3. "Best [Category] Tools" Roundup Pages
- Curated list of top tools/services in a category
- Ranking criteria clearly stated
- Target keyword: `best [category] tools [year]`, `top [category] software`

### 4. Comparison Table Pages
- Feature matrix with multiple products in columns
- Sortable/filterable if interactive
- Target keyword: `[category] comparison`, `[category] comparison chart`

## Comparison Table Generation

### Feature Matrix Layout
```
| Feature          | Your Product | Competitor A | Competitor B |
|------------------|:------------:|:------------:|:------------:|
| Feature 1        | ✅           | ✅           | ❌           |
| Feature 2        | ✅           | ⚠️ Partial   | ✅           |
| Feature 3        | ✅           | ❌           | ❌           |
| Pricing (from)   | $X/mo        | $Y/mo        | $Z/mo        |
| Free Tier        | ✅           | ❌           | ✅           |
```

### Data Accuracy Requirements
- All feature claims must be verifiable from public sources
- Pricing must be current (include "as of [date]" note)
- Update frequency: review quarterly or when competitors ship major changes
- Link to source for each competitor data point where possible

## Schema Markup Recommendations

### Product Schema with AggregateRating
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[Product Name]",
  "description": "[Product Description]",
  "brand": {
    "@type": "Brand",
    "name": "[Brand Name]"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "[Rating]",
    "reviewCount": "[Count]",
    "bestRating": "5",
    "worstRating": "1"
  }
}
```

### SoftwareApplication (for software comparisons)
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "[Software Name]",
  "applicationCategory": "[Category]",
  "operatingSystem": "[OS]",
  "offers": {
    "@type": "Offer",
    "price": "[Price]",
    "priceCurrency": "USD"
  }
}
```

### ItemList (for roundup pages)
```json
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "name": "Best [Category] Tools [Year]",
  "itemListOrder": "https://schema.org/ItemListOrderDescending",
  "numberOfItems": "[Count]",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "[Product Name]",
      "url": "[Product URL]"
    }
  ]
}
```

## Keyword Targeting

### Comparison Intent Patterns
| Pattern | Example | Search Volume Signal |
|---------|---------|---------------------|
| `[A] vs [B]` | "Slack vs Teams" | High |
| `[A] alternative` | "Figma alternatives" | High |
| `[A] alternatives [year]` | "Notion alternatives 2026" | High |
| `best [category] tools` | "best project management tools" | High |
| `[A] vs [B] for [use case]` | "AWS vs Azure for startups" | Medium |
| `[A] review [year]` | "Monday.com review 2026" | Medium |
| `[A] vs [B] pricing` | "HubSpot vs Salesforce pricing" | Medium |
| `is [A] better than [B]` | "is Notion better than Confluence" | Medium |

### Title Tag Formulas
- X vs Y: `[A] vs [B]: [Key Differentiator] ([Year])`
- Alternatives: `[N] Best [A] Alternatives in [Year] (Free & Paid)`
- Roundup: `[N] Best [Category] Tools in [Year], Compared & Ranked`

### H1 Patterns
- Match title tag intent
- Include primary keyword naturally
- Keep under 70 characters

## Conversion-Optimized Layouts

### CTA Placement
- **Above fold**: Brief comparison summary with primary CTA
- **After comparison table**: "Try [Your Product] free" CTA
- **Bottom of page**: Final recommendation with CTA
- Avoid aggressive CTAs in competitor description sections (reduces trust)

### Social Proof Sections
- Customer testimonials relevant to comparison criteria
- G2/Capterra/TrustPilot ratings (with source links)
- Case studies showing migration from competitor
- "Switched from [Competitor]" stories

### Pricing Highlights
- Clear pricing comparison table
- Highlight value advantages (not just lowest price)
- Include hidden costs (setup fees, per-user pricing, overage charges)
- Link to full pricing page

### Trust Signals
- "Last updated [date]" timestamp
- Author with relevant expertise
- Methodology disclosure (how comparisons were conducted)
- Disclosure of own product affiliation

## Fairness Guidelines & FTC Disclosure

- **Accuracy**: All competitor information must be verifiable from public sources
- **No defamation**: Never make false or misleading claims about competitors
- **Cite sources**: Link to competitor websites, review sites, or documentation
- **Timely updates**: Review and update when competitors release major changes
- **Disclose affiliation**: Clearly state which product is yours
- **Balanced presentation**: Acknowledge competitor strengths honestly
- **Pricing accuracy**: Include "as of [date]" disclaimers on all pricing data
- **Feature verification**: Test competitor features where possible, cite documentation otherwise

### FTC Disclosure Requirements (EE.UU. — aplica a todos los clientes SaaS USA)

La FTC (Federal Trade Commission) exige disclosure claro y prominente en páginas de comparación y alternativas cuando existe relación comercial:

**Cuándo es obligatorio:**
- La página compara tu propio producto vs. competidores (eres parte interesada)
- Recibes compensación por recomendar alguno de los productos listados (afiliados)
- El contenido puede interpretarse como revisión imparcial pero no lo es

**Formato requerido (FTC Endorsement Guides 2023):**
```html
<!-- Ejemplo de disclosure correcto -->
<p class="disclosure">
  <strong>Disclosure:</strong> This page was created by [Company Name]. 
  We obviously believe our product is the best choice, but we've done our 
  best to represent competitor information fairly and accurately. 
  Affiliate links are marked with [*].
</p>
```

**Posicionamiento del disclosure:**
- Debe ser visible ANTES de que el usuario consuma el contenido de comparación
- No vale ponerlo solo al final o en el footer
- Tamaño de fuente comparable al contenido principal (no letra pequeña ilegible)
- No requiere lenguaje legal complejo — solo claro y honesto

**En roundup pages con afiliados:**
```
"We may earn a commission if you purchase through our links. 
This does not affect our editorial independence."
```

> **Nota práctica:** Google también premia el disclosure transparente — se alinea con E-E-A-T (honesty signal). Nunca omitirlo.

## AI Citation Optimization para Comparison Pages

Las páginas de comparación y alternativas son uno de los formatos con mayor potencial de aparición en AI Overviews y AI Mode de Google — si están bien optimizadas para ser citadas por IA.

**Fuente:** Semrush AI Visibility Study 2025, BrightEdge AI Search Research 2025

### Datos clave de AI citations para este tipo de páginas

| Factor | Dato | Implicación |
|--------|------|-------------|
| Longitud > 20,000 chars | **10.18 citations avg** en AI Mode | Más largo = más citable |
| Formato listicle (numerado) | **40.86% de AI Mode citations** provienen de listicles | Usar listas numeradas en comparativas |
| Páginas con schema ≥ 2 tipos | Alta presencia en AI citations | Añadir Product + ItemList + Review schema |
| Párrafos auto-suficientes (>40 palabras) | Alta citabilidad passage-level | Cada punto de comparación debe ser comprensible aislado |

### Share of Model (SoM) — métrica emergente

**Qué es:** El % de veces que tu producto aparece mencionado (positivamente) en respuestas AI cuando alguien pregunta por soluciones en tu categoría.

```
Share of Model = Menciones positivas de tu producto en AI
                 ─────────────────────────────────────────
                 Total de menciones de productos en esa categoría
```

**Por qué importa para comparison pages:**
- Las páginas de comparación bien construidas aumentan el SoM de tu producto
- Google AI y ChatGPT entrenan con contenido web — tus páginas "enseñan" a los LLMs cómo posicionar tu producto
- Una página "/alternatives-to-competitor" puede llevar a la IA a mencionar tu producto cuando alguien pregunte por alternativas

**Cómo trackear SoM:**
- Hacer queries en ChatGPT/Gemini/Perplexity: "What are the best alternatives to [Competitor]?"
- Registrar si tu producto aparece, en qué posición y con qué descripción
- Herramientas: Otterly.ai, Peec AI, Share of Voice AI (SE Ranking)
- Frecuencia: mensual — comparar baseline con crecimiento

### Checklist de citabilidad AI para comparison pages

```
✅ Longitud total > 15,000 caracteres (idealmente > 20,000)
✅ Formato: numbered lists para cada alternativa/comparativa
✅ Cada sección es auto-suficiente (párrafo intro + bullets + veredicto)
✅ Schema: ItemList + Product/SoftwareApplication + Review schema
✅ H2/H3 claros que responden preguntas directas ("Is X better than Y for [use case]?")
✅ Tabla de features con celdas descriptivas (no solo ✅/❌ — añadir "por qué")
✅ Sección de FAQ al final con preguntas de comparación directas
✅ llms.txt no bloquea esta sección del sitio
✅ GPTBot y PerplexityBot permitidos en robots.txt
✅ Fecha de actualización visible (señal de freshness para AI)
```

### Formato de lista numerada para AI citations (ejemplo)

En lugar de:
```markdown
## Alternatives to [Competitor]
- Product A
- Product B
- Product C
```

Usar:
```markdown
## 5 Best Alternatives to [Competitor] in 2026

### 1. [Your Product] — Best for [Primary Use Case]
[Your Product] offers [key differentiator]. Unlike [Competitor], it [specific advantage].
Best for: [target user]. Pricing: from $X/month.

### 2. [Alternative B] — Best for [Use Case B]
[Description paragraph that works standalone]...
```

> Este formato numerado con contexto auto-suficiente por item es el que tiene mayor tasa de citation en AI Mode según datos de Semrush 2025.

---

## Internal Linking

- Link to your own product/service pages from comparison sections
- Cross-link between related comparison pages (e.g., "A vs B" links to "A vs C")
- Link to feature-specific pages when discussing individual features
- Breadcrumb: Home > Comparisons > [This Page]
- Related comparisons section at bottom of page
- Link to case studies and testimonials mentioned in the comparison

## Output

### Comparison Page Template
- `COMPARISON-PAGE.md`: Ready-to-implement page structure with sections
- Feature matrix table
- Content outline with word count targets (minimum 1,500 words)

### Schema Markup
- `comparison-schema.json`: Product/SoftwareApplication/ItemList JSON-LD

### Keyword Strategy
- Primary and secondary keywords
- Related long-tail opportunities
- Content gaps vs existing competitor pages

### Recommendations
- Content improvements for existing comparison pages
- New comparison page opportunities
- Schema markup additions
- Conversion optimization suggestions

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Identify which competitors to target | `/seo competitive [domain]` | SERP competitor mapping and keyword gap analysis |
| Keyword targeting for comparison pages | `/seo keywords [topic]` | "X vs Y" and "alternatives" intent clusters and volume data |
| Schema validation and generation | `/seo schema <url>` | Product, SoftwareApplication, and ItemList JSON-LD validation |
| Live SERP positions for comparison keywords | `/seo dataforseo serp <keyword>` | Verify ranking opportunity before investing in page creation |

---

## Error Handling

| Scenario | Action |
|----------|--------|
| Competitor URL unreachable | Report which competitor URLs failed. Proceed with available data and note gaps in the comparison. |
| Insufficient competitor data (pricing, features unavailable) | Flag missing data points clearly. Use "Not publicly available" in comparison tables rather than guessing. |
| No product/service overlap found | Report that the products serve different markets. Suggest alternative competitors that share feature overlap, or pivot to a category roundup format. |
