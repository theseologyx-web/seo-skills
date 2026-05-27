---
name: seo-content-types
description: >
  SEO requirements per content type: schema markup, mandatory visible content elements,
  rich results eligibility, and type-specific on-page checklist. Covers: blog article,
  news, how-to, recipe, service page, ecommerce product, SaaS/app, local/location page,
  case study, review, directory, event, job posting, course, video, FAQ, category page.
  Use when user says "tipo de contenido", "qué schema usar", "requisitos de contenido",
  "rich results", "receta SEO", "noticia SEO", "how-to SEO", "producto ecommerce SEO",
  "landing local SEO", "directorio", "job posting", or asks about a specific content type.
user-invokable: true
argument-hint: "[tipo de contenido o url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# SEO por Tipo de Contenido — Requisitos Completos

> **Scope:** Este skill es una referencia rápida de requisitos por tipo de contenido (schema, rich results, checklist). Para análisis de calidad editorial (E-E-A-T, naturalidad, utilidad) usar `seo-content`. Para generación o validación de schema en profundidad usar `seo-schema`.

## Regla de Oro: Schema = Contenido Visible

**El schema NO reemplaza el contenido — lo describe.**

Google rechaza o ignora schema cuyas propiedades no tienen un equivalente visible en el HTML de la página. Cada propiedad en el JSON-LD debe corresponder a algo que el usuario pueda leer en la página.

```
❌ MAL: JSON-LD con author:"Juan García" pero sin nombre de autor visible en la página
✅ BIEN: Byline visible "Por Juan García" en la página + author:"Juan García" en JSON-LD

❌ MAL: Recipe JSON-LD con lista de ingredientes pero la página no los muestra
✅ BIEN: Lista de ingredientes visible en HTML + recipeIngredient en JSON-LD
```

**Consecuencia de incumplimiento:** Manual action de Google por "Structured data markup with content not visible to users" + pérdida del rich result.

---

## 2024–2026: Deprecaciones Críticas

| Schema / Rich Result | Estado | Qué hacer |
|---------------------|--------|-----------|
| **HowTo rich results** | ❌ Eliminado (2024) | Schema sigue ayudando a AI search pero NO genera rich snippet en Google — implementar solo por GEO |
| **FAQPage (sitios comerciales)** | ❌ Restringido Google (2023) | Solo gov y health en Google. **Sí implementar** para ChatGPT, Perplexity y Bing Copilot (siguen usando FAQ schema para Q&A parsing) |
| **CourseInfo, LearningVideo** | ❌ Eliminado (jun 2025) | Reemplazar por Course schema estándar |
| **SpecialAnnouncement** | ❌ Eliminado (jun 2025) | Post-COVID cleanup |
| **PracticeProblem** | ❌ Eliminado (ene 2026) | — |
| **Sitelinks SearchBox** | ❌ Eliminado (ene 2026) | Eliminar `WebSite` schema con `potentialAction` SearchAction. Google muestra sitelinks search automáticamente en Knowledge Panel sin schema |
| **Q&A schema (QAPage)** | ❌ Eliminado (ene 2026) | No genera rich results en Google. Mantener FAQPage para AI platforms pero no Q&A |

---

## Por Tipo de Contenido

---

### 1. ARTÍCULO DE BLOG (BlogPosting / Article)

**Intent:** Informacional | **Schema:** `Article` o `BlogPosting`

#### Contenido visible OBLIGATORIO en la página

| Elemento | Dónde aparece | Por qué es obligatorio |
|----------|--------------|----------------------|
| Título del artículo | H1 | `headline` en schema |
| Nombre del autor | Byline visible ("Por [nombre]") | `author.name` en schema |
| Fecha de publicación | Visible cerca del título | `datePublished` en schema |
| Fecha de modificación | Visible (si se actualiza) | `dateModified` — señal de frescura |
| Imagen principal | Hero image visible | `image` en schema (mín. 1200×630px, 16:9) |
| Nombre del sitio/publicación | Footer o header | `publisher.name` en schema |

#### Schema mínimo
```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Título del artículo (max 110 chars)",
  "image": ["https://ejemplo.com/imagen-1200x630.webp"],
  "datePublished": "2025-03-15",
  "dateModified": "2025-03-20",
  "author": {
    "@type": "Person",
    "name": "Nombre Autor",
    "url": "https://ejemplo.com/autor/nombre"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Nombre del Sitio",
    "logo": {
      "@type": "ImageObject",
      "url": "https://ejemplo.com/logo.png"
    }
  }
}
```

#### Checklist SEO adicional
- [ ] Keyword informacional en H1, primer párrafo, al menos 1 H2
- [ ] Estructura pirámide invertida: respuesta directa antes del primer H2
- [ ] Tabla de contenidos si > 1500 words
- [ ] Sección de artículos relacionados al final (3-4 links internos)
- [ ] CTA suave (newsletter, guía gratuita) — NO CTA transaccional en TOFU
- [ ] `dateModified` actualizado cada vez que el contenido cambia
- [ ] Bio del autor con link a página de autor (para E-E-A-T)
- [ ] Etiquetas/categorías asignadas (para topical authority de la categoría)

#### Rich results que habilita
- Fecha e imagen en resultados de búsqueda
- Inclusión en Google News si cumple políticas

---

### 2. NOTICIA (NewsArticle)

**Intent:** Informacional urgente | **Schema:** `NewsArticle`

#### Contenido visible OBLIGATORIO

| Elemento | Dónde aparece | Por qué es obligatorio |
|----------|--------------|----------------------|
| Titular | H1 | `headline` — obligatorio |
| Autor con nombre y apellido | Byline visible | `author` — requerido para Google News |
| Fecha y hora de publicación | Visible, prominente | `datePublished` — crítico para frescura |
| Fecha de modificación | Visible si hubo correcciones | `dateModified` |
| Imagen con caption | Visible en el artículo | `image` mín. 1200px ancho |
| Nombre del medio | Header o footer | `publisher` |
| Sección/categoría de la noticia | Tag visible o breadcrumb | `articleSection` |

#### Schema adicional vs. BlogPosting
```json
{
  "@type": "NewsArticle",
  "articleSection": "Tecnología",
  "author": {
    "@type": "Person",
    "name": "María López",
    "url": "https://medio.com/autor/maria-lopez",
    "jobTitle": "Redactora de Tecnología"
  }
}
```

#### Checklist SEO adicional
- [ ] Titular factual, no clickbait (política de Google News)
- [ ] Autor identificable, con página de perfil verificable
- [ ] Sin contenido satírico disfrazado de noticia (prohibido en Google News)
- [ ] URL sin fechas si el artículo puede actualizarse (`/noticias/titulo-noticia/`)
- [ ] Política editorial y correcciones transparentes (visible para E-E-A-T)
- [ ] `datePublished` con hora exacta (ISO 8601 con timezone)
- [ ] Imágenes representan el contenido de la noticia (no decorativas)
- [ ] AMP: **NO implementar** — AMP está efectivamente muerto para SEO en 2025-2026. Google confirmó que AMP no es factor de ranking y el carrusel Top Stories ya no requiere AMP. El beneficio de implementarlo es ~0 y el costo de mantenimiento es alto

#### Rich results que habilita
- Carrusel de noticias en Google Search
- Google News + Google Discover
- Top Stories en SERPs móviles

---

### 3. HOW-TO (HowTo)

**Intent:** Informacional procedimental | **Schema:** `HowTo`

> ⚠️ **HowTo rich results fueron eliminados por Google en 2024.** El schema sigue siendo útil para AI search (ChatGPT, Perplexity, AI Overviews) pero NO genera rich snippets en Google Search. Implementar igual por GEO.

#### Contenido visible OBLIGATORIO

| Elemento | Dónde aparece | Nota |
|----------|--------------|------|
| Título del procedimiento | H1 | `name` en schema |
| Lista de herramientas/materiales | Sección visible "Necesitas" | `tool` y `supply` en schema |
| Pasos numerados | Lista `<ol>` visible | `step` en schema — **todos los pasos visibles** |
| Tiempo estimado | Visible en intro o sidebar | `totalTime` / `performTime` |
| Imagen de cada paso | Recomendado | `step[].image` |
| Costo estimado | Si aplica, visible | `estimatedCost` |

#### Schema
```json
{
  "@type": "HowTo",
  "name": "Cómo optimizar imágenes para SEO",
  "description": "Guía paso a paso para optimizar imágenes...",
  "totalTime": "PT15M",
  "tool": [{"@type": "HowToTool", "name": "Squoosh"}],
  "step": [
    {
      "@type": "HowToStep",
      "position": 1,
      "name": "Comprimir la imagen",
      "text": "Abre Squoosh y arrastra tu imagen...",
      "image": "https://ejemplo.com/paso-1.webp",
      "url": "https://ejemplo.com/guia#paso-1"
    }
  ]
}
```

#### Checklist SEO adicional
- [ ] Cada paso en `<li>` dentro de `<ol>` — HTML semántico
- [ ] Keyword en el H1 tipo "Cómo [hacer X]"
- [ ] Respuesta rápida antes de los pasos (para AI Overviews)
- [ ] Imágenes para pasos complejos (especialmente visual)
- [ ] Sin saltarse pasos para "hacer el artículo más corto" — el usuario debe poder seguirlo

---

### 4. RECETA (Recipe)

**Intent:** Informacional | **Schema:** `Recipe`

> ✅ Recipe schema sigue generando rich results activamente en Google Search, Google Images y Discover.

#### Contenido visible OBLIGATORIO (todos obligatorios)

| Elemento HTML | Schema property | Regla |
|--------------|----------------|-------|
| Nombre del plato | `name` | Exacto, sin términos promocionales |
| Foto del plato terminado | `image` | Mín. 1200×630px, aspecto 16:9 |
| Lista de ingredientes | `recipeIngredient[]` | **TODOS deben ser visibles en la página** |
| Pasos de preparación | `recipeInstructions[]` | **TODOS los pasos visibles en la página** |
| Tiempo de preparación | `prepTime` visible | ISO 8601 en schema (PT10M) |
| Tiempo de cocción | `cookTime` visible | ISO 8601 en schema |
| Tiempo total | `totalTime` visible | |
| Porciones | `recipeYield` visible | Requerido si hay datos de nutrición |
| Categoría del plato | `recipeCategory` visible | "Postre", "Plato principal", etc. |
| Tipo de cocina | `recipeCuisine` visible | "Italiana", "Mexicana", etc. |
| Calorías | `nutrition.calories` visible | Solo con `recipeYield` definido |
| Nombre del autor/chef | `author` visible | Byline visible |

#### Schema
```json
{
  "@type": "Recipe",
  "name": "Tacos de Carnitas",
  "image": ["https://ejemplo.com/tacos-carnitas.webp"],
  "author": {"@type": "Person", "name": "Chef María"},
  "datePublished": "2025-01-10",
  "description": "Tacos de carnitas jugosos con limón y cilantro",
  "recipeYield": "4 porciones",
  "prepTime": "PT20M",
  "cookTime": "PT2H",
  "totalTime": "PT2H20M",
  "recipeCategory": "Plato principal",
  "recipeCuisine": "Mexicana",
  "recipeIngredient": [
    "500g de cerdo para carnitas",
    "1 naranja",
    "2 dientes de ajo"
  ],
  "recipeInstructions": [
    {
      "@type": "HowToStep",
      "text": "Cortar el cerdo en trozos de 5cm..."
    }
  ],
  "nutrition": {
    "@type": "NutritionInformation",
    "calories": "450 calorías"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.7",
    "ratingCount": "234"
  }
}
```

#### Checklist SEO adicional
- [ ] Nombre del plato como H1 (la keyword es el nombre del plato)
- [ ] Foto del plato terminado, alta resolución, 16:9
- [ ] Jump-to-recipe button above the fold (UX + baja bounce rate)
- [ ] Ingredientes en lista `<ul>` con cantidades exactas
- [ ] Pasos en lista `<ol>` numerada con foto por paso si es complejo
- [ ] Variantes y sustituciones en sección aparte (no mezclar en ingredientes principales)
- [ ] Rating widget funcional y visible (agregateRating requiere reseñas reales)
- [ ] Video de la preparación si está disponible

#### Rich results que habilita
- Recipe card en Google Search (ingredientes, tiempo, porciones)
- Badge en Google Images
- Carrusel de recetas
- Información en Google Discover

---

### 5. PÁGINA DE SERVICIO (Service / LocalBusiness)

**Intent:** Transaccional / Comercial | **Schema:** `Service` + `Organization` (o `LocalBusiness`)

#### Contenido visible OBLIGATORIO

| Elemento | Por qué es obligatorio |
|----------|----------------------|
| Nombre del servicio | `name` en schema |
| Descripción del servicio | `description` en schema |
| Área de cobertura o ubicación | `areaServed` / `serviceArea` |
| Precio o rango de precio | `offers.price` — si es invisible = desconfianza |
| Nombre de la empresa | `provider.name` |
| Contacto (tel, email, formulario) | `telephone` / `contactPoint` |
| Beneficios/features del servicio | Contenido visible no duplicado del schema |

#### Schema
```json
{
  "@type": "Service",
  "name": "Auditoría SEO Técnica",
  "description": "Análisis completo de errores técnicos SEO...",
  "provider": {
    "@type": "Organization",
    "name": "SEO Agency",
    "url": "https://ejemplo.com",
    "telephone": "+1-555-0100"
  },
  "areaServed": {"@type": "Country", "name": "US"},
  "serviceType": "SEO Consulting",
  "offers": {
    "@type": "Offer",
    "price": "500",
    "priceCurrency": "USD"
  }
}
```

#### Checklist SEO adicional
- [ ] Keyword del servicio en H1 y URL
- [ ] Features descritos como beneficios para el cliente (no specs internos)
- [ ] Social proof en la misma página: testimonios, logos de clientes, métricas
- [ ] FAQ de objeciones al final (responde por qué contratar, qué incluye)
- [ ] CTA claro y visible above the fold
- [ ] Schema `BreadcrumbList` para indicar jerarquía dentro del sitio

---

### 6. PRODUCTO ECOMMERCE (Product + Offer)

**Intent:** Transaccional | **Schema:** `Product` + `Offer` (Merchant Listing)

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Nota |
|----------|----------------|------|
| Nombre del producto | `name` | Marca + modelo + característica |
| Precio actual | `offers.price` | **Visible sin scroll en desktop** |
| Disponibilidad | `offers.availability` | "En stock", "Agotado" visible |
| Moneda | `offers.priceCurrency` | Implícita o explícita |
| Imagen del producto | `image[]` | Mín. 4-6 fotos: frente, lateral, detalle, en uso |
| Descripción | `description` | Beneficios primero, specs después |
| Marca | `brand.name` | Visible en la página |
| SKU | `sku` | Visible o en metadata |
| Reviews/ratings | `aggregateRating` | **Solo si son reales y visibles en la página** |
| Política de envío | `shippingDetails` | Tiempo y costo visibles |
| Política de devolución | `hasMerchantReturnPolicy` | Días y condiciones visibles |

#### Schema
```json
{
  "@type": "Product",
  "name": "Nike Air Max 270 Negro Talla 42",
  "image": [
    "https://ejemplo.com/nike-air-max-270-negro-frontal.webp",
    "https://ejemplo.com/nike-air-max-270-negro-lateral.webp"
  ],
  "description": "Zapatilla con amortiguación Air Max visible...",
  "brand": {"@type": "Brand", "name": "Nike"},
  "sku": "NK-AM270-BK-42",
  "offers": {
    "@type": "Offer",
    "price": "129.99",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "priceValidUntil": "2025-12-31",
    "shippingDetails": {
      "@type": "OfferShippingDetails",
      "shippingRate": {"@type": "MonetaryAmount", "value": "0", "currency": "USD"},
      "deliveryTime": {"@type": "ShippingDeliveryTime", "handlingTime": {"@type": "QuantitativeValue", "minValue": 0, "maxValue": 1, "unitCode": "DAY"}}
    },
    "hasMerchantReturnPolicy": {
      "@type": "MerchantReturnPolicy",
      "returnPolicyCategory": "https://schema.org/MerchantReturnFiniteReturnWindow",
      "merchantReturnDays": 30
    }
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.6",
    "ratingCount": "1842"
  }
}
```

#### Checklist SEO adicional
- [ ] Precio visible above the fold sin scroll
- [ ] Mínimo 4-6 imágenes: frente, laterales, detalle, en uso (lifestyle)
- [ ] Alt text: `[marca] [modelo] [color/material] [ángulo]`
- [ ] Selector de variantes funcional (talla, color)
- [ ] Tabla de especificaciones técnicas (H2 propio)
- [ ] Sección de reviews visible y reales (no fake reviews)
- [ ] "También te puede gustar" para cross-sell (schema `isRelatedTo`)
- [ ] Breadcrumb: Inicio > Categoría > Subcategoría > Producto
- [ ] Sin texto de precio o descuento dentro de imágenes (usar overlays CSS)
- [ ] Stock indicator visible ("Últimas 3 unidades" si aplica)
- [ ] **Certification markup** (si aplica): productos con certificaciones (USDA Organic, Energy Star, FDA, CE) deben usar `hasCertification` (añadido abr 2025):
  ```json
  "hasCertification": {
    "@type": "Certification",
    "issuedBy": {"@type": "Organization", "name": "USDA"},
    "name": "USDA Organic",
    "certificationIdentification": "CERT-12345"
  }
  ```

#### Rich results que habilita
- Product snippet (precio, disponibilidad, rating)
- Popular products carousel
- Google Shopping
- Google Images badge
- Certification badge (si tiene `hasCertification` schema)

---

### 7. APLICACIÓN / SaaS LANDING (SoftwareApplication)

**Intent:** Transaccional / Comercial | **Schema:** `SoftwareApplication` o `WebApplication`

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Nota |
|----------|----------------|------|
| Nombre de la app | `name` | |
| Precio o "Gratis" | `offers.price` | **Obligatorio para rich result** |
| Categoría de la app | `applicationCategory` | Visible como tag o en descripción |
| Sistema operativo compatible | `operatingSystem` | iOS, Android, Web, Windows |
| Rating / reviews | `aggregateRating` | Si existen, deben ser visibles |
| Screenshots | No en schema pero obligatorio para UX | Mín. 3 screenshots del producto |
| Descripción de la app | `description` | Qué hace y para quién |

#### applicationCategory valores válidos
```
GameApplication, SocialNetworkingApplication, TravelApplication,
ShoppingApplication, SportsApplication, LifestyleApplication,
BusinessApplication, DesignApplication, DeveloperApplication,
DriverApplication, EducationalApplication, HealthApplication,
FinanceApplication, SecurityApplication, BrowserApplication,
CommunicationApplication, DesktopEnhancementApplication,
EntertainmentApplication, MultimediaApplication,
HomeApplication, UtilitiesApplication, ReferenceApplication
```

#### Schema
```json
{
  "@type": "SoftwareApplication",
  "name": "Nombre de la App",
  "operatingSystem": "Web, iOS, Android",
  "applicationCategory": "BusinessApplication",
  "description": "Gestiona tus proyectos SEO...",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "ratingCount": "3200"
  }
}
```

#### Checklist SEO adicional
- [ ] Propuesta de valor en H1 (no solo el nombre de la app)
- [ ] Screenshots del producto en uso (no solo mockups vacíos)
- [ ] Video demo si está disponible
- [ ] Badges de tiendas (App Store, Google Play) con links directos
- [ ] Social proof: reviews de G2/Capterra embebidos o citados
- [ ] Tabla de precios visible (Freemium, Pro, Enterprise)
- [ ] Compatibilidad visible (Chrome, Safari, iOS 16+, Android 12+)
- [ ] HTTPS obligatorio (no negociable para apps)

---

### 8. LANDING PAGE LOCAL / UBICACIÓN (LocalBusiness)

**Intent:** Transaccional local | **Schema:** `LocalBusiness` (o subtipo específico)

#### Contenido visible OBLIGATORIO (NAP — Name, Address, Phone)

| Elemento | Schema property | Regla crítica |
|----------|----------------|--------------|
| Nombre del negocio | `name` | Exacto igual que en GMB |
| Dirección completa | `address` (PostalAddress) | **Igual en GMB, directorios y web** |
| Teléfono con código de área | `telephone` | Mismo número en todos lados |
| Horario de atención | `openingHoursSpecification` | **Visible en la página, no solo en schema** |
| Coordenadas (lat/lng) | `geo` | Extraídas del mapa embebido |
| Servicio o tipo de negocio | `@type` específico | Ver lista de subtipos |
| Rango de precios | `priceRange` | "$", "$$", "$$$" — visible en la página |
| Mapa embebido | No en schema | Google Maps embed visible |

#### Subtipos de LocalBusiness
```
Restaurant, Bakery, Bar, CafeOrCoffeeShop, FastFoodRestaurant,
Hospital, Dentist, Physician, MedicalClinic, Pharmacy,
Hotel, BedAndBreakfast, Hostel, Resort,
AutoRepair, CarDealer, GasStation,
Gym, SportsClub, HealthClub, BeautySalon, HairSalon,
RealEstateAgent, LegalService, AccountingService,
ClothingStore, ElectronicsStore, GroceryStore, HardwareStore,
Library, Museum, MovieTheater, AmusementPark
```

#### Schema
```json
{
  "@type": "Restaurant",
  "name": "La Trattoria",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main St",
    "addressLocality": "Miami",
    "addressRegion": "FL",
    "postalCode": "33101",
    "addressCountry": "US"
  },
  "telephone": "+1-305-555-0100",
  "geo": {"@type": "GeoCoordinates", "latitude": 25.7617, "longitude": -80.1918},
  "url": "https://latrattoria.com",
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday"],
      "opens": "11:00",
      "closes": "22:00"
    },
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Friday","Saturday"],
      "opens": "11:00",
      "closes": "23:30"
    }
  ],
  "priceRange": "$$",
  "servesCuisine": "Italian",
  "menu": "https://latrattoria.com/menu"
}
```

#### Checklist SEO adicional
- [ ] NAP idéntico en web, Google Business Profile y todos los directorios
- [ ] Mapa de Google Maps embebido visible en la página
- [ ] Horarios de atención visibles en texto (no solo schema)
- [ ] Fotos reales del local, no solo stock
- [ ] Reviews de Google/Yelp citadas o embebidas
- [ ] Página específica por ubicación si hay múltiples sucursales
- [ ] Keyword local: "[servicio] + [ciudad]" en H1, URL, meta description
- [ ] Schema `BreadcrumbList` indicando jerarquía
- [ ] Link a Google Maps en "Cómo llegar" o dirección

---

### 9. CASO DE ESTUDIO / CASO DE ÉXITO (Article + Review signals)

**Intent:** Comercial (social proof MOFU/BOFU) | **Schema:** `Article` + `Review` o `ItemReviewed`

#### Contenido visible OBLIGATORIO

| Elemento | Por qué es obligatorio | Nota |
|----------|----------------------|------|
| Nombre del cliente | Credibilidad | Con permiso explícito |
| Cargo y empresa del testimonial | E-E-A-T | "CMO de [Empresa]" |
| El problema específico antes | Contexto narrativo | Sin "antes" el "después" no impresiona |
| La solución implementada | Descripción del producto/servicio | Cómo se resolvió el problema |
| Resultados con cifras concretas | **Absoluto obligatorio** | "+147% tráfico", "de $2k a $12k/mes" |
| Período de tiempo | Contexto de los resultados | "En 6 meses" |
| Foto o logo del cliente | E-E-A-T visual | Si tiene permiso |
| Cita directa del cliente | Credibilidad | En `<blockquote>` |
| Autor del caso de estudio | E-E-A-T | Quién lo escribió/documentó |

#### Schema
```json
{
  "@type": "Article",
  "headline": "Cómo [Cliente] aumentó su tráfico 147% en 6 meses",
  "author": {
    "@type": "Person",
    "name": "Nombre del autor"
  },
  "about": {
    "@type": "Organization",
    "name": "Empresa Cliente"
  },
  "datePublished": "2025-02-10",
  "image": "https://ejemplo.com/caso-cliente.webp"
}
```

#### Checklist SEO adicional
- [ ] Datos específicos y verificables (no "mejoró mucho")
- [ ] Estructura narrativa: Problema → Solución → Resultados
- [ ] Keyword: "[industria/producto] resultados" o "[empresa] caso de éxito"
- [ ] CTA al final: "¿Quieres resultados similares? → Demo/Contacto"
- [ ] Tags de industria para filtrar por sector (si hay múltiples cases)
- [ ] Internal links a la página del producto/servicio mencionado

---

### 10. PÁGINA DE REVIEWS / DIRECTORIO (Review + ItemList)

**Intent:** Comercial | **Schema:** `Review`, `AggregateRating`, `ItemList`

#### Contenido visible OBLIGATORIO por review individual

| Elemento | Schema property | Regla |
|----------|----------------|-------|
| Nombre del reviewer | `author.name` | **No "Usuario Anónimo"** — nombre real obligatorio |
| Texto de la review | `reviewBody` | **Review text DEBE ser visible** |
| Rating numérico o estrellas | `reviewRating.ratingValue` | Visible, no solo en schema |
| Fecha de la review | `datePublished` | Visible |
| Título de la review | `name` | Opcional pero recomendado |
| Mejor rating posible | `reviewRating.bestRating` | Típicamente 5 |

#### ⚠️ Restricciones críticas de Review schema
- **Reviewer name MUST be a valid person or organization** — no "50% off", no emojis, no genéricos
- Reviews en la misma página del producto (no en página externa separada sin canonical)
- Las estrellas deben corresponder a reviews reales — no autoreseñas sin disclosure
- Solo poner `aggregateRating` si tienes reviews visibles en la página

#### Schema para directorio (ItemList)
```json
{
  "@type": "ItemList",
  "name": "Mejores herramientas SEO 2025",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "url": "https://ejemplo.com/herramientas/ahrefs",
      "name": "Ahrefs"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "url": "https://ejemplo.com/herramientas/semrush",
      "name": "SEMrush"
    }
  ]
}
```

#### Checklist SEO adicional
- [ ] Filtros funcionales por categoría/industria/rating
- [ ] Fecha de la última actualización de la página visible
- [ ] Disclosure si hay relación comercial con los listados
- [ ] Cada item del directorio con su propia página de detalle (para schema más rico)
- [ ] Schema `BreadcrumbList` en cada item
- [ ] Canonicals correctos si hay paginación

---

### 11. EVENTO (Event)

**Intent:** Informacional/Transaccional | **Schema:** `Event`

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Nota |
|----------|----------------|------|
| Nombre del evento | `name` | |
| Fecha y hora de inicio | `startDate` | Con timezone (2025-06-15T19:00:00-05:00) |
| Fecha y hora de fin | `endDate` | Recomendado |
| Modalidad | `eventAttendanceMode` | Presencial / Online / Híbrido — **visible en página** |
| Ubicación | `location` | Dirección si presencial, URL si online |
| Precio o "Gratis" | `offers.price` | Visible |
| Organizador | `organizer` | Nombre y URL visibles |
| Imagen del evento | `image` | No logo — imagen representativa |
| Estado del evento | `eventStatus` | Scheduled / Cancelled / Postponed |

#### Disponibilidades de oferta válidas
`InStock` (en venta) | `PreOrder` (pre-venta) | `SoldOut` (agotado)

#### Checklist SEO adicional
- [ ] Título del evento en H1 con fecha o año ("Conferencia SEO Medellín 2025")
- [ ] Botón de registro/compra visible above the fold
- [ ] Agenda / programa del evento visible en la página
- [ ] Speakers con nombres, fotos y bios (E-E-A-T + Person schema)
- [ ] FAQ del evento (horarios, parking, política de cancelación)
- [ ] Actualizar `eventStatus` inmediatamente si se cancela o pospone

---

### 12. OFERTA DE EMPLEO (JobPosting)

**Intent:** Informacional | **Schema:** `JobPosting`

#### Contenido visible OBLIGATORIO (todos requeridos por Google)

| Elemento | Schema property | Crítico |
|----------|----------------|---------|
| Título del cargo | `title` | ✅ Obligatorio |
| Descripción del trabajo | `description` | ✅ Obligatorio — **texto completo visible** |
| Nombre de la empresa | `hiringOrganization.name` | ✅ Obligatorio |
| Ubicación | `jobLocation.address` | ✅ Obligatorio |
| Fecha de publicación | `datePosted` | ✅ Obligatorio |
| Fecha de vencimiento | `validThrough` | Muy recomendado |
| Tipo de empleo | `employmentType` | FULL_TIME, PART_TIME, CONTRACTOR... |
| Salario | `baseSalary` | Muy recomendado (mejora CTR) |
| Si es remoto | `jobLocationType` | "TELECOMMUTE" si es remoto |

#### ⚠️ Sin `validThrough` = job posting desaparece de Google after 30 days

#### Checklist SEO adicional
- [ ] Descripción del trabajo completa y específica (no solo título + email)
- [ ] Salario visible (mejora significativamente el CTR en Google Jobs)
- [ ] Benefits/beneficios visibles en la página
- [ ] Logo de la empresa visible
- [ ] Proceso de aplicación claro (cuántos pasos, qué documentos)
- [ ] Actualizar `validThrough` o remover schema si el puesto ya fue cubierto

---

### 13. CURSO / TUTORIAL (Course)

**Intent:** Informacional/Transaccional | **Schema:** `Course` + `ItemList`

> ⚠️ Requiere mínimo 3 cursos marcados con `ItemList` para calificar para carrusel.

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Nota |
|----------|----------------|------|
| Nombre del curso | `name` | Sin precios ni términos promocionales |
| Descripción breve | `description` | Max 60 chars para display |
| Proveedor/instructor | `provider.name` | Visible |
| URL del curso | `url` | |
| Imagen del curso | `image` | |

#### Para carrusel de cursos (ItemList)
```json
{
  "@type": "ItemList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "url": "https://ejemplo.com/cursos/seo-basico"},
    {"@type": "ListItem", "position": 2, "url": "https://ejemplo.com/cursos/seo-tecnico"},
    {"@type": "ListItem", "position": 3, "url": "https://ejemplo.com/cursos/seo-contenido"}
  ]
}
```

#### Checklist SEO adicional
- [ ] Cada curso con su propia URL (no todo en una sola landing)
- [ ] Prerequisitos visibles
- [ ] Syllabus/programa del curso visible
- [ ] Testimonios de estudiantes
- [ ] Certificación que otorga (si aplica)

---

### 14. PÁGINA DE VIDEO (VideoObject)

**Intent:** Informacional/Entretenimiento | **Schema:** `VideoObject`

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Nota |
|----------|----------------|------|
| Título del video | `name` | |
| Descripción del video | `description` | Visible debajo del player |
| Thumbnail del video | `thumbnailUrl` | Múltiples aspectos (16:9, 4:3, 1:1) |
| Player embebido funcional | `contentUrl` o `embedUrl` | **El video DEBE reproducirse en la página** |
| Fecha de subida | `uploadDate` | Visible |
| Duración | `duration` | Visible (ej: "5:33") |

#### Advanced VideoObject — Clip, SeekToAction, BroadcastEvent

**Clip markup (Key Moments)** — Añadir cuando el video tiene secciones/capítulos navegables:
```json
{
  "@type": "VideoObject",
  "name": "Tutorial completo de SEO técnico",
  "hasPart": [
    {
      "@type": "Clip",
      "name": "Crawlability y robots.txt",
      "startOffset": 0,
      "endOffset": 185,
      "url": "https://ejemplo.com/video#t=0"
    },
    {
      "@type": "Clip",
      "name": "Core Web Vitals",
      "startOffset": 185,
      "endOffset": 420,
      "url": "https://ejemplo.com/video#t=185"
    }
  ]
}
```

**SeekToAction (Seeking rich result):**
```json
"potentialAction": {
  "@type": "SeekToAction",
  "target": "https://ejemplo.com/video?t={seek_to_second_number}",
  "startOffset-input": "required name=seek_to_second_number"
}
```

**BroadcastEvent (Live streams):**
```json
{
  "@type": "VideoObject",
  "publication": {
    "@type": "BroadcastEvent",
    "name": "SEO Live Q&A",
    "isLiveBroadcast": true,
    "startDate": "2026-05-01T18:00:00+00:00",
    "endDate": "2026-05-01T19:30:00+00:00"
  }
}
```

> **Regla crítica — dos contextos distintos:**
> - **VideoObject para Video Rich Result** (carousel en SERP): el video DEBE ser el contenido principal de la página. Google no muestra rich result de video si el video es secundario.
> - **VideoObject como schema embebido** en Article, Recipe, BlogPosting, HowTo u otro schema: **válido aunque el video sea complementario**. Usarlo siempre que haya un video relevante en la página. Se añade como propiedad `video` del schema principal o como schema independiente adicional en el mismo `@graph`. Beneficios: Google indexa el video, AI puede citar el contenido multimedia, mejora entendimiento semántico de la página.

```json
// Ejemplo: Article con VideoObject embebido
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Article",
      "headline": "Cómo mejorar tu LCP en 2026",
      "video": {
        "@type": "VideoObject",
        "name": "Tutorial: Fix LCP paso a paso",
        "thumbnailUrl": "https://example.com/thumb.jpg",
        "uploadDate": "2026-03-01",
        "duration": "PT8M",
        "embedUrl": "https://www.youtube.com/embed/VIDEOID"
      }
    }
  ]
}
```

#### Checklist SEO adicional
- [ ] Transcripción del video en texto debajo del player (indexable + accesibilidad)
- [ ] Capítulos/timestamps con Clip markup si el video es largo (Key Moments)
- [ ] SeekToAction implementado si la plataforma lo soporta
- [ ] Alt text descriptivo en el thumbnail image
- [ ] Subtítulos en español e inglés si aplica
- [ ] VideoObject como schema principal si el video ES el contenido principal; como propiedad embebida si es complementario
- [ ] BroadcastEvent schema si es live stream

---

### 15. PÁGINA DE CATEGORÍA (CollectionPage / BreadcrumbList)

**Intent:** Navegacional + Informacional | **Schema:** `CollectionPage` + `BreadcrumbList`

#### Contenido visible OBLIGATORIO

| Elemento | Por qué |
|----------|---------|
| H1 con keyword de la categoría | La categoría DEBE ser una keyword buscable |
| Párrafo introductorio (150-300 words) | Contexto de la categoría — no solo lista de items |
| Lista/grid de items (artículos o productos) | El contenido principal de la categoría |
| Breadcrumb visible | Orientación + schema |
| Filtros si hay > 20 items | UX + paginación correcta |

#### Schema
```json
{
  "@type": "CollectionPage",
  "name": "Email Marketing para SaaS",
  "description": "Guías, tutoriales y estrategias de email marketing...",
  "breadcrumb": {
    "@type": "BreadcrumbList",
    "itemListElement": [
      {"@type": "ListItem", "position": 1, "name": "Blog", "item": "https://ejemplo.com/blog/"},
      {"@type": "ListItem", "position": 2, "name": "Email Marketing", "item": "https://ejemplo.com/blog/email-marketing/"}
    ]
  }
}
```

#### Checklist SEO adicional
- [ ] El nombre de la categoría es una keyword con volumen de búsqueda real
- [ ] Texto introductorio único, no genérico ("Bienvenido a la categoría...")
- [ ] Artículo/producto destacado al inicio
- [ ] Paginación con `rel="next"` y `rel="prev"` o carga infinita con canonical
- [ ] Sin canonical apuntando a page-1 para todas las páginas paginadas
- [ ] Categoría con < 3 artículos → no tiene topical authority suficiente → consolidar

---

### 16. PÁGINA DE AUTOR / PERSONA (Person + ProfilePage)

**Intent:** Navegacional/E-E-A-T | **Schema:** `Person` + `ProfilePage` (Feb 2026)

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Para E-E-A-T |
|----------|----------------|-------------|
| Nombre completo | `name` | |
| Foto real | `image` | No stock, no avatar |
| Cargo/rol | `jobTitle` | |
| Bio detallada | `description` | Experiencia relevante al tema |
| Redes sociales y perfiles | `sameAs[]` | LinkedIn, Twitter, Wikipedia si existe |
| Organización de afiliación | `affiliation` | |
| Artículos publicados | Listado visible en la página | Prueba de experiencia |

#### Schema — Person + ProfilePage (Feb 2026)

> **Feb 2026:** Google añadió soporte explícito para `ProfilePage` en Search Central. Esta es la manera oficial de marcar páginas de autor/creador para E-E-A-T. Usar `ProfilePage` como tipo primario con `Person` anidado.

```json
{
  "@context": "https://schema.org",
  "@type": "ProfilePage",
  "dateCreated": "2022-01-01",
  "dateModified": "2026-03-15",
  "mainEntity": {
    "@type": "Person",
    "@id": "https://ejemplo.com/autor/maria-lopez#persona",
    "name": "María López",
    "jobTitle": "SEO Specialist",
    "description": "SEO specialist con 8 años de experiencia en SaaS y e-commerce.",
    "image": {
      "@type": "ImageObject",
      "url": "https://ejemplo.com/maria-lopez.webp"
    },
    "url": "https://ejemplo.com/autor/maria-lopez",
    "sameAs": [
      "https://www.linkedin.com/in/maria-lopez-seo/",
      "https://twitter.com/marialopez_seo"
    ],
    "affiliation": {
      "@type": "Organization",
      "name": "SEO Agency"
    }
  }
}
```

#### Checklist SEO adicional
- [ ] Link a la página de autor en todos los artículos del autor
- [ ] La bio menciona el área de expertise directamente relacionada con los temas que escribe
- [ ] Link a publicaciones externas, entrevistas o apariciones en medios
- [ ] Página indexable (no bloqueada en robots.txt)
- [ ] `ProfilePage` schema con `Person` anidado como `mainEntity`
- [ ] `dateModified` actualizado cuando se actualiza la bio

---

---

### 17. PODCAST PAGE (PodcastEpisode / PodcastSeries)

**Intent:** Informacional/Entretenimiento | **Schema:** `PodcastEpisode` + `PodcastSeries`

> Google indexes podcast content and AI systems (ChatGPT, Perplexity) increasingly cite podcast episodes. Each episode page should be a standalone indexable asset.

#### Contenido visible OBLIGATORIO

| Elemento | Schema property | Nota |
|----------|----------------|------|
| Título del episodio | `name` | |
| Nombre del podcast (serie) | `partOfSeries.name` | |
| Número de episodio | `episodeNumber` | |
| Descripción del episodio | `description` | 100+ words para indexabilidad |
| Audio player embebido | `associatedMedia.contentUrl` | Player reproducible en la página |
| Fecha de publicación | `datePublished` | |
| Duración | `timeRequired` | Visible (ej: "45 min") |
| Transcripción | No en schema | **Crítico para indexabilidad y AI citation** |

#### Schema
```json
{
  "@context": "https://schema.org",
  "@type": "PodcastEpisode",
  "name": "Cómo construir topical authority en 2026",
  "episodeNumber": 42,
  "datePublished": "2026-03-15",
  "timeRequired": "PT45M",
  "description": "En este episodio exploramos...",
  "associatedMedia": {
    "@type": "MediaObject",
    "contentUrl": "https://ejemplo.com/podcast/ep42.mp3",
    "encodingFormat": "audio/mpeg"
  },
  "partOfSeries": {
    "@type": "PodcastSeries",
    "name": "SEO en Español",
    "url": "https://ejemplo.com/podcast/"
  }
}
```

#### Checklist SEO adicional
- [ ] Transcripción completa del episodio en texto (indexable + AI citation)
- [ ] Resumen estructurado con timestamps para Key Moments
- [ ] Links a recursos mencionados en el episodio
- [ ] Episodios anteriores/siguientes enlazados (internal linking)
- [ ] Feed RSS con `<podcast:transcript>` tag para plataformas de podcast

---

### 18. WEBINAR / EVENTO ONLINE (Event)

**Intent:** Transaccional (registro) / Informacional (replay) | **Schema:** `Event`

> Los webinars tienen dos fases SEO: pre-evento (landing de registro) y post-evento (página de replay). Cada fase tiene requerimientos distintos.

#### Schema — Fase PRE-evento (registro)
```json
{
  "@context": "https://schema.org",
  "@type": "Event",
  "name": "Cómo hacer SEO técnico en 2026 — Webinar gratuito",
  "startDate": "2026-05-15T18:00:00-05:00",
  "endDate": "2026-05-15T19:30:00-05:00",
  "eventAttendanceMode": "https://schema.org/OnlineEventAttendanceMode",
  "eventStatus": "https://schema.org/EventScheduled",
  "location": {
    "@type": "VirtualLocation",
    "url": "https://zoom.us/webinar/..."
  },
  "organizer": {
    "@type": "Organization",
    "name": "SEO Agency",
    "url": "https://ejemplo.com"
  },
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "validFrom": "2026-04-01",
    "url": "https://ejemplo.com/webinar/registro/"
  }
}
```

#### Schema — Fase POST-evento (replay)
```json
{
  "@type": "Event",
  "eventStatus": "https://schema.org/EventPostponed",
  "recordedIn": {
    "@type": "VideoObject",
    "name": "Grabación: SEO técnico 2026",
    "uploadDate": "2026-05-16"
  }
}
```

#### Checklist SEO adicional
- [ ] Pre-evento: CTA de registro above the fold con formulario embebido
- [ ] Pre-evento: Fecha, hora y timezone claramente visibles
- [ ] Post-evento: URL no cambia (misma URL para registro y replay)
- [ ] Post-evento: H1 actualizado a "Grabación: [título]" o similar
- [ ] Transcripción del webinar en texto para indexabilidad

---

### 19. HERRAMIENTA INTERACTIVA / CALCULADORA (WebApplication)

**Intent:** Informacional/Transaccional | **Schema:** `WebApplication` + `SoftwareApplication`

> Las herramientas interactivas (calculadoras, quizzes, configuradores) rankean para long-tail transaccionales y tienen excelente engagement. Son difíciles de copiar, lo que genera backlinks naturales.

#### Consideraciones de indexabilidad
- Si la herramienta usa JavaScript para renderizar resultados, verificar que el contenido sea accesible para Googlebot (usar URL Inspection API)
- Los resultados de la calculadora que aparecen en el DOM después de la interacción del usuario NO son indexados por Google (solo el HTML inicial)
- Incluir ejemplos de outputs típicos en HTML estático para que Google entienda el valor de la herramienta

#### Schema
```json
{
  "@context": "https://schema.org",
  "@type": "WebApplication",
  "name": "Calculadora de ROI SEO",
  "description": "Calcula el retorno de inversión de una estrategia SEO...",
  "url": "https://ejemplo.com/herramientas/calculadora-roi-seo/",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "Web",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  }
}
```

#### Checklist SEO adicional
- [ ] H1 con la keyword de la herramienta ("Calculadora de ROI de SEO")
- [ ] Descripción estática de qué hace la herramienta (para indexación)
- [ ] Ejemplos de resultados típicos en HTML estático debajo de la herramienta
- [ ] La herramienta funciona sin JavaScript deshabilitado (o tiene fallback)
- [ ] Página optimizada para mobile (herramienta usable en móvil)
- [ ] Schema `WebApplication` con `applicationCategory` correcto

---

## AI-Generated Image Requirements (IPTC — Mandatorio Google)

> **APLICA A TODOS LOS CONTENT TYPES** que incluyan imágenes generadas por IA.

Google requiere que todas las imágenes generadas por IA tengan **IPTC DigitalSourceType metadata** con el valor `TrainedAlgorithmicMedia`. Esto aplica a:
- Google Images
- Google Discover (carrusel de imágenes)
- Google Merchant Center (imágenes de producto generadas por IA)
- Google Shopping

### Cómo añadir el metadata
```bash
# Con ExifTool (recomendado)
exiftool -IPTC:DigitalCreator="TrainedAlgorithmicMedia" imagen.jpg

# O con el campo estándar IPTC4 xmpRights
exiftool -XMP-iptcExt:DigitalSourceType="TrainedAlgorithmicMedia" imagen.jpg
```

### C2PA (Coalition for Content Provenance and Authenticity)
Estándar emergente soportado por Google, Adobe, Microsoft. Añade un "manifiesto de contenido" que declara el origen de la imagen. Adobe Firefly, DALL-E 3 y Midjourney ya incluyen C2PA automáticamente. Para imágenes propias:
- Usar Adobe Content Authenticity tools
- O implementar `c2pa` npm package

### Implicaciones por tipo de contenido
| Tipo | Riesgo sin IPTC | Acción |
|------|----------------|--------|
| E-commerce product images | Eliminación de Google Shopping/Merchant | Alto — añadir IPTC antes de subir |
| Blog hero images | Reducción visibilidad Google Images/Discover | Medio — añadir IPTC |
| Social OG images | Bajo riesgo (no indexadas por Google Images) | Recomendado pero no crítico |

---

## Output Format

Cuando se analiza un tipo de contenido específico:

### Diagnóstico de Tipo de Contenido

**Tipo detectado:** [Tipo]
**Intent:** [Informacional / Transaccional / Comercial / Navegacional]
**Schema recomendado:** [Schema type(s)]

### Elementos Obligatorios — Estado

| Elemento requerido | ¿Presente en la página? | Estado |
|-------------------|------------------------|--------|
| [elemento] | Sí / No / Parcial | ✅ / ⚠️ / ❌ |

### Schema Validation

| Propiedad schema | Valor en JSON-LD | Visible en página | Alineación |
|-----------------|-----------------|------------------|-----------|
| `name` | "..." | Sí (H1) | ✅ |
| `author` | "..." | No | ❌ Falta byline |

### Rich Results Elegibles
- [Lista de rich results disponibles con este schema]
- [Cualquier restricción o deprecación relevante]

### Checklist por Tipo
[Checklist específico del tipo de contenido detectado]

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Schema JSON-LD completo y validación | `/seo schema [url]` | Generación y testing de schema |
| Análisis completo de la página | `/seo page [url]` | On-page + rank math checklist |
| Calidad del contenido y E-E-A-T | `/seo content [url]` | Pirámide invertida, escaneabilidad, keyword density |
| Estrategia de keyword por tipo | `/seo keywords [tema]` | Keyword research + intent classification |
| SXO — intent match | `/seo sxo [url]` | Verificar que el contenido satisface la búsqueda |
