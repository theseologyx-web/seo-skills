---
name: seo-sxo
description: >
  Search Experience Optimization (SXO): the bridge between ranking and user satisfaction.
  Analyzes intent-to-experience match, SERP appearance, engagement quality signals,
  pogo-sticking prevention, featured snippet opportunities, and the search-to-conversion journey.
  Use when user says "SXO", "search experience", "intent match", "SERP appearance",
  "featured snippet", "People Also Ask", "pogo-sticking", "dwell time", "CTR bajo",
  "bounce desde búsqueda", or "experiencia de búsqueda".
user-invokable: true
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# Search Experience Optimization (SXO)

> **Scope:** Este skill cubre la capa SERP — intent match, pogo-sticking, CTR, featured snippets, experiencia del primer clic. Para optimización de conversión dentro de la página (CTAs, A/B) usar `seo-cro`. Para el journey completo del cliente (formularios, onboarding, fricción) usar `seo-cx`.

SXO es la intersección entre SEO y UX: no basta con rankear, la página debe satisfacer al usuario que llega desde búsqueda. Google mide la satisfacción post-click (dwell time, pogo-sticking, engagement rate) como señales de calidad. Un mal SXO baja rankings aunque el contenido sea bueno.

## CRITICAL: Data Extraction

```bash
# Verificar title tag y meta description (SERP appearance)
curl -sL [URL] | grep -i -E '(<title|meta name="description"|rel="canonical")'

# Verificar estructura de headings (intent match)
curl -sL [URL] | grep -i -E '<h[1-6][^>]*>' | head -20

# Verificar schema para rich results
curl -sL [URL] | grep -i 'application/ld+json' -A 20 | head -40
```

---

## 1. Intent-to-Experience Match

El error más costoso en SXO: la página rankea para una keyword pero no satisface la intención real del usuario.

### Los 4 tipos de intención de búsqueda
| Intención | Señales en la query | Qué espera encontrar | ¿Cuándo pogo-sticking? |
|-----------|--------------------|--------------------|----------------------|
| **Informacional** | "cómo", "qué es", "por qué" | Explicación clara, respuesta directa | Si la respuesta está enterrada o no existe |
| **Navegacional** | Nombre de marca, "login", "homepage" | La página exacta que busca | Si llega a página incorrecta |
| **Transaccional** | "comprar", "precio", "contratar" | CTA, precio, formulario de contacto | Si no hay precio visible o CTA claro |
| **Comercial** | "mejor", "comparación", "vs", "review" | Comparativas, pros/cons, recomendaciones | Si solo hay contenido promocional sin análisis |

### Checklist Intent Match
- [ ] H1 responde directamente la query principal (no es genérico)
- [ ] El tipo de página coincide con la intención (blog para informacional, landing para transaccional)
- [ ] La respuesta principal aparece en los primeros 100 words (no después de scroll)
- [ ] El tono coincide con el stage del funnel (educativo vs comercial)
- [ ] No hay mismatch SERP title → H1 → contenido

### Message Match: SERP → Página
```
Title tag en SERP:    "Mejor CRM para startups 2025 — Comparativa"
               ↓ El usuario espera una comparativa, no un pitch
H1 en página:         "¿Por qué [Marca] es el CRM perfecto para tu startup?"
               ↗ MISMATCH: prometiste comparativa, das contenido de ventas
               → Resultado: pogo-sticking alto
```

**Regla:** El título en SERP es una promesa. La página debe cumplirla en los primeros 3 segundos de lectura.

---

## 2. SERP Appearance Optimization

### Title Tag como elemento UX

El title tag no es solo SEO — es el primer contacto visual con el usuario en el SERP.

| Criterio | Óptimo | Penalizable |
|----------|--------|------------|
| Longitud | 50-60 caracteres (≈ 580px desktop) | > 60ch: Google lo recorta mid-word |
| Promesa clara | Beneficio específico o respuesta directa | Genérico o clickbait |
| Keyword principal | Al inicio (posición importa para relevancia visual) | Buried al final |
| Números y datos | Aumentan CTR ("7 formas de...", "Guía 2025") | Sin diferenciador |
| Brand al final | "Título descriptivo — Marca" | Brand al inicio (reduce espacio útil) |

**Title tags que Google reescribe (señal de mismatch):**
- Cuando el title no coincide con el H1
- Cuando el title es demasiado largo o corto
- Cuando el title es genérico o poco descriptivo
- Verificar con GSC si Google está reescribiendo tus titles

### Meta Description como CTA

La meta description no rankea pero sí **afecta el CTR**. Trátala como un anuncio de 155 caracteres.

| ✅ Alta CTR | ❌ Baja CTR |
|------------|------------|
| Beneficio concreto ("Ahorra 3h/semana") | Descripción vaga del sitio |
| Verbo de acción ("Descubre", "Aprende") | "Bienvenido a nuestra página de..." |
| Dato específico ("+10.000 usuarios") | Keyword stuffing |
| Urgencia o diferenciador | Mismo texto para múltiples páginas |
| Responde "¿por qué hacer clic?" | No dice qué encontrará el usuario |

```bash
# Detectar meta descriptions duplicadas o ausentes
curl -sL [URL] | grep -i 'meta name="description"'
```

### Rich Results que mejoran CTR

| Rich Result | Schema requerido | CTR lift estimado |
|-------------|-----------------|-------------------|
| Review stars | Product, Recipe, LocalBusiness | +15-30% |
| Breadcrumbs | BreadcrumbList | Mejora navegación visual |
| FAQ | FAQPage (solo gov/health en Google, funciona en otros motores) | +20% en CTR |
| Video thumbnail | VideoObject | +40% en consultas de video |
| Sitelinks | Estructura de nav + popularidad interna | +50% en branded |
| Price / Stock | Offer, Product | Crítico para ecommerce |
| Event dates | Event | Muy alto para eventos |

**IMPORTANTE:** Google deprecó HowTo snippets para desktop (Nov 2023) y FAQPage solo aparece para sitios gov/health. No implementar FAQPage para sitios comerciales esperando rich result.

---

## 3. Engagement Quality Signals

Lo que Google mide (indirectamente) después del click:

### Métricas clave en GA4
| Métrica GA4 | Qué indica para SXO | Umbral referencia |
|------------|--------------------|--------------------|
| Engagement Rate | % sesiones de ≥10s + ≥1 pageview + conversión | > 55% = bien |
| Average Engagement Time | Tiempo promedio de sesión activa | > 1min para blogs, > 30s para landing |
| Scroll Depth (evento) | % de la página consumida | < 25% scroll = contenido no satisface |
| Bounce Rate (GA4 def.) | Sesiones NO engaged (opuesto de engaged) | < 45% landing pages |
| Pages/Session | Profundidad de exploración | > 1.5 para contenido editorial |

### Detectar pogo-sticking en GSC
El pogo-sticking (usuario vuelve al SERP y hace clic en otro resultado) no es una métrica directa en GSC, pero se infiere:

```
Señales de pogo-sticking:
1. CTR alto pero ranking bajando = usuarios entran pero no quedan
2. Posición oscila sin cambios en el contenido
3. Página con buen link authority pero ranking bajo para su keyword principal
4. En GA4: Engagement Rate < 30% en páginas de entrada orgánica
```

### Dwell Time por tipo de página
| Tipo de página | Dwell time normal | Preocupante si |
|----------------|------------------|----------------|
| Blog / artículo | 3-7 min | < 1 min |
| Landing page SaaS | 1-3 min | < 30s |
| Página de producto | 1-2 min | < 20s |
| Página de precios | 30s-2 min | < 15s (no leyó) |
| Homepage | 30s-1 min | < 10s |

---

## 4. Zero-Click Content Strategy

El **68%+** de búsquedas de high-intent terminan sin clic (2026). AI Overviews son la causa principal del aumento — responden la query directamente en el SERP. SXO implica capturar visibilidad incluso sin tráfico, y optimizar para ser citado en AI Overviews, no solo para el clic.

### Featured Snippet — Cómo ganarlos

**Formato correcto por tipo de query:**

| Tipo de query | Formato óptimo | Longitud ideal |
|--------------|---------------|----------------|
| "Qué es X" / "Definición de X" | Párrafo definitorio: "X es..." | 40-60 palabras |
| "Cómo hacer X" | Lista numerada con pasos | 5-8 pasos, 40-50 palabras totales |
| "Mejor X" / "X vs Y" | Tabla comparativa | 3-5 columnas, ≤ 5 filas |
| "Cuánto cuesta X" | Párrafo con rango + factores | 40-60 palabras |

**Estructura HTML óptima para featured snippets:**
```html
<!-- Para párrafo -->
<h2>¿Qué es el SXO?</h2>
<p>El SXO (Search Experience Optimization) es la disciplina que combina SEO y UX para garantizar que los usuarios que llegan desde búsqueda encuentren exactamente lo que buscaban, reduciendo el pogo-sticking y aumentando las conversiones.</p>

<!-- Para lista numerada -->
<h2>¿Cómo optimizar para featured snippets?</h2>
<ol>
  <li>Identifica queries con featured snippet existente en tu nicho</li>
  <li>Escribe la respuesta directa en los primeros 60 words bajo el H2</li>
  <li>Usa el formato que Google prefiere para ese tipo de query</li>
</ol>
```

### People Also Ask (PAA) — Oportunidades

PAA aparece en el 75%+ de las SERPs. Capturarlo da visibilidad adicional.

**Estrategia:**
1. Identificar PAA boxes para keywords objetivo (manual o DataForSEO)
2. Crear secciones H2/H3 con la pregunta exacta como heading
3. Responder en 40-60 palabras directamente bajo el heading
4. Expandir con detalle adicional para usuarios que quieren más

```html
<h3>¿Cuánto tiempo tarda en verse resultados de SEO?</h3>
<p>Los primeros resultados de SEO se ven entre 3 y 6 meses para sitios nuevos. Para sitios con autoridad existente, cambios técnicos pueden impactar en 2-4 semanas. El contenido nuevo tarda entre 4-12 meses en rankear competitivamente.</p>
<!-- Expandir con tabla o lista para mayor detalle -->
```

### Knowledge Panel Signals
- Implementar schema `Organization` o `Person` con `sameAs` a Wikipedia, LinkedIn, Crunchbase
- NAP consistente en todas las páginas y directorios externos
- Menciones en fuentes autoritativas (Wikipedia, prensa, industry sites)

---

## 5. Content Satisfaction by Search Stage

### TOFU — Top of Funnel (Informacional)
**Qué busca:** Entender un problema o concepto
**Señales de satisfacción:** Scroll > 60%, tiempo > 3min, clic a artículo relacionado
**SXO checklist:**
- [ ] Respuesta directa en primeros 100 words
- [ ] Estructura con H2s que responden subpreguntas
- [ ] Definiciones claras de términos técnicos
- [ ] Links a contenido MOFU para guiar el journey
- [ ] Sin CTAs agresivos de venta (no es el momento)

### MOFU — Middle of Funnel (Comercial/Consideración)
**Qué busca:** Comparar opciones, evaluar soluciones
**Señales de satisfacción:** Múltiples páginas vistas, tiempo > 2min, descarga de recurso
**SXO checklist:**
- [ ] Comparativa objetiva (no solo hablar bien de la marca propia)
- [ ] Casos de uso o escenarios de aplicación
- [ ] Social proof (testimonios, casos de éxito, números)
- [ ] CTA soft (demo, guía gratuita, newsletter) — no "compra ya"
- [ ] Links a páginas de producto específicas

### BOFU — Bottom of Funnel (Transaccional)
**Qué busca:** Tomar decisión, comprar, contratar
**Señales de satisfacción:** Conversión, formulario completado, tiempo en pricing page
**SXO checklist:**
- [ ] Precio visible sin scroll en desktop
- [ ] CTA principal above the fold
- [ ] Friction reducida: sin popups al entrar
- [ ] Garantías y políticas claras cerca del CTA
- [ ] Trust signals: reviews, seguridad, logos de clientes
- [ ] Responde objeciones comunes inline (no en FAQ page separada)

---

## 6. Internal Linking para Search Journey

El enlazado interno no es solo distribución de PageRank — es guía del journey de búsqueda.

### Mapa de journey por search intent
```
[Artículo informacional] → [Guía comparativa MOFU] → [Página de producto BOFU]
        ↓                            ↓                         ↓
   "Aprende más"              "Ver comparativa"          "Solicitar demo"
  (anchor descriptivo)       (anchor descriptivo)       (CTA claro)
```

### Reglas de internal linking SXO
- Ancla texto describe el beneficio de hacer clic, no solo la URL destino
- Contextualizar el enlace: aparece donde es relevante, no en sidebar genérico
- No más de 5-7 internal links por pantalla visible (cognitive load)
- Páginas BOFU reciben más links internos que cualquier otra (PageRank + journey)
- Breadcrumbs en todas las páginas profundas (orientación + schema)

---

## 7. Herramientas SXO

| Función | Herramienta | Costo |
|---------|------------|-------|
| CTR real por página/query | Google Search Console | Gratis |
| Engagement Rate, scroll depth | GA4 | Gratis |
| Heatmaps y scroll maps | Microsoft Clarity | Gratis |
| Grabaciones de sesión | Microsoft Clarity / Hotjar | Gratis/Pago |
| Featured snippets existentes | Ahrefs / SEMrush | Pago (Ahrefs gratuito limitado) |
| PAA por keyword | DataForSEO MCP | Pago por uso |
| SERP SERP appearance check | GSC → Search results por URL | Gratis |

---

## Output Format

### SXO Score: XX/100

| Dimensión | Score | Hallazgo clave |
|-----------|-------|---------------|
| Intent Match | XX/25 | ... |
| SERP Appearance | XX/25 | ... |
| Engagement Signals | XX/20 | ... |
| Content Satisfaction | XX/15 | ... |
| Zero-Click Opportunities | XX/15 | ... |

### SERP Appearance Analysis
- **Title Tag:** [texto actual] — [evaluación + recomendación]
- **Meta Description:** [texto actual] — [evaluación + recomendación]
- **Rich Results activos:** [lista]
- **Rich Results perdidos:** [oportunidades]

### Intent Match Score
- **Intent detectado:** [informacional / navegacional / transaccional / comercial]
- **¿La página lo satisface?:** [Sí / Parcial / No]
- **Pogo-sticking risk:** [Alto / Medio / Bajo]
- **Señal clave:** [qué evidencia el problema o la fortaleza]

### Top Acciones SXO

| Prioridad | Acción | Métrica esperada a mejorar | Esfuerzo |
|-----------|--------|--------------------------|---------|
| 🔴 | ... | CTR / Dwell time / Conversión | Bajo/Medio/Alto |
| 🟡 | ... | ... | ... |
| 🟢 | ... | ... | ... |

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Diseño visual de la experiencia | `/seo ux-visual [url]` | Jerarquía, tipografía, WCAG, CTAs |
| CRO y A/B testing | `/seo cro [url]` | Conversión, heatmaps, A/B |
| Schema para rich results | `/seo schema [url]` | Rich results que mejoran CTR en SERP |
| E-E-A-T y calidad de contenido | `/seo content [url]` | Profundidad, autoridad, AI citations |
| Experiencia del cliente completa | `/seo cx [url]` | Journey map, micro-copy, formularios |
| Datos reales de clicks y posiciones | `/seo google [url]` | GSC data, CWV field data |

## Error Handling

| Escenario | Acción |
|-----------|--------|
| Sin acceso a GSC | Inferir desde HTML y estructura de página. Indicar que CTR/engagement data requiere GSC. |
| Sin acceso a GA4 | Analizar señales indirectas (estructura, contenido, CTA placement). |
| URL inaccesible | Reportar error. No inferir. |
| Contenido JavaScript-rendered | Indicar riesgo de intent mismatch si el crawler ve contenido diferente al usuario. |
