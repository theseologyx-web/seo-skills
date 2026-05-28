---
name: seo-plan
description: >
  Strategic SEO planning for new or existing websites. Industry-specific
  templates, competitive analysis, content strategy, and implementation
  roadmap. Use when user says "SEO plan", "SEO strategy", "SEO planning",
  "content strategy", "keyword strategy", "content calendar",
  "site architecture", or "SEO roadmap".
  Not for site diagnosis or audit — use seo-audit. For new clients, run seo-client-discovery before planning.
user-invokable: true
argument-hint: "[business-type]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  author: AgriciDaniel
  version: "2.0.0"
  category: seo
---

# Strategic SEO Planning

---

## Marco conceptual: estrategia vs plan táctico

**Estrategia** = QUÉ y POR QUÉ. Define la dirección, las prioridades y el posicionamiento.
> "Atacamos primero BOFU porque la competencia tiene contenido débil ahí y el cliente puede cerrar ventas rápido."

**Plan táctico** = CÓMO, CUÁNDO y QUIÉN. Traduce la estrategia en acciones concretas con fechas y responsables.
> "Semana 2: crear `/inspeccion-visual-industrial`, 1.400 palabras, KW principal D=32, schema Article, 3 enlaces internos."

**Orden obligatorio:** Diagnóstico → Estrategia → Plan táctico. No al revés.

---

## CAPA 1 — ESTRATEGIA

### Paso 0: Acceso a herramientas (OBLIGATORIO antes de cualquier análisis)

Sin datos reales el trabajo es especulativo. Verificar antes de continuar:

#### Herramientas obligatorias (siempre):

| Herramienta | Qué aporta | Verificar |
|-------------|------------|-----------|
| **Google Search Console (GSC)** | Rendimiento orgánico, indexación, errores de rastreo, manual actions, sitemaps | Propiedad verificada + acceso Owner o Full User |
| **Google Analytics 4 (GA4)** | Tráfico, comportamiento, conversiones, canales | Propiedad configurada + evento de conversión activo |
| **Bing Webmaster Tools (BWT)** | Rendimiento en Bing/Copilot, IndexNow, errores, keywords Bing | Propiedad verificada + sitemap enviado |
| **Ahrefs Webmaster Tools (gratis)** | Backlinks, broken links, keywords orgánicas, CWV básicos | Propiedad verificada en ahrefs.com/webmaster-tools |

#### Herramientas condicionales:

| Herramienta | Cuándo activar |
|-------------|----------------|
| **Google Business Profile (GBP)** | Cliente local o con sede física |
| **Bing Places for Business** | Cliente local |
| **Google Tag Manager (GTM)** | Múltiples etiquetas o conversiones complejas |
| **Google Looker Studio** | Reportes automáticos para cliente |
| **IndexNow** | CMS compatible (WordPress, Wix, Webflow) |
| **Screaming Frog** (free hasta 500 URLs) | Auditorías puntuales o sitios pequeños |

#### Checklist de acceso:

```
☐ GSC — propiedad verificada + usuario con acceso Owner o Full User
☐ GA4 — propiedad configurada + evento de conversión definido
☐ Bing WMT — propiedad verificada + sitemap enviado
☐ Ahrefs WMT — propiedad verificada (gratis)
☐ GBP — reclamado y verificado [si local]
☐ Bing Places — reclamado y verificado [si local]
☐ IndexNow — activado en CMS [si compatible]
☐ GTM — contenedor instalado y funcionando [si aplica]
```

> Si alguna herramienta obligatoria no está activa, comunicarlo antes de continuar. No se puede diagnosticar sin datos históricos reales.

---

### Paso 1: Diagnóstico (la auditoría es parte de la estrategia)

El diagnóstico es la base de toda decisión estratégica. Sin diagnóstico no hay estrategia real — solo suposiciones.

**Regla de cobertura:** Todos los frentes se revisan siempre — incluso si el resultado es "no existe actualmente". La ausencia de algo (hreflang, schema, video, GBP) no es motivo para saltarlo: puede existir en planes futuros del cliente o ser una oportunidad estratégica. Se documenta el estado actual + se pregunta si hay planes.

---

#### 1a. Situación del sitio

Identificar primero en qué situación está el sitio. Esto condiciona el orden de prioridades de todo lo demás.
→ Ver `assets/situations.md` para el enfoque estratégico según situación.

| Situación | Indicadores clave |
|-----------|-------------------|
| **Sitio nuevo** | Sin GSC data, sin indexación, DA=0 |
| **Estancado** | Tráfico plano >3 meses, keywords sin movimiento |
| **Caída de tráfico** | Bajada visible en GSC, pérdida de posiciones |
| **Penalización** | Manual Action en GSC, caída brusca post-update |
| **Migración en curso** | Cambio de dominio, CMS, o arquitectura URL |
| **Competitivo maduro** | Tráfico estable, necesita diferenciación o escala |

---

#### 1b. Consolidación de URLs y analítica de datos

**Este paso va antes de cualquier análisis.** Los datos en GSC, GA4, BWT y otras plataformas están fragmentados por URL. Una página que cambió de slug, pasó de HTTP a HTTPS, o migró de dominio tiene su historial repartido en múltiples entradas. Analizar solo la URL actual es analizar una fracción de la realidad.

##### Paso 0 — Normalizar URLs (limpiar antes de analizar)

Antes de cualquier consolidación, limpiar todas las URLs para obtener su forma canónica real. Una misma página puede aparecer fragmentada en decenas de variantes si no se normaliza primero.

**Parámetros a eliminar:**

| Tipo | Ejemplos | Acción |
|------|---------|--------|
| UTM de tracking | `?utm_source=`, `?utm_medium=`, `?utm_campaign=`, `?utm_content=`, `?utm_term=` | Eliminar — son parámetros de analítica, no forman parte de la URL real |
| Parámetros de referencia | `?ref=`, `?source=`, `?from=`, `?via=` | Eliminar |
| Parámetros de plataformas sociales | `?fbclid=`, `?gclid=`, `?msclkid=`, `?twclid=`, `?igshid=` | Eliminar |
| Parámetros de sesión o usuario | `?sessionid=`, `?userid=`, `?token=` | Eliminar |
| Parámetros de paginación/filtros | `?page=`, `?sort=`, `?filter=`, `?color=` | Evaluar — algunos son indexables, otros no |
| Fragmentos de página | `#section`, `#anchor` | Eliminar para comparación (no afectan a la URL canónica) |
| Variantes de protocolo y www | `http://`, `https://`, `www.`, sin `www.` | Unificar bajo la versión canónica definida en GSC |
| Trailing slash | `/pagina/` vs `/pagina` | Unificar según la configuración del sitio |

**Regla:** la URL canónica real es la que aparece en el `<link rel="canonical">` de la página. Si no existe canonical, es la URL definida como preferida en GSC.

**En la práctica:**
- En GA4: usar la dimensión "Página + cadena de consulta" y filtrar/agrupar excluyendo parámetros UTM y de tracking en la configuración de la propiedad (Data Settings → Data Filters)
- En GSC: los parámetros UTM no suelen indexarse, pero verificar en Cobertura si hay URLs con parámetros indexadas accidentalmente
- En BWT: igual — verificar URLs con parámetros en el crawl report
- En Ahrefs/backlinks: unificar manualmente al exportar si hay backlinks apuntando a versiones con parámetros

---

##### Paso 1 — Mapear historial de redirects por página

Para cada página importante del sitio:
- Rastrear todas las versiones anteriores de la URL (HTTP→HTTPS, con/sin www, slugs anteriores, cambios de estructura de carpetas, migraciones de dominio)
- Verificar que cada redirect anterior apunta correctamente a la URL final (301, no cadenas)
- Documentar el árbol completo: `URL v1 → URL v2 → URL v3 (actual)`

> Una URL con 3 versiones anteriores puede tener el 60% de su tráfico histórico, backlinks y datos de keywords registrados en versiones viejas. Sin consolidar, el diagnóstico es incompleto.

##### Paso 2 — Consolidar datos de todas las versiones hacia la URL final

Para cada URL, sumar los datos de todas sus versiones anteriores en cada fuente:

| Dimensión | Qué consolidar |
|-----------|----------------|
| **Keywords** | Todas las queries por las que rankeó cualquier versión de la URL — en GSC filtrar por cada versión de URL y unificar |
| **Clics e impresiones** | Sumar histórico de clics/impresiones de todas las versiones en GSC |
| **Tráfico** | Sesiones/usuarios de todas las versiones de URL en GA4 |
| **Backlinks** | Links apuntando a versiones antiguas (Ahrefs WMT) — si el redirect está bien hecho suman autoridad, si no están rotos son pérdida |
| **Posiciones** | Posición media histórica de cada versión para entender tendencia real |
| **Conversiones** | Eventos de conversión en GA4 bajo cualquier versión de la URL |

##### Paso 3 — Análisis completo por URL (perfil real de cada página)

Con los datos consolidados, construir el perfil real de cada página importante:

**Rendimiento en búsqueda — GSC (análisis avanzado):**
- Keywords totales consolidadas (todas las versiones de URL) — volumen, posición, CTR, tendencia
- **CTR real vs CTR esperado por posición:** un CTR bajo para la posición que ocupa indica title/meta description débil, no problema de ranking. Benchmark: posición #1 ~8-12% no-branded, ~28%+ branded
- **Branded vs no-branded:** segmentar queries de marca de queries genéricas — crecimiento diferenciado indica si el problema es awareness o contenido
- **Segmentación por tipo de búsqueda:** web / imagen / video / noticias / Discover / Shopping — cada uno tiene su propio CTR esperado y estrategia
- **SERP features activas:** ¿hay featured snippets, PAA, Knowledge Panel, Shopping? ¿se están canibalizando clics orgánicos?
- **Canibalización via GSC:** filtrar por query y ver si múltiples URLs alternaron posiciones en los últimos 90 días — indica que Google no tiene claro cuál es la página ganadora
- Distribución por país: ¿de dónde viene el tráfico orgánico? ¿coincide con el target del negocio?
- Distribución por dispositivo: ¿desktop vs mobile? ¿el comportamiento difiere?
- Evolución temporal: ¿creciendo, estancada, cayendo? ¿coincide con algún Google Update o cambio interno?
- **Comparativa YoY normalizada:** comparar clics YoY ajustando por impresiones — si impresiones crecieron 10% pero clics cayeron, el problema es CTR o SERP features, no rankings
- Rendimiento en Bing (BWT): ¿keywords distintas a Google? ¿posiciones muy diferentes?

**Discover y News (si aplica):**
- ¿El sitio recibe tráfico de Google Discover o Google News? (GSC → Tipo de búsqueda)
- Curva de decay de Discover: el boost máximo es días 1-7, moderado 8-14, limitado 15-30, decay >30 días — si el sitio publica poco, sistemáticamente pierde el ciclo de freshness
- Verificar og:image ≥ 1200px — sin eso, las cards de Discover son thumbnail o no aparecen
- Hard block signals: si tráfico Discover cae bruscamente (no gradualmente), puede ser bloqueo de usuario o dominio, no decay orgánico

**Comportamiento de usuarios — GA4 (análisis avanzado):**
- **Tres scopes de atribución — analizar los tres, no solo session:**
  - *Session scope (last-click):* ¿de dónde vino la sesión? — visión corta
  - *First user source:* ¿de dónde vino el usuario la primera vez? — revela canales de adquisición real y señales de LTV
  - *Data-driven (event scope):* ¿qué canales contribuyeron a cada conversión? — visión multi-touch
- Tráfico por canal: orgánico / directo / referral / paid / social / email — cruzar con first user source para ver si "directo" es en realidad dark social (TikTok, WhatsApp, etc.)
- Tasa de engagement (antiguo bounce rate): ¿los usuarios interactúan o rebotan?
- Tiempo en página y scroll depth: ¿leen el contenido?
- **Comportamiento cross-device:** ¿usuarios investigan en desktop y convierten en mobile? Si session duration en mobile es bajo pero conversion rate es alto, puede ser micro-moment — optimizar para eso
- Flujo de navegación post-página: ¿a dónde van después? ¿el flujo lleva hacia conversión o se pierde?
- **Key events con valor monetario:** si GA4 tiene eventos configurados con valor económico, ¿esta página contribuye a revenue? (no solo tráfico)
- **Rutas de conversión multi-touch:** ¿esta página es typically "first", "middle", o "last" en el camino a conversión?
- Segmentos: ¿el comportamiento varía significativamente por país, dispositivo, o canal de entrada?
- **Análisis de cohortes:** ¿los usuarios que entran por esta página vuelven? ¿retienen mejor que los de otros canales?
- **Estacionalidad:** comparar el mismo período año anterior — ¿hay picos predecibles que deberían guiar el calendario editorial?

**Autoridad acumulada — Ahrefs WMT:**
- Total de backlinks a todas las versiones de la URL
- Referring domains únicos apuntando a cualquier versión
- Anchor text distribution consolidada — ¿natural o sobreoptimizado?
- ¿Hay backlinks valiosos apuntando a URLs antiguas sin redirect correcto? → pérdida de autoridad recuperable

**CWV — CrUX (field) vs lab data:**
- **Field data (CrUX):** lo que Google usa para ranking — tiene lag de 28 días, solo disponible si la página tiene suficiente tráfico real en Chrome
- **Lab data (PageSpeed/Lighthouse):** útil para diagnóstico y para verificar optimizaciones antes de que el CrUX lo refleje
- Si lab data muestra "Good" pero CrUX muestra "Poor": los usuarios reales tienen redes o dispositivos peores que el entorno de lab — optimizar específicamente para mobile en redes lentas
- Si la página no tiene CrUX data propio: Google usa datos a nivel de dominio — mejorar CWV en páginas con más tráfico primero

**Log files (condicional — sitios con >10k páginas o ecommerce):**
- ¿Qué páginas prioriza Googlebot? ¿coincide con las estratégicamente importantes?
- ¿Hay 404s o redirect chains consumiendo crawl budget?
- ¿Crawlers de AI (GPTBot, ClaudeBot, PerplexityBot) están drenando recursos? — en 2025-2026 pueden representar >40% del tráfico de bots en algunos sitios
- Spike o caída brusca de crawls de Googlebot: señal anticipada de cambio de ranking

**Share of Voice y marca:**
- **Share of Search (SoS):** % de búsquedas de marca del cliente sobre el total de búsquedas de marca del sector — indicador de salud de marca
- **Momentum de marca:** crecimiento MoM del volumen de búsquedas branded — si competidores crecen más rápido, la brecha se amplía
- **High-intent ratio:** % de búsquedas branded que son comerciales ("pricing", "review", "vs [competidor]") vs informacionales — alto ratio = audiencia con intención de compra
- **AI mentions:** ¿aparece la marca citada en Google AI Overviews, ChatGPT, Perplexity para queries relevantes del sector?

**Datos de negocio local — GBP (si aplica):**
- Búsquedas que activan la ficha: directas (buscan la marca), descubrimiento (buscan categoría), marca (buscan nombre exacto)
- Acciones: llamadas, clics a web, solicitudes de ruta — tendencia mensual
- Fotos: vistas propias vs competencia
- Reseñas: volumen, media, tendencia, sentiment de reseñas negativas recientes

**Datos de App Store / Google Play (si aplica):**
- Impresiones y descargas por keyword en la store
- Conversion rate por keyword (impresiones → instalaciones)
- Ratings y reseñas: volumen, media, tendencia
- Retención D1/D7/D30 y desinstalaciones — señal de calidad del producto que afecta el algoritmo de la store
- Keywords que generan instalaciones vs keywords que generan impresiones sin conversión

---

##### Paso 4 — Síntesis analítica: hallazgos → implicaciones estratégicas

Antes de pasar al diagnóstico técnico y on-page, documentar los hallazgos clave de cada fuente y traducirlos en decisiones:

| Fuente | Hallazgo clave | Implicación estratégica |
|--------|---------------|------------------------|
| GSC — CTR analysis | | |
| GSC — Canibalización | | |
| GSC — SERP features | | |
| GSC — Discover/News | | |
| GA4 — Atribución scopes | | |
| GA4 — Comportamiento | | |
| GA4 — Cohortes/retención | | |
| GA4 — Estacionalidad | | |
| BWT | | |
| Ahrefs WMT | | |
| CrUX vs lab data | | |
| Share of Search | | |
| GBP (si local) | | |
| App Store / Play (si app) | | |
| Log files (si aplica) | | |

> Cada hallazgo sin implicación estratégica es dato sin valor. Este cuadro obliga a convertir números en decisiones antes de avanzar al Paso 5.

---

#### 1c. Diagnóstico técnico

Estructura jerárquica — cada nivel depende del anterior:

**Nivel 1 — Crawl (descubrimiento):**
- robots.txt: ¿bloquea URLs que deberían indexarse?
- XML sitemap: ¿existe, está enviado en GSC, está actualizado?
- Estructura de URLs: lógica, sin parámetros innecesarios, hyphens no underscores
- Páginas bloqueadas por noindex/nofollow no intencionales

**Nivel 2 — Rendering (ejecución JS):**
- ¿El sitio depende de JS para renderizar contenido principal?
- ¿Googlebot ve el mismo contenido que el usuario?
- Server-side rendering vs client-side rendering
→ Skill `/seo technical <url>` para diagnóstico de JS rendering

**Nivel 3 — Indexación:**
- Cobertura en GSC: páginas excluidas, errores, advertencias
- Contenido duplicado: canonical tags correctos, páginas con contenido idéntico o muy similar
- Redirect chains: cadenas de redirección > 2 saltos
- Errores 404 en URLs con tráfico o backlinks

**Nivel 4 — Señales de ranking:**
- HTTPS en todo el sitio
- Mobile-first: ¿el sitio está optimizado para mobile?
- Manual actions en GSC → Seguridad y acciones manuales
- Hreflang: ¿implementado? Si no, ¿hay planes de expansión internacional? → ver 1h

**Nivel 5 — CTR y visibilidad:**
- Title tags: únicos, 50-60 chars, keyword principal
- Meta descriptions: únicas, 95-110 chars, CTA implícito
- Rich results: ¿el sitio es elegible? ¿está aprovechando snippets?

→ Skill `/seo technical <url>` para diagnóstico técnico completo

---

#### 1c. Diagnóstico de rendimiento

- **LCP** (Largest Contentful Paint): objetivo < 2.5s — field data en GSC, lab data en PageSpeed
- **INP** (Interaction to Next Paint): objetivo < 200ms
- **CLS** (Cumulative Layout Shift): objetivo < 0.1
- **TTFB** (Time to First Byte): objetivo < 600ms — indica calidad de servidor/hosting
- Recursos render-blocking: CSS y JS que retrasan la primera carga
- Imágenes: ¿WebP/AVIF? ¿tamaño apropiado? ¿lazy loading?
- CDN: ¿hay CDN activo? ¿los assets estáticos se sirven desde edge?
- Caché: ¿configurado correctamente?

→ Skill `/seo performance <url>` para diagnóstico CWV completo

---

#### 1d. Diagnóstico de contenido on-page (completo)

Contenido no es solo texto. Cubre todos los elementos on-page:

**HTML5 semántico y estructura:**
- H1 único por página, con keyword principal, visible en el HTML inicial (no generado por JS)
- Jerarquía de headings correcta: H1 → H2 → H3 sin saltos
- Etiquetas semánticas HTML5: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<footer>`, `<aside>`
- Sin div-soup donde deberían ir etiquetas semánticas

**Metadata:**
- Title tag: único, 50-60 chars, keyword principal al inicio
- Meta description: única, 95-110 chars
- Open Graph: `og:title`, `og:description`, `og:image` (1200×630px), `og:url`, `og:type`
- Twitter Card: `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`
- Canonical URL: correcta, sin apuntar a páginas erróneas
- Viewport meta tag presente
- Lang attribute en `<html>`

**URLs:**
- Slug descriptivo con keyword principal (hyphens, no underscores)
- Sin parámetros de tracking en URLs indexables
- Longitud razonable (< 75 chars)
- Consistencia de estructura (sin cambios históricos innecesarios)

**Texto y keywords:**
- ¿Qué páginas rankean y para qué keywords? (GSC → Rendimiento)
- Keyword principal en primeros 100 palabras
- Variaciones semánticas y LSI keywords naturales en el cuerpo
- Intención de búsqueda: ¿el contenido responde lo que Google espera para esa KW?
- Canibalización: ¿varias URLs compitiendo por la misma KW?
- Thin content: páginas con < 300 palabras sin justificación
- Freshness: ¿el contenido está actualizado? ¿la fecha de actualización es visible?
- Readability: párrafos cortos, sin relleno, CTA claro

**Imágenes:**
- Nombres de archivo descriptivos (keyword-relevante, no IMG_1234.jpg)
- Alt text descriptivo en todas las imágenes (no keyword stuffing)
- Formato moderno (WebP o AVIF)
- Tamaño apropiado (no imágenes de 3MB en páginas web)
- Lazy loading en imágenes below the fold
- ¿Texto importante quemado en imagen en lugar de HTML? (problema de indexación)

**Video:**
- ¿Hay video embebido? ¿Está optimizado? (título, descripción, transcript)
- VideoObject schema donde haya video
- Subtítulos/captions disponibles
- ¿El video aporta valor SEO o es decorativo?
- Canal de YouTube vinculado al sitio (si aplica)

**Schema / Structured data:**
- ¿Qué schema tiene implementado? ¿es el correcto para cada tipo de página?
- Validar en Rich Results Test: ¿hay errores o advertencias?
- Oportunidades de schema no aprovechadas (ver tipo de negocio + tipo de contenido)
- Schema en HTML inicial vs generado por JS (debe estar en HTML inicial)

→ Skill `/seo content <url>` para análisis de contenido y E-E-A-T
→ Skill `/seo schema <url>` para diagnóstico de structured data
→ Skill `/seo images <url>` para diagnóstico de imágenes

---

#### 1e. Diagnóstico de E-E-A-T

**Experiencia (Experience):**
- ¿El contenido muestra experiencia de primera mano? (casos reales, ejemplos propios)
- Testimonios y reseñas verificables
- Case studies con resultados reales y medibles

**Expertise (Pericia):**
- ¿Hay byline de autor en el contenido? ¿con bio y credenciales?
- ¿Las credenciales son relevantes al tema?
- ¿Hay "Expert reviewed" o similar donde aplica?
- ¿El contenido cita fuentes externas de calidad?

**Autoridad (Authoritativeness):**
- Menciones en medios o sitios del sector
- Press coverage en últimos 12 meses
- Presencia en Knowledge Graph de Google
- Backlinks de sitios temáticos relevantes (no directorios genéricos)
- ¿El brand name genera búsquedas propias? (branded search volume)

**Confianza (Trustworthiness):**
- HTTPS completo
- Privacy policy visible y actualizada
- Terms of service presentes
- About page detallada con información real del equipo/empresa
- Información de contacto clara (dirección, teléfono, email)
- Fechas de publicación y actualización visibles en artículos
- ¿Hay reseñas en plataformas terceras (Google, Trustpilot, G2)?
- Sentiment de brand mentions: ¿hay menciones negativas relevantes?

→ Skill `/seo content <url>` para análisis E-E-A-T completo

---

#### 1f. Diagnóstico de enlazado interno

- Páginas huérfanas: sin ningún enlace interno apuntándoles
- Profundidad de click: ¿hay páginas importantes a más de 3 clics de la home?
- Distribución de link equity: ¿las páginas clave reciben suficientes enlaces internos?
- Anchor text: ¿descriptivo y variado? ¿o "click aquí" y "ver más"?
- Clusters temáticos: ¿las páginas relacionadas están enlazadas entre sí?
- Ratio de links por página: ¿hay páginas con > 100 links internos salientes?

→ Skill `/seo internal-linking <url>` para análisis de enlazado interno

---

#### 1g. Diagnóstico de autoridad y backlinks

- DA/DR actual vs DA/DR de los 3-5 competidores principales
- Número de referring domains (RDs) y calidad media
- Distribución de anchor text: ¿natural o sobreoptimizado?
- Backlinks tóxicos: ¿hay links de sitios spam o penalizados?
- Páginas más enlazadas del sitio (¿coinciden con las más estratégicas?)
- Menciones de marca sin enlace (oportunidades de link building)
- Perfil de crecimiento de backlinks: ¿orgánico o picos artificiales?

→ Skill `/seo backlinks <domain>` para análisis de backlinks completo

---

#### 1h. Diagnóstico SMO (Social Media Optimization)

- Open Graph correcto en todas las páginas principales (og:image en tamaño correcto)
- Twitter Card implementado
- ¿Los shares en redes muestran preview correcto? (probar con validadores)
- Perfiles sociales del negocio: ¿activos? ¿enlazados desde el sitio?
- ¿El sitio enlaza a los perfiles sociales y viceversa? (señal de entidad)
- Botones de share: ¿existen? ¿funcionan?
- ¿Hay contenido con potencial viral/shareable que no está siendo distribuido?

→ Skill `/seo smo` para diagnóstico SMO completo

---

#### 1i. Diagnóstico GEO / AI Search Visibility

No es condicional — es relevante para todos los sitios en 2025-2026.

- ¿El sitio aparece citado en Google AI Overviews para keywords clave?
- ¿Aparece en respuestas de ChatGPT, Perplexity, o Claude?
- ¿Hay featured snippets activos? (base para citations en AI)
- ¿El contenido está estructurado para ser extraíble? (headings como preguntas, respuestas directas, tablas, listas)
- ¿robots.txt bloquea crawlers de LLMs (GPTBot, ClaudeBot, PerplexityBot)?
- ¿Existe `llms.txt` en la raíz?
- Presencia en Knowledge Graph: ¿el brand/entidad está reconocido por Google?
- ¿El structured data (schema) está completo y sin errores?

→ Skill `/seo geo` para diagnóstico GEO completo

---

#### 1j. Diagnóstico SEO Local

Revisar siempre. Si no aplica hoy, preguntar si hay planes.

- ¿Tiene o tendrá sede física, área de servicio geográfica, o clientes locales?
- Google Business Profile: ¿reclamado, verificado, completo?
- NAP consistency (Name, Address, Phone): ¿igual en GBP, sitio web, y directorios?
- Reseñas: cantidad, media, estrategia de respuesta
- LocalBusiness schema en homepage y páginas de contacto/ubicación
- Bing Places: ¿configurado?
- Citations en directorios relevantes del sector/ciudad

→ Skill `/seo local <url>` para diagnóstico local completo

---

#### 1k. Diagnóstico Internacional / Hreflang

Revisar siempre. Si no aplica hoy, preguntar si hay planes de expansión.

- ¿El sitio opera en varios idiomas o países?
- Si sí: ¿hreflang implementado correctamente? ¿hay duplicate content entre variantes?
- ¿Geo-targeting configurado en GSC?
- Si no: ¿hay planes de expansión internacional en los próximos 12-24 meses? ¿a qué mercados?
- Estructura recomendada si hay expansión futura: subdirectorios (`/es/`, `/mx/`) vs subdominios vs dominios separados

→ Skill `/seo international` para diagnóstico internacional completo
→ Skill `/seo hreflang` para validación de hreflang

---

#### 1l. Diagnóstico ASO y visibilidad en directorios de apps/software

Condicional: aplica si el cliente tiene una app móvil o producto de software listable.
Preguntar siempre: ¿tiene o planea lanzar una app o producto listable en directorios?

**App Stores (ASO):**
- App Store (iOS) y Google Play: ¿la app está publicada?
- Título y subtítulo de la app: ¿incluyen keywords relevantes?
- Descripción corta y larga: ¿optimizadas para búsqueda dentro de la store?
- Screenshots y preview video: ¿comunican el valor claramente?
- Ratings y reseñas: cantidad, media, estrategia de respuesta
- Categoría seleccionada: ¿es la más relevante y competitiva?
- In-app events y features destacadas (App Store)
- Actualizaciones frecuentes: señal positiva en algoritmos de stores

**Google Search — búsquedas de app:**
- ¿Aparece la ficha de Google Play en resultados web cuando se busca la app o categoría?
- SoftwareApplication schema en el sitio web (vincula sitio ↔ app)
- Página de la app en el sitio web optimizada para keywords de búsqueda tipo "app de [categoría]"
- Deep links y App Indexing configurados (Android)

**Directorios de software y apps:**
SEO en su sentido amplio = aparecer en cualquier buscador. Estos directorios tienen motores de búsqueda propios con millones de búsquedas mensuales:
- **G2** — reviews B2B, muy peso en búsquedas Google también
- **Capterra / GetApp / Software Advice** — mismo ecosistema (Gartner)
- **Product Hunt** — lanzamiento y visibilidad inicial
- **AlternativeTo** — búsquedas de tipo "alternativa a [competidor]"
- **Trustpilot** — reseñas con impacto en branded search
- **Crunchbase** — autoridad de marca y entidad
- ¿Está listado en los directorios relevantes del nicho? ¿el perfil está completo y optimizado?
- ¿Las reseñas en directorios están siendo gestionadas activamente?

→ Skill `/seo aso` para diagnóstico ASO completo

---

#### 1m. Baseline de KPIs

Documentar el punto de partida antes de cualquier acción:

| Métrica | Valor actual | Fuente |
|---------|-------------|--------|
| Clics orgánicos/mes | | GSC |
| Impresiones/mes | | GSC |
| Posición media | | GSC |
| Keywords en top 10 | | GSC / Ahrefs WMT |
| Páginas indexadas | | GSC → Cobertura |
| DA / DR | | Ahrefs WMT |
| LCP / INP / CLS | | GSC → CWV / PageSpeed |
| Referring domains | | Ahrefs WMT |
| Featured snippets activos | | GSC / Ahrefs |
| AI citations activas | | Manual check |
| Reseñas Google (si local) | | GBP |

---

### Paso 2: Contexto competitivo

Los datos analíticos del Paso 1b son el punto de partida: saber desde dónde parte el cliente antes de comparar con la competencia.

- Identificar top 3-5 competidores reales en SERP (no solo los que el cliente cree que son — cruzar con GSC para ver quién aparece en las mismas queries)
- Comparar DA/DR, RDs, volumen de tráfico estimado vs el cliente
- Analizar qué keywords rankean, con qué contenido, y en qué posición
- Gaps de contenido: temas que la competencia cubre y el cliente no
- Gaps de keywords: búsquedas con volumen donde el cliente no aparece pero la competencia sí
- Gaps de autoridad: diferencia de DA y cómo afecta la capacidad de competir en head terms
- Gaps de canales: ¿la competencia tiene presencia en canales que el cliente no? (app stores, directorios, YouTube, etc.)
- Gaps de países: ¿la competencia rankea en mercados geográficos donde el cliente no tiene visibilidad?
- Gaps de dispositivo: ¿la competencia está más optimizada para mobile?
- Comparativa de E-E-A-T: ¿la competencia tiene señales de autoridad más fuertes (team pages, estudios propios, menciones en medios)?

> Cruzar los hallazgos de la competencia con los datos reales del cliente (analítica del Paso 1b) para identificar las brechas más rentables de cerrar.

→ Skill `/seo competitive [domain]` para inteligencia competitiva completa

---

### Paso 3: Tipo de negocio

Cargar el template correspondiente de `assets/` para guiar arquitectura y contenido:

| Tipo de negocio | Template | Cuándo usar |
|-----------------|----------|-------------|
| SaaS / Software | `assets/saas.md` | Producto digital con free trial o suscripción |
| Negocio local | `assets/local-service.md` | Servicio con área geográfica definida |
| Ecommerce | `assets/ecommerce.md` | Tienda online con productos físicos o digitales |
| Publisher / Media | `assets/publisher.md` | Sitio de contenido, noticias, blog de alto volumen |
| Agencia / Consultoría | `assets/agency.md` | Servicios profesionales B2B |
| Genérico | `assets/generic.md` | Cualquier negocio que no encaje en los anteriores |

> La situación del sitio (Paso 1) tiene prioridad sobre el tipo de negocio. Un SaaS con penalización sigue primero el flujo de `situations.md → penalización`, luego usa `saas.md` como referencia de arquitectura.

> **Playbooks tácticos:** Una vez identificada la situación, cargar `assets/playbooks.md` para seleccionar la táctica según el diagnóstico: mangos bajitos, content refresh, gaps de keywords, SEO programático, topical authority, link building sprint, E-E-A-T boost o GEO sprint.

> **Roadmap de ejecución:** Para traducir el diagnóstico a fases, milestones y entregables con tiempos, cargar `assets/roadmap-framework.md`.

---

### Paso 3b: Modelo de venta — B2B vs B2C

Determinar antes de definir objetivos y tácticas. Cambia la estrategia de keywords, contenido, funnel, timelines y KPIs.

**Señales de detección:**

| Señal | B2B | B2C |
|-------|-----|-----|
| Buyer | Empresa / decision maker | Consumidor final |
| CTAs principales | "Solicitar demo", "Hablar con ventas", "Cotizar" | "Comprar", "Registrarse", "Descargar" |
| Precios | Sin precios públicos o precios enterprise | Precios visibles, carrito de compra |
| Ciclo de compra | Semanas / meses | Minutos / días |
| Ticket promedio | Alto | Bajo-medio |
| Contenido dominante | Case studies, whitepapers, ROI calculators, comparativas | Product pages, reviews, UGC, tutoriales |
| Social dominante | LinkedIn, G2, Capterra | Instagram, TikTok, Pinterest, Facebook |

**Diferencias estratégicas:**

| Dimensión | B2B | B2C |
|-----------|-----|-----|
| **Keyword approach** | Volumen bajo, alta intención, jargon de industria, long-tail | Volumen alto, branded + discovery, estacional |
| **Contenido prioritario** | Thought leadership, TOFU educativo largo, MOFU comparativas, BOFU demos | Páginas de producto, landing transaccional, contenido de tendencia |
| **Funnel** | TOFU → MOFU → BOFU alineado con el ciclo de ventas (largo) | Más corto: awareness → conversión directa |
| **Conversión objetivo** | Lead / MQL / demo request | Compra / signup / descarga |
| **Autoridad** | Trade publications, directorios de software (G2/Capterra), asociaciones industriales | Consumer media, influencers, UGC, reseñas de producto |
| **Timeline SEO** | 12-18 meses para ROI visible | 6-12 meses para impacto claro |
| **CRO priority** | Formulario de contacto, demo flow, trust (case studies, logos clientes) | Checkout, add-to-cart, social proof (reviews, UGC) |
| **AI visibility** | Crítico: buyers B2B usan ChatGPT/Perplexity para research de soluciones | Importante pero menos decisivo en compras de impulso |

**Casos B2B2C:** Empresas que venden a negocios (B2B) pero cuyo producto llega al consumidor final (ej. SaaS de RRHH que compra el dpto. de IT pero usan los empleados). Estrategia: contenido B2B para decision makers (ROI, integrations, security) + contenido B2C para usuarios finales (UX, how-to, tutorials). Dos audiencias, dos keyword clusters.

**Casos híbridos (B2B + B2C simultáneo):** Plataformas como marketplaces, herramientas freemium, o SaaS con plan individual y plan enterprise. Requieren arquitectura de contenido separada — no mezclar mensajes en las mismas URLs.

---

### Paso 4: Objetivos estratégicos

Basados en los datos reales del diagnóstico — no en expectativas genéricas ni benchmarks de industria desconectados de la situación real del cliente.

**Preguntas clave antes de definir objetivos:**
- ¿Cuál es la métrica principal que importa al negocio? (tráfico, leads, ventas, descargas de app, visibilidad de marca)
- ¿Qué dice la analítica sobre la tendencia actual? ¿creciendo, estancado, cayendo?
- ¿Desde qué países viene el tráfico actual y desde cuáles debería venir?
- ¿Qué canales están infrautilizados según los datos de GA4?
- ¿Qué dispositivo domina el tráfico? ¿el sitio está optimizado para ese dispositivo?
- ¿Qué es realista en 3/6/12 meses dado el DA, la situación del sitio, y los recursos disponibles?
- ¿Qué KPIs de SEO se conectan directamente con los objetivos de negocio?

**Los objetivos deben tener baseline real** — extraído de la analítica consolidada del Paso 1b:

| Métrica | Baseline real | 3 meses | 6 meses | 12 meses | Fuente baseline |
|---------|--------------|---------|---------|----------|----------------|
| Clics orgánicos/mes | | | | | GSC |
| Impresiones/mes | | | | | GSC |
| Keywords en top 10 | | | | | GSC / Ahrefs |
| Páginas indexadas | | | | | GSC |
| DA / DR | | | | | Ahrefs WMT |
| CWV passing (field) | | | | | GSC → CWV |
| Referring domains | | | | | Ahrefs WMT |
| Tráfico orgánico por país principal | | | | | GA4 |
| Conversiones desde orgánico | | | | | GA4 |
| [KPI de negocio específico] | | | | | |
| Ratings en app store (si app) | | | | | App Store Connect / Play Console |
| Reseñas Google (si local) | | | | | GBP |
| MQLs desde orgánico (si B2B) | | | | | CRM + GA4 |
| SQLs / demos desde orgánico (si B2B) | | | | | CRM |
| Pipeline generado desde orgánico (si B2B) | | | | | CRM |

---

### Paso 5: Decisiones estratégicas

El output real de la estrategia. Responde: ¿qué atacamos primero y por qué?

- **Prioridad 1:** ¿Qué problema bloquea todo lo demás? (si hay técnico grave, va primero)
- **Prioridad 2:** ¿Qué oportunidad tiene el mayor ratio impacto/esfuerzo?
- **Qué NO tocar todavía:** páginas que rankean bien sin intervención, redirects que ya funcionan, etc.
- **Orden de ataque general:** técnico → contenido → autoridad (puede variar según diagnóstico)

#### Frentes estratégicos a confirmar según diagnóstico

Cada frente detectado en el Paso 1 debe tener una decisión explícita: ¿entra en la estrategia? ¿en qué fase? ¿con qué prioridad?

| Frente | ¿Entra en estrategia? | Fase | Prioridad |
|--------|----------------------|------|-----------|
| SEO técnico | Siempre | 1 si hay bloqueos críticos | — |
| Rendimiento / CWV | Siempre | 1 si falla field data | — |
| Contenido on-page | Siempre | 1-2 | — |
| E-E-A-T | Siempre | 1-3 según profundidad | — |
| Enlazado interno | Siempre | 1-2 | — |
| Autoridad / link building | Siempre | 2-3 | — |
| SMO | Según presencia social activa | 1-2 | — |
| GEO / AI visibility | Siempre — en 2026 incluir desde Fase 1 para sitios SaaS/publisher | 1-2 | — |
| SEO Local | Si tiene/planea presencia local | 1 si es core del negocio | — |
| Internacional / hreflang | Si opera o planea expandirse | Según timing de expansión | — |
| ASO + directorios de software | Si tiene app o producto listable | 1-2 si es canal relevante | — |

> **Regla:** si el diagnóstico detectó un frente con potencial o problema, no puede quedar sin decisión explícita en este paso. "No aplica ahora" es una decisión válida — lo que no es válido es no haberlo evaluado.

#### Matriz impacto/esfuerzo

| Acción | Frente | Impacto | Esfuerzo | Fase |
|--------|--------|---------|----------|------|
| | | Alto/Medio/Bajo | Alto/Medio/Bajo | |

---

## CAPA 2 — PLAN TÁCTICO

Una vez definida la estrategia, el plan táctico traduce las decisiones en acciones concretas.

### Fase 1: Fundación (semanas 1-4)
- Correcciones técnicas críticas (errores de indexación, CWV bloqueantes)
- Páginas core (home, servicios/productos principales, contacto)
- Schema básico por tipo de página
- Tracking y analytics verificados

### Fase 2: Expansión (semanas 5-12)
- Contenido para páginas de prioridad media
- Blog / contenido de soporte (2-4 piezas/mes mínimo)
- Estructura de enlazado interno
- SEO local si aplica

### Fase 2b: GEO / AI Search (semanas 5-12 — integrar en paralelo con Expansión)
- Optimización de crawlers (training vs retrieval — permitir OAI-SearchBot, PerplexityBot)
- Entity density (15+ entidades por página clave)
- Passage-level citability en páginas top (bloques 134-167 palabras)
- llms.txt básico
- AI citation tracking: Otterly.ai o SE Ranking AI Tracker

**Nota 2026:** GEO ya no va en Fase 3. AI Overviews afecta 13%+ de queries y creciendo — iniciar desde Fase 1-2.

### Fase 3: Escala (semanas 13-24)
- Contenido avanzado y clusters temáticos
- Link building y outreach
- Optimización de rendimiento técnico

### Fase 4: Autoridad (meses 7-12)
- Thought leadership y contenido diferencial
- PR, menciones en medios
- Schema avanzado
- Optimización continua basada en datos

---

## Métricas 2026 — Qué medir y qué deprecar

### Métricas prioritarias 2026
| Métrica | Por qué |
|---------|---------|
| Revenue contribution por canal orgánico | Conecta SEO con negocio real |
| Conversion-weighted visibility | No todas las keywords valen igual |
| Share of AI Voice (SAIV) | % de queries donde tu marca aparece en AI responses |
| AI citation frequency | Cuántas veces te citan AI Overviews, ChatGPT, Perplexity |
| Topical authority score | Cobertura del cluster temático vs competidores |
| Engagement rate (GA4) | Reemplaza bounce rate |

### Métricas a deprecar
- **Bounce rate** → GA4 ya lo eliminó; usar engagement rate
- **DA/DR como KPI de éxito** → métrica de terceros, no de Google; una marca con DA 35 puede ganar a DA 65
- **Posición media como única métrica** → sin contexto de revenue ni SERP features, es engañosa
- **Tráfico orgánico sin revenue** → vanity metric sin conversiones

### GEO ROI Framework
```
GEO ROI = ((AI-Attributed Revenue − Total GEO Investment) / Total GEO Investment) × 100
```
AI search visitors convierten **4.4x más** que orgánico tradicional — medir separado.

### Timeline expectations realistas

#### B2C / SaaS con ciclo corto (free trial, freemium, ecommerce)

| Período | Qué esperar |
|---------|------------|
| Meses 1-3 | Mejoras técnicas visibles, indexación estabilizada, quick wins en long-tail |
| Meses 4-6 | Crecimiento en keywords de producto y categoría, conversiones orgánicas visibles |
| Meses 7-12 | Crecimiento fuerte si estrategia es sólida, ROI demostrable |
| Año 2+ | Compounding: autoridad + AI citations + brand |

#### B2B / SaaS enterprise / servicios profesionales

| Período | Qué esperar |
|---------|------------|
| Meses 1-3 | Técnico resuelto, indexación correcta, primeros contenidos TOFU publicados |
| Meses 4-6 | Tráfico TOFU creciendo, primeros MQLs desde orgánico (pocos, validar atribución) |
| Meses 7-12 | Pipeline atribuible a SEO visible, keywords de intención media/alta ganando terreno |
| Año 2+ | SEO como canal predecible de MQLs, compounding en keywords competitivos, AI visibility en queries de research de soluciones |

> **Por qué B2B tarda más:** El buyer journey es largo (semanas/meses). Un lead generado hoy puede convertir en cliente en 6-9 meses. El ROI real de SEO B2B no se ve en GA4 a corto plazo — requiere CRM attribution. Comunicar esto al cliente desde el día 1 para gestionar expectativas.

---

## Entregables

| Entregable | Contenido |
|------------|-----------|
| `SEO-STRATEGY.md` | Diagnóstico + decisiones estratégicas + KPIs |
| `IMPLEMENTATION-ROADMAP.md` | Plan táctico por fases con acciones concretas |
| `COMPETITOR-ANALYSIS.md` | Análisis competitivo con gaps identificados |
| `CONTENT-CALENDAR.md` | Calendario de contenido priorizado |
| `SITE-STRUCTURE.md` | Arquitectura URL y jerarquía de contenido |
| `GEO-VISIBILITY-BASELINE.md` | AI search visibility baseline + SAIV inicial |

---

## Skills relacionados por fase

| Fase | Skill | Cuándo usarlo |
|------|-------|---------------|
| Diagnóstico técnico | `/seo technical <url>` | Crawlability, CWV, indexación |
| Diagnóstico de contenido | `/seo content <url>` | Calidad, E-E-A-T, intención |
| Diagnóstico de autoridad | `/seo backlinks <domain>` | Perfil de backlinks |
| Datos reales de tráfico | `/seo google gsc <property>` | GSC performance data |
| Análisis competitivo | `/seo competitive [domain]` | Gaps de keywords y contenido |
| KW research | `/seo keywords [topic]` | Clusters, intención, calendario |
| Arquitectura IA | `/seo architecture create [topic]` | Silos, jerarquía URL, enlazado interno |
| SEO local | `/seo local <url>` | GBP, NAP, páginas de ubicación |
| Contenido programático | `/seo programmatic [url]` | Páginas template a escala |
| Datos en volumen | `/seo dataforseo competitors <domain>` | Tráfico estimado, overlap de KW |
| Reporting | `/seo reporting [domain]` | KPIs mensuales, ROI, dashboards |

---

## DataForSEO Integration (opcional)

Si los MCP tools de DataForSEO están disponibles:
- `dataforseo_labs_google_competitors_domain` → competidores reales en SERP
- `dataforseo_labs_google_domain_intersection` → overlap de keywords con competidores
- `dataforseo_labs_bulk_traffic_estimation` → estimación de tráfico
- `kw_data_google_ads_search_volume` + `dataforseo_labs_bulk_keyword_difficulty` → KW research
- `business_data_business_listings_search` → datos de negocio local

---

## Error Handling

| Escenario | Acción |
|-----------|--------|
| Tipo de negocio no reconocido | Usar `generic.md`. Informar al usuario y continuar |
| Sin URL del sitio | Modo new-site. Saltar diagnóstico de sitio existente |
| Template de industria no encontrado | Usar `generic.md`. Registrar el template faltante en el output |
| Sin acceso a GSC/GA4 | Advertir antes de continuar. El diagnóstico será incompleto |
| Situación del sitio poco clara | Preguntar al usuario: ¿hay caída de tráfico? ¿cuándo se lanzó el sitio? |
