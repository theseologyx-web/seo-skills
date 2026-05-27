---
name: seo-aso
description: >
  App Store Optimization (ASO) para iOS (App Store) y Android (Google Play). Cubre
  keyword research para apps, optimización de metadata (título, subtítulo, keyword field,
  descripción), creatives (screenshots, preview video, icon), ratings y reviews, A/B testing
  nativo (Product Page Optimization, Custom Store Listings), localización, factores técnicos
  de ranking y conexión ASO↔SEO web. Use cuando el usuario diga "ASO", "App Store
  Optimization", "posicionamiento en App Store", "Google Play ranking", "keywords para app",
  "optimizar app", "reviews en Play Store", "screenshots app".
user-invokable: true
argument-hint: "[nombre de la app, bundle ID, o URL de la app en el store]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# ASO — App Store Optimization: iOS y Android

Optimización completa para aparecer, convertir e instalarse en App Store (iOS) y Google Play (Android).

---

## 0. Diferencias fundamentales iOS vs Android

| Factor | App Store (iOS) | Google Play (Android) |
|--------|----------------|----------------------|
| **Indexación** | Solo 3 campos: título + subtítulo + keyword field | Descripción completa indexada |
| **Backlinks web** | No afectan ranking | Sí afectan ranking |
| **Algoritmo principal** | Relevancia + retención del usuario | Installs + engagement |
| **Tiempo de publicación** | 1-3 días (revisión manual) | Horas (revisión automatizada) |
| **A/B testing nativo** | Product Page Optimization (PPO) | Custom Store Listings (CSL) |
| **Descripción** | NO indexada (solo conversión) | SÍ indexada (keywords importante) |

---

## 1. Keyword Research para ASO

Las keywords de ASO no son las mismas que las de Google SEO. Los usuarios buscan de forma diferente en los stores.

### 1.1 Tipos de keywords en ASO

| Tipo | Ejemplo | Estrategia |
|------|---------|-----------|
| **Branded** | "visualogyx", "vlx app" | Defender — incluir en título |
| **Competidores** | "fieldwire app", "procore" | En keyword field (iOS) / descripción (Android) |
| **Funcionalidad** | "inspection software", "field forms" | Alta prioridad — intención directa |
| **Problema** | "manage field teams", "digital checklists" | Awareness — descripción y subtítulo |
| **Long tail** | "oil and gas inspection app" | Menor competencia, más conversión |

### 1.2 Cómo encontrar keywords para ASO

**Gratuito:**
- **App Store / Play Store Autocomplete**: escribir keyword raíz y ver sugerencias — las más buscadas
- **Reviews de la propia app y competidores**: el vocabulario que usan los usuarios = keywords reales
- **Título y descripción de competidores**: qué keywords priorizan
- **App Radar** (freemium): volumen de búsqueda y dificultad
- **Keyword Tool** (modo App Store): autocomplete a escala

**De pago:**
- **AppTweak**: keyword suggestions + volumen + dificultad + competitor spy (~€69/mes)
- **MobileAction**: keyword tracking + oportunidades (~€15/mes)
- **Sensor Tower**: datos más profundos, precios de competidores ($$$)
- **AppFollow**: especializado en reviews + keyword tracking (~€69/mes)

### 1.3 Métricas clave para evaluar keywords ASO

| Métrica | Qué indica | Objetivo |
|---------|-----------|---------|
| **Search Volume** | Cuánta gente busca esa keyword | Alto |
| **Keyword Difficulty** | Qué tan difícil rankear | Bajo-Medio al inicio |
| **Relevance** | Qué tan relevante es para la app | Muy alta — conversiones bajan si no es relevante |
| **Ranking actual** | En qué posición está la app | Mejorar las top 10-20 primero |

### 1.4 Reglas de keyword research ASO

- **No repetir keywords** entre campos — Apple/Google solo indexan una vez cada keyword
- Priorizar keywords con **Search Volume alto + Difficulty bajo** (oportunidades)
- Revisar keywords con las que ya rankea la app (App Store Connect → Analytics → Search Terms)
- Usar singular Y plural si difieren semánticamente en el uso

---

## 2. Metadata — Los Campos que Indexan

### 2.1 App Store (iOS) — Solo 3 campos indexados

#### Título (30 chars) — Mayor peso de todos
```
[Nombre de Marca] - [Keyword Principal]

Ejemplos:
✅ "Visualogyx - Inspection App"      (marca + keyword)
✅ "VLX: Field Inspection Software"   (marca + 2 keywords)
❌ "Visualogyx"                        (solo marca, sin keywords)
❌ "The Best Inspection Software Ever" (sin marca, exceso adjetivos)
```

#### Subtítulo (30 chars) — 2º campo más indexado
```
[Keyword secundaria] + [beneficio o diferenciador]

Ejemplos:
✅ "Digital Forms & Checklists"
✅ "Manage Field Teams & Reports"
❌ "Download now for free!"       (no indexa, no aporta)
```

#### Keyword Field (100 chars) — Hidden, indexado, iOS exclusivo
```
Reglas críticas:
- Separar por comas sin espacios: "inspection,checklist,field,audit,compliance"
- NO repetir keywords que ya están en título o subtítulo
- NO incluir el nombre de la app o marca
- NO incluir nombres de competidores (viola políticas de Apple)
- Usar singular (Apple infiere plurales automáticamente)
- Incluir keywords que no caben en título/subtítulo

Estrategia de 100 chars:
[keyword3],[keyword4],[keyword5],[keyword6],[keyword7],[keyword8],[keyword9],[keyword10]
```

#### Descripción (4,000 chars) — NO indexada por Apple, solo conversión
```
Estructura recomendada:
- Primeras 255 chars: resumen del valor (visible sin "Leer más")
- Cuerpo: casos de uso, funcionalidades, beneficios
- Último párrafo: CTA + rating prompt indirecto
- Bullets/emojis: mejoran scannability
```

#### In-App Purchases (IAP) — Indexados por Apple
- Nombre del IAP: 35 chars — incluir keywords relevantes
- Descripción del IAP: 55 chars — complementar keywords

### 2.2 Google Play (Android) — Descripción completa indexada

#### Título (30 chars) — Mayor peso
```
Misma estrategia que iOS: [Marca] - [Keyword Principal]
```

#### Short Description (80 chars) — Indexada, visible en búsqueda
```
[Keyword primaria] + [beneficio + diferenciador]

✅ "Digital inspection software for field teams & compliance"
✅ "Manage inspections, checklists & reports from your phone"
```

#### Long Description (4,000 chars) — Completamente indexada
```
Estrategia de densidad de keywords:
- Keyword principal: 3-5 veces máximo (no keyword stuffing)
- Keywords secundarias: 1-3 veces cada una
- Estructura: problema → solución → funcionalidades → casos de uso → CTA

Formato recomendado:
[Párrafo 1: propuesta de valor con keyword principal]

✔️ [Feature 1 con keyword]
✔️ [Feature 2 con keyword]
✔️ [Feature 3 con keyword]

[Párrafo 2: casos de uso con keywords secundarias]

[CTA final: "Descarga gratis / Prueba gratuita"]
```

---

## 3. Creatives — Icono, Screenshots y Vídeo

### 3.1 Icono

| Store | Tamaño | Reglas |
|-------|--------|--------|
| App Store | 1024×1024px | Sin esquinas redondeadas (Apple las aplica) |
| Google Play | 512×512px | Sin esquinas redondeadas |

**Buenas prácticas:**
- Minimalista — legible a 50px
- Alto contraste — destaca en fondo oscuro y claro
- Sin texto si es posible (ilegible en tamaño pequeño)
- Único y reconocible — diferente de competidores directos
- A/B testear variantes de icono (es el elemento con más impacto en CVR)

### 3.2 Screenshots

#### App Store (iOS) — Críticos: primeros 3 aparecen en resultados de búsqueda

| Formato | Especificaciones |
|---------|-----------------|
| Portrait | 1290×2796px (iPhone 15 Pro Max) |
| Landscape | 2796×1290px |
| iPad | 2048×2732px |
| Hasta | 10 screenshots por localización |

**Estrategia de screenshots iOS:**
```
Screenshot 1: Propuesta de valor principal — "El hook"
              → Headline grande con keyword + visual del momento aha del producto
Screenshot 2: Feature más diferenciadora
Screenshot 3: Caso de uso más común / pain point resuelto
Screenshots 4-10: Features adicionales, integraciones, prueba social
```

**⚠️ CAMBIO 2025:** Los captions/textos en screenshots ahora están **indexados por Apple**. Incluir keywords relevantes en los títulos/subtítulos de las capturas.

#### Google Play (Android)

| Formato | Especificaciones |
|---------|-----------------|
| Portrait | 1080×1920px (16:9) |
| Landscape | 1920×1080px |
| Hasta | 8 screenshots |

**Nota:** Los screenshots de Android NO aparecen en resultados de búsqueda general, pero sí en la ficha del producto.

**Reglas generales para ambos stores:**
- El primer screenshot debe vender en 1 segundo (sin necesidad de leer)
- Usar mockups de dispositivo reales
- Consistencia de marca: mismo estilo, paleta, tipografía
- Texto grande y legible (se ve a 150px en el store)
- Orientación: portrait si la app es portrait, landscape si tiene ambas

### 3.3 Preview Video / App Preview

| Store | Duración | Formato | Autoplay |
|-------|---------|---------|---------|
| App Store | 15-30s | MP4 / MOV, portrait | Sí, sin sonido |
| Google Play | Hasta 30s | YouTube embed | Sí |

**Buenas prácticas:**
- Primeros 3 segundos: el momento más impactante de la app
- Sin sonido en autoplay — el texto en pantalla es obligatorio
- Mostrar UI real de la app (no comercial de stock footage)
- Portrait preferred en Android (+9% completions, +5% conversión)
- No obligatorio — un vídeo malo puede bajar la conversión

---

## 4. Ratings y Reviews

### 4.1 Impacto en ranking y conversión

- Mejora de 0.5 ⭐ = **hasta 2× la conversión**
- El **90% de las apps featured** tienen rating ≥ 4.0
- El **77% de usuarios** lee reviews antes de instalar
- En Google Play: el texto de las reviews **está indexado** (keywords adicionales gratuitas)
- Apps con rating ≥ 4.5 tienen ~65% más installs que apps con 3.5

### 4.2 Cómo solicitar reviews correctamente

**iOS — SKStoreReviewRequest (API nativa):**
```
Cuándo pedir:
✅ Después de completar una acción exitosa (formulario enviado, inspección completada)
✅ Después de varios usos de la app (3-5 sesiones)
✅ Después de una actualización positiva

❌ Al abrir la app por primera vez
❌ Después de un error o crash
❌ En medio de un flujo de trabajo

Límite Apple: máximo 3 prompts/año por usuario
```

**Android — In-App Review API:**
```
Misma lógica: contexto positivo + timing inteligente
Sin límite oficial pero Google recomienda no abusar
```

### 4.3 Gestión de reviews — Responder siempre

| Tipo | Tiempo respuesta | Estrategia |
|------|-----------------|-----------|
| Negativa (1-2 ⭐) | < 24-48h | Pedir disculpas + ofrecer solución + canal de contacto |
| Neutral (3 ⭐) | < 72h | Agradecer + preguntar qué mejorar |
| Positiva (4-5 ⭐) | 1 semana | Agradecer brevemente y personalizar |

**Impacto de responder reviews negativas:** +0.7 ⭐ de media en el rating global.

**Nunca:** respuestas de template idénticas, defensivo, ignorar críticas recurrentes.

### 4.4 Reviews como fuente de keywords

Los usuarios describen la app con sus propias palabras = keywords que no habías contemplado.
- Leer reviews mensualmente buscando términos recurrentes
- Añadir esas keywords a metadata si tienen volumen de búsqueda
- Las reviews positivas que mencionan casos de uso = contenido para screenshots

---

## 5. A/B Testing Nativo

### 5.1 App Store — Product Page Optimization (PPO)

**Qué se puede testear:** icon, screenshots, preview video, preview text
**Cuántos tests simultáneos:** hasta 70 Custom Product Pages (CPP) — actualizado julio 2025
**⚠️ NOVEDAD 2025:** Los CPPs ahora aparecen en resultados de **búsqueda orgánica** (linked por keywords) — se pueden crear CPPs específicos para distintas queries

**Cómo configurar:**
```
App Store Connect → Tu App → Product Page Optimization
→ Create Test → Seleccionar elementos a testear → Porcentaje de tráfico → Publicar
Duración recomendada: mínimo 7-14 días para significancia estadística
```

**Estrategia de CPPs para búsqueda orgánica:**
```
CPP 1: Para query "inspection software" → screenshots con foco en inspecciones
CPP 2: Para query "field forms" → screenshots con foco en formularios digitales
CPP 3: Para query "compliance app" → screenshots con foco en compliance
```

### 5.2 Google Play — Custom Store Listings (CSL)

**Qué se puede testear:** todo el listing (título, descripción, screenshots, icono, vídeo)
**Cuántos:** hasta 50 listados simultáneos
**Por:** país, idioma, campaña de adquisición, segmento de usuario

**Cómo configurar:**
```
Google Play Console → Tu App → Grow → Store Listing Experiments
→ Crear experimento → Seleccionar variante → % de tráfico → Publicar
```

---

## 6. Localización ASO

**Impacto:** localizar el listing completo genera **+38% de descargas** por mercado.

### 6.1 Mercados prioritarios por tipo de app

| Tipo de app | Mercados prioritarios |
|------------|----------------------|
| B2B SaaS USA | EN-US (principal), EN-GB, EN-AU, EN-CA |
| B2C global | EN + ES + PT + FR + DE + JA + KO + ZH |
| LATAM | ES-MX, ES-AR, ES-CO, PT-BR |
| Local | País específico + idiomas locales |

### 6.2 Cómo localizar correctamente

- **No traducir literalmente** — adaptar al vocabulario de búsqueda local
- Las keywords de cada mercado son DIFERENTES (keyword research por locale)
- iOS indexa múltiples locales en paralelo — cada locale = conjunto de keywords independiente
- Screenshots: adaptar texto overlay al idioma + referencias culturales
- Usar nativos para revisión — traductores automáticos dan keywords incorrectas

### 6.3 Campos localizables

**iOS:** título, subtítulo, keywords, descripción, capturas, vídeo, nombre del IAP
**Android:** título, short description, long description, screenshots, vídeo, gráfico destacado

---

## 7. Factores Técnicos que Afectan el Ranking

### 7.1 Estabilidad de la app — Android Vitals y iOS Crash Rate

| Métrica | Google Play (líderes) | App Store |
|---------|----------------------|-----------|
| Crash-free sessions | > 99.81% | > 99.93% |
| ANR rate (App Not Responding) | < 0.1 por 10k sesiones | — |
| Crash rate referencia | < 0.1% | < 0.1% |

Las apps con muchos crashes bajan en ranking y pueden ser removidas de featured.

### 7.2 Update frequency

- Updates frecuentes = señal de desarrollo activo → boost temporal post-update
- Google Play y App Store priorizan apps actualizadas en featuring
- Actualizar metadata cada **2-4 semanas** (keywords, screenshots estacionales)
- No publicar updates solo para manipular el boost (viola políticas)

### 7.3 Tamaño de la app

- Apps más ligeras tienen mayor tasa de instalación (especialmente en mercados emergentes)
- Google Play: el tamaño afecta indirectamente a retention y descargas exitosas
- iOS: On-Demand Resources y App Thinning para reducir tamaño inicial

### 7.4 Retention y engagement

- **Retention D1/D7/D30** (usuarios que vuelven) = señal de calidad
- **Uninstall rate** alto → penalización en ranking
- **Session length** y **DAU/MAU ratio** → señales de engagement
- En Google Play: redownloads representan 2:1 respecto a nuevas descargas (2025) — optimizar para retención tanto como para adquisición

---

## 8. ASO ↔ SEO Web — La Estrategia Integrada

| Señal | Impacto en App Store | Impacto en Google Play |
|-------|---------------------|----------------------|
| Backlinks web al listing de la app | ❌ No afecta | ✅ Sí afecta ranking |
| Landing page web de la app optimizada SEO | Indirecto (brand awareness) | Directo (Google indexa) |
| Reviews en G2/Capterra que mencionan la app | Alimenta AI Overviews | Alimenta AI Overviews |
| AI Overviews (Google) citan la app | Pre-store awareness | Pre-store awareness |

### 8.1 Landing page web de la app (AppSEO)

Cada app debe tener una landing page en el sitio web optimizada para:
```
URL: /app/ o /mobile-app/ o /[nombre-app]/
Title: "[App Name] — [Keyword principal] | App para iOS y Android"
H1: "[Keyword principal] — Descarga gratis para iOS y Android"
Contenido: features, screenshots, reviews, links al App Store y Google Play
Schema: SoftwareApplication con operatingSystem, applicationCategory, aggregateRating
```

**Schema recomendado:**
```json
{
  "@context": "https://schema.org",
  "@type": "MobileApplication",
  "name": "Visualogyx",
  "operatingSystem": "iOS, Android",
  "applicationCategory": "BusinessApplication",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.7",
    "ratingCount": "1250"
  },
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "url": "https://apps.apple.com/app/[ID]"
}
```

### 8.2 Ciclo integrado ASO + SEO + SMO

```
SEO web → brand awareness → usuario busca la app en el store
ASO → la app aparece en la búsqueda del store → instalación
Reviews en app → UGC para web → señal E-E-A-T
SMO → menciones de marca → usuario busca la app
Email → tráfico web → usuario descarga desde landing page
```

---

## 9. AI en App Stores 2026

### Apple App Store — AI Features

**AI-generated Tags (2025-2026):** Apple genera tags automáticos desde metadata de la app incluyendo screenshots. Son mecanismo de browse-discovery (no keyword search). Para optimizar:
- Asegurar que screenshots muestren claramente los casos de uso principales
- Metadata debe ser precisa y específica — el AI los lee e infiere categorías
- Revisar en App Store Connect qué tags genera Apple para la app

**NLP Semántico:** App Store ya no depende solo de keyword matching exacto — interpreta sinónimos e intent conversacional. El keyword field debe incluir variaciones semánticas, no solo keywords exactas.

**Apple App Store Ads expansion (marzo 2026):** Apple expandió ads a todo el search experience. Más ads = menos espacio orgánico = mayor importancia de ASO diferenciado para mantener visibilidad orgánica.

**SDK requirements (abril 2026):** Apps que no usen SDKs actualizados con mejoras de seguridad pueden ser rechazadas o perder visibilidad.

### Google Play — AI Features

**Guided Search con AI:** Google Play organiza resultados por intent del usuario. El usuario describe un goal ("app para inspeccionar equipos") y el AI categoriza apps automáticamente. Implicación: la descripción debe articular claramente qué problema resuelve la app, no solo funcionalidades.

**Gemini en Play Console:** Traducciones automáticas disponibles. Ventaja para localización rápida, pero requieren revisión humana — el vocabulario de búsqueda local puede diferir de la traducción automática.

**Short-form Video como canal de discovery:** Video corto es oficialmente canal de discovery en Play Store. Considerar contenido de video corto que muestre el value prop de la app.

---

## 9b. LiveOps & In-App Events

### Google Play — LiveOps

Apps con **eventos activos** obtienen **+15-20% más impresiones** de editorial/browse.

**Frecuencia óptima:** 2-4 eventos activos simultáneamente por mes

**Tipos de eventos:** nuevas features, eventos estacionales, torneos, descuentos, colaboraciones

**Setup:** Google Play Console → Store Presence → Promotional Content → Create Promotional Content

### Apple App Store — In-App Events

Eventos in-app en iOS atraen nuevos usuarios y re-engagement. Aparecen en el listing y en resultados de búsqueda relevantes.

**Best practices:**
- Planificar eventos con 2-3 semanas de antelación (revisión de Apple)
- Imagen del evento: 2048×1024px
- Nombre del evento: 30 chars (incluir keyword relevante si natural)
- Vincular eventos a momentos estacionales o releases de features

---

## 9c. Alternative App Stores (EU & Japón)

Cambios regulatorios obligan a Apple a permitir marketplaces alternativos:

| Región | Regulación | Estado | Implicación ASO |
|--------|-----------|--------|----------------|
| EU | DMA (Digital Markets Act) | Vigente | Apple permite sideloading y stores alternativos |
| Japón | MSCA | iOS 26.2+ | Marketplaces alternativos obligatorios |
| USA | En litigio | Pendiente | Posibles cambios post-Epic vs Apple |

**Stores alternativos relevantes:** AltStore, Epic Games Store (iOS EU), F-Droid (Android)

**Para apps en EU/Japón:** considerar listado en stores alternativos para mayor alcance. Requiere ASO específico por store (metadata, screenshots, pricing).

---

## 10. Herramientas ASO 2026

| Herramienta | Función principal | Precio |
|------------|------------------|--------|
| **AppTweak** | Keyword research + competitive intelligence + AI tag analysis | ~€69/mes |
| **AppFollow** | Review management + keyword tracking + alerts | ~€69/mes |
| **MobileAction** | PPO + keyword tracking + store ads intelligence | ~€15/mes |
| **App Radar** | Keyword research + rank tracking | Freemium |
| **Sensor Tower** | Data profundo + market intelligence | $$$ |
| **data.ai (App Annie)** | Market analytics + competitive | $$$ |
| **AppFigures** | Analytics + reviews + keyword tracking | ~$15/mes |
| **AppStore Connect** | Analytics nativos iOS (Search Terms, Impressions, In-App Events) | Gratis |
| **Google Play Console** | Analytics nativos Android (Acquisition, Reviews, LiveOps) + Gemini traducciones | Gratis |
| **SearchAds.com** | Apple Search Ads intelligence + competitor ad spy | Paid |

**Empezar con:** AppStore Connect + Google Play Console (gratis) → App Radar (freemium) → AppTweak o AppFollow cuando escale.

**Empezar con:** AppStore Connect + Google Play Console (gratis) → App Radar (freemium) → AppTweak o AppFollow cuando escale.

---

## 10. Audit ASO — Checklist Completo

### Keyword Research
- [ ] Keyword research específico para ASO (no usar directamente keywords de Google SEO)
- [ ] Keywords identificadas por: funcionalidad, problema, competidores, long tail
- [ ] Volumen y dificultad evaluados para priorizar
- [ ] Keywords revisadas en App Store Connect / Play Console (Search Terms)

### Metadata iOS
- [ ] Título: marca + keyword principal (30 chars)
- [ ] Subtítulo: keyword secundaria + beneficio (30 chars)
- [ ] Keyword field: sin repeticiones de título/subtítulo, sin marca, separado por comas (100 chars)
- [ ] Descripción: primeras 255 chars con propuesta de valor clara
- [ ] IAP nombres con keywords si aplica

### Metadata Android
- [ ] Título: marca + keyword principal (30 chars)
- [ ] Short description: keyword + beneficio (80 chars)
- [ ] Long description: keyword principal 3-5×, keywords secundarias 1-3×, bien estructurada (4,000 chars)

### Creatives
- [ ] Icono: minimalista, legible a 50px, diferente de competidores
- [ ] Screenshot 1: propuesta de valor + keyword en caption (iOS)
- [ ] Screenshots siguientes: features + casos de uso
- [ ] Captions de screenshots con keywords (iOS — ahora indexadas)
- [ ] Preview vídeo: primeros 3s impactantes, subtítulos visible sin sonido

### Ratings y Reviews
- [ ] Rating ≥ 4.0
- [ ] Proceso de solicitud de reviews en momento correcto
- [ ] Reviews negativas respondidas en < 48h
- [ ] Reviews revisadas mensualmente para extraer keywords

### Técnico
- [ ] Crash-free sessions > 99.8%
- [ ] ANR rate < 0.1% (Android)
- [ ] Updates frecuentes (al menos cada 4-6 semanas)
- [ ] Landing page web de la app con SoftwareApplication schema

### Localización
- [ ] Listing en inglés optimizado antes de localizar
- [ ] Keyword research por locale para mercados secundarios
- [ ] Screenshots localizados (texto en idioma correcto)

---

## 11. Output — Reporte ASO para Cliente

**App:** [Nombre]
**Stores:** App Store iOS / Google Play
**Fecha:** [Fecha]
**Herramienta usada:** [AppTweak / AppFollow / App Radar / Play Console / ASC]

### Estado actual

| Área | iOS | Android | Prioridad |
|------|-----|---------|-----------|
| Keyword research | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Título optimizado | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Subtítulo/Short desc | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Keyword field (iOS) | ✅/⚠️/❌ | — | |
| Long description | — | ✅/⚠️/❌ | |
| Screenshots | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Preview vídeo | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Rating | X.X ⭐ | X.X ⭐ | |
| Respuesta a reviews | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Localización | ✅/⚠️/❌ | ✅/⚠️/❌ | |
| Landing page web | ✅/⚠️/❌ | ✅/⚠️/❌ | |

### Top keywords — Estado actual

| Keyword | Volumen | Dificultad | Posición iOS | Posición Android |
|---------|---------|-----------|-------------|-----------------|
| | | | | |

### Plan de acción priorizado

| Prioridad | Store | Acción | Impacto en installs | Esfuerzo |
|-----------|-------|--------|-------------------|---------|
| 🔴 Crítica | | | | |
| 🟡 Alta | | | | |
| 🟢 Quick win | | | | |

### KPIs de seguimiento mensual

| KPI | Store | Valor actual | Meta 90 días |
|-----|-------|-------------|-------------|
| Impresiones | iOS / Android | | |
| CVR (impresiones → installs) | iOS / Android | | |
| Rating | iOS / Android | | |
| Posición keyword principal | iOS / Android | | |
| Installs orgánicos/mes | iOS / Android | | |
