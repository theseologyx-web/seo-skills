---
name: seo-benchmark
description: >
  SEO benchmarking against industry averages. Compares CWV by industry vertical,
  CTR by SERP position, domain authority, content length, backlinks, and local signals
  against sector standards — not specific competitors. Use when user says "benchmark",
  "comparar con la industria", "estoy por encima o debajo de la media", "average industria",
  "qué es normal para mi sector", "cómo estoy vs el mercado", or "industry standards".
  Not for competitor strategy or keyword gap analysis — use seo-competitive.
user-invokable: true
argument-hint: "[url o dominio] [--industry saas|ecommerce|local|finance|health|media|travel|education|realestate|b2b]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.1.0"
  category: seo
---

# SEO Benchmark — Comparación vs. Promedios de Industria

Compara las métricas SEO de un sitio contra los promedios de su sector. Diferente de `seo-competitive` (que compara contra competidores específicos): aquí la referencia son los estándares de la industria, útiles para contexto en reportes de cliente y para priorizar esfuerzos.

> **Fuentes de datos:** Google CrUX Tech Report 2024, Backlinko CTR Study 2022 (n=4M keywords), Advanced Web Ranking CTR data 2024, Ahrefs Industry Reports 2024, Search Engine Journal Benchmarks 2025.

---

## Detección de Industria

Si no se especifica `--industry`, detectar desde el sitio:

| Señal | Industria |
|-------|-----------|
| /pricing, /features, /docs, "free trial", "sign up" | SaaS / Tech |
| /cart, /checkout, /products, /collections, "add to cart" | Ecommerce |
| dirección física, teléfono, "serving [city]" | Local Service |
| /cotizar, /inversión, "tasa", "rendimiento", "crédito" | Finance / Fintech |
| /pacientes, /tratamientos, /doctores, "consulta" | Health / Healthcare |
| /noticias, /artículos, /categorías, article schema | Media / Publisher |
| /hoteles, /vuelos, /destinos, "reservar", "tarifa" | Travel |
| /cursos, /lecciones, /certificaciones, "inscribirse" | Education |
| /propiedades, /inmuebles, /listings, "metros cuadrados", "habitaciones" | Real Estate |
| /soluciones, /empresa, /clientes, "solicitar demo", "hablar con ventas", no ecommerce | B2B |

---

## 1. Core Web Vitals Benchmarks por Industria

**Fuente:** Google CrUX Technology Report 2024 (datos de campo, 75th percentile, mobile)

| Industria | LCP bueno (≤2.5s) | INP bueno (≤200ms) | CLS bueno (≤0.1) | % sitios "Good" (todos) |
|-----------|------------------|--------------------|-----------------|------------------------|
| **SaaS / Tech** | 62% | 71% | 78% | 48% |
| **Ecommerce** | 41% | 58% | 65% | 32% |
| **Finance / Fintech** | 55% | 67% | 72% | 44% |
| **Health / Healthcare** | 49% | 63% | 69% | 38% |
| **Media / Publisher** | 38% | 54% | 61% | 28% |
| **Travel** | 44% | 61% | 67% | 34% |
| **Education** | 58% | 69% | 75% | 45% |
| **Local Services** | 53% | 65% | 71% | 41% |
| **Promedio global** | 51% | 64% | 70% | 39% |

**Interpretación:**
- Si tu LCP está en Good y el 62% de SaaS también → estás en la media, no destaca
- Si tu LCP está en Good y solo el 38% de Media lo logra → ventaja competitiva significativa
- El % "Good" en todos significa que LCP + INP + CLS son simultáneamente Good

### Benchmarks numéricos por industria (mediana, móvil)

| Industria | LCP mediana | INP mediana | CLS mediana | TTFB mediana |
|-----------|------------|------------|------------|-------------|
| SaaS / Tech | 2.8s | 175ms | 0.07 | 0.6s |
| Ecommerce | 3.5s | 245ms | 0.11 | 0.8s |
| Finance | 3.1s | 205ms | 0.09 | 0.7s |
| Health | 3.2s | 220ms | 0.10 | 0.75s |
| Media / Publisher | 2.9s | 195ms | 0.09 | 0.65s |
| Travel | 3.4s | 230ms | 0.10 | 0.78s |
| Education | 2.7s | 170ms | 0.08 | 0.58s |
| Local Services | 3.0s | 210ms | 0.09 | 0.70s |

> Usar CrUX API (via `seo-google`) para obtener datos reales del sitio y comparar contra esta tabla.

---

## 2. CTR Benchmarks por Posición SERP

**⚠️ Contexto 2026:** El 69% de las búsquedas son zero-click. AI Overviews (presentes en ~13.14% de queries) reducen el CTR pos 1 de ~19% a ~13.8% en esas queries. Los benchmarks pre-2023 son misleading — usar solo como referencia relativa, no absoluta.

**Fuente:** Backlinko (n=4M keywords, 2022 — referencia histórica) + Advanced Web Ranking 2024 + datos post-AI Overviews 2026

### CTR orgánico por posición (promedio general)

| Posición | CTR promedio (2022 ref) | CTR 2026 sin AIO | CTR 2026 con AIO presente |
|----------|------------------------|-----------------|--------------------------|
| **1** | 27.6% *(dato 2022)* | ~19% | ~13.8% |
| **2** | 15.8% | ~10.5% | ~7.5% |
| **3** | 11.0% | ~7.5% | ~5.5% |
| **4** | 8.4% | ~5.5% | ~4.0% |
| **5** | 6.3% | ~4.2% | ~3.0% |
| **6** | 4.9% | ~3.2% | ~2.3% |
| **7** | 3.9% | ~2.5% | ~1.8% |
| **8** | 3.3% | ~2.1% | ~1.5% |
| **9** | 2.7% | ~1.7% | ~1.2% |
| **10** | 2.5% | ~1.5% | ~1.1% |
| **11-20** (pág 2) | 0.5-1.5% | ~0.3-0.8% | ~0.2-0.5% |

> **Regla 10:1:** Pasar de posición 10 a posición 1 multiplica los clics por ~11x.

> **Mobile vs Desktop CTR:** El CTR en móvil es típicamente 30-40% menor que en desktop para la misma posición. Benchmark de referencia: pos 1 desktop ~22-25% sin AIO, pos 1 móvil ~14-17% sin AIO. GSC permite filtrar por device — usar siempre ese filtro para comparar correctamente.

### CTR por tipo de query

| Tipo de búsqueda | CTR pos 1 típico | Por qué varía |
|-----------------|-----------------|---------------|
| Branded (nombre de marca) | 50-70% | Alta intención, sin distractores |
| Navigational (ir a sitio específico) | 40-60% | Usuario ya sabe a dónde va |
| Informational ("qué es X", "cómo hacer X") | 20-35% | Compite con featured snippet, PAA |
| Commercial ("mejor X", "X vs Y") | 15-25% | Ads + shopping results compiten |
| Transactional ("comprar X", "precio X") | 10-20% | Ads muy presentes en este tipo |
| Local ("X cerca de mí", "X en [ciudad]") | 15-30% | Compite con Local Pack (map results) |

### Zero-Click Benchmarks (2024-2026)

**Fuente:** SparkToro / Rand Fishkin 2024, SimilarWeb Zero-Click Study 2024

| Métrica | Dato |
|---------|------|
| Búsquedas que terminan en zero-click (global) | **69%** |
| Búsquedas que terminan en clic orgánico | ~22% |
| Búsquedas que terminan en clic a Google (Maps, Shopping, etc.) | ~9% |

#### Zero-click rate por tipo de query

| Tipo de query | Zero-click rate estimado | Razón principal |
|--------------|--------------------------|----------------|
| Informational ("qué es X", "cuánto mide X") | ~75-85% | Knowledge Panel, PAA, AIO responde directamente |
| Navigational ("Gmail login", "Amazon") | ~25-35% | Usuario sí hace clic al sitio destino |
| Local ("restaurante cerca", "fontanero Madrid") | ~40-55% | Local Pack satisface la necesidad in-SERP |
| Branded (nombre de empresa) | ~15-25% | Alta intención de clic al sitio |
| Commercial ("mejor X", "X vs Y") | ~45-60% | AIO + comparativas in-SERP creciendo |
| Transactional ("comprar X", "precio X") | ~30-45% | Ads + Shopping consumen clics pagos, orgánico sí recibe |

#### Implicaciones para estrategia SEO

```
Queries informacionales → Optimizar para AI citations (GEO) más que para clic
Queries branded + transaccionales → Mayor ROI del clic orgánico → priorizar estas
Queries con featured snippet → CTR pos 1 puede bajar al 19-24% si no eres el snippet
Medir con GSC: CTR < 5% en pos 1-3 → probable zero-click SERP → investigar tipo de query
```

### Señales de CTR anómalo (oportunidad)

```
CTR real (GSC) < Benchmark por posición → Oportunidad de mejorar title/meta description
CTR real (GSC) > Benchmark por posición → Title/meta funciona bien → replicar patrón
CTR bajo en posición 1-3 → SERP tiene featured snippet, PAA, AIO o ads que roban clics
CTR < 2% en posición 1 → SERP casi seguro zero-click → evaluar si vale la pena rankear o solo AI visibility
```

---

## 3. Benchmarks de Autoridad de Dominio

**Fuente:** Ahrefs DR / Moz DA distribución por antigüedad y sector (2024)

### Domain Rating (Ahrefs DR) por antigüedad

| Antigüedad del sitio | DR mediana | DR top 25% |
|---------------------|-----------|-----------|
| < 1 año | 5-15 | 20-35 |
| 1-2 años | 15-30 | 35-50 |
| 2-4 años | 25-45 | 45-60 |
| 4-7 años | 35-55 | 55-70 |
| > 7 años | 45-65 | 65-80 |

### Referring Domains necesarios para rankear (por competitividad)

| KD (Keyword Difficulty) | Referring domains mínimos típicos |
|------------------------|----------------------------------|
| KD 0-20 (baja) | 5-30 RDs |
| KD 20-40 (media-baja) | 30-100 RDs |
| KD 40-60 (media) | 100-300 RDs |
| KD 60-80 (alta) | 300-800 RDs |
| KD 80-100 (muy alta) | 800+ RDs |

### Benchmarks de referring domains por industria (sitios que rankean)

| Industria | RDs mediana (sitios pág 1) | RDs para top 3 |
|-----------|--------------------------|----------------|
| SaaS / Tech | 150-400 RDs | 400-1,200 RDs |
| Ecommerce | 100-300 RDs | 300-900 RDs |
| Finance / Legal | 300-800 RDs | 800-2,000 RDs |
| Health / Medical | 200-600 RDs | 600-1,500 RDs |
| Local Services | 20-80 RDs | 50-200 RDs |
| Media / Blog | 50-200 RDs | 200-600 RDs |

> Para Local, el número de RDs importa menos que la relevancia geográfica y las citas NAP.

---

## 4. Benchmarks de Contenido

### Longitud de contenido por tipo de página (para rankear)

**Fuente:** Ahrefs Content Analysis 2024, Semrush State of Content Marketing 2024

| Tipo de página | Palabras mínimas para rankear | Óptimo para top 3 | Evitar si |
|---------------|------------------------------|------------------|-----------|
| Blog post / artículo | 800 palabras | 1,500-2,500 palabras | > 5,000 sin estructura clara |
| Página de servicio | 400 palabras | 800-1,500 palabras | < 300 → thin content |
| Página de producto (ecom) | 200 palabras | 400-800 palabras | < 100 → thin |
| Homepage SaaS | 300 palabras | 500-1,000 palabras | > 2,000 sin secciones |
| Página de pricing | 200 palabras | 400-700 palabras | — |
| Landing page | 300 palabras | 600-1,200 palabras | — |
| FAQ page | 500 palabras | 1,000-2,000 palabras | — |
| Caso de estudio | 800 palabras | 1,500-3,000 palabras | — |
| Pillar page / guía | 2,000 palabras | 3,000-6,000 palabras | — |

### Frecuencia de publicación por tipo de sitio

| Tipo de sitio | Frecuencia competitiva | Mínimo para crecer |
|---------------|----------------------|-------------------|
| Blog de nicho | 4-8 posts/mes | 2 posts/mes |
| SaaS blog | 2-4 posts/mes | 1 post/mes |
| Media / Publisher | 10-30 artículos/mes | 8 artículos/mes |
| Ecommerce (categorías) | 1-2 páginas nuevas/mes | Actualizar existentes |
| Local business | 1-2 posts/mes | 1 post/mes |

### Freshness benchmark (cuándo actualizar)

| Tipo de contenido | Vida útil antes de actualizar |
|------------------|------------------------------|
| News / trending | Días |
| How-to / tutoriales | 12-18 meses |
| Best-of / comparativas | 6-12 meses |
| Guías definitivas | 18-24 meses |
| Páginas de servicio | 24-36 meses (o si cambia el negocio) |
| Páginas de producto | Al cambiar el producto |

### Freshness decay rates (pérdida de tráfico sin actualizar)

**Fuente:** Ahrefs Content Decay Study 2024, HubSpot Blog Research 2024

| Tipo de contenido | Pérdida mensual promedio | Pérdida a 12 meses | Señal de urgencia |
|------------------|------------------------|-------------------|------------------|
| Comparativas / "mejor X" | 5-8%/mes | 40-65% del tráfico | Caída > 20% en 3 meses |
| How-to / tutoriales con herramientas | 3-5%/mes | 25-45% | Caída > 15% en 3 meses |
| Guías definitivas evergreen | 1-2%/mes | 10-20% | Caída > 10% en 6 meses |
| News / trending | 50-80%/mes | ~0% residual | Inmediato si hay update |
| Páginas de servicio | < 1%/mes | < 10% | Solo si hay cambio de negocio |

> **Cómo detectar decay:** GSC → filtrar URL específica → últimos 16 meses → buscar tendencia descendente sostenida. Caída en impresiones + posición = decay real. Caída solo en clics = SERP se volvió más zero-click.

### Topical authority benchmarks

**Fuente:** Ahrefs Topical Authority Study 2024, Semrush Content Marketing 2025

| Métrica | Mínimo para señal de autoridad | Competitivo | Líder de nicho |
|---------|-------------------------------|-------------|----------------|
| Artículos en un cluster temático | 5-8 artículos | 15-25 artículos | 40+ artículos |
| Cobertura de subtemas (% de KWs del cluster) | 30-40% | 60-75% | > 85% |
| Internal links entre artículos del cluster | 2-3 por artículo | 4-6 por artículo | 7+ por artículo |
| Pillar page con links a todos los artículos | Presente | Presente + actualizada | Presente + datos propios |

> **Regla de topical authority:** Google premia la profundidad temática. Un sitio con 20 artículos sobre "email marketing" puede superar a uno con DA mayor que tiene 2 artículos sobre el tema. El benchmark no es cuántos artículos tienes en total, sino cuántos tienes **en el cluster específico** de las keywords objetivo.

---

## 5. Benchmarks Técnicos SEO

### Index Ratio (páginas indexadas / páginas totales)

| Situación | Index ratio saludable | Señal de problema |
|-----------|----------------------|------------------|
| Blog / publisher | 70-90% | < 60% → demasiadas páginas bloqueadas o thin |
| Ecommerce | 50-80% | > 90% → posible index bloat (variantes, filtros) |
| SaaS | 75-95% | < 50% → páginas importantes no indexadas |
| Local | 80-95% | < 70% → páginas de localización mal configuradas |

### Core Web Vitals aprobación (Pass Rate esperado)

Para que Google considere el sitio en "buena forma" de CWV, el 75% de las sesiones deben pasar cada métrica.

**Fuente:** Google CrUX Tech Report 2024 (datos de campo reales)

#### Pass rates globales — TODOS los dispositivos

| Meta | Pass rate global (all devices) |
|------|-------------------------------|
| LCP Good en 75%+ sesiones | ~51% de sitios |
| INP Good en 75%+ sesiones | ~64% de sitios |
| CLS Good en 75%+ sesiones | ~70% de sitios |
| **Todos Good simultáneamente** | **~39% de sitios** |

#### Pass rates — Mobile únicamente (más exigente)

| Meta | Pass rate mobile |
|------|----------------|
| LCP Good (móvil) | ~44% de sitios |
| INP Good (móvil) | ~58% de sitios |
| CLS Good (móvil) | ~66% de sitios |
| **Todos Good simultáneamente (móvil)** | **~48% de sitios** |

> **Por qué móvil es diferente:** Los dispositivos móviles tienen menos CPU/RAM, conexiones más lentas y mayor variabilidad. Un sitio puede pasar CWV en desktop y fallar en móvil. Google usa las métricas de campo del dispositivo del usuario, por lo que móvil es el punto de referencia crítico para la mayoría de sectores (>60% del tráfico es móvil en casi todos los verticales).

> **Nota:** El 48% de pass rate móvil (todos Good) vs 39% global se debe a que CrUX pondera por visitas y los sitios con más tráfico móvil tienden a ser los más optimizados (grandes marcas, publishers).

### Crawl Budget (sitios > 1,000 páginas)

| Métrica | Saludable | Problema |
|---------|-----------|---------|
| % páginas crawleadas en 30 días | > 80% | < 60% → páginas importantes no se recrawlean |
| Ratio páginas útiles / crawleadas | > 70% | < 50% → crawl wasters (redirects, 404s, params) |
| Frecuencia de re-crawl (páginas clave) | < 7 días | > 30 días → señal de baja autoridad |

---

## 6. Benchmarks Local SEO

**Aplica cuando:** negocio con presencia física, SAB o multi-ubicación.

### Google Business Profile — benchmarks por industria

| Métrica | Restaurante | Médico/Clínica | Abogado | Servicio del hogar | Retail |
|---------|------------|---------------|---------|-------------------|--------|
| Reviews mínimas para aparecer prominente | 25-50 | 15-30 | 10-20 | 15-30 | 30-60 |
| Rating promedio competitivo | ≥ 4.3 | ≥ 4.4 | ≥ 4.2 | ≥ 4.3 | ≥ 4.2 |
| Fotos mínimas para perfil completo | 20+ | 10+ | 8+ | 10+ | 15+ |
| Frecuencia de posts recomendada | 2-4/semana | 1-2/semana | 1/semana | 1-2/semana | 2-4/semana |
| Respuestas a reviews (%) | > 80% | > 90% | > 85% | > 80% | > 75% |

### Velocidad de respuesta a reviews

| Tiempo de respuesta | Impacto en GBP signals |
|--------------------|-----------------------|
| < 24h | ✅ Señal fuerte de engagement |
| 1-3 días | 🟡 Aceptable |
| > 7 días | 🔴 Señal negativa |
| Sin respuesta | 🔴 Señal muy negativa para YMYL |

### Review velocity benchmarks (ritmo de nuevas reviews)

**Fuente:** BrightLocal Local Consumer Review Survey 2024, Whitespark Local Search Ranking Factors 2024

| Tipo de negocio | Reviews/mes para crecer en pack | Reviews/mes para mantener posición |
|----------------|--------------------------------|-------------------------------------|
| Restaurante | 8-15/mes | 3-5/mes |
| Médico / Clínica | 4-8/mes | 2-3/mes |
| Abogado | 3-6/mes | 1-2/mes |
| Servicio del hogar | 4-8/mes | 2-3/mes |
| Retail / Tienda | 5-10/mes | 2-4/mes |
| Hotel / Alojamiento | 10-20/mes | 5-8/mes |

> **Regla de review velocity:** Google premia la consistencia sobre los picos. 5 reviews/mes durante 12 meses supera a 60 reviews en un mes. Un pico brusco de reviews puede activar filtros de spam.

### GBP engagement benchmarks

**Fuente:** Google Business Profile Insights (promedios por categoría, 2024)

| Métrica GBP | Bajo (oportunidad) | Promedio competitivo | Alto (top performer) |
|-------------|-------------------|---------------------|---------------------|
| Vistas de fotos / mes | < 200 | 500-2,000 | > 5,000 |
| Búsquedas directas (nombre de marca) | < 30% del total | 40-60% | > 70% |
| Búsquedas por descubrimiento (categoría) | > 70% | 40-60% | — |
| Clics a sitio web desde GBP | < 50/mes | 100-400/mes | > 800/mes |
| Solicitudes de ruta / mes | < 20 | 50-200 | > 400 |
| Llamadas directas desde GBP / mes | < 10 | 30-100 | > 200 |

> **Interpretación:** Alto % de búsquedas directas = brand awareness fuerte. Alto % de búsquedas por descubrimiento = oportunidad (usuarios nuevos te encuentran), pero también señal de que el nombre de marca no es suficientemente conocido aún.

### Apple Business Connect (nuevo benchmark 2026)

**Fuente:** Apple Maps Connect data 2025, BrightLocal 2025

Apple Maps tiene ~25-30% de cuota en iOS USA. Benchmark mínimo para presencia competitiva:

| Señal | Mínimo | Competitivo |
|-------|--------|-------------|
| Perfil reclamado y verificado | Sí | Sí |
| Fotos de negocio | 5+ | 15+ |
| Horarios actualizados | Sí | Sí + horarios especiales |
| Showcase (equivalente a GBP posts) | — | 1-2/mes |

> En USA, ignorar Apple Maps significa perder visibilidad en ~30% de búsquedas locales móviles. Prioridad especialmente para clientes SaaS/B2C con audiencia iPhone.

---

## 7. Conversion Rate Benchmarks por Industria

**Fuente:** WordStream Industry Benchmarks 2024, Unbounce Conversion Benchmark Report 2024, Semrush eCommerce Report 2024

> **Conversión en SEO:** el tráfico orgánico generalmente convierte peor que branded PPC pero mejor que display. Usa estos benchmarks para contextualizar el CVR orgánico del cliente.

### Tasa de conversión por industria (tráfico orgánico)

| Industria | CVR bajo | CVR promedio | CVR alto (top 25%) | Definición de conversión |
|-----------|---------|-------------|-------------------|--------------------------|
| **Ecommerce (compra)** | 0.5% | 1.5-2.5% | 3.5-5% | Transacción completada |
| **SaaS — Free Trial** | 1% | 2-5% | 7-10% | Registro trial |
| **SaaS — Demo request** | 0.5% | 1.5-3% | 5-7% | Formulario demo |
| **SaaS — Freemium signup** | 3% | 5-10% | 15% | Registro gratuito |
| **Finance / Seguro (lead)** | 2% | 4-8% | 10-15% | Formulario / cotización |
| **Health / Healthcare (cita)** | 2% | 3-6% | 8-12% | Reserva de cita |
| **Legal (lead)** | 2% | 3-7% | 10% | Formulario de contacto |
| **B2B (lead gen)** | 0.5% | 1-3% | 5-8% | Form / descarga / demo |
| **Educación (inscripción)** | 1% | 2-5% | 8% | Registro / matrícula |
| **Local Services (contacto)** | 3% | 5-10% | 15% | Llamada / formulario |
| **Travel (reserva)** | 0.5% | 1-3% | 5% | Reserva completada |
| **Real Estate (lead)** | 1% | 2-5% | 8% | Formulario / contacto |

### Conversion rate: orgánico vs. otros canales

| Canal | CVR relativo vs. orgánico |
|-------|--------------------------|
| Branded PPC | 2-4x más alto que orgánico |
| Email marketing | 2-3x más alto |
| Orgánico (SEO) | Referencia base (1x) |
| Display / Programático | 0.1-0.3x del orgánico |
| Social orgánico | 0.3-0.5x |
| Social paid | 0.5-1x |

### Señales de CVR anómalo

```
CVR orgánico < mitad del benchmark industria → Problema de UX, trust signals o intent mismatch
CVR orgánico > top 25% benchmark → Documentar qué funciona → replicar en otras landings
CVR muy bajo en queries transaccionales → Revisar page relevance, CTAs, velocidad móvil
CVR bueno en desktop pero bajo en móvil → CWV o UX móvil bloqueando conversiones
```

> **Para reportar a cliente:** Contextualizar siempre. Un CVR del 2% en SaaS puede ser excelente (si vende demos) o bajo (si vende trials gratuitos). Especificar siempre el tipo de conversión.

---

## 8. Engagement Rate Benchmarks (GA4)

**Fuente:** Databox Industry Benchmarks 2024, HubSpot Marketing Benchmarks 2025, Semrush Analytics 2024

> **Nota GA4:** GA4 reemplazó "bounce rate" por **Engagement Rate** = sesiones con ≥1 evento de conversión, ≥2 páginas vistas, o duración ≥10 segundos. No son comparables directamente. La industria tiene benchmarks tanto del "engagement rate GA4" como del viejo "bounce rate" — se presentan ambos para compatibilidad con clientes con datos históricos.

### Engagement Rate (GA4) por industria

| Industria | Engagement rate bajo | Promedio | Alto (top 25%) |
|-----------|---------------------|---------|---------------|
| **SaaS / Tech** | < 50% | 55-65% | > 70% |
| **Ecommerce** | < 45% | 50-60% | > 65% |
| **Finance / Fintech** | < 48% | 54-62% | > 68% |
| **Health / Healthcare** | < 52% | 58-68% | > 72% |
| **Media / Publisher** | < 35% | 40-55% | > 60% |
| **Travel** | < 45% | 52-62% | > 68% |
| **Education** | < 55% | 60-70% | > 75% |
| **Local Services** | < 50% | 55-65% | > 70% |
| **B2B** | < 52% | 58-68% | > 72% |

> **Referencia inversa (bounce rate antiguo):** engagement rate de 55% ≈ bounce rate de 45%. Un "buen" bounce rate pre-GA4 era < 40% para blogs de nicho, < 60% para ecommerce.

### Duración media de sesión por industria

| Industria | Duración baja | Promedio | Alta |
|-----------|--------------|---------|------|
| **SaaS / Tech** | < 1:30 min | 2:30-4:00 min | > 4:30 min |
| **Ecommerce** | < 2:00 min | 3:00-5:00 min | > 6:00 min |
| **Finance** | < 2:00 min | 3:00-4:30 min | > 5:00 min |
| **Health** | < 2:30 min | 3:30-5:00 min | > 5:30 min |
| **Media / Blog** | < 1:00 min | 2:00-3:30 min | > 4:00 min |
| **Education** | < 3:00 min | 4:00-6:00 min | > 7:00 min |
| **Local Services** | < 1:30 min | 2:00-3:30 min | > 4:00 min |

### Páginas por sesión por industria

| Industria | Bajo | Promedio | Alto |
|-----------|------|---------|------|
| SaaS / Tech | < 1.5 | 2.0-3.0 | > 3.5 |
| Ecommerce | < 2.5 | 3.5-6.0 | > 7.0 |
| Finance | < 1.5 | 2.0-3.5 | > 4.0 |
| Media / Publisher | < 1.5 | 2.0-4.0 | > 5.0 |
| Education | < 2.0 | 3.0-5.0 | > 6.0 |
| Local Services | < 1.3 | 1.5-2.5 | > 3.0 |

### Señales de engagement anómalo

```
Engagement rate < 40% en SaaS → Problema de relevancia del tráfico o UX
Duración < 30s promedio → Pogo-sticking → señal negativa para Google
Páginas/sesión > 8 en local → Navegación confusa (no es positivo, es perdido)
Engagement rate alto + CVR bajo → El contenido engancha pero no convierte → revisar CTAs
```

---

## 9. Benchmarks de Visibilidad AI (GEO)

**Aplica en 2025-2026 para clientes que quieren aparecer en AI Overviews / ChatGPT / Perplexity**

**Fuente:** Semrush AI Visibility Study 2025, BrightEdge AI Search Research 2025, Authoritas AI Overviews Study 2024, datos de Otterly.ai y Peec AI (2025)

### Adopción técnica AI (benchmark de mercado)

| Señal técnica | % sitios top 1M | Interpretación |
|---------------|----------------|---------------|
| llms.txt presente | ~8% | Adopción temprana — ventaja si se implementa ahora |
| GPTBot permitido (no bloqueado) | ~72% | Mayoría lo permite — bloquearlo = quedar fuera de ChatGPT |
| ClaudeBot permitido | ~68% | Mayoría lo permite — relevante para Claude |
| PerplexityBot permitido | ~65% | Creciendo — importante para Perplexity citations |
| OAI-SearchBot permitido | ~70% | SearchGPT / ChatGPT Search — crítico para SaaS USA |
| Structured data ≥ 1 tipo | ~44% de sitios | Mínimo para AI entity understanding |
| Structured data ≥ 3 tipos relevantes | ~18% de sitios | Benchmark competitivo para AI visibility |
| FAQ schema presente | ~22% de sitios | Alta correlación con AI Overviews citations |

### AI Overviews — cobertura por tipo de query

**Fuente:** BrightEdge AI Search Research 2025, Authoritas 2024

| Tipo de query | % queries con AIO presente | Tendencia |
|--------------|--------------------------|-----------|
| Informational ("cómo hacer X", "qué es X") | 25-40% | Creciendo |
| Commercial ("mejor X", "X vs Y") | 10-20% | Moderado |
| Health / Medical | 30-45% | Alto (YMYL tratado con cuidado) |
| Finance / Legal | 15-25% | Moderado (YMYL) |
| Local ("X en [ciudad]") | 5-12% | Bajo — Local Pack sigue dominando |
| Transactional ("comprar X") | 3-8% | Bajo |
| Branded | 2-5% | Muy bajo |

> **Implicación:** Si tu cliente tiene queries principalmente informacionales, el impacto de AIO es MAYOR. Queries transaccionales/branded son más seguros.

### Benchmarks de content para AI citations

**Fuente:** Semrush AI Visibility Study 2025

| Factor de contenido | Benchmark mínimo | Óptimo para citations |
|--------------------|-----------------|----------------------|
| Longitud de página (AI Mode citations) | — | > 20,000 chars = **10.18 citations avg** |
| Formato listicle / numbered list | — | **40.86% de AI Mode citations** provienen de listicles |
| Párrafos citables (> 40 palabras, self-contained) | — | > 60% del contenido principal |
| Schema markup ≥ 2 tipos | Presente en top AI-cited pages | Mínimo para trust signals |
| E-E-A-T signals (autor con bio, fecha, fuentes) | Presente en ~55% de AI-cited pages | Recomendado para YMYL |
| HTTPS + Core Web Vitals Good | Presente en ~80% de AI-cited pages | Prerequisito |

### SAIV — Share of AI Visibility (métrica principal 2026)

**Qué mide:** % de veces que tu marca/dominio aparece citado en respuestas de AI para un set de queries objetivo.

| SAIV benchmark | Situación |
|---------------|-----------|
| SAIV = 0% | No visible en AI → gap crítico en GEO |
| SAIV 1-10% | Visibilidad incipiente → oportunidad de crecimiento |
| SAIV 10-30% | Presencia establecida en AI para ese vertical |
| SAIV > 30% | Liderazgo AI → defensivo (mantener citations) |
| SAIV > 50% | Dominancia en AI (raro, típico de Wikipedia/grandes marcas) |

> **Herramientas para medir SAIV:** Otterly.ai, Peec AI, Rank Prompt, SE Ranking AI Visibility. No hay estándar único aún — establecer baseline con queries del cliente y trackear mes a mes.

### Señales de AI visibility gap

```
GPTBot bloqueado en robots.txt → Excluido de ChatGPT → fix inmediato
Sin structured data → Entidad no reconocida → añadir Organization + FAQ + Article schema
Contenido sin párrafos citables (todo bullet points sin contexto) → No citable por AI
Sin llms.txt → No declara preferencias AI → oportunidad diferencial
SAIV = 0% en queries de marca propia → Urgente: contenido E-E-A-T + citations externas
```

---

## Proceso de Benchmarking

### Paso 1 — Recopilar métricas del sitio

```bash
# CWV reales (field data)
# → usar seo-google para CrUX API

# Index ratio
# → site:dominio.com en Google → páginas indexadas
# → comparar con total de URLs en sitemap

# CTR por posición
# → GSC: Performance → comparar CTR real vs benchmark

# Referring domains y DR
# → Ahrefs / Semrush / Moz (datos del cliente)
```

### Paso 2 — Detectar industria y seleccionar benchmarks

Usar tabla de detección de industria del inicio.

### Paso 3 — Calcular posición vs benchmark

Para cada métrica:
```
Estado = "Por encima" si métrica_sitio > benchmark_industria
Estado = "En la media" si diferencia < 10%
Estado = "Por debajo" si métrica_sitio < benchmark_industria
Gap = ((benchmark_industria - métrica_sitio) / benchmark_industria) × 100
```

### Paso 4 — Priorizar por impacto

Métricas más lejos del benchmark + mayor impacto en ranking = prioridad más alta.

---

## Output Format

### SEO Benchmark Report

**Dominio:** [dominio]
**Industria detectada:** [industria]
**Fecha:** [fecha]

#### Dashboard de Posición vs. Industria

| Categoría | Tu Métrica | Benchmark Industria | Posición | Gap |
|-----------|-----------|--------------------|---------|----|
| LCP (móvil) | X.Xs | X.Xs mediana | 🟢 Arriba / 🟡 Media / 🔴 Abajo | X% |
| INP (móvil) | XXXms | XXXms mediana | 🟢/🟡/🔴 | X% |
| CLS | X.XX | X.XX mediana | 🟢/🟡/🔴 | X% |
| CTR posición 1 | X% | ~19% (2026 sin AIO) | 🟢/🟡/🔴 | X% |
| Zero-click exposure | X% queries informacionales | ~75-85% zero-click | 🟢/🟡/🔴 | — |
| Engagement Rate (GA4) | X% | X% rango industria | 🟢/🟡/🔴 | X% |
| Conversion Rate (orgánico) | X% | X% rango industria | 🟢/🟡/🔴 | X% |
| Domain Rating / DA | XX | XX mediana industria | 🟢/🟡/🔴 | — |
| Referring Domains | X | X mediana pág 1 | 🟢/🟡/🔴 | X RDs |
| Index Ratio | X% | X% rango saludable | 🟢/🟡/🔴 | — |
| SAIV (AI Visibility) | X% | 10-30% establecido | 🟢/🟡/🔴 | — |

#### Análisis por Área

**Core Web Vitals:**
[Comparación vs benchmarks de industria + interpretación]

**CTR y SERP:**
[CTR real (GSC) vs benchmark por posición + oportunidades]

**Autoridad:**
[DR/DA y RDs vs. lo necesario para el sector + gap a cerrar]

**Contenido:**
[Longitud, frecuencia y freshness vs. benchmarks del sector]

#### Prioridades Benchmark

| Prioridad | Área | Brecha actual | Acción recomendada |
|-----------|------|--------------|-------------------|
| 🔴 | [Área] | X% debajo del benchmark | [Acción] |
| 🟡 | [Área] | X% debajo del benchmark | [Acción] |
| 🟢 | [Área] | En/sobre benchmark | Mantener y monitorear |

#### Para Reporte de Cliente

```
Contexto: El benchmark de la industria [industria] sitúa a los sitios en la media
en un LCP de X.Xs en móvil. [Nombre del cliente] tiene un LCP de X.Xs, 
lo que lo coloca [por encima / en la media / por debajo] del sector.
```

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| CWV datos reales de campo (CrUX) | `/seo google pagespeed [url]` | Datos precisos para comparar vs benchmarks |
| Análisis profundo de CWV y diagnosis | `/seo performance [url]` | Entender qué causa el gap de CWV |
| CTR y posiciones reales (GSC) | `/seo google search-console [url]` | CTR real por query y posición |
| Comparación vs competidores específicos | `/seo competitive [dominio]` | Benchmark relativo, no absoluto |
| Métricas de backlinks (RDs, DR) | `/seo backlinks [dominio]` | Autoridad real para comparar |
| Reporte de cliente con contexto | `/seo reporting [dominio]` | Integrar benchmarks en reporte mensual |

## Error Handling

| Escenario | Acción |
|-----------|--------|
| Sin datos de CWV (sitio con poco tráfico) | Usar lab data como proxy. Indicar que CrUX requiere suficiente tráfico. Usar benchmarks de referencia de todas formas para orientación. |
| Industria ambigua | Presentar los dos sectores más probables con sus benchmarks. Pedir al usuario que confirme. |
| Métricas del cliente no disponibles | Indicar qué datos necesitas y de qué herramienta (GSC, Ahrefs, etc.). Ofrecer análisis parcial con los datos disponibles. |
| Benchmark no disponible para nicho muy específico | Usar el benchmark del sector más cercano. Indicar la limitación. |
