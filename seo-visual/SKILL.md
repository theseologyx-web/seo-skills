---
name: seo-visual
description: >
  Visual rendering audit: captures screenshots (desktop + mobile), tests mobile rendering,
  analyzes above-fold content, detects rendering issues, and compares visual vs crawled content.
  Use when user says "screenshot", "captura de pantalla", "mobile rendering", "renderizado",
  "above the fold", "primera impresión visual", "cómo se ve", or "visual check".
  Not for UX analysis or conversion — use seo-ux-visual or seo-cro.
user-invokable: true
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# Visual Rendering Audit

Captura y analiza cómo se renderiza una página visualmente: desktop, mobile, above-fold, y diferencias entre lo que ve el usuario vs. lo que crawlea Google.

> Para análisis de diseño visual (jerarquía, tipografía, WCAG, branding): usar `seo-ux-visual`.
> Este skill se enfoca en **captura técnica, rendering y comparación HTML vs visual**.

---

## Herramientas por Disponibilidad

| Herramienta | Disponible cuando | Qué hace |
|-------------|-----------------|---------|
| **Playwright** | Instalado en sistema | Screenshot real, JS ejecutado, interacciones |
| **Firecrawl MCP** | MCP configurado | Screenshot + HTML renderizado |
| **curl + WebFetch** | Siempre | HTML sin JS, análisis de estructura |
| **seo-crawler** | MCP Firecrawl | HTML Cloudflare-rendered (como Googlebot) |

**Prioridad:** Playwright > Firecrawl > WebFetch.

---

## 1. Captura de Screenshots

### Con Playwright (si disponible)
```bash
# Verificar si Playwright está instalado
npx playwright --version 2>/dev/null || echo "No instalado"

# Screenshot desktop (1280x720)
npx playwright screenshot --browser chromium --viewport-size "1280,720" URL screenshots/desktop.png

# Screenshot mobile (iPhone 12 viewport)
npx playwright screenshot --browser chromium --viewport-size "390,844" --device-scale-factor 3 URL screenshots/mobile.png

# Screenshot above-fold únicamente (full-page=false)
npx playwright screenshot --browser chromium --viewport-size "390,844" --no-full-page URL screenshots/mobile-fold.png
```

### Con Firecrawl MCP (si disponible)
```
firecrawl_scrape con screenshot: true → devuelve screenshot base64
```

### Sin herramientas de rendering
Indicar al usuario que la captura de pantalla no es posible sin Playwright o Firecrawl. Analizar el HTML para inferir la experiencia visual.

---

## 2. Análisis Above-the-Fold

El above-the-fold es lo que el usuario ve SIN hacer scroll. Es el factor crítico para:
- Bounce rate (¿el usuario entiende qué es el sitio en < 3 segundos?)
- LCP (el elemento LCP casi siempre está above-the-fold)
- Google Page Experience signals

### Viewports de referencia (2025)
| Dispositivo | Viewport | % del tráfico |
|-------------|---------|---------------|
| Mobile S (iPhone SE) | 375 × 667 | ~8% |
| Mobile M (iPhone 14) | 390 × 844 | ~22% |
| Mobile L (iPhone Pro Max) | 430 × 932 | ~15% |
| Tablet | 768 × 1024 | ~8% |
| Desktop | 1280 × 720 | ~35% |
| Desktop large | 1440 × 900 | ~12% |

> **Diseñar para 390px móvil y 1280px desktop** cubre el 80%+ del tráfico.

### Checklist above-the-fold
- [ ] La propuesta de valor (qué hace el sitio) es visible sin scroll en móvil
- [ ] El H1 es visible above-the-fold
- [ ] El CTA principal es visible sin scroll
- [ ] El logo/marca es reconocible en el header
- [ ] El elemento LCP (hero image o H1) está above-the-fold
- [ ] No hay popups/interstitials cubriendo el contenido al cargar
- [ ] El cookie banner no cubre más del 20% del viewport
- [ ] No hay ads above-the-fold que desplacen el contenido principal

---

## 3. Mobile Rendering Check

### Verificar viewport meta (CRÍTICO)
```bash
curl -sL URL | grep -i 'viewport'
# ✅ Correcto: content="width=device-width, initial-scale=1"
# ❌ Problema: content="width=1024" o ausente → Google penaliza
```

### Detectar problemas de rendering móvil
```bash
# Verificar si hay scroll horizontal (overflow)
curl -sL URL | grep -i 'overflow\|max-width\|width:.*px' | head -10

# Detectar elementos con ancho fijo que rompen el layout móvil
curl -sL URL | grep -i 'style=' | grep -i 'width:[[:space:]]*[0-9]\{4,\}px'

# Verificar touch targets y tap areas
curl -sL URL | grep -i -E '(<a |<button)' | head -20
```

### Problemas comunes de rendering móvil

| Problema | Síntoma | Fix |
|----------|---------|-----|
| Sin viewport meta | Google penaliza en mobile usability | Añadir `<meta name="viewport">` |
| Elementos con ancho fijo | Scroll horizontal en móvil | Reemplazar px fijo por % o max-width |
| Fuente < 16px en body | Texto ilegible sin zoom | Aumentar a mínimo 16px |
| Tap targets < 44px (iOS) / 48px (Android) | Links y botones no clicables en móvil | Aumentar padding |
| Contenido solo en hover | Invisible en touch (no hay hover en móvil) | Proporcionar alternativa tap |
| Tablas sin scroll horizontal | Rompen layout | `overflow-x: auto` en contenedor |
| Imágenes sin `max-width: 100%` | Overflow del contenedor | CSS global `img { max-width: 100% }` |

---

## 4. Comparación Visual vs. HTML Crawlado

Detectar diferencias entre lo que ve el usuario y lo que ve Googlebot.

### Test de cloaking básico
```bash
# Lo que ve el usuario (Chrome normal)
curl -sL -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36" URL | wc -w

# Lo que ve Googlebot (usa Chrome moderno, no Googlebot/2.1 standalone)
curl -sL -A "Mozilla/5.0 (Linux; Android 6.0.1; Nexus 5X Build/MMB29P) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.204 Mobile Safari/537.36 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" URL | wc -w

# Diferencia significativa (> 20%) → posible cloaking o content inyectado por JS
```

### Contenido visible vs. contenido indexable
```bash
# HTML inicial (lo que recibe el crawler sin ejecutar JS)
curl -sL URL > /tmp/raw_html.txt
wc -w /tmp/raw_html.txt

# Verificar si el contenido principal está en el HTML inicial
curl -sL URL | grep -i -E '<h1|<h2|<p' | head -20
```

**Si el contenido principal no está en el HTML inicial:**
- Googlebot (wave 1) no lo ve en la primera visita
- Depende del rendering presupuesto de Google (wave 2)
- Riesgo: indexación incompleta o retraso de semanas

### Elementos que DEBEN estar en el HTML inicial (no solo en JS)
- [ ] H1 de la página
- [ ] Meta title y meta description
- [ ] Canonical tag
- [ ] Schema/JSON-LD principal
- [ ] Texto del cuerpo principal (primeros 2-3 párrafos)
- [ ] Internal links de navegación
- [ ] El elemento LCP (imagen o texto hero)

---

## 5. Detección de Problemas Visuales por HTML

Cuando no hay herramienta de screenshot, inferir problemas visuales desde el HTML:

```bash
# Detectar elementos ocultos que podrían ser contenido principal
curl -sL URL | grep -i -E '(display:none|visibility:hidden|opacity:0)' | head -10

# Detectar texto en imágenes (burns text — invisible para Google)
curl -sL URL | grep -i '<img' | grep -i -E '(titulo|title|banner|hero|cta|boton)' | head -5

# Detectar modales/overlays que pueden bloquear contenido
curl -sL URL | grep -i -E '(modal|overlay|popup|interstitial)' | head -5

# Verificar que el menú de navegación está en HTML (no solo JS)
curl -sL URL | grep -i -E '<nav|<ul|<li.*<a' | head -10
```

### Señales de problemas visuales detectables sin screenshot

| Señal en HTML | Problema probable |
|--------------|------------------|
| `display:none` en H1 o contenido principal | Contenido oculto para crawlers |
| `<img>` con alt que describe texto ("banner precio") | Texto quemado en imagen |
| `<div class="modal">` early in DOM | Popup posiblemente bloqueante |
| Sin `<nav>` o links de navegación en HTML | Menú solo en JS → no crawleable |
| `position:fixed; top:0; width:100%` sin z-index check | Header puede cubrir contenido |
| Sin `<main>` o `<article>` semántico | Estructura invisible para screen readers + Google |

---

## 6. Comparación Desktop vs. Mobile (sin screenshot)

```bash
# Verificar media queries para responsive design
curl -sL URL | grep -i -E '@media|max-width|min-width' | head -10

# Detectar elementos solo visibles en desktop (hidden-xs, d-none d-md-block, etc.)
curl -sL URL | grep -i -E '(hidden-xs|hidden-sm|d-none|sm:hidden|mobile-hidden)' | head -5

# Detectar si hay versión AMP (mobile alternativa)
curl -sL URL | grep -i -E '(amphtml|<html amp|<html ⚡)' | head -3
```

---

## 7. Renderizado en Redes Sociales (Open Graph)

Cuando se comparte en redes, el scraper social NO ejecuta JS. La imagen OG debe estar en el HTML.

```bash
# Verificar OG tags en HTML inicial
curl -sL URL | grep -i -E '(og:image|og:title|og:description|twitter:image|twitter:card)'
```

| OG tag | Dimensión óptima | Crítico para |
|--------|-----------------|-------------|
| `og:image` | 1200 × 630px | Facebook, LinkedIn, WhatsApp |
| `twitter:image` | 1200 × 628px | X (Twitter) |
| `og:image` cuadrado | 1200 × 1200px | Instagram cuando se comparte link |

**Si los OG tags se generan por JS:** scrapers sociales no los ven. Requiere SSR o meta tags en HTML estático.

---

## Output Format

### Visual Rendering Report

**URL analizada:** [URL]
**Fecha:** [Fecha]
**Herramientas usadas:** Playwright / Firecrawl / curl (indicar cuál)

#### Screenshots capturados
- Desktop (1280×720): `screenshots/desktop.png`
- Mobile (390×844): `screenshots/mobile.png`
- Above-fold mobile: `screenshots/mobile-fold.png`

#### Above-the-Fold Analysis
| Elemento | Desktop | Mobile | Estado |
|----------|---------|--------|--------|
| H1 visible | ✅/❌ | ✅/❌ | |
| CTA visible | ✅/❌ | ✅/❌ | |
| Propuesta de valor clara | ✅/❌ | ✅/❌ | |
| Sin popups bloqueantes | ✅/❌ | ✅/❌ | |

#### Mobile Rendering Issues

| Issue | Severidad | Fix |
|-------|-----------|-----|
| [Issue] | 🔴/🟡/🟢 | [Fix] |

#### Contenido en HTML inicial vs. JS-only

| Elemento | En HTML inicial | Solo en JS | Riesgo indexación |
|----------|----------------|-----------|------------------|
| H1 | ✅/❌ | ❌/✅ | Alto/Bajo |
| Meta tags | ✅/❌ | ❌/✅ | Alto/Bajo |
| Contenido principal | ✅/❌ | ❌/✅ | Alto/Bajo |
| Schema JSON-LD | ✅/❌ | ❌/✅ | Medio/Bajo |

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Análisis de diseño visual (WCAG, tipografía, jerarquía) | `/seo ux-visual [url]` | Profundidad en diseño e identidad visual |
| HTML renderizado por Cloudflare (como Googlebot) | `/seo crawler [url]` | Ver exactamente qué indexa Google |
| Performance y LCP element | `/seo performance [url]` | Diagnóstico técnico de velocidad |
| OG tags y schema en head | `/seo technical [url]` | Verificación completa de meta tags |

## Error Handling

| Escenario | Acción |
|-----------|--------|
| Playwright no instalado | Indicar cómo instalar (`npm install -g playwright`). Ofrecer análisis HTML como alternativa. |
| Firecrawl no disponible | Usar curl para HTML analysis. Indicar limitación (sin JS rendering). |
| URL inaccesible | Reportar error. No inferir rendering. |
| Sitio detrás de auth | Analizar solo páginas públicas. Solicitar HTML/screenshot al usuario si necesita análisis interno. |
| Sitio en construcción / maintenance mode | Reportar y no continuar. |
