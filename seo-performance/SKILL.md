---
name: seo-performance
description: >
  Core Web Vitals measurement, diagnosis, and optimization. Covers LCP, INP, CLS with
  lab vs field data comparison, root cause analysis per metric, resource optimization
  (render-blocking, JS, CSS, images, fonts, third-party scripts), and fix prioritization.
  Use when user says "performance", "Core Web Vitals", "CWV", "LCP", "INP", "CLS",
  "page speed", "site speed", "lento", "velocidad", or "PageSpeed Insights".
  Not for crawl errors, redirects, or indexation issues — use seo-technical.
user-invokable: true
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "2.0.0"
  category: seo
---

# Core Web Vitals — Medición, Diagnóstico y Optimización

Mide, diagnostica y prioriza fixes de rendimiento web. Los CWV son factores de ranking confirmados por Google desde 2021 y se evalúan con datos de campo reales (CrUX), no lab.

## CRITICAL: Lab Data ≠ Field Data

| Tipo | Fuente | Qué mide | Cuándo usarlo |
|------|--------|----------|---------------|
| **Field data** | CrUX (75th percentil usuarios reales) | Experiencia real de tus usuarios | Siempre — es lo que Google usa para ranking |
| **Lab data** | Lighthouse / PageSpeed Insights | Condiciones controladas simuladas | Diagnóstico y debugging — no refleja ranking |

> Un Lighthouse score de 95 no garantiza buenos CWV en campo. Siempre contrastar con CrUX.

---

## Umbrales CWV 2026 (75th percentile)

| Métrica | Good | Needs Improvement | Poor |
|---------|------|-------------------|------|
| **LCP** (Largest Contentful Paint) | ≤ 2.5s | 2.5s – 4.0s | > 4.0s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | 200ms – 500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | 0.1 – 0.25 | > 0.25 |

> **FID fue eliminado el 12 de marzo de 2024.** INP es su reemplazo. No referenciar FID.

> **⚠️ Verificación de umbrales:** Múltiples fuentes de baja calidad afirman cambios (LCP a 2.0s, INP a 150ms) — NO confirmado. Fuente canónica: [web.dev/articles/vitals](https://web.dev/articles/vitals). Verificar antes de reportar a cliente.

> **INP percentil two-level:** INP = p98 de todas las interacciones *por visita individual*. CrUX luego reporta el p75 de esos valores INP de todas las visitas. Esta doble agregación confunde a practitioners — el valor que ves en CrUX es p75(p98), no p98 del total de interacciones.

---

## Paso 1 — Medir

### Field Data (CrUX)
```bash
# PageSpeed Insights API (field + lab)
curl "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=URL&strategy=mobile&key=API_KEY"

# CrUX API (solo field data, por URL o dominio)
curl -X POST "https://chromeuxreport.googleapis.com/v1/records:queryRecord?key=API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com", "formFactor": "PHONE"}'
```

### Lab Data (Lighthouse)
```bash
# Via DataForSEO MCP (si disponible)
# on_page_lighthouse → score por categoría + audits

# Via Google API (seo-google skill)
# PageSpeed Insights con estrategia mobile y desktop
```

### Extracción rápida sin API
```bash
# Verificar si el LCP element está en el HTML inicial (no JS-rendered)
curl -sL URL | grep -i -E '(fetchpriority|loading="eager"|<img|<video|background-image)'

# Detectar recursos render-blocking
curl -sL URL | grep -i -E '(<script(?!.*defer|.*async)|<link rel="stylesheet")'

# Verificar preconnect y preload
curl -sL URL | grep -i -E '(preconnect|preload|dns-prefetch)'
```

---

## LCP — Largest Contentful Paint

El LCP mide cuándo el elemento más grande visible en el viewport termina de renderizar.

### Identificar el elemento LCP
El LCP suele ser:
- Hero image (`<img>` o `background-image`)
- Texto H1 o bloque de texto grande
- Video poster frame

```bash
# Verificar si el LCP candidate está en el HTML inicial
curl -sL URL | grep -i -E '<img|<h1' | head -5
# Si el hero solo aparece después de JS → LCP penalizado
```

### Árbol de diagnóstico LCP

```
LCP > 2.5s
├── TTFB lento (> 600ms)?
│   ├── Sí → problema de servidor/CDN/HTTP3 → ver seo-cdn, seo-server (¿HTTP/3 habilitado?)
│   └── No → problema de render
│       ├── LCP element bloqueado por render-blocking resources?
│       │   ├── Sí → eliminar/diferir CSS y JS bloqueantes
│       │   └── No → LCP element se carga tarde?
│       │       ├── Imagen sin fetchpriority="high" → añadir atributo
│       │       ├── Imagen lazy loaded → cambiar a loading="eager"
│       │       ├── Imagen en CSS background-image → mover a <img>
│       │       └── Imagen cargada por JS → prerenderizar en SSR/SSG
```

### Fixes LCP por causa

| Causa | Fix | Impacto |
|-------|-----|---------|
| TTFB alto | CDN, server-side cache, hosting upgrade, HTTP/3 (QUIC) | Alto |
| Render-blocking CSS | Inline critical CSS, diferir el resto | Alto |
| Render-blocking JS | `defer` / `async` en scripts no críticos | Alto |
| LCP image sin prioridad | `<img fetchpriority="high" loading="eager">` | Alto |
| LCP image en CSS | Mover a `<img>` en HTML | Medio |
| LCP image sin preload | `<link rel="preload" as="image" href="..." fetchpriority="high">` | Medio |
| Fuente bloqueando texto | `font-display: optional` o `swap` | Medio |
| LCP cargado por JS | SSR/SSG para que esté en HTML inicial | Alto |

### Código correcto para LCP image
```html
<!-- ✅ Correcto: alta prioridad, en HTML inicial, dimensiones explícitas -->
<img
  src="/hero.webp"
  alt="Descripción del hero"
  width="1200"
  height="600"
  fetchpriority="high"
  loading="eager"
  decoding="async"
/>

<!-- ❌ Incorrecto: lazy loaded, sin prioridad -->
<img src="/hero.webp" loading="lazy" />
```

---

## INP — Interaction to Next Paint

INP mide la latencia de las interacciones del usuario (clics, taps, pulsaciones de teclado). Reemplazó a FID en marzo 2024.

> **Diferencia clave con FID:** FID medía solo el primer input. INP mide TODAS las interacciones durante la visita completa y reporta el percentil 98.

### Diagnóstico INP

INP alto = main thread bloqueado durante o después de la interacción.

```
INP > 200ms
├── Input delay alto (> 50ms)?
│   → Main thread ocupado en el momento del click
│   → Causa: JS de terceros, analytics, ads, hydration
│   → Fix: diferir scripts no críticos, reducir trabajo en main thread
│
├── Processing time alto (> 100ms)?
│   → Event handler demasiado pesado
│   → Causa: re-renders React/Angular, DOM mutations masivas
│   → Fix: debounce, virtualización, web workers
│
└── Presentation delay alto (> 50ms)?
    → Rendering pipeline bloqueada
    → Causa: forzar layout/reflow (thrashing), animaciones en main thread
    → Fix: CSS animations en lugar de JS, `transform` en lugar de `top/left`
```

### Causas comunes INP alto

| Causa | Fix |
|-------|-----|
| Scripts de terceros (ads, chat, analytics) bloqueando main thread | Cargar con `async`/`defer`, mover a Web Worker |
| React re-renders masivos en cada interacción | `useMemo`, `useCallback`, `React.memo` |
| Angular change detection en cada evento | `OnPush` strategy, `NgZone.runOutsideAngular` |
| Event handlers con trabajo pesado | Debounce, dividir en tareas con `scheduler.yield()` |
| Animaciones con JS (top/left) | Migrar a `transform`/`opacity` (GPU accelerated) |
| Long Tasks (> 50ms) bloqueando main thread | Dividir con `setTimeout(fn, 0)` o `scheduler.postTask` |

```bash
# Detectar scripts de terceros que pueden bloquear INP
curl -sL URL | grep -i -E '<script.*src=' | grep -v 'defer\|async' | head -10
```

### Web Worker pattern para INP
```javascript
// ✅ Mover cálculos pesados a Web Worker para liberar main thread
// worker.js
self.onmessage = ({ data }) => {
  const result = heavyComputation(data); // sorting, filtering, parsing
  self.postMessage(result);
};

// main.js — event handler liviano
button.addEventListener('click', () => {
  worker.postMessage(inputData);
  worker.onmessage = ({ data }) => updateUI(data);
});
```

### scheduler.yield() — API moderna para dividir long tasks
```javascript
// ✅ Ceder al main thread entre bloques de trabajo (reemplaza setTimeout hacks)
async function processItems(items) {
  for (const item of items) {
    doWork(item);
    await scheduler.yield(); // permite al browser procesar eventos pendientes → mejor INP
  }
}
// Fallback para browsers sin soporte:
const yieldToMain = () => scheduler?.yield?.() ?? new Promise(r => setTimeout(r, 0));
```

### content-visibility: auto — rendering performance
```css
/* ✅ Browser skips rendering of off-screen sections until scroll */
.below-fold-section {
  content-visibility: auto;
  contain-intrinsic-size: auto 500px; /* placeholder height para evitar CLS */
}
/* Impacto: reduce initial render time significativamente en páginas largas */
```

---

## CLS — Cumulative Layout Shift

CLS mide la inestabilidad visual: cuánto se mueven los elementos durante la carga.

### Causas más comunes de CLS

| Causa | Fix |
|-------|-----|
| Imágenes sin dimensiones explícitas | Siempre incluir `width` y `height` en `<img>` |
| Ads / embeds sin espacio reservado | Contenedor con altura mínima fija |
| Fuentes web causando FOUT | `font-display: optional`; o preload + `font-display: swap` con `size-adjust` |
| Contenido inyectado dinámicamente arriba del fold | Insertar desde abajo o reservar espacio |
| Animaciones sin `transform` | Usar `transform: translateY()` en lugar de cambiar `top` |
| Lazy hydration que desplaza contenido | Reservar espacio antes de hidratar |

### Código correcto para imágenes
```html
<!-- ✅ Dimensiones explícitas → sin CLS -->
<img src="imagen.webp" width="800" height="450" alt="..." loading="lazy" />

<!-- Para imágenes responsive -->
<img
  src="imagen.webp"
  srcset="imagen-400.webp 400w, imagen-800.webp 800w"
  sizes="(max-width: 600px) 400px, 800px"
  width="800"
  height="450"
  alt="..."
/>

<!-- ✅ Chrome 136+: sizes="auto" para lazy-loaded images (calcula tamaño automáticamente) -->
<img src="imagen.webp" srcset="..." sizes="auto" loading="lazy" width="800" height="450" alt="..." />
```

> **⚠️ `loading="lazy"` y LCP:** Chrome ajustó el threshold de distancia al viewport para cargar lazy images. Si el hero image tiene `loading="lazy"` por error, el LCP se penaliza severamente. **Nunca** lazy-load el LCP element — usar `loading="eager"` + `fetchpriority="high"`.

### Reservar espacio para ads/embeds
```css
/* ✅ Contenedor con aspect-ratio → sin CLS */
.ad-container {
  aspect-ratio: 970 / 250;  /* Billboard */
  width: 100%;
}

.embed-container {
  aspect-ratio: 16 / 9;
  width: 100%;
}
```

---

## Recursos Render-Blocking

### CSS render-blocking
Todo CSS en `<head>` bloquea el renderizado por defecto.

```bash
# Detectar stylesheets bloqueantes
curl -sL URL | grep -i '<link rel="stylesheet"' | head -10
```

**Estrategia Critical CSS:**
```html
<!-- ✅ Critical CSS inline, resto diferido -->
<style>/* Solo CSS above-fold: ~14KB máximo */</style>
<link rel="preload" href="/styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="/styles.css"></noscript>
```

### JS render-blocking
```bash
# Detectar JS sin defer/async
curl -sL URL | grep -i '<script' | grep -v 'defer\|async\|type="module"' | head -10
```

| Atributo | Comportamiento | Cuándo usar |
|----------|---------------|-------------|
| Sin atributo | Bloquea parsing + ejecución inmediata | Nunca (salvo inline crítico) |
| `async` | Descarga en paralelo, ejecuta al terminar | Analytics, ads independientes |
| `defer` | Descarga en paralelo, ejecuta tras parsing | Scripts que necesitan DOM |
| `type="module"` | Diferido por defecto | ES modules |

### Fuentes web
```bash
# Verificar preload de fuentes
curl -sL URL | grep -i 'preload.*font\|font.*preload'
```

```html
<!-- ✅ Preload fuente principal -->
<link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossorigin>

<!-- ✅ font-display para evitar FOIT/FOUT -->
<style>
@font-face {
  font-family: 'Main';
  src: url('/fonts/main.woff2') format('woff2');
  font-display: optional; /* No espera: usa fuente del sistema si no carga a tiempo */
}
</style>
```

---

## Scripts de Terceros

Los scripts de terceros (analytics, chat, ads, tag managers) son frecuentemente el mayor problema de INP y LCP.

```bash
# Detectar scripts de terceros
curl -sL URL | grep -i '<script.*src=' | grep -v "$(echo URL | sed 's|https://||;s|/.*||')" | head -20
```

### Impacto por tipo de script

| Script | Impacto típico | Mitigación |
|--------|---------------|-----------|
| Google Tag Manager | Alto (carga N scripts adicionales) | Auditar tags innecesarios, activar firing rules |
| Intercom / Drift (chat) | Alto INP + CLS | Cargar tras `load` event, no en `<head>` |
| Google Ads / AdSense | Alto CLS + bloqueo | Reservar espacio, cargar async |
| Hotjar / Clarity | Medio INP | Cargar después de interacción o tras 3s |
| Facebook Pixel | Medio | `async` + diferir carga |
| YouTube embed | Alto LCP si above-fold | Lite-youtube-embed o facade |
| **Consent banners (OneTrust, Cookiebot, CookieYes)** | Alto INP + CLS | Cargar async, reservar espacio fijo para banner, evitar overlays que bloqueen interacción |

### Facade pattern para embeds pesados
```html
<!-- ✅ YouTube facade: carga el iframe solo al hacer clic -->
<lite-youtube videoid="VIDEO_ID" playlabel="Ver video"></lite-youtube>
<!-- Lib: github.com/paulirish/lite-youtube-embed -->
```

---

## Modern Performance APIs (2025-2026)

### Speculation Rules API — prerender/prefetch instantáneo
```html
<!-- En <head> o inline script — Chrome 121+ (2024), amplio soporte 2026 -->
<script type="speculationrules">
{
  "prerender": [{
    "where": { "href_matches": "/products/*" },
    "eagerness": "moderate"
  }],
  "prefetch": [{
    "where": { "selector_matches": "a[href^='/blog']" },
    "eagerness": "conservative"
  }]
}
</script>
```
- **Prerender**: carga y renderiza la página completa en background → navegación instantánea (~0ms)
- **Prefetch**: solo descarga el HTML → navegación rápida (~200ms en lugar de 1-3s)
- **Eagerness levels**: `immediate` (al cargar), `eager` (pronto), `moderate` (hover 200ms), `conservative` (click/mousedown)
- **Impacto SEO**: mejora dramáticamente perceived performance sin afectar crawling
- **Auditoría**: verificar con `chrome://speculation-rules-internals/`

### Early Hints (103)
```
HTTP/1.1 103 Early Hints
Link: </style.css>; rel=preload; as=style
Link: <https://cdn.example.com>; rel=preconnect

HTTP/1.1 200 OK
...
```
- El servidor envía hints de recursos críticos **antes** de terminar de procesar la respuesta
- Reduce LCP al permitir al browser descargar CSS/fonts mientras el server piensa
- Soportado por Cloudflare, Fastly, y la mayoría de CDNs modernos
- **Cuándo usarlo**: TTFB > 400ms y LCP bloqueado por recursos render-blocking

### HTTP/3 (QUIC) — TTFB reduction
- Adopción global: ~35% del tráfico web (2026)
- **25% faster downloads promedio**, 52% más rápido en mobile con conexiones inestables
- Elimina head-of-line blocking de HTTP/2
- Verificar soporte: `curl -I --http3 https://example.com` o DevTools → Protocol column
- **Acción**: habilitar en CDN (Cloudflare: automático; AWS CloudFront: config needed)

---

## Lighthouse v13 (Oct 2025) — Cambios en auditorías

La estructura de auditorías cambió significativamente:
- **v12.6** (Chrome 137, May 2025): introdujo "performance insight audits"
- **v12.7**: nuevo formato como default
- **v13** (Oct 2025): retiró auditorías legacy completamente

### Nueva estructura:
| Antes (legacy) | Ahora (v13) |
|----------------|-------------|
| Auditorías individuales sueltas | **Insights** (agrupadas por área: "CLS Culprits", "Image Delivery", "Render Blocking") |
| Lista plana de oportunidades | **Diagnostics** (datos técnicos sin juicio de impacto) |

> **Impacto CI/CD**: Si tu pipeline referencia audit IDs específicos de Lighthouse (ej. `largest-contentful-paint-element`), verificar que siguen existiendo en v13.

---

## CrUX Dashboard → CrUX Vis (Nov 2025)

- **CrUX Dashboard fue deprecado** después de noviembre 2025
- **Reemplazo**: CrUX Vis (cruxvis.withgoogle.com)
  - Datos semanales (vs mensuales del dashboard antiguo)
  - URL-level data (no solo origen)
  - Historial de 40 semanas
- CrUX API sigue siendo la fuente canónica para integraciones automatizadas

---

## Checklist por Prioridad

### 🔴 Crítico (fix inmediato)
- [ ] LCP > 4s en móvil (field data)
- [ ] INP > 500ms en móvil (field data)
- [ ] CLS > 0.25 (field data)
- [ ] LCP element cargado por JS (no en HTML inicial)
- [ ] Imágenes principales sin dimensiones → CLS masivo

### 🟡 Alto (fix en 1 semana)
- [ ] LCP entre 2.5s – 4s
- [ ] INP entre 200ms – 500ms
- [ ] CSS/JS render-blocking en above-fold
- [ ] LCP image sin `fetchpriority="high"`
- [ ] Fuentes sin preload causando FOIT
- [ ] Scripts de terceros sin async/defer

### 🟢 Medio (fix en 1 mes)
- [ ] TTFB > 600ms (problema de servidor/CDN)
- [ ] Long Tasks > 50ms en main thread
- [ ] Ads/embeds sin espacio reservado
- [ ] `font-display: block` en fuentes no críticas
- [ ] YouTube/Vimeo sin facade pattern

---

## Output Format

### Performance Score: XX/100

| Métrica | Field Data (CrUX) | Lab Data (Lighthouse) | Estado |
|---------|------------------|----------------------|--------|
| LCP | X.Xs | X.Xs | 🔴/🟡/🟢 |
| INP | XXXms | XXXms | 🔴/🟡/🟢 |
| CLS | X.XX | X.XX | 🔴/🟡/🟢 |
| TTFB | X.Xs | — | 🔴/🟡/🟢 |

> Field data: 75th percentile, móvil. Lab data: Lighthouse simulado, throttled 4G.

### Root Causes Identificados

| Causa | Métrica afectada | Impacto | Esfuerzo fix |
|-------|-----------------|---------|-------------|
| [Causa] | LCP/INP/CLS | Alto/Medio/Bajo | Bajo/Medio/Alto |

### Plan de Acción

| Prioridad | Fix | Métrica | Ganancia estimada |
|-----------|-----|---------|------------------|
| 🔴 | [Fix] | [Métrica] | [Estimación] |
| 🟡 | [Fix] | [Métrica] | [Estimación] |

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| CDN y TTFB | `/seo cdn [url]` | Edge caching, provider config, TTFB reduction |
| Datos reales de Google (CrUX) | `/seo google pagespeed [url]` | Field CWV desde la API de Google |
| Imágenes y CLS | `/seo images [url]` | Dimensiones, formatos, lazy loading |
| JS rendering (React/Angular) | `/seo technical [url]` | Hydration, bundle size, SSR |
| Live data (Lighthouse API) | `/seo dataforseo [domain]` | on_page_lighthouse via DataForSEO MCP |

## Error Handling

| Escenario | Acción |
|-----------|--------|
| Sin CrUX data (tráfico bajo) | Usar solo lab data. Indicar que el ranking CWV requiere suficiente tráfico para CrUX. Recomendar aumentar tráfico antes de re-evaluar. |
| URL inaccesible | Reportar error. No inferir rendimiento. |
| Sitio detrás de auth | Analizar páginas públicas. Indicar limitación para páginas privadas. |
| Sin API key | Usar WebFetch a PageSpeed Insights web UI o curl sin key (límite de rate apply). |
