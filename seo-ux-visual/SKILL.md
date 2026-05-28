---
name: seo-ux-visual
description: >
  Visual UX audit for SEO: design hierarchy, typography, color contrast (WCAG 2.2),
  above-fold experience, brand consistency, CTA visual effectiveness, and accessibility.
  Use when user says "visual audit", "diseño visual", "tipografía", "colores", "accesibilidad",
  "WCAG", "above the fold", "contraste", "UX visual", "first impression" or "brand consistency".
  Not for page-level conversion optimization — use seo-cro.
user-invokable: true
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# Visual UX Audit

Analiza la experiencia visual de una página: jerarquía, tipografía, accesibilidad, coherencia de marca, y efectividad de CTAs. El diseño visual afecta directamente el engagement, dwell time, pogo-sticking y conversiones — todos señales de ranking.

## CRITICAL: Data Extraction

WebFetch convierte HTML a markdown y pierde atributos de estilo, clases CSS y estructura visual. Usar `curl` para datos precisos:

```bash
# Extraer estilos inline y clases relevantes
curl -sL [URL] | grep -i -E '(style=|class=|font-size|color:|background|<h[1-6]|<button|<a .*href)' | head -30

# Detectar meta viewport y configuración responsive
curl -sL [URL] | grep -i -E '(viewport|media|preload.*font|font-display)'

# Verificar Open Graph image (impacto visual en SERP)
curl -sL [URL] | grep -i 'og:image'
```

---

## 1. Jerarquía Visual

El ojo humano sigue patrones predecibles. Un diseño que los respeta reduce la carga cognitiva y mantiene al usuario en la página.

### Patrones de lectura
| Patrón | Tipo de página | Cómo optimizar |
|--------|---------------|----------------|
| **F-pattern** | Páginas con texto largo (blogs, artículos) | Keywords en inicio de líneas, bullets a la izquierda |
| **Z-pattern** | Páginas simples (landing, home) | Logo → CTA principal → contenido → CTA secundario |
| **Gutenberg** | Páginas con alto contenido visual | CTA principal en zona de terminal (abajo-derecha) |

### Checklist jerarquía visual
- [ ] H1 visualmente dominante (mayor, más bold)
- [ ] Progresión clara H1 > H2 > H3 en tamaño y peso
- [ ] CTA principal destaca sobre el resto (contraste, tamaño)
- [ ] La mirada fluye naturalmente hacia el punto de conversión
- [ ] No más de 3 niveles de énfasis visual por pantalla
- [ ] Whitespace suficiente: los bloques de contenido respiran
- [ ] Imágenes apoyan el contenido, no compiten con él

### Problemas comunes
| Problema | Impacto SEO | Impacto UX |
|----------|------------|------------|
| Demasiados elementos del mismo peso visual | ❌ Baja dwell time | ❌ Fatiga cognitiva |
| CTA enterrado bajo el fold | ➖ Neutral | ❌ Pierde conversiones |
| Texto sobre imágenes sin contraste | ❌ Ilegible para crawlers | ❌ Inaccesible |
| Sidebar + contenido + ads en paralelo | ❌ CLS potencial | ❌ Distracción |

---

## 2. Tipografía

La tipografía no es solo estética — afecta la legibilidad, el tiempo en página y la accesibilidad.

### Tamaños mínimos
| Elemento | Desktop | Mobile | Crítico si |
|----------|---------|--------|------------|
| Body text | 16px | 16px | < 14px → penalizable por Google |
| Labels, captions | 14px | 13px | < 12px → WCAG fail |
| Botones / CTAs | 16px | 16px | < 14px → tap targets pequeños |
| H1 | 28-36px | 24-28px | Menos que H2 → jerarquía rota |

### Legibilidad
- **Line-height:** ≥ 1.5 para body text (WCAG 1.4.12)
- **Ancho de línea:** 45-75 caracteres por línea (óptimo: 65ch)
- **Letter-spacing:** nunca negativo en body text
- **Font-weight:** body ≥ 400, bold ≥ 600 (no 500 como "bold")
- **Contraste tipográfico:** mínimo 4.5:1 texto normal, 3:1 texto grande (>18px o >14px bold)

### Fuentes web y rendimiento
```bash
# Detectar fuentes cargadas
curl -sL [URL] | grep -i -E '(font-face|fonts.googleapis|font-display|preload.*font)'
```

| Issue | Impacto |
|-------|---------|
| `font-display: block` | FOIT: texto invisible durante carga (bad LCP) |
| `font-display: swap` | FOUT: flash de texto sin estilo (CLS risk) |
| `font-display: optional` | Óptimo para performance |
| Sin `preload` en fuente principal | Retrasa renderizado visible |
| Más de 2-3 familias tipográficas | Carga innecesaria + inconsistencia |

### Recomendación fuentes web
```html
<!-- Preload de fuente principal para evitar FOIT -->
<link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossorigin>
<style>
  @font-face {
    font-family: 'Main';
    src: url('/fonts/main.woff2') format('woff2');
    font-display: optional; /* No espera: usa sistema si no carga a tiempo */
  }
</style>
```

---

## 3. Color y Accesibilidad WCAG 2.2

### Ratios de contraste
| Elemento | Mínimo WCAG AA | Óptimo WCAG AAA |
|----------|---------------|----------------|
| Texto normal (< 18px) | 4.5:1 | 7:1 |
| Texto grande (≥ 18px o ≥ 14px bold) | 3:1 | 4.5:1 |
| Componentes UI (botones, inputs, iconos) | 3:1 | — |
| Placeholders en formularios | 4.5:1 | — |
| Focus indicators | 3:1 mínimo | — |

**Herramientas para verificar contraste:**
- Chrome DevTools → Accessibility → Color contrast
- WebAIM Contrast Checker
- Lighthouse → Accessibility audit

### No usar solo color para comunicar
```
❌ MAL: Campo con borde rojo = error (invisible para daltónicos)
✅ BIEN: Borde rojo + ícono ⚠️ + texto "Este campo es requerido"

❌ MAL: Enlace solo diferenciado por color
✅ BIEN: Enlace con color + subrayado
```

### Dark mode
```css
/* Verificar soporte */
@media (prefers-color-scheme: dark) {
  /* ¿Hay estilos para modo oscuro? */
}
```

| Dark mode status | Impacto |
|-----------------|---------|
| Soportado correctamente | UX: 82% usuarios prefieren dark mode (OLED savings) |
| No soportado | Puede forzar inversión automática del navegador → diseño roto |
| Parcialmente soportado | Peor que no soportarlo |

### Checklist WCAG 2.2 Core
- [ ] Contraste texto body ≥ 4.5:1
- [ ] Contraste CTAs/botones ≥ 3:1
- [ ] Focus visible en todos los elementos interactivos
- [ ] No hay trampas de teclado (keyboard trap)
- [ ] Imágenes con alt text (ya cubierto en seo-images)
- [ ] Videos con subtítulos o transcripciones
- [ ] Formularios con labels asociados (`<label for="id">`)
- [ ] Errores en formularios identificados textualmente
- [ ] Estructura de headings lógica para screen readers
- [ ] Links con texto descriptivo (no "clic aquí")
- [ ] Tamaño tap target ≥ 24×24px (WCAG 2.2 nuevo criterio 2.5.8)

---

## 4. Above-the-Fold: Primera Impresión

Los primeros 3 segundos determinan si el usuario se queda o vuelve al SERP.

### Checklist above-the-fold
- [ ] Propuesta de valor clara sin scroll
- [ ] CTA principal visible y con buen contraste
- [ ] H1 comunica qué es el sitio/página
- [ ] Sin popups intrusivos al cargar (penalizable por Google en mobile)
- [ ] LCP element (hero image/texto) carga < 2.5s
- [ ] Sin banners de cookies que tapen el 30%+ de la pantalla
- [ ] Logo + navegación reconocible como header
- [ ] No hay información crítica solo en imágenes (invisible para Google)

### Popups e interstitials — Regla Google Mobile
Google penaliza si al entrar desde búsqueda móvil se muestra:
- Popup que cubre el contenido principal
- Interstitial que hay que cerrar antes de ver el contenido
- Layout donde el contenido visible es solo el popup

**Excepciones NO penalizadas:** cookie banners, age verification, login requerido por ley.

---

## 5. CTAs — Efectividad Visual

### Anatomía de un CTA efectivo
| Atributo | Recomendación | Por qué |
|----------|--------------|---------|
| Color | Contraste ≥ 4.5:1 vs fondo | Accesibilidad + visibilidad |
| Tamaño | Mínimo 44×44px (iOS) / 48×48px (Android) | Tap target usable |
| Forma | Bordes redondeados o pill = mayor CTR | Psicología: amigable, clickeable |
| Texto | Verbo + beneficio ("Obtener presupuesto gratis") | Acción clara |
| Espacio | Whitespace alrededor (no apretado) | Respira, llama atención |
| Posición | Above fold + al final del contenido | Captura intención temprana y tardía |

### Error más común: button que no parece button
```html
<!-- ❌ Link estilizado como texto plano — nadie lo ve como acción -->
<a href="/contacto">Contáctanos</a>

<!-- ✅ CTA visual claro -->
<a href="/contacto" class="btn-primary" role="button">
  Solicitar presupuesto gratis →
</a>
```

---

## 6. Consistencia de Marca Visual

### Checklist
- [ ] Paleta de colores coherente (max 2 primarios + 1 acento)
- [ ] Tipografía uniforme: máx 2 familias (heading + body)
- [ ] Estilo de imágenes consistente (stock vs propias, ilustraciones vs fotos)
- [ ] Iconografía del mismo set/estilo
- [ ] Spacing consistente (usa múltiplos de 4 o 8px)
- [ ] Tono visual coherente entre páginas (home = blog = product)
- [ ] Logo siempre en misma posición y tamaño

### Señales de inconsistencia visual que dañan la confianza
- Páginas con estilos diferentes (indica rediseño parcial)
- Mezcla de imágenes stock con selfies sin criterio
- Botones de 3 colores distintos sin sistema
- Fonts mezclados aleatoriamente

---

## 7. Responsive Design Visual

```bash
# Verificar viewport meta
curl -sL [URL] | grep -i viewport
```

### Checklist responsive
- [ ] `<meta name="viewport" content="width=device-width, initial-scale=1">`
- [ ] Sin scroll horizontal en mobile
- [ ] Texto legible sin zoom (≥ 16px)
- [ ] Imágenes no se desbordan del contenedor
- [ ] Tablas horizontalmente scrolleables en mobile (no rompen layout)
- [ ] Menú hamburger usable con pulgar (zona inferior de pantalla)
- [ ] Formularios con inputs del tipo correcto para mobile

---

## Output Format

### Visual UX Score: XX/100

| Dimensión | Score | Issues | Estado |
|-----------|-------|--------|--------|
| Jerarquía visual | XX/20 | X issues | 🔴/🟡/🟢 |
| Tipografía | XX/20 | X issues | 🔴/🟡/🟢 |
| Color y contraste (WCAG) | XX/20 | X issues | 🔴/🟡/🟢 |
| Above-the-fold | XX/20 | X issues | 🔴/🟡/🟢 |
| Consistencia de marca | XX/10 | X issues | 🔴/🟡/🟢 |
| Responsive visual | XX/10 | X issues | 🔴/🟡/🟢 |

### Hallazgos por Prioridad

| Prioridad | Problema | Impacto SEO | Impacto UX | Esfuerzo |
|-----------|----------|------------|------------|---------|
| 🔴 Crítico | ... | ... | ... | Bajo/Medio/Alto |
| 🟡 Alto | ... | ... | ... | ... |
| 🟢 Quick win | ... | ... | ... | ... |

### WCAG 2.2 Compliance Status
- **Nivel A:** PASS / FAIL (X issues)
- **Nivel AA:** PASS / FAIL (X issues)
- **Nivel AAA:** PASS / FAIL (informativo)

---

## 8. Accesibilidad Web Completa — WCAG 2.2

Google indexa y rankea teniendo en cuenta señales de accesibilidad. Más importante: es obligatorio legal en muchos mercados (ADA en USA, EAA en Europa). Un sitio inaccesible pierde el 15-20% de usuarios con discapacidad.

> **Fechas legales críticas:**
> - **26 abril 2026** — ADA Title II: gobiernos estatales/locales con 50K+ población deben cumplir WCAG 2.1 AA
> - **April 2027** — ADA Title II: entidades gubernamentales menores
> - **Desde junio 2025** — European Accessibility Act (EAA): businesses en 27 países EU deben cumplir accesibilidad digital
> - **5,000+ lawsuits ADA en 2025** (+20% YoY). Settlements: $5K-$75K + attorney fees
> - **No usar accessibility overlays** (accessiBe, UserWay) — no son compliance real; varios tribunales los han rechazado como defensa

### Niveles WCAG 2.2

| Nivel | Requisito | Cuándo aplicar |
|-------|-----------|---------------|
| **A** | Mínimo absoluto | Siempre — sin excepción |
| **AA** | Estándar legal (ADA, WCAG oficial) | Obligatorio para sitios públicos |
| **AAA** | Excelencia | Sitios gov, salud, educación |

### Principios POUR — Checklist Completo

#### P — Perceptible (el contenido debe ser percibible)

**1.1 Alternativas de texto**
- [ ] Toda imagen tiene `alt` (decorativas: `alt=""`)
- [ ] Íconos sin texto tienen `aria-label`
- [ ] Imágenes complejas (infografías) tienen descripción larga o texto alternativo
- [ ] Botones de imagen tienen texto alternativo
- [ ] CAPTCHA tiene alternativa accesible

**1.2 Medios de tiempo (audio/video)**
- [ ] Videos tienen subtítulos (no auto-generados sin revisión)
- [ ] Videos tienen transcripción de texto disponible
- [ ] Audio con información tiene descripción de texto
- [ ] Videos con narración tienen audiodescripción si hay info visual importante

**1.3 Adaptable (estructura sin depender de sensorial)**
- [ ] Estructura semántica correcta: `<header>`, `<main>`, `<nav>`, `<footer>`, `<article>`
- [ ] Headings H1→H2→H3 sin saltar niveles
- [ ] Formularios con `<label>` asociado a cada input (`for="id"`)
- [ ] Tablas con `<th>` y `scope` para datos tabulares
- [ ] El orden de lectura del DOM tiene sentido sin CSS
- [ ] No usar solo posición, forma o color para dar instrucciones ("el botón verde de la derecha")

**1.4 Distinguible (contraste y legibilidad)**
- [ ] Contraste texto normal ≥ 4.5:1 (nivel AA)
- [ ] Contraste texto grande (≥18px o ≥14px bold) ≥ 3:1
- [ ] Contraste componentes UI (botones, inputs, iconos) ≥ 3:1
- [ ] Sin audio que se reproduzca automáticamente > 3 segundos sin control
- [ ] El texto puede escalarse al 200% sin pérdida de contenido
- [ ] Sin scroll horizontal en viewport de 320px (mobile crítico)
- [ ] Espaciado de texto ajustable sin romper layout: line-height ≥ 1.5, letter-spacing ≥ 0.12em
- [ ] Contenido en hover/focus es descartable, hoverable y persistente (WCAG 2.2 1.4.13)

#### O — Operable (la interfaz debe poder operarse)

**2.1 Accesible por teclado**
- [ ] Toda funcionalidad operable con teclado solo (Tab, Enter, Space, flechas)
- [ ] Sin "trampa de teclado" (keyboard trap) — Tab siempre puede salir de cualquier componente
- [ ] Atajos de teclado desactivables o reasignables si usan solo letras

**2.2 Tiempo suficiente**
- [ ] Contenido con límite de tiempo tiene opción de desactivar/extender
- [ ] Sin movimiento/animación que dure más de 5 segundos sin control de pausa

**2.3 Convulsiones y reacciones físicas**
- [ ] Sin contenido que destelle más de 3 veces por segundo
- [ ] Sin animaciones que puedan causar reacciones vestibulares sin `prefers-reduced-motion`

**2.4 Navegable**
- [ ] Skip link al inicio: `<a href="#main">Saltar al contenido principal</a>`
- [ ] Título de página (`<title>`) único y descriptivo en cada página
- [ ] Focus visible en todos los elementos interactivos (`:focus-visible`)
- [ ] Propósito de cada link claro desde el texto del link (no "clic aquí")
- [ ] Múltiples formas de encontrar una página: nav, búsqueda, sitemap
- [ ] Headings y labels describen el tema/propósito

**2.5 Modalidades de entrada (WCAG 2.2 — nuevo)**
- [ ] Gestos complejos (pinch, swipe) tienen alternativa simple con 1 punto de contacto
- [ ] Sin activación accidental por pointer down (acción en pointer up)
- [ ] Tap targets ≥ 24×24px con separación suficiente (WCAG 2.2 — 2.5.8)
- [ ] Sin autenticación con pruebas cognitivas sin alternativa (WCAG 2.2 — 3.3.7)

#### U — Comprensible

**3.1 Legible**
- [ ] `lang` en `<html>`: `<html lang="es">` o `lang="en"` según idioma
- [ ] Cambios de idioma dentro del texto marcados con `lang` en el elemento
- [ ] Sin abreviaciones sin explicación al primer uso

**3.2 Predecible**
- [ ] Sin cambios de contexto al recibir focus (no submit automático al hacer focus)
- [ ] Sin cambios de contexto al cambiar valor de input sin avisarlo
- [ ] Navegación consistente en todas las páginas (mismo orden, misma posición)
- [ ] Elementos con misma función tienen el mismo nombre en todo el sitio

**3.3 Asistencia al input**
- [ ] Errores de formulario identifican qué campo y qué está mal
- [ ] Labels o instrucciones claras en todos los campos de formulario
- [ ] Sugerencia de corrección en errores conocidos (formato de email, teléfono)
- [ ] Para acciones importantes (legal, financiera, datos personales): confirm, review, o opción de deshacer
- [ ] Sin autenticación que requiera recordar o transcribir (WCAG 2.2 — 3.3.8)

#### R — Robusto

**4.1 Compatible**
- [ ] HTML válido: sin errores que afecten parseo (IDs duplicados, tags mal cerrados)
- [ ] Todos los componentes UI tienen `name`, `role` y `value` expuestos a tecnología asistiva
- [ ] Mensajes de estado (loading, error, success) anunciados a screen readers (`aria-live`, `role="alert"`)

### Herramientas de Auditoría de Accesibilidad

```bash
# Lighthouse accessibility audit (score + issues)
# Chrome DevTools → Lighthouse → Accessibility

# Verificar contraste de color
# Chrome DevTools → Elements → seleccionar texto → ver ratio en Styles

# Verificar orden de Tab
# Chrome DevTools → Accessibility → Tab order visualization

# Screen reader test manual
# Windows: NVDA (gratis) → Tab por la página
# Mac: VoiceOver (Cmd+F5)
```

| Herramienta | Qué detecta | Gratis |
|------------|-------------|--------|
| Lighthouse (Chrome) | ~30% de issues automáticos | ✅ |
| axe DevTools (extensión) | ~57% de issues automáticos | ✅ básico |
| WAVE (extensión) | Errores + alertas + estructura | ✅ |
| Colour Contrast Analyser | Ratios exactos | ✅ |
| NVDA + Firefox | Screen reader real | ✅ |

> **Ninguna herramienta automática detecta el 100% de los issues.** El 30-40% restante requiere prueba manual con teclado y screen reader.

### Impacto SEO de Accesibilidad

| Signal accesible | Impacto SEO directo |
|-----------------|-------------------|
| `alt` text en imágenes | ✅ Ranking en Google Images |
| `<label>` en formularios | ✅ Mejor form completion → conversión |
| Headings semánticos | ✅ Estructura entendida por Googlebot |
| `lang` en HTML | ✅ Idioma correcto para búsquedas locales |
| Skip links | ➖ No rankeo directo, pero reduce bounce |
| Contraste 4.5:1 | ➖ No ranking directo, pero UX → dwell time |
| Tap targets 24px+ | ✅ Mobile usability en GSC |

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Optimización de imágenes técnica | `/seo images [url]` | Alt text, formatos, CLS, lazy loading |
| CRO y conversiones | `/seo cro [url]` | A/B testing, engagement signals, funnels |
| Experiencia de búsqueda completa | `/seo sxo [url]` | Intent match, satisfaction, SERP continuity |
| Experiencia del cliente end-to-end | `/seo cx [url]` | Journey mapping, formularios, micro-copy |
| Rendimiento y Core Web Vitals | `/seo google pagespeed [url]` | LCP, CLS, INP datos reales |

## Error Handling

| Escenario | Acción |
|-----------|--------|
| URL inaccesible | Reportar error. No inferir diseño. Pedir HTML o screenshot directamente. |
| Contenido detrás de login | Analizar solo lo visible. Indicar limitación. |
| Sitio JavaScript-rendered | Indicar que los estilos en CSS externo no son analizables sin herramienta de rendering. Usar Playwright/Lighthouse si disponible. |
