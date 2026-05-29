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

---

## Precondition — Live data sources must be active

The skills in this system are designed to work with live data sources when available. If the relevant MCPs or connectors are not active during execution, some output blocks may be populated from prior analyses, cached references, or partial evidence.

For a full live run, enable the relevant sources before auditing:
- `seo-google` for GSC, CrUX, PageSpeed, and Google-side signals.
- `seo-dataforseo` for SERP, keyword, backlink, Lighthouse, and AI mention data.
- `seo-firecrawl` for crawl, search context, and rendered page capture.
- `seo-crawler` only as a fallback helper when needed.
- Bing-side sources when cross-engine validation is required.

Interpretation rule:
- Do not label a skill as "limited" if the missing input is due to a disabled source.
- Instead, mark the execution as `partial` and specify which live source was unavailable.
- If live sources are active, use live data rather than historical placeholders.

---

## Execution gaps when live sources are unavailable

| Condition | What it affects | Live source that resolves it |
|---|---|---|
| No pre-launch GSC data | Historical comparison and migration baselines | `seo-google` |
| Missing recent search context | Update-sensitive diagnosis and timing risk | Google Search Status Dashboard + Search Central |
| No field performance data | CWV interpretation and prioritization | `seo-google` + CrUX / PSI |
| No rendered live content | Content analysis and page-level QA | `seo-firecrawl` or `seo-crawler` |
| No live SERP / entity view | Brand, knowledge panel, and competitor visibility | `seo-dataforseo` |
| No AI mention visibility data | AI Search visibility assessment | `seo-dataforseo` |

---

## Execution status

Every audit should end with one of these execution states:
- complete
- partial
- cached
- provisional

Definitions:
- complete: all required live sources were available and used.
- partial: one or more required live sources were unavailable.
- cached: the analysis was based mainly on prior outputs or historical material.
- provisional: recent search context or live update context could change the conclusion.

The output must explicitly state:
- which sources were active,
- which sources were missing,
- which findings depend on live data,
- which findings remain valid without live data.

---

## Global rule — Recent Search Context Check

Before producing any audit, roadmap, recovery plan, or strong causal diagnosis about SEO performance, first validate whether recent Google Search system changes may affect the interpretation of findings.

Minimum required checks:
1. Google Search Status Dashboard — confirm whether a core update, spam update, reviews update, or ranking incident is active or has recently completed.
2. Google Search Central Blog — review recent official announcements affecting crawling, indexing, structured data, rendering, spam policy, or search appearance.
3. If the task touches rich results or structured data, verify that the result type is still supported, restricted, deprecated, or removed.
4. If the task touches performance, use current Core Web Vitals terminology and thresholds; do not reference deprecated metrics.

Interpretation rule:
- Do not attribute ranking, traffic, or visibility changes purely to site issues until recent algorithmic or systems context has been checked.
- If an active or recently completed update exists, label conclusions as provisional where appropriate.
- Distinguish:
  - site defect,
  - competitive loss,
  - seasonality,
  - measurement change,
  - algorithm/update volatility.

Output rule:
Every audit or strategy skill that can issue diagnostic conclusions should include a short `recent_search_context` block in its output with:
- checked_sources
- active_updates_or_incidents
- relevant_recent_documentation_changes
- interpretation_risk
- confidence_level

---

## Global rule — Agentic Browsing / Machine Interaction Check

Before producing a full audit, technical diagnosis, AI Search assessment, or post-migration strategy for sites that may be consumed by AI agents, browser agents, or machine-assisted interfaces, check whether the site is usable for machine interaction — not only for human browsing.

Interpretation rule:
- Lighthouse Agentic Browsing is not a classic 0–100 weighted score and should not be treated as a direct ranking score.
- Treat it as a machine interaction readiness signal set.
- Use it to assess whether agents can reliably read, navigate, and interact with the site.

Minimum required checks when relevant:
1. Accessibility tree integrity and machine-readable semantics.
2. Programmatic names and labels for interactive elements.
3. Visual stability and layout shift risk.
4. Discoverability of key content and flows after rendering.
5. Presence of machine-readable interaction surfaces such as WebMCP and `llms.txt`, where relevant.

Output rule:
Any skill that performs technical, AI Search, UX, or strategy diagnosis may include an `agentic_browsing_context` block with:
- checked_signals
- machine_interaction_risk
- accessibility_tree_risk
- stability_risk
- declarative_tooling_status
- llms_txt_status
- recommendations
