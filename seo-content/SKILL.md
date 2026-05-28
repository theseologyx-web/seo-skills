---
name: seo-content
description: >
  Content quality and E-E-A-T analysis with AI citation readiness assessment.
  Use when user says "content quality", "E-E-A-T", "content analysis",
  "readability check", "thin content", or "content audit".
  Not for schema markup or rich results implementation — use seo-content-types.
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Content Quality & E-E-A-T Analysis

## E-E-A-T Framework (updated March 2026 core update)

Read `skills/seo/references/eeat-framework.md` for full criteria.

> **March 2026 shift:** Google's March 2026 core update elevated **Experience** as the primary differentiator. Two pages with equal expertise/authority signals will now rank based on which demonstrates more genuine first-hand experience. E-E-A-T also expanded beyond YMYL — it now applies to all competitive queries, not just health/finance/legal.

> **February 2026 Authors section:** Google added a dedicated [Authors section](https://developers.google.com/search/docs/fundamentals/creating-helpful-content) to Search Central documentation — the clearest signal yet that authorship/entity transparency is a direct quality consideration. Author identity now directly influences page-level authority.

### Experience (first-hand signals) — PRIMARY DIFFERENTIATOR
- Original research, case studies, before/after results with specific data
- Personal anecdotes, process documentation, first-person observations
- Unique proprietary data (surveys, experiments, client results)
- Photos/videos from direct experience (not stock)
- Author demonstrably used/tested the product or service

**Implementation checklist:**
- [ ] Visible author byline with full name
- [ ] Link from byline to author bio page
- [ ] Author bio page with verifiable credentials (LinkedIn, publications, portfolio)
- [ ] Author Person schema with `sameAs` links to external profiles
- [ ] ProfilePage schema on the author bio page (Google-supported type for E-E-A-T)
- [ ] Consistent author entity across the site (name spelling, role, photo)

### Expertise
- Author credentials, certifications, bio relevant to the topic
- Professional background and years of experience
- Technical depth appropriate for target audience
- Accurate, well-sourced claims with citations

### Authoritativeness
- External citations, backlinks from authoritative sources
- Brand mentions, industry recognition, press coverage
- Published in or cited by recognized outlets
- Consistent entity presence (Wikipedia, Wikidata, Knowledge Panel)

### Trustworthiness
- Contact information, physical address
- Privacy policy, terms of service
- Customer testimonials, reviews with specifics
- Date stamps (published + last modified), transparent corrections
- Secure site (HTTPS)

### E-E-A-T Quick Implementation Audit
| Signal | Where to add | Priority |
|--------|-------------|----------|
| Author byline visible on page | Article/blog pages | High |
| Author bio page with credentials | `/about/[author]` | High |
| Person schema on author page | JSON-LD | High |
| ProfilePage schema | Author bio page | High |
| First-person experience language | Body content | Medium |
| Original data/stats cited | Any content | Medium |
| External validation links | Credentials section | Medium |

## Content Metrics

---

## Content Type Optimization Matrix

Cada tipo de página tiene un rol SEO diferente. El error más común es aplicar la misma estrategia de contenido y keywords a todos los tipos. Antes de analizar o crear cualquier contenido, identificar el tipo y aplicar las reglas correspondientes.

> **Requisitos técnicos por tipo (schema, contenido visible obligatorio, rich results elegibles, deprecaciones 2024-2026):** ver `seo-content-types`. Este skill cubre estrategia editorial e intención; seo-content-types cubre la capa técnica/schema.

### ⚠️ Deprecaciones críticas 2024-2026
| Schema / Rich Result | Estado | Qué hacer |
|---------------------|--------|-----------|
| HowTo rich results | ❌ Eliminado (2024) | Schema útil para AI search pero sin rich snippet en Google |
| FAQPage (sitios comerciales) | ❌ Restringido Google (2023) | Solo gov/health en Google. Sí implementar para ChatGPT/Perplexity/Bing |
| CourseInfo, LearningVideo | ❌ Eliminado (jun 2025) | Usar Course schema estándar |
| Sitelinks SearchBox | ❌ Eliminado (ene 2026) | Eliminar WebSite schema con SearchAction |
| Q&A schema (QAPage) | ❌ Eliminado (ene 2026) | No genera rich results en Google |

---

### 1. Homepage

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Navegacional + branded |
| **Keywords que pertenecen aquí** | Marca, nombre del producto/empresa, términos paraguas ("software de gestión", "agencia SEO") |
| **Keywords que NUNCA van aquí** | Keywords de producto específico, features individuales, long tails informacionales |
| **Longitud** | 500-800 words (no es un blog — no sobrecargar) |
| **Schema** | Organization, WebSite, SiteLinksSearchBox |
| **CTA principal** | Claro, único, orientado a conversión o exploración ("Ver planes", "Cómo funciona") |

**Estructura recomendada:**
```
H1: [Nombre de marca o propuesta de valor principal]
  → Propuesta de valor en 1-2 oraciones
  → CTA primario above the fold
H2: [Beneficio o problema que resuelve] — no: "Nuestros servicios"
H2: [Para quién es / casos de uso principales]
H2: [Social proof: clientes, métricas, testimonios]
H2: [Cómo funciona / proceso]
H2: [CTA final]
```

**Errores comunes:**
- Optimizar la homepage para una keyword de producto específica (cannibalism con la página del producto)
- H1 genérico: "Bienvenido a [Marca]" — sin propuesta de valor
- Demasiado texto técnico que debería estar en páginas internas

---

### 2. Página de Producto / Servicio

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Transaccional / Comercial |
| **Keywords que pertenecen aquí** | "[producto] + [beneficio]", "[servicio] + [ciudad/industria]", "software de [función]", "agencia de [tipo]" |
| **Keywords que NUNCA van aquí** | Brand terms (homepage), "qué es [producto]" (blog), comparativas (página de comparación) |
| **Longitud** | 800-1500 words dependiendo de la complejidad del producto |
| **Schema** | Product, Service, Offer (si tiene precio), SoftwareApplication (para SaaS) |
| **CTA principal** | "Solicitar demo", "Empezar gratis", "Ver precios", "Contratar" |

**Estructura recomendada:**
```
H1: [Keyword producto] + [beneficio principal]
  → Hero: propuesta de valor + CTA above fold
H2: ¿Qué es / Cómo funciona [producto]?
H2: Características clave (features como beneficios, no como specs)
H2: Para quién es / Casos de uso
H2: [Keyword secundaria: precio, integración, industria]
H2: Testimonios / Resultados de clientes
H2: Preguntas frecuentes (objections handling)
H2: CTA final
```

**Reglas de keyword:**
- Keyword en H1, primer párrafo, al menos 1 H2, URL
- Features como beneficios: no "API REST disponible" → "Conecta con tus herramientas en minutos"
- Precio o rango visible (aunque sea orientativo) — evita "contactar para precio" si hay competidores con precio público

**Errores comunes:**
- Texto corporativo sin beneficios concretos ("Somos líderes en...")
- Lista de features sin explicar el beneficio para el usuario
- Sin precio o señal de precio → alto abandono en BOFU

---

### 3. Página de Caso de Uso (Use Case)

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Comercial / Informacional |
| **Keywords que pertenecen aquí** | "[producto] para [industria/rol]", "[tipo de empresa] + [problema]", "software para [use case específico]" |
| **Keywords que NUNCA van aquí** | Brand terms generales, keywords de "qué es" (esas van al blog), keywords del producto principal |
| **Longitud** | 800-1200 words |
| **Schema** | Service, Product (con `audience` property), Article |
| **CTA** | Demo o trial segmentado por el caso de uso ("Empezar para startups", "Ver plan Enterprise") |

**Estructura recomendada:**
```
H1: [Keyword: producto] para [segmento/use case]
  → Párrafo: el problema específico de este segmento
H2: Cómo [producto] resuelve [problema del segmento]
H2: Funcionalidades más usadas por [segmento]
H2: Resultados de [segmento] usando [producto] (+ social proof específico)
H2: Cómo empezar
```

**Diferencia clave con página de producto:**
- La página de producto habla del producto en general
- La página de use case habla del producto desde el punto de vista del cliente específico
- Mismos features, diferente lenguaje, diferente pain point

**Errores comunes:**
- Copiar y pegar la página de producto cambiando solo el H1
- Sin testimonios o casos específicos del segmento
- Keywords del use case no están en URL ni H2s

---

### 4. Caso de Éxito / Case Study

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Comercial (validación social para MOFU/BOFU) |
| **Keywords que pertenecen aquí** | "[empresa cliente] + [resultado]", "caso de éxito [industria]", "[producto] resultados" |
| **Longitud** | 800-1500 words (más largo = más credibilidad) |
| **Schema** | Article, con `author` y `datePublished` |
| **CTA** | Soft: "Lee más casos", "Habla con ventas", "Ver cómo funciona para tu empresa" |

**Estructura recomendada (narrativa):**
```
H1: Cómo [Cliente] logró [resultado específico] con [Producto]
  → Datos clave: resultado en cifras (KPIs)
H2: El desafío: qué problema tenía [cliente] antes
H2: La solución: por qué eligieron [producto]
H2: Implementación: cómo lo pusieron en marcha
H2: Los resultados: datos concretos y comparativa antes/después
H2: Próximos pasos / lo que viene
CTA: "¿Quieres resultados similares? Habla con nosotros"
```

**Reglas críticas de contenido:**
- SIEMPRE datos específicos: no "mejoró el tráfico" → "aumentó el tráfico orgánico un 147% en 4 meses"
- Citar al cliente con nombre, cargo y empresa (con permiso)
- Antes/después claro y cuantificable
- No esconder los números difíciles — la especificidad genera confianza

**Errores comunes:**
- Case study sin cifras concretas (inútil como social proof)
- Redactado como pieza de ventas en lugar de historia real
- Sin el "antes" — sin contexto el resultado no impresiona

---

### 5. Artículo de Blog (Informacional)

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Informacional |
| **Keywords que pertenecen aquí** | "qué es X", "cómo hacer X", "guía de X", "mejores X", "X vs Y" (comparativa neutral), long tails informativos |
| **Keywords que NUNCA van aquí** | Keywords transaccionales ("comprar X", "precio X") — esas van a landing/producto |
| **Longitud** | 1500-3000 words para artículos completos. Puede ser más si el tema lo justifica |
| **Schema** | Article o BlogPosting, con `author`, `datePublished`, `dateModified` |
| **CTA** | Soft (TOFU): newsletter, recurso descargable, link a contenido MOFU. No "Compra ahora" |

**Estructura recomendada:**
```
H1: [Keyword informacional] — guía, cómo, qué es, etc.
  → Párrafo de introducción: responde la pregunta principal en 2-3 oraciones (pirámide invertida)
  → [Opcional] Tabla de contenidos si > 1500 words
H2: [Subtema 1 — keyword secundaria]
H2: [Subtema 2 — pregunta relacionada que hace el usuario]
H2: [Subtema 3 — profundización]
H2: Conclusión / Resumen / Próximos pasos
CTA: Link a recurso MOFU o suscripción
Sección: Artículos relacionados (3-4 links internos)
```

**Reglas de keyword para blog:**
- La keyword debe ser informacional. Si alguien busca "qué es email marketing" no quiere comprar — quiere aprender
- No incluir CTAs transaccionales agresivos en contenido TOFU (aleja al usuario)
- Responder la pregunta completa: no dejar al usuario con dudas que lo manden de vuelta al SERP

**Errores comunes:**
- Introducción de 3-4 párrafos que no responden nada
- Keyword informacional pero contenido de ventas ("email marketing es importante y por eso deberías contratar nuestro servicio")
- Sin sección de contenidos relacionados (oportunidad de linking perdida)
- Fecha de publicación desactualizada sin haber actualizado el contenido

---

### 6. About Us / Páginas de Marca

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Navegacional (el usuario ya conoce la marca) |
| **Keywords que pertenecen aquí** | "[Marca] + quiénes somos", "[Marca] + historia", "[Marca] + equipo" — SOLO branded terms |
| **Keywords que NUNCA van aquí** | Keywords de producto, keywords informacionales, long tails de servicios |
| **Longitud** | 400-800 words. No es un artículo — no sobreexplicar |
| **Schema** | Organization, Person (para team pages), AboutPage |
| **CTA** | Secundario: "Ver nuestros servicios", "Habla con el equipo", "Ver empleos" |

**Función SEO real de About Us:**
- No rankea para keywords competitivas — no es su función
- **Sí aporta E-E-A-T**: señal de autoridad, transparencia, experiencia
- Es la primera página que revisa un rater de Google Quality Rater en sitios YMYL
- Mejora la confianza del usuario que llegó por otra keyword y quiere verificar quién es la empresa

**Estructura recomendada:**
```
H1: [Sobre nosotros / Quiénes somos / Nuestra historia]
  → Párrafo de misión en 2-3 oraciones: quiénes son, para quién trabajan, qué los diferencia
H2: Nuestra historia (o: Por qué existimos)
H2: El equipo (con fotos, nombres, cargos, bios breves)
H2: Nuestros valores / Cómo trabajamos
CTA: Ver cómo podemos ayudarte → link a servicios/productos
```

**Elementos obligatorios para E-E-A-T:**
- Fotos reales del equipo (no stock)
- Nombres completos y cargos
- Años de experiencia o historia de la empresa
- Menciones a prensa, premios, certificaciones si existen
- Ubicación física si aplica

**Errores comunes:**
- About Us genérica: "Somos una empresa comprometida con la excelencia" — sin información real
- Sin fotos del equipo (reduce confianza)
- Intentar optimizar para keywords de producto (contra la intención de búsqueda)

---

### 7. Pricing Page

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Transaccional (alta intención de compra) |
| **Keywords que pertenecen aquí** | "[producto] precio", "[producto] planes", "[producto] cuánto cuesta", "[producto] tarifas" |
| **Keywords que NUNCA van aquí** | Keywords informacionales, brand terms genéricos |
| **Longitud** | 300-600 words de texto + tabla de planes. No sobrecargar con texto |
| **Schema** | Offer, PriceSpecification, SoftwareApplication con precio |
| **CTA** | Por plan: "Empezar gratis", "Contratar plan [X]", "Hablar con ventas" |

**Estructura recomendada:**
```
H1: Planes y precios de [Producto] — o: "Elige tu plan"
  → Tabla de planes: Free / Starter / Pro / Enterprise
  → Precios visibles (o rango claro)
  → Beneficios más importantes por plan (no lista exhaustiva de features)
H2: ¿Qué incluye cada plan? (tabla de comparación de features)
H2: Preguntas frecuentes sobre precios
  → ¿Puedo cambiar de plan? ¿Cómo funciona la facturación? ¿Hay período de prueba?
CTA: [Por plan] + CTA general de ventas para Enterprise
```

**Reglas críticas:**
- Precio visible sin necesidad de contactar (si es posible) — el precio oculto genera desconfianza
- Si es pricing enterprise o custom, poner un rango orientativo ("desde $X/mes") o "solicitar demo"
- FAQ de precios responde objeciones de compra (no FAQ de producto)
- Toggle anual/mensual si aplica — mostrar el ahorro en %

**Errores comunes:**
- Precio completamente oculto o "contáctanos para precio" sin ninguna señal
- Tabla de planes con 30 features iguales y diferencia mínima (confusión = no conversión)
- Sin FAQ que responda objeciones de precio

---

### 8. Comparison Page (X vs Y / Alternativas a X / Mejor X)

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Comercial (investigación pre-compra) |
| **Keywords que pertenecen aquí** | "[tu producto] vs [competidor]", "alternativas a [competidor]", "mejor [categoría de software]", "[producto A] o [producto B]" |
| **Keywords que NUNCA van aquí** | Keywords de producto propio (esas van a la página de producto), informacionales de educación |
| **Longitud** | 1200-2000 words para comparativas completas |
| **Schema** | Article, FAQPage (para H2 en forma de preguntas) |
| **CTA** | "Ver por qué elegir [tu producto]", "Empezar gratis", "Migrar desde [competidor]" |

**Estructura recomendada:**
```
H1: [Producto A] vs [Producto B]: Comparativa completa [año]
  → Tabla resumen: 5-7 criterios clave con ganador en cada uno
H2: ¿Cuándo elegir [Producto A]? (puntos fuertes)
H2: ¿Cuándo elegir [Producto B]? (puntos fuertes del competidor — ser objetivo)
H2: Comparativa detallada por categoría
H2: Precios: comparativa de planes
H2: Valoraciones de usuarios (G2, Capterra, Trustpilot)
H2: Conclusión y recomendación
CTA: [Tu producto] + prueba gratis
```

**Regla fundamental: ser objetivo o parecer objetivo**
- Si atacas al competidor sin objetividad, el usuario no te cree
- Reconocer dónde el competidor es mejor (para casos de uso específicos) aumenta credibilidad
- La tabla de comparación debe tener criterios donde también el competidor tenga checkmarks
- Citar reseñas reales de terceros (G2, Capterra) para ambos productos

**Errores comunes:**
- Tabla donde tu producto gana en TODO sin objetividad
- No actualizar la página cuando el competidor actualiza su producto
- Atacar al competidor directamente con afirmaciones no verificables

---

### 9. Category Page (Blog y Ecommerce)

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Navegacional + Informacional |
| **Keywords que pertenecen aquí** | "[categoría temática]", "[tema general]", "artículos sobre X" — terms paraguas de la categoría |
| **Longitud** | 300-500 words de texto introductorio + lista de artículos/productos |
| **Schema** | CollectionPage, BreadcrumbList |
| **CTA** | Link a artículos destacados, filtros de contenido, suscripción |

**Estructura recomendada:**
```
H1: [Nombre de la categoría como keyword]
  → Párrafo introductorio: qué temas cubre esta categoría (150-300 words)
  → Artículos/productos destacados above the fold
Lista: Todos los artículos/productos de la categoría (paginada si > 20)
```

**Reglas de categorías para topical authority:**
- El nombre de la categoría debe ser una keyword que la gente busca (no "Misc", "Varios", "General")
- Una categoría = un cluster temático coherente
- El texto introductorio de la categoría no es decorativo: debe incluir la keyword y describir el tema
- Las categorías de 1-3 artículos no tienen suficiente topical authority — consolidar o eliminar

---

### 10. FAQ / Knowledge Base

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Informacional (soporte + búsqueda) |
| **Keywords que pertenecen aquí** | "[producto] cómo", "[producto] problema", "[feature] configurar", preguntas directas sobre el producto |
| **Longitud** | Respuesta directa en 100-200 words por pregunta. No sobreexplicar |
| **Schema** | FAQPage (para FAQs generales, no rich result en Google para sitios comerciales, pero sí en Bing y AI search) |
| **CTA** | Link a documentación completa, link a soporte, link a página de producto |

**Estructura recomendada:**
```
[Pregunta como H2 o H3] — usar la pregunta exacta que hace el usuario
  → Respuesta directa en el primer párrafo (pirámide invertida)
  → Detalles adicionales si son necesarios
  → Link a documentación completa si es muy técnico
```

**Reglas:**
- La pregunta en el H2/H3 debe coincidir con el lenguaje del usuario (no jerga interna)
- Respuesta directa en primeras 2 oraciones — no introducción
- Cada FAQ debe tener su propia URL si tiene suficiente volumen de búsqueda (para rankear individualmente)

---

### 11. Landing Page (Conversión)

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Transaccional (conversión única) |
| **Keywords que pertenecen aquí** | Keyword muy específica de la oferta: "[oferta] + [audiencia]", "[evento] + registro", "[recurso] + descarga gratis" |
| **Longitud** | 300-800 words. Depende de la complejidad de la oferta (más cara = más larga) |
| **Schema** | Event (para webinars), Product (para offers), Article (para lead magnets) |
| **CTA** | Único, claro, above the fold, repetido al final. No dar opciones múltiples |

**Reglas críticas de landing pages:**
- **Una landing = una keyword = un CTA** — no diluir
- Sin nav principal en muchos casos (evita distracción y abandono)
- Message match SERP → Landing obligatorio: si el anuncio o snippet dice "Guía gratuita de SEO", la landing dice "Guía gratuita de SEO" — exactamente
- No pedir información que no se va a usar (menos campos = más conversiones)

---

### 12. Producto Ecommerce (Product Detail Page)

| Atributo | Especificación |
|----------|---------------|
| **Intent primario** | Transaccional |
| **Keywords que pertenecen aquí** | "[marca producto] [modelo]", "[tipo producto] + [característica]", "[producto] + [color/talla/versión]" |
| **Keywords que NUNCA van aquí** | Keywords genéricas de categoría (esas van a la category page), keywords informacionales |
| **Longitud** | 300-500 words mínimo. Incluir descripción larga técnica + descripción corta de beneficios |
| **Schema** | Product, Offer, AggregateRating (si tiene reviews), ItemAvailability |
| **CTA** | "Añadir al carrito", "Comprar ahora", "Ver disponibilidad" |

**Estructura recomendada:**
```
H1: [Nombre del producto — marca + modelo + característica diferenciadora]
  → Precio visible
  → Selector de variantes (talla, color, cantidad)
  → CTA "Añadir al carrito" above the fold
  → Imágenes múltiples (frente, lateral, detalle, en uso)
H2: Descripción del producto (beneficios primero, specs después)
H2: Especificaciones técnicas (tabla)
H2: Opiniones de clientes (UGC para E-E-A-T)
H2: Productos relacionados / También te puede gustar
```

**Reglas de imágenes en ecommerce:**
- Mínimo 4-6 fotos: principal, ángulos, detalles, en uso/lifestyle
- Alt text: "[marca] [modelo] [color/material] — [ángulo/uso]"
- Nombre de archivo: `nike-air-max-270-negro-frontal.webp`
- Zoom habilitado (desktop) y swipe (mobile)
- **Sin texto con precio o descuento dentro de la imagen** — usar overlays HTML/CSS

---

## Search Intent × Content Type Matrix

Tabla de referencia rápida: dada una keyword, ¿qué tipo de contenido corresponde?

| Tipo de query | Intent | Tipo de página correcto |
|--------------|--------|------------------------|
| "qué es [X]" | Informacional | Blog artículo |
| "cómo hacer [X]" | Informacional | Blog artículo / guía |
| "[X] para [industria/rol]" | Comercial | Use case page |
| "mejor [X]" | Comercial | Blog comparativa o category page |
| "[X] vs [Y]" | Comercial | Comparison page |
| "alternativas a [X]" | Comercial | Comparison page |
| "[marca] precio" | Transaccional | Pricing page |
| "comprar [X]" | Transaccional | Product page o ecommerce |
| "[X] gratis" | Transaccional | Landing page o feature page |
| "[marca] quiénes son" | Navegacional | About us |
| "[marca] [producto]" | Navegacional/Trans | Homepage o product page |
| "[problema específico]" | Informacional/Comercial | Blog artículo o use case |
| "[producto] review" | Comercial | Blog o case study |
| "[producto] tutorial" | Informacional | Blog / documentación |
| "[ciudad] + [servicio]" | Transaccional local | Location page |

**Regla de oro:** Si Google muestra en el top 10 para una keyword X principalmente blogs → esa keyword pertenece a un blog. Si muestra product pages → pertenece a una product page. El SERP te dice qué tipo de contenido quiere Google para esa query.

---

### Word Count Analysis
Compare against page type minimums:
| Page Type | Minimum |
|-----------|---------|
| Homepage | 500 |
| Service page | 800 |
| Blog post | 1,500 |
| Blog comparison / guide | 1,500-3,000 |
| Use case page | 800-1,200 |
| Case study | 800-1,500 |
| About us | 400-800 |
| Pricing page | 300-600 (texto) |
| Comparison page | 1,200-2,000 |
| Ecommerce product page | 300-500 |
| Category page | 300-500 (intro) |
| FAQ single answer | 100-200 |
| Landing page | 300-800 |
| Location page | 500-600 |

> **Important:** These are **topical coverage floors**, not targets. Google has confirmed word count is NOT a direct ranking factor. The goal is comprehensive topical coverage; a 500-word page that thoroughly answers the query will outrank a 2,000-word page that doesn't. Use these as guidelines for adequate coverage depth, not rigid requirements.

### Readability
- Flesch Reading Ease: target 60-70 for general audience

> **Note:** Flesch Reading Ease is a useful proxy for content accessibility but is NOT a direct Google ranking factor. John Mueller has confirmed Google does not use basic readability scores for ranking. Yoast deprioritized Flesch scores in v19.3. Use readability analysis as a content quality indicator, not as an SEO metric to optimize directly.
- Grade level: match target audience
- Sentence length: average 15-20 words
- Paragraph length: 2-4 sentences

### Keyword Optimization
- Primary keyword in title, H1, first 100 words
- Natural density (1-3%)
- Semantic variations present
- No keyword stuffing

### Content Structure
- Logical heading hierarchy (H1 -> H2 -> H3)
- Scannable sections with descriptive headings
- Bullet/numbered lists where appropriate
- Table of contents for long-form content

### Multimedia
- Relevant images with proper alt text
- Videos where appropriate
- Infographics for complex data
- Charts/graphs for statistics

### Internal Linking
- 3-5 relevant internal links per 1000 words
- Descriptive anchor text
- Links to related content
- No orphan pages

### External Linking
- Cite authoritative sources
- Open in new tab for user experience
- Reasonable count (not excessive)

---

## Writing Strategy: Pirámide Invertida

La pirámide invertida es el modelo de redacción que maximiza la satisfacción del usuario (y evita el pogo-sticking). Lo más importante va primero.

### Estructura pirámide invertida
```
[NIVEL 1 — Respuesta directa]
La respuesta, conclusión o dato más importante va en el primer párrafo.
El usuario no debe hacer scroll para encontrar lo que buscó.

[NIVEL 2 — Contexto y desarrollo]
Explica el "por qué" y "cómo" de la respuesta principal.
Datos, ejemplos, casos de uso, evidencia de soporte.

[NIVEL 3 — Detalle y profundidad]
Información complementaria, casos edge, referencias,
comparativas, alternativas. Para el usuario que quiere más.

[NIVEL 4 — Recursos adicionales]
Links relacionados, herramientas, lecturas recomendadas,
sección de FAQ, contenidos relacionados.
```

### Anti-patrón: introducción que no dice nada
```
❌ MAL (4 párrafos antes de responder):
"En el mundo actual, el email marketing es más importante que nunca.
Las empresas de todos los tamaños se preguntan cómo mejorar sus
resultados. En este artículo exploraremos todos los aspectos del
email marketing para SaaS. ¡Empecemos!"

✅ BIEN (responde en el primer párrafo):
"El email marketing para SaaS genera el mayor ROI de todos los
canales digitales: $36 por cada $1 invertido. En esta guía
encontrarás la estrategia completa: desde la construcción de lista
hasta la automatización de onboarding."
```

### Checklist pirámide invertida
- [ ] El primer párrafo responde la query o resume el beneficio principal
- [ ] La respuesta clave aparece antes del primer H2
- [ ] No hay más de 2-3 oraciones de "calentamiento" antes de entrar al tema
- [ ] Cada sección (H2) abre con su punto más importante
- [ ] La conclusión no agrega información nueva — resume y propone siguiente paso

---

## Redacción Natural y Control de Densidad

### Reglas de escritura natural
| Regla | Descripción | Check |
|-------|-------------|-------|
| Sin repetición de frase exacta | No usar la keyword exacta más de 1 vez por párrafo | — |
| Variantes semánticas | Usar sinónimos, abreviaciones, términos relacionados | — |
| Voz activa | "Google analiza el contenido" > "El contenido es analizado por Google" | — |
| Segunda persona | "Puedes configurar X" > "Se puede configurar X" (más directo) | — |
| Sin relleno | Eliminar: "Es importante destacar que", "En el mundo actual", "Cabe mencionar" | — |
| Oraciones cortas | Promedio 15-20 palabras. Oraciones de 30+ palabras = difíciles de escanear | — |
| Párrafos cortos | 2-4 oraciones por párrafo en web. Máximo 5-6 en contenido técnico | — |

### Detección de over-optimization
Señales de que el contenido está sobre-optimizado para keyword (lo que Rank Math marcaria en rojo):
- La keyword exacta aparece más de 3 veces en los primeros 200 words
- Múltiples H2s con la keyword exacta (en lugar de variantes)
- Alt texts de imágenes que son solo la keyword
- Anchor texts todos iguales apuntando al mismo destino
- Keyword en cada párrafo sin variaciones

### Densidad recomendada por elemento
| Elemento | Keyword principal | Keywords secundarias |
|----------|------------------|---------------------|
| Title tag | 1 vez (al inicio) | 0 (no espacio) |
| H1 | 1 vez | 0 |
| H2s (total) | 1-2 veces | 1 por H2 relevante |
| Primer párrafo | 1 vez | 1-2 variantes |
| Body text (1500 words) | 10-20 menciones totales | Distribuidas naturalmente |
| Meta description | 1 vez (al inicio) | 0-1 variante |
| Alt text | 1 imagen con keyword | 0-1 en otras |

---

## Elementos de Contenido: Escaneabilidad y Peso Visual

El "peso del contenido" es el balance entre bloques de texto, elementos visuales y espacio. Un artículo solo de texto en bloques es abandono garantizado.

### Elementos que deben estar presentes según el tipo de contenido

| Elemento | Blog informacional | Landing page | Guía larga | Página de producto |
|----------|--------------------|-------------|------------|-------------------|
| Bullets / listas | ✅ Obligatorio | ✅ | ✅ | ✅ |
| Negritas en conceptos clave | ✅ | ✅ | ✅ | ✅ |
| Imágenes ilustrativas | ✅ 1 cada 400-600 words | ✅ | ✅ | ✅ |
| Tablas comparativas | Si hay comparaciones | Precios/features | ✅ | ✅ |
| Infografías | Para procesos o datos | Opcional | ✅ | Opcional |
| Callout / caja destacada | Para datos clave | CTAs | ✅ | Trust signals |
| Video embebido | Si existe contenido relacionado | Demo | Tutorial | Demo |
| Tabla de contenidos | Posts > 1500 words | ❌ | ✅ Obligatorio | ❌ |
| Hipervínculos internos | ✅ 3-5 por 1000 words | 2-3 | ✅ | 2-3 |
| Hipervínculos externos | A fuentes citadas | Solo si necesario | ✅ | Reviews externos |

### Uso correcto de negritas
```
❌ MAL — negritas decorativas sin criterio:
"El email marketing es **una de las** estrategias **más** utilizadas **hoy en día**"

✅ BIEN — negritas en conceptos clave o datos:
"El email marketing genera un **ROI promedio de $36 por cada $1 invertido**,
lo que lo convierte en el canal de mayor rendimiento en B2B SaaS."
```

Regla: Las negritas son para el usuario que escanea. Si alguien solo lee las negritas, debe poder entender la idea central del párrafo.

### Uso correcto de bullets y listas
```
✅ Usar listas cuando:
- Hay 3 o más items enumerables
- Los items son paralelos (misma estructura gramatical)
- El orden importa (usar lista numerada) o no importa (usar bullets)
- La lista agiliza la lectura vs. un párrafo

❌ NO usar listas cuando:
- Solo hay 2 items (queda mejor en prosa)
- Los items son demasiado largos (cada uno > 2 líneas)
- La lista rompe el flujo narrativo de una argumentación
```

### Infografías y elementos visuales complejos
- Siempre incluir un `alt` descriptivo que resuma el contenido visual
- Incluir el dato o información clave del infográfico en texto HTML (Google no lee el texto dentro de la imagen)
- No usar infografía para reemplazar texto indexable — usarla para complementarlo
- Formato: WebP o SVG (no PNG pesado)
- Infografías con demasiado texto → dividir en secciones de texto + imagen simple

### Regla absoluta: sin texto crítico dentro de imágenes
El texto dentro de imágenes es **invisible para Google** y para usuarios con lectores de pantalla.

| Tipo de "texto en imagen" | Problema | Solución |
|--------------------------|---------|---------|
| Estadísticas en infografía | Google no las indexa | Repetir el dato clave en párrafo HTML |
| CTA dentro de banner | No clickeable como texto | Usar HTML + CSS para el CTA |
| Pasos de un proceso en imagen | No rankeable para "cómo hacer X" | Añadir lista numerada en HTML debajo |
| Precio o feature en screenshot | No aparece en fragmentos | Describir en texto el dato visible |
| Título de infografía en la imagen | H2 o H3 no detectado | Añadir el título como H2/H3 HTML |

---

## Content Scannability Audit

Antes de publicar, hacer el "test de escaneo": cubrir el texto y ver solo los elementos visuales (headings, bullets, negritas, imágenes). El usuario que escanea debe poder entender de qué trata el artículo y cuál es el valor.

### Checklist de escaneabilidad
- [ ] H2s comunican el tema de cada sección sin leer el texto
- [ ] Las negritas destacan datos, beneficios o conceptos clave (no decorativas)
- [ ] Hay al menos 1 imagen o elemento visual cada 400-600 words
- [ ] Los bullets son paralelos y autoexplicativos
- [ ] Ningún bloque de texto supera 150-200 words sin un heading o elemento visual
- [ ] Las tablas tienen headers claros y no son demasiado anchas para mobile
- [ ] Hay espaciado visual entre secciones (whitespace, no solo `<br>`)
- [ ] Los links son reconocibles visualmente (subrayado o color diferente)
- [ ] La introducción no supera 100-120 words antes del primer H2

## AI Content Assessment (2026 update)

Google's 2026 stance: **"human-curated"**, not "human-created." Content can be AI-assisted at any stage — what matters is human curation, editorial oversight, and genuine E-E-A-T signals.

> **Data point:** 86.5% of top-ranking pages use AI assistance in content creation (Ahrefs 600K-page study, 2025). AI assistance is not penalized. AI-generated content without human value-add and E-E-A-T signals IS penalized.

### Acceptable AI Content
- Human-curated: an expert reviewed, edited, and takes responsibility for accuracy
- Demonstrates genuine E-E-A-T (Experience layer is hardest to fake with AI)
- Provides unique value not available elsewhere
- Contains original insights, data, or perspectives
- Author attribution is clear and verifiable

### Low-Quality AI Content Markers (Scaled Content Abuse)
- Generic phrasing with no specificity ("This is an important topic...")
- No original insight, data, or perspective
- Repetitive structure across many pages (programmatic pattern)
- No author attribution or unverifiable bylines
- Factual inaccuracies or hallucinated citations
- Thin pages created purely to rank, with no user value

### AI-Generated Image Requirements (IPTC — Google mandatory)
If content includes AI-generated images, Google now **requires** IPTC DigitalSourceType metadata:
```
IPTC DigitalSourceType: TrainedAlgorithmicMedia
```
This metadata must be embedded in the image file (Exif/IPTC) AND disclosed to platforms:
- Google requires this for Google Images, Google Discover, and Google Merchant Center
- Use ExifTool: `exiftool -IPTC:DigitalCreator="TrainedAlgorithmicMedia" image.jpg`
- Reference: [Google Image License Metadata](https://developers.google.com/search/docs/appearance/structured-data/image-license-metadata)

### HCU Recovery Context
> **Helpful Content System (March 2024):** Merged into Google's core ranking algorithm — no longer a standalone classifier. However, sites hit by HCU should note:
> - Recovery is partial: sites typically recover only **~1/3 of original traffic** even with improvements
> - Impact is page-level (not site-wide) as of 2024
> - March 2024 core update reduced low-quality content in results by **45%**
> - Recovery path: remove/noindex thin content, strengthen E-E-A-T on remaining pages, demonstrate topical authority through content clusters

## AI Citation Readiness (GEO signals)

> **Key data point:** 96% of AI Overview citations come from sources with verified E-E-A-T signals. Pages scoring 8.5/10+ on semantic completeness are **4.2x more likely** to be cited in AI Overviews.

### Tactical Playbook for AI Citation

**1. TL;DR Summary Box (50-70 words)**
Add a summary box immediately after the H1 (or at article start) with the core answer. This is the most-cited element by AI systems:
```html
<div class="tldr-box">
  <strong>TL;DR:</strong> [Direct answer to the page's primary query in 50-70 words,
  including 1-2 key data points.]
</div>
```

**2. Fact Density (statistics every 150-200 words)**
- Include a statistic, data point, or verifiable fact every 150-200 words
- Each stat needs a source citation (inline or footnote)
- Prioritize first-party data > industry studies > secondary sources

**3. Quotability Scoring — aim for 3+ quotable sentences per 1,000 words**
A quotable sentence is: specific (has a number), standalone (understandable without context), and attributable (tied to your brand/research):
```
✅ Quotable: "Companies that publish 11+ posts/month generate 3x more traffic than those publishing 0-1 posts/month (HubSpot, 2025)."
❌ Not quotable: "Publishing more content generally improves traffic results."
```

**4. Structured Q&A Pairs**
For each major question your content answers, create an explicit H2/H3 question heading + direct-answer first paragraph:
```
H2: What is the ideal blog post length for SEO?
[Answer in first 40 words] Blog posts between 1,500-2,500 words perform best for organic rankings, with long-form content (3,000+ words) dominating AI citation rates. However, word count is a proxy for topical coverage, not a direct ranking factor.
```

**5. Entity Declaration**
State the primary entity (brand, author, organization) clearly in the first 100 words. AI systems use this to determine attribution.

### AI Search Visibility & GEO (2025-2026)

**Google AI Mode** launched publicly in May 2025 as a separate tab in Google Search, available in 180+ countries. Unlike AI Overviews (which appear above organic results), AI Mode provides a fully conversational search experience with **zero organic blue links**, making AI citation the only visibility mechanism.

**Key optimization strategies for AI citation:**
- **Structured answers:** Clear question-answer formats, definition patterns, and step-by-step instructions that AI systems can extract and cite
- **First-party data:** Original research, statistics, case studies, and unique datasets are highly cited by AI systems
- **Schema markup:** Article, FAQ (for non-Google AI platforms), and structured content schemas help AI systems parse and attribute content
- **Topical authority:** AI systems preferentially cite sources that demonstrate deep expertise. Build content clusters, not isolated pages
- **Entity clarity:** Ensure brand, authors, and key concepts are clearly defined with structured data (Organization, Person schema)
- **Multi-platform tracking:** Monitor visibility across Google AI Overviews, AI Mode, ChatGPT, Perplexity, and Bing Copilot, not just traditional rankings. Treat AI citation as a standalone KPI alongside organic rankings and traffic.

**Generative Engine Optimization (GEO):**
GEO is the emerging discipline of optimizing content specifically for AI-generated answers. Key GEO signals include: quotability (clear, concise extractable facts), attribution (source citations within your content), structure (well-organized heading hierarchy), and freshness (regularly updated data). Cross-reference the `seo-geo` skill for detailed GEO workflows.

## Content Freshness

### Basics
- Publication date visible
- Last updated date if content has been revised (update the dateModified in schema too)
- Flag content older than 12 months without update for fast-changing topics

### QDF (Query Deserves Freshness) Algorithm
Google's QDF system boosts freshness for queries where recency signals matter. Triggers:
- **Breaking/trending topics:** News, events, product launches → freshness window: hours to days
- **Recurring events:** Annual reports, seasonal content → re-rank spikes around the event date
- **Evergreen with freshness decay:** Most informational content → freshness boost fades over time

### Freshness Decay Rates by Industry
| Industry | Decay window (significant ranking drop) | Update frequency recommendation |
|----------|----------------------------------------|--------------------------------|
| Technology / SaaS | 4-6 months | Quarterly |
| Finance / Investing | 6-9 months | Every 6 months or after major market events |
| Healthcare / Medical | 8-12 months | Annual + after guideline changes |
| Education | 12-18 months | Annual |
| Legal | 6-12 months | After legislation changes |
| Travel | 6-9 months | Pre-season + after major changes |
| News / Current events | Days to weeks | Continuous |
| General evergreen | 12-24 months | Annual review minimum |

### Content Velocity Impact
Sites that publish on a **weekly cadence** produce **3.2x better ranking improvements** than sites publishing monthly. This is driven by:
- More crawl frequency (Googlebot crawls sites with fresh content more often)
- More topical coverage expansion
- More internal linking opportunities
- Faster compounding authority

### Content Update vs. New Content Decision
| Scenario | Action |
|----------|--------|
| Existing page ranks 5-15, content is outdated | Update + expand the existing page |
| Existing page ranks 15-30, thin content | Rewrite substantially or consolidate |
| Keyword gap with no existing page | Create new page |
| Existing page ranks 1-5 | Update data/dates, do NOT restructure |
| Multiple pages covering same topic | Consolidate into one definitive page |

---

## SXO Layer: Search-First Content Design

El contenido no existe en el vacío — se descubre desde el SERP. Esta capa analiza si el contenido satisface al usuario que llega desde búsqueda.

### Search Intent Satisfaction Score

Evaluar si el contenido resuelve la intención **en los primeros 100 words** (antes de que el usuario considere volver al SERP):

| Tipo de intención | ¿Cómo satisfacerla rápido? | Señal de fail |
|------------------|--------------------------|---------------|
| Informacional | Respuesta directa en párrafo 1 | Intro genérica de 3 párrafos antes de la respuesta |
| Transaccional | Precio/CTA visible sin scroll | "Contáctanos para precio" sin cifra indicativa |
| Comercial/comparativa | Tabla o resumen de opciones al inicio | Artículo promocional sin comparativa real |
| Navegacional | Acceso directo al recurso o sección | Página genérica de la marca |

### Content by Funnel Stage (TOFU / MOFU / BOFU)

Cada pieza de contenido tiene un stage. El tono, la profundidad y los CTAs deben coincidir:

| Stage | Intención | Tono | CTA apropiado | CTA a evitar |
|-------|-----------|------|---------------|--------------|
| TOFU | Aprender / explorar | Educativo, sin venta | "Leer más", "Descargar guía" | "Comprar ahora", "Solicitar demo" |
| MOFU | Comparar / evaluar | Analítico, objetivo | "Ver demo", "Comparar planes" | "¡Oferta limitada!" |
| BOFU | Decidir / comprar | Directo, enfocado en beneficios | "Empezar gratis", "Contratar" | Artículos teóricos sin CTA |

### Emotional Resonance and Persuasion

El contenido que rankea Y convierte activa uno o más triggers:
- **Curiosidad:** datos inesperados, preguntas sin respuesta inmediata
- **Urgencia:** contexto temporal (tendencias 2025, cambios recientes)
- **Autoridad:** datos propios, experiencia directa, credenciales específicas
- **Empatía:** reconocer el dolor del usuario antes de ofrecer la solución
- **Especificidad:** "ahorra 3h/semana" > "ahorra tiempo"

### Narrative and Storytelling for SEO

El contenido estructurado como narrativa mantiene al usuario más tiempo (señal de satisfacción):
- **Problema → Consecuencias → Solución** (clásico, funciona en MOFU)
- **Situación → Complicación → Resolución** (ideal para casos de estudio)
- **Dato sorpresa → Contexto → Implicaciones** (funciona bien en TOFU)
- Usar la segunda persona ("tú/vos/usted") para crear conexión directa
- Evitar pasiva impersonal que distancia: "se debe" → "debes"

### Micro-copy in Content

Textos pequeños que impactan el engagement:
- **Títulos de sección (H2/H3):** ¿Describen el beneficio o solo el tema? ("Cómo reducir el CPC" > "Optimización de costos")
- **Bullet points:** Cada bullet debe ser autosuficiente, no fragmento de oración
- **CTAs en texto:** Anclas de internal links como mini-CTAs ("Ver guía completa de X →")
- **Pull quotes / callouts:** Destacar la frase más citable para AI overviews

---

## Google Discover Optimization

Google Discover shows content to users based on their interests — without a search query. It can drive significant traffic for content sites, blogs, and news.

### Requirements (Google official, 2025)
- Page must be **indexed** by Google — no special markup required
- Content must comply with Discover content policies
- No clickbait, sensationalism, or misleading headlines

### Image Requirements — CRITICAL
- Minimum width: **1200 pixels**
- Minimum resolution: **300,000 total pixels** (e.g., 1280×720)
- Aspect ratio: **16:9 landscape** preferred
- Must depict actual page content (no logos, no text-heavy images)
- Declare preferred image via:
  ```html
  <!-- Option 1: OG meta tag -->
  <meta property="og:image" content="https://example.com/image-1200w.jpg" />

  <!-- Option 2: schema.org image property in JSON-LD -->
  ```
- **Enable large image previews:**
  ```html
  <meta name="robots" content="max-image-preview:large" />
  ```
  Without this tag, Google may show a small thumbnail in Discover instead of a full card.

### Content Signals for Discover
- Timely, story-driven, or uniquely insightful content
- Strong E-E-A-T signals (same as Search)
- Good page experience / Core Web Vitals
- Content aligns with user interests (topical authority signals)

### What Does NOT Appear in Discover
- Job applications, petitions, forms
- Code repositories
- Context-free satirical content
- Commercial-only pages (product listings, pricing)

### February 2026 Discover Core Update
Google's **first-ever Discover core update** (February 5, 2026) introduced a separate algorithm for Discover, independent from Search:

- **Visual Quality Score** is now a standalone ranking factor for Discover (not just image size)
  - Images must be high-quality, contextually relevant, visually appealing
  - Preferred width: **1,600px** (upgraded from 1,200px minimum — still the floor)
- **AI summaries now comprise 51% of the Discover feed** — optimizing for AI citation directly impacts Discover
- **Clickbait penalty:** Misleading or exaggerated headlines trigger 30-60% traffic drops. Google's algorithms now detect clickbait patterns beyond simple keyword matching
- **Discover has its own ranking algorithm** — a page can rank in Search but not in Discover, and vice versa

### Discover Image Requirements (updated Feb 2026)
- Minimum width: **1,200px** (hard floor)
- **Recommended width: 1,600px** for optimal card display
- Minimum resolution: **300,000 total pixels** (e.g., 1280×720)
- Aspect ratio: **16:9 landscape** preferred
- Must depict actual page content (no logos, no text-heavy images)
- Declare preferred image via:
  ```html
  <!-- Option 1: OG meta tag -->
  <meta property="og:image" content="https://example.com/image-1600w.jpg" />
  ```
- **Enable large image previews:**
  ```html
  <meta name="robots" content="max-image-preview:large" />
  ```

### Content Signals for Discover
- Timely, story-driven, or uniquely insightful content
- Strong E-E-A-T signals (same as Search)
- Good page experience / Core Web Vitals
- Content aligns with user interests (topical authority signals)
- No clickbait headlines (triggers 30-60% traffic penalties)
- Seasonal content: **publish within 48-72 hours** of the trend peak (Discover traffic peaks fast, then drops rapidly — very different from organic search which takes 2-6 weeks to rank)

### What Does NOT Appear in Discover
- Job applications, petitions, forms
- Code repositories
- Context-free satirical content
- Commercial-only pages (product listings, pricing)

### Discover vs Search
| Dimension | Google Search | Google Discover |
|-----------|--------------|----------------|
| Trigger | User query | User interest (proactive) |
| Content type | Any | Editorial, articles, videos |
| Image requirement | Optional | 1,200px+ (1,600px recommended) |
| Image quality | Optional | Visual Quality Score (standalone signal) |
| Ranking signals | Keywords + authority | Freshness + interest + E-E-A-T |
| max-image-preview | Optional | Required for full card display |
| Algorithm | Core Search | Separate Discover algorithm (Feb 2026) |
| Clickbait penalty | Moderate | Severe (30-60% traffic drop) |

---

## Google Ranking Systems — Reference

Understanding Google's active ranking systems helps prioritize content strategy. These are NOT separate algorithm updates but permanent components of Google's ranking infrastructure.

| System | What It Does | SEO Implication |
|--------|-------------|----------------|
| **BERT** | Understands word combinations and intent | Write naturally, optimize for meaning not keywords |
| **RankBrain** | Matches queries to concepts (not just words) | Cover related concepts comprehensively |
| **Neural Matching** | Matches page concepts to query concepts | Topical depth > keyword density |
| **MUM** | Understands complex multi-modal queries | Comprehensive content covering multiple angles |
| **Passage Ranking** | Ranks individual page sections independently | Well-structured H2/H3 sections can rank for specific queries |
| **PageRank** | Evaluates link quality and authority | Earn relevant, quality backlinks |
| **Freshness System** | Favors newer content for time-sensitive queries | Update evergreen content regularly |
| **Original Content System** | Rewards original reporting and research | Prioritize first-hand content, unique data |
| **Reviews System** | Rewards expert, insightful reviews | In-depth reviews with genuine analysis outrank thin reviews |
| **Site Diversity System** | Limits same domain to ~2 results in top SERP | Quality > quantity of pages; don't over-optimize for one query |
| **Deduplication System** | Removes near-duplicate results | Avoid copying content; use canonical for syndicated content |
| **Exact Match Domain** | Prevents EMD gaming | Domain name alone doesn't boost rankings |
| **SpamBrain** | Detects keyword stuffing, cloaking, manipulative links | Never use black-hat techniques |
| **Removal-Based Demotion** | Demotes sites with many DMCA/defamation removals | Maintain clean copyright record |

**Retired/Merged systems (for reference):**
- Helpful Content System → merged into core ranking (March 2024)
- Panda → merged into core (2015)
- Penguin → merged into core (2016)
- Hummingbird → foundational, superseded by BERT/MUM

## Zero-Click & SERP Feature Optimization

> **Context:** 60%+ of Google searches in 2025-2026 are zero-click — the user gets their answer directly in the SERP without visiting any page. Mobile users are 66% more likely to experience zero-click. This changes the measurement framework: brand visibility and impressions matter even when clicks don't follow.

### People Also Ask (PAA) — Appears in ~75% of Searches

PAA appeared in **75% of Google searches** as of 2025, with visibility growing 34.7% in 2024-2025. PAA results feed directly into AI Overviews.

**PAA Mining Workflow:**
1. Search target keyword → capture all PAA questions (expand all)
2. Use AlsoAsked.com for visual PAA tree maps (better than Answer The Public)
3. Cluster questions by intent type: definitional, procedural, comparative, factual
4. Map each cluster to a content section (H2 or H3)

**PAA Answer Formatting:**
- Answer under **50 words** in the first sentence after the H2/H3 question heading
- Follow the pirámide invertida: answer first, then elaborate
- Use question-based H2/H3 headings that match PAA phrasing exactly
- Add schema markup for questions (useful for AI platforms even if Google restricts rich results)

**PAA Clustering Strategy:**
- Group PAA questions by topic and address multiple in one section
- Questions in PAA clusters are highly correlated with Featured Snippet eligibility
- Definitional PAA questions ("What is X?") have lower AI Overview displacement than procedural ones

### Featured Snippet Optimization

**AI Overviews appear on 58% of queries** — but pages optimized for featured snippets are the most likely to be cited in AI Overviews. Snippet optimization = AI citation optimization.

| Snippet type | Optimal format | Query type | AI Overview rate |
|-------------|---------------|------------|-----------------|
| Paragraph | 40-60 word answer, plain prose | Definitional "what is" | High (~80%) |
| List (ordered) | 5-8 steps, imperative tense | Procedural "how to" | Low (~30%) |
| List (unordered) | 5-8 bullets, consistent structure | "Best X", "Types of X" | Medium |
| Table | 3 columns × 5-6 rows | Comparative, pricing | Medium |
| Video | YouTube embed as primary content | Tutorial "how to" | Low |

**Snippet targeting checklist:**
- [ ] H2/H3 heading is phrased as the target question
- [ ] Direct answer in first 40-60 words after the heading
- [ ] If targeting table snippet: use 3-column HTML table with clear headers
- [ ] If targeting list snippet: ordered list with 5-8 items, under 10 words each
- [ ] Page already ranks on page 1 (snippets are taken from page 1 results)
- [ ] No AI Overview currently shown for the query (or optimize to be the cited source)

### Zero-Click Brand Visibility Metrics
When 60%+ of searches don't produce a click, measure:
- **Impressions** in GSC (your brand appears = value, even without clicks)
- **SERP feature captures:** Count featured snippets, PAA inclusions, Knowledge Panel appearances
- **Share of Voice** in your keyword cluster (what % of SERP features do you own)
- **AI citation tracking:** Use tools like Semrush AI Toolkit, Profound.co, or manual sampling of target queries in AI Mode/ChatGPT/Perplexity

---

## Reddit & UGC Strategy

> **Context:** Reddit appears in **97% of Google search results** (2025). After the Google-Reddit partnership ($60M/year deal), Reddit's organic traffic jumped from 500M to 3.4B monthly visits in one year. Reddit is now a mandatory consideration in content strategy — both as competition and as an opportunity.

### Why Reddit Ranks Everywhere
- Google's "Experience" layer in E-E-A-T elevates UGC because real users provide genuine first-person experience
- Reddit posts often outrank expert content because they have authentic discussion, real use cases, and high user engagement signals
- Google-Reddit data partnership gives Google access to Reddit's full content graph

### How to Leverage Reddit for SEO
**1. Reddit as Keyword Research Source:**
- Search target topic on Reddit → find exact language users use ("I tried X and...")
- Reddit vocabulary ≠ Google keyword vocabulary. Use Reddit phrasing in headings
- High-upvote Reddit threads = validated content angles (people care about these questions)

**2. Reddit as Competitor Monitoring:**
- If Reddit ranks #1-3 for your target keywords, analyze what angle/question it's answering
- Create content that answers those questions more completely and with more structure

**3. Brand Visibility via Reddit:**
- Participate authentically in relevant subreddits (not promotional)
- Answer questions with genuine expertise — Reddit community upvotes build brand entity signals
- Reddit mentions count toward Google's "Experience" layer for brand authority
- Be cited on Reddit → higher chance of Google surfacing your content when users search related topics

**4. Content Format Insights from Reddit:**
- What Reddit threads go viral in your niche = what multi-format, engaging content you should create
- Reddit's top posts by upvotes = your content calendar for high-demand topics

---

## Multi-Format Content Requirements

> **Data:** Pages with embedded YouTube videos rank for **2x more keywords on page 1** and produce **2.6x longer session durations**. Text-only pages are increasingly uncompetitive for mid-to-high volume queries.

### Format Requirements by Content Type
| Content type | Minimum required formats | Ideal formats |
|-------------|------------------------|--------------|
| Blog article (1,500+ words) | Text + 2-3 images | + Video embed + infographic |
| How-to guide | Text + numbered steps + screenshots | + Video walkthrough |
| Comparison page | Text + comparison table | + Visual feature matrix |
| Product/service page | Text + product images | + Demo video |
| Case study | Text + before/after data | + Charts + testimonial video |
| News/editorial | Text + hero image | + Video if breaking news |

### YouTube Video Integration Strategy
- Embed YouTube video as the **primary content element** (above the fold or early in article) for tutorial/how-to content
- Video transcript in the page body counts as indexable content
- Video boosts session duration → reduces pogo-sticking back to SERP → positive behavioral signal
- Add VideoObject schema with `description`, `thumbnailUrl`, `uploadDate`, `duration`
- Create a dedicated page per video (if high-volume topic) for standalone ranking

### Infographic and Visual Content Rules
- Infographic must have all key data points repeated in **HTML text** (Google can't index text inside images)
- Minimum infographic dimensions: 800px wide (1200px preferred)
- Add `alt` text that summarizes the infographic's main finding
- WebP or SVG format (not PNG for web use unless transparent background needed)

---

## Output

### Content Quality Score: XX/100

### E-E-A-T Breakdown
| Factor | Score | Key Signals |
|--------|-------|-------------|
| Experience | XX/25 | ... |
| Expertise | XX/25 | ... |
| Authoritativeness | XX/25 | ... |
| Trustworthiness | XX/25 | ... |

### AI Citation Readiness: XX/100

### Issues Found
### Recommendations

## Related Skills

### On-Page Content
> - **`seo-page`** — Análisis on-page completo de una sola URL (meta tags, headings, schema, imágenes, performance)
> - **`seo-schema`** — Schema.org estructurado: comunica a Google qué ES el contenido (Article, Person, Organization, Product). Clave para E-E-A-T y rich results
> - **`seo-entity`** — Entidades y Knowledge Graph: autoridad de marca, relaciones semánticas, Wikipedia/Wikidata, topical authority
> - **`seo-images`** — Optimización de imágenes: alt text, formatos, lazy loading, CLS
> - **`seo-image-gen`** — Generación de assets visuales SEO: OG images, hero images, infografías, product photos con IA
> - **`seo-video`** — Contenido en video: YouTube SEO, TikTok, Reels, Google Video Search
> - **`seo-cro`** — UX y conversión: contenido que convierte, A/B testing, bounce rate
> - **`seo-internal-linking`** — Enlazado interno: distribución de link equity, orphan pages, topic clusters
> - **`seo-geo`** — Contenido optimizado para IA: AI Overviews, ChatGPT, Perplexity, passage-level citability ✅ (ya referenciado arriba)

### On-Page Local
> - **`seo-local`** — Schema local (LocalBusiness, PostalAddress, OpeningHours), calidad de location pages, NAP on-page, keywords locales. El componente on-page del SEO local alimenta directamente la calidad de contenido de páginas de ubicación

### Estrategia de Contenido
> - **`seo-keywords`** — Base de todo el contenido on-page: qué escribir, cómo estructurarlo, intención de búsqueda, keyword-to-page mapping, topical authority
> - **`seo-competitive`** — Content gap analysis: identifica qué contenido crear basado en lo que rankea la competencia y qué temas no tienes cubiertos
> - **`seo-programmatic`** — Contenido on-page a escala: templates, páginas dinámicas desde data sources, linking interno automatizado, salvaguardas contra thin content

### Off-Page / Autoridad
> - **`seo-backlinks`** — Perfil de backlinks, auditoría de enlaces tóxicos, gap analysis, guest posting y estrategias de link building
> - **`seo-brand`** — Menciones de marca, reputación, branded SERPs, knowledge panel, entidad de marca

## DataForSEO Integration (Optional)

If DataForSEO MCP tools are available, use `kw_data_google_ads_search_volume` for real keyword volume data, `dataforseo_labs_bulk_keyword_difficulty` for difficulty scores, `dataforseo_labs_search_intent` for intent classification, and `content_analysis_summary` for content quality analysis.

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable (DNS failure, connection refused) | Report the error clearly. Do not guess page content. Suggest the user verify the URL and try again. |
| Content behind paywall (402/403, login wall) | Report that the content is not publicly accessible. Analyze only the visible portion (meta tags, headers) and note the limitation. |
| Thin content (fewer than 100 words retrievable) | Report the findings as-is rather than guessing. Flag the page as potentially JavaScript-rendered or gated, and suggest the user provide the full text directly. |
