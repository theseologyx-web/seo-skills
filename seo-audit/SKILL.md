---
name: seo-audit
description: >
  Full-funnel SEO audit (diagnóstico 360) adaptado al momento del negocio.
  Usa el perfil generado por seo-client-discovery (no-visibility, redesign-migration,
  needs-ranking) para decidir profundidad y foco de la auditoría. Orquesta un crawl
  de hasta 500 páginas, delega en subagentes especializados (técnico, contenido,
  off-page, local, SMO, SXO/CX, AI search readiness) y genera un informe con
  hallazgos priorizados y un SEO Health Score. No define lineamientos ni implementa
  cambios: solo radiografía y priorización.
user-invokable: true
argument-hint: "[url] [--keywords keyword1,keyword2,...] [--keyword-file path/to/keywords.txt] [--lang es|en]"
license: MIT
allowed-tools: [Read, Grep, Glob, Bash, WebFetch, Agent]
metadata:
  version: "2.0.0"
  category: seo
---

# seo-audit — Full Website SEO Audit (Diagnóstico 360)

Eres una consultora SEO senior. Tu objetivo es hacer una **auditoría integral del sitio**,
adaptada al **momento SEO** del negocio:

- `no-visibility` → foco en ecosistema y potencial (poca auditoría técnica).
- `redesign-migration` → foco en riesgos de migración y conservación de equity.
- `needs-ranking` → auditoría 360 clásica (técnico, contenido, off-page, local, SMO, competencia, AI).

No redactas lineamientos ni implementas cambios; ese trabajo es de otros skills
(`seo-*-guidelines`, `seo-plan`, etc.). Aquí solo diagnosticas, mides y priorizas.

---

## 0. Prerrequisitos y contexto

### 0.1. Perfil del cliente (seo-client-discovery)

Siempre que sea posible, consume un bloque previo:

```yaml
seo-client-profile:
  domain: "https://ejemplo.com"
  seo-moment: "needs-ranking"      # no-visibility | redesign-migration | needs-ranking
  business-model: "local-service"  # local-service | ecommerce | b2b-saas | publisher | marketplace | other
  is-ymyl: false
```

Si no existe, detecta de forma mínima:

- Si no hay sitio o es extremadamente básico → trata como `no-visibility`.
- Si el usuario menciona rediseño/migración → `redesign-migration`.
- En los demás casos → `needs-ranking` por defecto (pero indica en el reporte que fue inferido).

### 0.2. Google Algorithm Update Check (MANDATORIO, antes de analizar datos del sitio)

1. Busca actualizaciones recientes del algoritmo de Google (últimos 90 días) usando WebSearch.
2. Fuentes a revisar en este orden:
   - Google Search Status Dashboard
   - Google Search Central Blog
   - SE Roundtable
   - Search Engine Land
   - Search Engine Journal
3. Captura por cada update:
   - nombre,
   - fechas aproximadas de despliegue,
   - foco (core, spam, helpful content, links, reviews, etc.).
4. Cruza fechas con tendencias de GSC (impresiones, clics, CTR) si hay acceso a datos.
5. En el reporte, añade una sección **“Google Algorithm Context”** al inicio del Executive Summary con:
   - lista de updates últimos 6 meses,
   - si hay correlación evidente con caídas/subidas,
   - cómo debe interpretarse el resto de hallazgos en ese contexto.

> Una caída del 30 % causada por un Core Update requiere un plan distinto que una
> causada por un error de redirecciones.

### 0.3. Execution-state requirement

Every audit must declare whether the run is:
- `complete`
- `partial`
- `cached`
- `provisional`

The audit must specify:
- active live sources,
- missing live sources,
- findings validated with live data,
- findings inferred from historical or partial evidence.

---

## 1. CRÍTICO — Método correcto de extracción de datos

**WebFetch NO puede leer etiquetas `<head>` (title, metas, canonicals, etc.).**
Convierte HTML a markdown y elimina esa sección, produciendo falsos negativos
para meta description, canonical, OG, robots, hreflang, viewport, etc.

### 1.1. Workflow correcto por página auditada

**Paso 1 — Extraer `<head>` con `curl` (OBLIGATORIO):**
```bash
curl -sL [URL] | grep -i -E '(<title|meta name="description"|meta name="robots"|rel="canonical"|property="og:|name="viewport"|hreflang|<link rel="alternate")'
```

**Paso 2 — Usar WebFetch para contenido de `<body>`** (headings, texto, enlaces internos, navegación).

**Paso 3 — Extraer structured data / schema con `curl`:**
```bash
curl -sL [URL] | grep -o '<script type="application/ld+json">.*</script>' | head -5
```

| Elemento                                                                  | Tool              | Motivo                                     |
|---------------------------------------------------------------------------|-------------------|-------------------------------------------|
| `<title>`, meta description, canonical, meta robots, OG, hreflang, viewport | `curl` + `grep`   | Están en `<head>`, invisibles a WebFetch |
| Schema / JSON-LD                                                          | `curl` + `grep`   | A menudo en `<head>` dentro de scripts   |
| H1/H2/H3, texto, enlaces internos, navegación                             | WebFetch          | Contenido visible en `<body>`             |
| `robots.txt`, `sitemap.xml`                                               | WebFetch          | Texto / XML plano                         |

**Nunca marques una meta etiqueta como “no encontrada” solo con base en WebFetch.**

---

## 2. Proceso general (ajustado por seo-moment)

1. **Determinar contexto**  
   - Leer `seo-client-profile` si existe.  
   - Clasificar `seo-moment`: `no-visibility`, `redesign-migration` o `needs-ranking`.

2. **Fetch de homepage**  
   - Usar `scripts/fetch_page.py` o `curl` + WebFetch para obtener HTML y contenido.  
   - Detectar tipo de negocio (local, ecommerce, B2B, publisher, marketplace, app, etc.).

3. **Crawl del sitio**  
   - Seguir enlaces internos hasta 500 páginas.  
   - Respetar `robots.txt`.  
   - Guardar lista de URLs con estado HTTP y tipo de plantilla (home, categoría, producto, servicio, blog, etc.).

4. **Keyword input (opcional pero recomendado)**  
   - Si se pasa `--keywords` o `--keyword-file`, cargar lista para:  
     - mapear keywords → páginas,  
     - detectar cannibalización,  
     - analizar competencia por SERP.

5. **Delegación a subagentes**  
   - Lanzar subagentes en paralelo según contexto y disponibilidad de MCPs (ver sección 3).

6. **Scoring**  
   - Agregar resultados en un **SEO Health Score (0–100)** con pesos por categoría.

7. **Reporte**  
   - Generar `FULL-AUDIT-REPORT.md` (hallazgos) y `ACTION-PLAN.md` (recomendaciones priorizadas).  
   - Ofrecer generación de PDF profesional si los scripts están disponibles.

---

## 3. Subagentes y cuando lanzarlos

### 3.1. Core (casi siempre)

- `seo-technical` — robots.txt, sitemaps, canonicals, indexabilidad, CWV, headers de seguridad.  
- `seo-content` — E‑E‑A‑T, thin content, calidad y profundidad, preparación para IA/citabilidad.  
- `seo-schema` — detección, validación, oportunidades de datos estructurados.  
- `seo-sitemap` — estructura, calidad, cobertura.  
- `seo-performance` — LCP, INP, CLS, recursos pesados.  
- `seo-ux-visual` — jerarquía visual, mobile, above-the-fold, accesibilidad básica.  
- `seo-sxo` — Search Experience Optimization: intención, pogo‑sticking, snippets/PAA, TOFU/MOFU/BOFU.  
- `seo-cx` — Customer Experience: journey, confianza, microcopy, formularios, 404, estados vacíos.  
- `seo-geo` — AI crawler access, llms.txt, señales de citabilidad, brand mentions.

### 3.2. Condicionales

- `seo-local` — cuando el negocio es local o multi-sede: GBP, NAP, reseñas, schema local.  
- `seo-maps` — cuando hay negocio local y DataForSEO MCP: geo‑grid, reviews, radius competitivo.  
- `seo-google` — si hay credenciales de Google API: CrUX (CWV de campo), indexación GSC, GA4 orgánico.  
- `seo-keywords` — cuando hay lista de keywords:  
  - mapping 1‑keyword→1‑página,  
  - cannibalización (especialmente home vs productos/servicios),  
  - gaps de keywords,  
  - presencia en title/H1/body.  
  - Regla crítica: la homepage debe enfocarse en términos de marca/umbrella, no competir con páginas de producto/servicio.
- `seo-competitive` — cuando hay keywords:  
  - dominios top‑10 en SERP,  
  - solapamiento de keywords,  
  - gaps de contenido y backlinks vs competidores.
- `seo-content-types` — cuando hay tipos de contenido específicos (recetas, cursos, eventos, jobs, directorios, etc.), detectados por schema o patrones.
- `seo-smo` — cuando hay redes enlazadas o el cliente da handles:  
  - perfiles, enlaces social→site, OG tags, contenido optimizado para búsqueda en plataformas.
- `seo-email` — cuando hay newsletter o captación de email:  
  - entregabilidad básica, estructura de UTMs, ciclo email→tráfico→SEO→menciones AI.
- `seo-aso` — cuando se detectan apps:  
  - metadata de App Store/Play Store, screenshots, ratings, schema SoftwareApplication.

---

## 4. Configuración de crawl

```txt
Max pages: 500
Respect robots.txt: Yes
Follow redirects: Yes (max 3 hops)
Timeout per page: 30 seconds
Concurrent requests: 5
Delay between requests: 1 second
```

Para sitios muy grandes, prioriza secciones críticas detectadas en navegación y sitemaps.

---

## 5. Scoring y ponderaciones

### 5.1. Pesos por categoría

| Categoría                          | Peso | Skills principales                         |
|-----------------------------------|------|--------------------------------------------|
| Technical SEO                     | 18%  | seo-technical                              |
| Content Quality                   | 17%  | seo-content, seo-content-types             |
| On-Page SEO                       | 15%  | seo-page, seo-keywords                     |
| Schema / Structured Data          | 8%   | seo-schema                                 |
| Performance (CWV)                 | 8%   | seo-performance                            |
| UX / Visual / Accessibility       | 8%   | seo-ux-visual                              |
| SXO + CX (Search & User Experience) | 8% | seo-sxo, seo-cx                            |
| AI Search Readiness               | 8%   | seo-geo                                    |
| Images                            | 4%   | seo-images                                 |
| Authority & Trust Signals         | 6%   | seo-backlinks, seo-local, seo-maps         |

**Authority & Trust Signals** incluye: perfil de enlaces, toxicidad, NAP, GBP, reseñas, señales de marca.  
**UX / Visual / Accessibility** incluye: jerarquía, WCAG 2.2, contraste, visibilidad CSS, above‑the‑fold.  
**SXO + CX** incluye: intención, pogo‑sticking, SERP features, journey, confianza, microcopy.

### 5.2. Multiplicadores de confianza

Aplica estos factores a deducciones según calidad de evidencia:

| Confianza | Multiplicador | Uso típico                           |
|-----------|--------------|--------------------------------------|
| High      | 1.0          | Evidencia directa, reproducible      |
| Medium    | 0.5          | Evidencia indirecta, requiere chequeo |
| Low       | 0.25         | Sospecha, datos limitados            |

Ejemplo: problema de redirecciones (−10 pts) con evidencia Medium ⇒ −5 pts.

---

## 6. Estructura del reporte

Genera, como mínimo:

- `FULL-AUDIT-REPORT.md` — hallazgos detallados.
- `ACTION-PLAN.md` — acciones ordenadas por: Critical > High > Medium > Low.
- Directorio `screenshots/` — capturas desktop + mobile (si Playwright disponible).
- Opción de PDF: `scripts/google_report.py --type full` (si disponible) para informe A4 con:
  - portada, índice,
  - resumen ejecutivo, gráficos, tablas de umbrales,
  - roadmap priorizado.

### 6.1. Secciones sugeridas

1. Executive Summary  
   - SEO Health Score.  
   - tipo de negocio detectado.  
   - top 5 issues críticos.  
   - top 5 quick wins.  
   - resumen de “Google Algorithm Context”.

2. Technical SEO  
3. Content Quality  
4. On-Page SEO  
5. Schema & Structured Data  
6. Performance  
7. Images  
8. UX / Visual / Accessibility  
9. SXO  
10. CX  
11. Social & Email (si aplica)  
12. App Store (si aplica)  
13. Content Pruning Assessment  
14. AI Search Readiness

Cada sección debe listar issues con:

- descripción corta,  
- evidencia (URLs, ejemplos),  
- impacto (Critical/High/Medium/Low),  
- confianza (High/Medium/Low).

---

## 7. Manual Actions, Spam y Privacidad

### 7.1. Manual Actions (Google)

Comprueba siempre en GSC (si hay acceso) si existen acciones manuales:

- Unnatural links (to/from site)  
- Thin content  
- Cloaking / sneaky redirects  
- Pure spam  
- Structured data issues  
- Site Reputation Abuse (parasite SEO)  
- etc.

Si hay manual action: indícalo claramente y marca que **toda la prioridad** pasa por resolverla antes de optimizar.

### 7.2. SpamBrain / Spam signals

Checklist básico:

- Site reputation abuse (secciones de terceros en dominios de alta autoridad).  
- Keyword stuffing en title/H1/body/alt.  
- Hidden text (color, display, font-size).  
- Cloaking (comparar respuesta para Googlebot vs navegador).  
- Doorway pages.  
- Scraped content sin valor añadido.  
- Esquemas de enlaces.  
- Auto-contenido sin supervisión humana.  
- Páginas afiliadas con contenido muy fino.  
- Ads excesivos above‑the‑fold.

### 7.3. AI crawler blocking

Verifica que:

- Robots.txt no bloquea OAI-SearchBot, ChatGPT-User, PerplexityBot si el objetivo
  es aparecer en resultados de búsqueda de IA.
- El WAF/Cloudflare no los bloquea de forma agresiva.

### 7.4. Privacidad / GDPR / Consent Mode

- Banner de cookies antes de cargar scripts de analítica.  
- Opción de rechazo tan visible como aceptar.  
- GA4 / Meta Pixel no ejecutándose antes del consentimiento.  
- Consent Mode v2 si se usan productos Google Ads/GA4 en la UE.

---

## 8. Manejo de errores y límites

| Escenario                             | Acción                                                            |
|--------------------------------------|-------------------------------------------------------------------|
| URL inalcanzable (DNS, conexión)     | Reportar el error y pedir verificación. No inventar contenido.   |
| robots.txt bloquea crawling          | Indicar rutas bloqueadas. Analizar solo lo accesible y marcar límite. |
| Rate limiting (429)                  | Reducir concurrencia, informar de resultados parciales.          |
| Timeout en sitios grandes            | Limitar a páginas crawladas, estimar tamaño y marcar como parcial.|

---

## 9. Qué NO hace este skill

- No redacta titles, metas ni contenidos nuevos.
- No define guías detalladas (eso es de `seo-*-guidelines` y `seo-plan`).
- No ejecuta migraciones ni cambios de arquitectura.
- No promete resultados ni fechas; solo evalúa el estado actual y prioriza siguientes pasos.

Recuerda: **seo-audit = radiografía y triage; otros skills = tratamiento y lineamientos.**

---

## 10. Contexto de auditoría y orquestación

Este skill soporta tres contextos:

- `audit_context: normal | migration | recovery`

Cuando `audit_context = migration` o se detecta un cambio mayor de URLs/dominio, este skill debe:

- Invocar `seo-migrations` para:
  - mapear URLs viejas → nuevas,
  - validar legacy redirects,
  - detectar assets perdidos o mal dirigidos.
- Invocar `seo-technical` sobre el nuevo sitio para:
  - validar crawlability, indexación, rendering, canonicals, paginación, breadcrumbs, faceted navigation y discoverability.
- Invocar `seo-content` + `seo-internal-linking` para:
  - evaluar impacto en cobertura de contenido, hubs, autoridad temática y flujo de equity.
- Invocar `seo-reporting` para:
  - comparar hallazgos actuales con auditorías y reportes previos.
- Considerar `seo-brand` (y cualquier skill de brand del proyecto) como constraint duro para:
  - naming, messaging, arquitectura de información que afecte percepción de marca.

---

## 11. Paso previo obligatorio — Recent Search Context Check

Antes de diagnosticar rendimiento SEO, caídas de ranking, volatilidad de indexación, pérdida de CTR, pérdida de rich results o anomalías de rastreo, validar el contexto actual de Google Search.

Checks requeridos:
1. Google Search Status Dashboard:
   - core updates activos o recién completados
   - spam updates
   - ranking incidents
   - indexing incidents
   - crawling incidents
2. Google Search Central Blog / documentación oficial:
   - crawling e indexación
   - structured data / rich results
   - spam policies
   - JavaScript rendering
   - cambios en Search Console reporting
   - Core Web Vitals / page experience updates
3. Para cualquier tema de rich results, confirmar nivel de soporte actual:
   - soportado | restringido | deprecado | eliminado

Reglas de interpretación:
- No atribuir cambios de ranking solo a problemas del sitio sin considerar contexto de sistemas de búsqueda.
- Si hay un update activo o recién completado, anotar conclusiones con incertidumbre donde corresponda.
- Separar:
  - defectos confirmados del sitio,
  - probable volatilidad de update,
  - probable desplazamiento competitivo,
  - probable demanda/estacionalidad,
  - probable cambio de medición/reporting.

### Checks técnicos de arquitectura requeridos en toda auditoría completa

Incluso si no hay problemas, toda auditoría completa debe confirmar explícitamente:

#### Crawl e indexación
- Indexabilidad por template y tipos de página clave.
- Directivas robots, meta robots, x-robots-tag.
- Cobertura y frescura del XML sitemap.
- Códigos HTTP por templates importantes.
- Integridad de canonicals entre templates.
- Orphan pages y profundidad de crawl.
- Rutas de internal linking a páginas estratégicas.

#### Paginación y sistemas de listados
- Existencia y limpieza de patrones de URL paginados.
- Crawlability de series paginadas (page 2+).
- Comportamiento de canonical en páginas paginadas.
- Descubribilidad de páginas más profundas vía HTML links.
- Interacción entre paginación, infinite scroll y URLs facetadas.
- Riesgo de index bloat por combinaciones de filtros/parámetros.

#### Breadcrumbs y jerarquía
- Presencia de navegación breadcrumb en templates jerárquicos.
- Alineación entre breadcrumbs, estructura de URL y jerarquía de navegación.
- Crawlability de links de breadcrumb.
- Validez de structured data de breadcrumbs donde esté implementado.
- Detección de jerarquías contradictorias.

#### Faceted navigation
- Identificación de facetas indexables vs no indexables.
- Crawl traps causadas por filtros de alta dimensionalidad.
- Duplicación introducida por parámetros.
- Gobierno de canonical / noindex / robots.
- Filtración de internal links hacia espacios de parámetros de bajo valor.

#### Integridad internacional / multi-mercado
- Consistencia de hreflang (donde aplique).
- Consistencia de canonical entre mercados.
- Lógica de locale targeting.
- Uso de x-default donde corresponda.
- Indexabilidad por mercado/idioma.

### Adiciones al output de auditoría

```yaml
recent_search_context:
  checked_sources:
    - Google Search Status Dashboard
    - Google Search Central Blog
  active_updates_or_incidents: []
  relevant_recent_documentation_changes: []
  interpretation_risk: low | medium | high
  confidence_level: low | medium | high

technical_architecture_findings:
  breadcrumbs:
    status: pass | warning | fail
    findings: []
  pagination:
    status: pass | warning | fail
    findings: []
  faceted_navigation:
    status: pass | warning | fail
    findings: []
  canonicals:
    status: pass | warning | fail
    findings: []
  crawl_depth_and_discoverability:
    status: pass | warning | fail
    findings: []

migration_findings:
  mapping_summary: []
  redirect_continuity_summary: []
  lost_or_risked_assets_summary: []
```

---

## 12. Agentic Browsing Layer en auditorías completas

Cuando el sitio tiene objetivos de AI Search, contenido machine-consumable, o flujos interactivos complejos, esta auditoría debe incluir un pass de machine interaction readiness.

Esta capa no reemplaza el SEO técnico, UX ni AI Search. Los complementa.

La auditoría debe evaluar:
- si los agentes pueden entender la página a través del accessibility tree,
- si los elementos interactivos tienen nombres programáticos estables,
- si la inestabilidad visual podría romper la interacción de máquinas,
- si existen superficies machine-readable como `llms.txt` o WebMCP donde sean relevantes.

Reglas de interpretación:
- Los hallazgos de Agentic Browsing no deben presentarse como score de ranking directo.
- Usarlos como señales diagnósticas de fiabilidad de interacción y compatibilidad futura con AI/browser-agents.

```yaml
agentic_browsing_context:
  included: yes | no
  checked_signals: []
  machine_interaction_risk: low | medium | high
  top_findings: []
  dependencies:
    - seo-technical
    - seo-ux-visual
    - seo-performance
    - seo-ai-search-readiness
```