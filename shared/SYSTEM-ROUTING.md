# SYSTEM-ROUTING

Leer este archivo antes de invocar cualquier skill. Su función es resolver routing rápido, evitar solapamientos y reducir llamadas erróneas.

---

## Reglas globales

1. Si la tarea es una auditoría completa del sitio, usar `seo-audit`. No usar `seo` para auditar.
2. Si la tarea es estrategia o roadmap, usar `seo-plan`. No usar `seo-audit`.
3. Si la tarea es de una sola URL, usar `seo-page` salvo que el foco sea solo contenido (`seo-content`) o solo CWV (`seo-performance`).
4. Si el skill es `[utility]` o `[extension]`, no usarlo como análisis estratégico principal salvo que la tarea pida esa herramienta explícitamente.
5. Cuando dos skills parezcan válidos, elegir el más estrecho primero y escalar al más amplio solo si faltan señales.

---

## Entrada recomendada

| Tarea | Skill |
|-------|-------|
| Discovery inicial de cliente o proyecto | `seo-client-discovery` |
| Auditoría sitio completo | `seo-audit` |
| Estrategia / plan | `seo-plan` |
| AI Search holístico | `seo-ai-search-readiness` |
| Page audit individual | `seo-page` |

---

## Pares conflictivos

| Si pensabas usar… | Pero la necesidad real es… | Usa |
|-------------------|---------------------------|-----|
| `seo` | auditoría completa | `seo-audit` |
| `seo-audit` | roadmap o priorización | `seo-plan` |
| `seo-backlinks` | encontrar oportunidades y targets accionables | `seo-link-building` |
| `seo-link-building` | radiografía del perfil actual, links tóxicos | `seo-backlinks` |
| `seo-technical` | diagnóstico profundo de CWV | `seo-performance` |
| `seo-performance` | crawl / index / rendering issues | `seo-technical` |
| `seo-brand` | entity graph / Wikidata / schema de entidad | `seo-entity` |
| `seo-entity` | reputación / SERP de marca | `seo-brand` |
| `seo-local` | geo-grid / rankings de mapa | `seo-maps` |
| `seo-maps` | estrategia local general / GBP / NAP | `seo-local` |
| `seo-content` | decidir schema y requisitos por tipo de contenido | `seo-content-types` |
| `seo-schema` | elegir qué schema usar según tipo de página | `seo-content-types` |
| `seo-cro` | fricción del journey post-click (forms, onboarding) | `seo-cx` |
| `seo-cx` | conversión de una página (CTA, layout, A/B) | `seo-cro` |
| `seo-sxo` | conversión en página | `seo-cro` |
| `seo-cro` | intent match SERP, pogo-sticking, snippet | `seo-sxo` |
| `seo-geo` | answerability y formato de respuesta directa | `seo-aeo` |
| `seo-geo` | retrievability técnica para LLMs | `seo-llmo` |
| `seo-geo` | evaluación holística de AI Search | `seo-ai-search-readiness` |
| `seo-firecrawl` | capturar HTML de una sola URL específica | `seo-crawler` |
| `seo-crawler` | crawl del sitio o discovery masivo | `seo-firecrawl` |
| `seo-images` | generar nueva imagen OG o hero | `seo-image-gen` |
| `seo-image-gen` | auditar imágenes existentes | `seo-images` |
| `seo-dataforseo` | datos nativos Google (GSC, GA4, PageSpeed) | `seo-google` |
| `seo-google` | datos de terceros (SERP, keywords, backlinks) | `seo-dataforseo` |
| `seo-robots` | generar o validar sitemaps | `seo-sitemap` |
| `seo-sitemap` | crawl directives o reglas de bots | `seo-robots` |
| `seo-architecture` | migración de URLs en curso | `seo-migrations` |
| `seo-migrations` | diseño de IA sin cambio de URLs | `seo-architecture` |
| `seo-hreflang` | cualquier tarea de hreflang | `seo-international` (sección 11) |

---

## Estados especiales

- **`[utility]`** — helper técnico o generador; no actúa como skill estratégico principal.
- **`[extension]`** — conector o capability externa (requiere MCP o herramienta adicional).
- **`[stub]`** — alias técnico; enruta a otro skill principal sin lógica propia.

---

## Regla de output

Siempre que sea posible, producir:

```yaml
findings: []
opportunities: []
priority_actions: []
dependencies: []
handoff_to_skills: []
```

Esto permite encadenar skills sin perder contexto entre llamadas.
