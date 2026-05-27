---
name: seo-utm
description: >
  Utility de tracking UTM: naming conventions, builder, integración GA4 y reporting
  de campañas. No es análisis SEO estratégico — se usa puntualmente para crear o
  auditar parámetros UTM. Para reportes con atribución usar seo-reporting. Para
  privacidad y Consent Mode usar seo-privacy. Se invoca desde seo-link-building
  o seo-email cuando se necesitan UTMs para tracking. Use cuando diga "UTM",
  "parámetros UTM", "tracking de campañas", "utm_source", "utm_medium" o
  "medir tráfico de links".
user-invokable: true
argument-hint: "[url] [--source fuente] [--medium medio] [--campaign nombre]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# UTM Parameters — Tracking de Campañas

> **Nota de arquitectura:** Este skill es una utility de tracking — no es análisis SEO estratégico. Se activa puntualmente cuando se necesita construir UTMs o revisar convenciones de naming. Para reportes con datos de atribución usar `seo-reporting`. Para privacidad y consentimiento en tracking usar `seo-privacy`.

Los UTM (Urchin Tracking Module) son parámetros añadidos a una URL para identificar el origen exacto del tráfico en Google Analytics. Esenciales para medir el ROI de link building, email marketing, redes sociales y campañas pagadas.

---

## Anatomía de un UTM

```
https://ejemplo.com/pagina
  ?utm_source=newsletter
  &utm_medium=email
  &utm_campaign=black-friday-2025
  &utm_content=boton-cta
  &utm_term=oferta-limitada
```

### Los 5 parámetros UTM

| Parámetro | Obligatorio | Qué identifica | Ejemplos |
|-----------|------------|---------------|---------|
| `utm_source` | ✅ Sí | De dónde viene el usuario | `google`, `newsletter`, `twitter`, `guest-post` |
| `utm_medium` | ✅ Sí | El tipo de canal | `email`, `social`, `cpc`, `referral`, `link-building` |
| `utm_campaign` | ✅ Sí | El nombre de la campaña | `black-friday-2025`, `lanzamiento-producto` |
| `utm_content` | ❌ Opcional | Diferencia elementos dentro de la campaña | `boton-header`, `link-articulo`, `banner-lateral` |
| `utm_term` | ❌ Opcional | Para Ads: la keyword que activó el anuncio | `seo-tecnico`, `agencia-seo` |

---

## Naming Conventions (CRÍTICO)

Las convenciones de nomenclatura son lo más importante — sin consistencia, GA4 no agrega datos correctamente.

### Reglas obligatorias

| Regla | Correcto | Incorrecto |
|-------|----------|-----------|
| Todo en minúsculas | `utm_source=newsletter` | `utm_source=Newsletter` |
| Guiones en lugar de espacios | `utm_campaign=black-friday` | `utm_campaign=black friday` |
| Sin caracteres especiales | `utm_source=google-ads` | `utm_source=Google Ads!` |
| Sin tildes ni ñ | `utm_campaign=promocion` | `utm_campaign=promoción` |
| Consistencia siempre | `utm_medium=email` en todos los emails | `email`, `Email`, `correo`, `newsletter` mezclados |

> **Por qué:** GA4 es case-sensitive. `Email` y `email` son dos mediums distintos. Dos años de datos inconsistentes = segmentación rota.

### Convención de nomenclatura recomendada

```
utm_source:    [plataforma o sitio específico]
utm_medium:    [tipo de canal estandarizado]
utm_campaign:  [año-mes-nombre-campaña]
utm_content:   [elemento específico]
utm_term:      [keyword o variante] (solo para Ads)
```

---

## Valores Estándar por Canal

### utm_source (fuente)

| Canal | utm_source recomendado |
|-------|----------------------|
| Newsletter propio | `newsletter` |
| Welcome email | `welcome-email` |
| Email transaccional | `email-transaccional` |
| Guest post en [sitio] | `[nombre-del-sitio]` (ej: `hubspot-blog`) |
| Twitter/X | `twitter` |
| LinkedIn | `linkedin` |
| Facebook | `facebook` |
| Instagram | `instagram` |
| YouTube | `youtube` |
| Reddit | `reddit` |
| Quora | `quora` |
| Google Ads | `google` |
| Meta Ads | `facebook` |
| LinkedIn Ads | `linkedin` |
| Firma de email | `email-firma` |
| QR code offline | `qr-code` |
| Podcast (mencionado) | `podcast` |

### utm_medium (medio)

| Tipo de canal | utm_medium estándar | Por qué ese valor |
|--------------|-------------------|------------------|
| Email marketing | `email` | GA4 lo agrupa automáticamente como canal Email |
| Redes sociales (orgánico) | `social` | Canal Social en GA4 |
| Redes sociales (pagado) | `paid-social` | Canal Paid Social en GA4 |
| Google Ads (búsqueda) | `cpc` | Canal Paid Search en GA4 |
| Display / banner | `display` | Canal Display en GA4 |
| Link building / guest posts | `referral` | Canal Referral en GA4 |
| Link building (tracking detallado) | `link-building` | Canal custom — para separar de referral orgánico |
| Firma de email | `email` | Agrupa con email |
| Influencer / partnership | `partnership` | Canal custom |
| QR code | `qr` | Canal custom |
| Sin UTM (orgánico) | — | GA4 lo detecta solo como Organic Search |

> **Nota sobre `referral` vs `link-building`:** Si usas `utm_medium=referral` en tus links de guest posting, GA4 los mostrará junto con todos los referrals orgánicos (sin UTM). Si usas `utm_medium=link-building`, podrás segmentarlos específicamente. Recomendado para link building activo.

---

## UTMs por Canal — Ejemplos Prácticos

### Email Marketing

```
Newsletter semanal, CTA principal:
https://ejemplo.com/blog/articulo
  ?utm_source=newsletter
  &utm_medium=email
  &utm_campaign=2025-04-newsletter-semanal
  &utm_content=cta-principal

Newsletter semanal, link en texto:
  ?utm_source=newsletter
  &utm_medium=email
  &utm_campaign=2025-04-newsletter-semanal
  &utm_content=link-texto-parrafo2
```

### Link Building / Guest Posts

```
Guest post en HubSpot Blog:
https://ejemplo.com/herramienta
  ?utm_source=hubspot-blog
  &utm_medium=link-building
  &utm_campaign=guest-posts-2025-q2
  &utm_content=link-bio OR link-contextual-p3

Broken link replacement:
  ?utm_source=searchengineland
  &utm_medium=link-building
  &utm_campaign=broken-links-2025
```

### Redes Sociales

```
Post en LinkedIn (orgánico):
  ?utm_source=linkedin
  &utm_medium=social
  &utm_campaign=2025-04-contenido-organico
  &utm_content=post-caso-estudio

Story en Instagram:
  ?utm_source=instagram
  &utm_medium=social
  &utm_campaign=2025-04-lanzamiento
  &utm_content=story-swipe-up
```

### Digital PR / Menciones en Medios

```
Artículo en ElConfidencial:
  ?utm_source=elconfidencial
  &utm_medium=digital-pr
  &utm_campaign=estudio-tendencias-2025
```

---

## Error Crítico: UTMs en Links Internos

**NUNCA usar UTMs en links internos del propio sitio.**

```html
<!-- ❌ NUNCA hacer esto en un link interno -->
<a href="/blog/articulo?utm_source=homepage&utm_medium=internal">Leer más</a>

<!-- ✅ Links internos sin UTM -->
<a href="/blog/articulo">Leer más</a>
```

**Por qué es destructivo:**
- Los UTMs sobreescriben la sesión de GA4
- Un usuario que llegó desde Google (Organic Search) y hace clic en un link interno con UTM → GA4 crea una nueva sesión con la nueva fuente
- Resultado: el tráfico orgánico se atribuye incorrectamente al UTM interno
- Los datos de adquisición quedan completamente distorsionados

**Regla:** UTMs solo en links EXTERNOS que apuntan a tu sitio.

---

## Otros Errores Comunes

| Error | Consecuencia | Fix |
|-------|-------------|-----|
| UTMs en links internos | Sesiones cortadas, atribución rota | Eliminar UTMs de links internos |
| Mayúsculas mezcladas | `Email` ≠ `email` → canales duplicados en GA4 | Estandarizar todo en minúsculas |
| Espacios en los valores | URL rota o codificada como `%20` | Usar guiones en lugar de espacios |
| Sin utm_source o utm_medium | El parámetro se ignora o no funciona | Siempre incluir los dos obligatorios |
| UTM en la URL canónica | Google puede ver URLs con UTM como duplicados | Verificar que canonical apunta a URL sin UTM |
| Compartir URLs con UTM internamente | El equipo interno contamina los datos | Usar extensión de Chrome que oculte UTMs al navegar internamente |
| UTMs en URLs de landing page de pago | Google Ads puede usar auto-tagging que conflicta | Usar `gclid` de GA4 + UTM juntos es seguro; verificar en GA4 → Admin → Data Streams |

---

## Canonical y UTMs

Los parámetros UTM no afectan el SEO si el canonical está bien configurado, pero hay que verificarlo.

```bash
# Verificar que la página con UTM tiene canonical a la URL limpia
curl -sL "https://ejemplo.com/pagina?utm_source=newsletter&utm_medium=email" | grep -i canonical

# Resultado correcto:
# <link rel="canonical" href="https://ejemplo.com/pagina" />
# ✅ Google ignora el UTM y consolida el PageRank en la URL limpia
```

Si no hay canonical → Google puede indexar la URL con UTM como página separada → duplicado.

---

## UTM Builder

### Construcción manual

```bash
# Base URL
BASE="https://ejemplo.com/pagina-de-destino"

# Parámetros
SOURCE="newsletter"
MEDIUM="email"
CAMPAIGN="2025-04-lanzamiento"
CONTENT="boton-cta"

# URL completa
echo "${BASE}?utm_source=${SOURCE}&utm_medium=${MEDIUM}&utm_campaign=${CAMPAIGN}&utm_content=${CONTENT}"
```

### Herramientas de builder

| Herramienta | URL | Gratis |
|-------------|-----|--------|
| Google Campaign URL Builder | ga-dev-tools.google/campaign-url-builder | ✅ |
| Rebrandly (con UTM + acortador) | rebrandly.com | ✅ básico |
| Bitly (UTM en links acortados) | bitly.com | ✅ básico |
| UTM.io (gestión en equipo) | utm.io | ✅ básico |

---

## GA4 — Dónde Ver los Datos UTM

### Reportes de Adquisición

```
GA4 → Reports → Acquisition → Traffic Acquisition

Dimensiones disponibles:
- Session source / medium
- Session campaign
- Session source / medium / campaign

First user source / medium → para atribución de adquisición de usuario
```

### Crear Segmento de Link Building

```
GA4 → Explore → Free Form
Dimension: Session medium
Filter: session medium = "link-building"
Metrics: Sessions, Users, Engaged Sessions, Conversions
```

### Dashboard de UTM para Reporting

Métricas clave por campaña UTM:

| Métrica | Dónde en GA4 | Para qué sirve |
|---------|-------------|---------------|
| Sessions por source/medium | Acquisition → Traffic | Volumen por canal |
| Engaged sessions | Acquisition → Traffic | Calidad del tráfico (> 2 páginas o > 10s) |
| Engagement rate | Acquisition → Traffic | % de sessions de calidad |
| Conversions | Conversions → Overview | ROI por canal |
| Revenue (ecom) | Monetization | Revenue por canal |
| New users | Acquisition → User | Nuevas audiencias por canal |

---

## UTMs para Link Building — Workflow Integrado

Combinando `seo-link-building` con `seo-utm`:

### Paso 1: Crear URL con UTM antes del outreach
```
Por cada guest post / digital PR / link placement:
→ Crear URL única con utm_source=[nombre del sitio]
→ utm_medium=link-building (o digital-pr, o guest-post)
→ utm_campaign=[nombre de la campaña de link building]
→ utm_content=[tipo de link: contextual, bio, resource]
```

### Paso 2: Registrar en hoja de seguimiento
```
Columnas adicionales en la hoja de link building:
→ UTM completo
→ URL con UTM (la que se da al sitio linking)
→ Tráfico enviado (GA4, primeros 30 días)
→ Conversiones desde ese link
```

### Paso 3: Evaluar calidad del tráfico por link

En GA4, comparar engagement rate por link:
- Engagement rate > 60% → tráfico de calidad, audiencia relevante
- Engagement rate < 30% → tráfico irrelevante o bot → deprioritizar ese sitio

```
GA4 → Explore → Free Form
Segment: utm_medium = link-building
Group by: utm_source
Metrics: Sessions, Engaged Sessions, Engagement Rate, Conversions
→ Ordenar por Conversions para ver qué links tienen ROI real
```

---

## Consent Mode y UTM Data Loss (2024-2026)

**El problema:** Consent Mode v2 (obligatorio en EU desde marzo 2024) hace que GA4 no registre sesiones de usuarios que rechazan las cookies. Esto afecta directamente a los datos UTM.

### Impacto real del data loss por Consent Mode

| Región | % sesiones no consentidas típico | Impacto en datos UTM |
|--------|--------------------------------|---------------------|
| EU (GDPR) | **40-60%** de sesiones perdidas | UTMs de esas sesiones no aparecen en GA4 |
| UK | 30-50% | Igual que EU |
| USA (CCPA) | 10-20% | Menor, pero Consent Mode recomendado |
| LATAM | 5-15% | Bajo si no se implementa gestión de consentimiento |

> **Para clientes SaaS USA con tráfico EU:** Si el 30% del tráfico es EU y el 50% rechaza cookies, estás perdiendo ~15% de tus datos de atribución UTM globalmente.

### Consent Mode v2 + GA4 — Cómo funciona

```
Usuario rechaza cookies en EU
→ GA4 NO registra la sesión con datos reales
→ GA4 SÍ usa "behavioral modeling" para ESTIMAR esas sesiones
→ Los datos modelados aparecen en GA4 con flag "modeled" (si está activado)

Para activar behavioral modeling:
GA4 → Admin → Data collection → Reporting Identity
→ Activar "Blended" (combina datos reales + modelados)
→ Requiere mínimo ~1,000 conversiones mensuales para que el modelo sea preciso
```

### UTMs que sobreviven vs. los que no (Consent Mode)

```
✅ UTMs registrados siempre (first-party, no cookie-dependent):
   → utm_source/medium/campaign en URL → GA4 los captura si hay sesión consentida
   → Server-side tagging → captura UTMs sin depender de cookies de browser

❌ UTMs perdidos (sesiones no consentidas sin behavioral modeling):
   → Todo el tráfico de usuarios que rechazaron cookies
   → GA4 no puede atribuir esas sesiones a ningún UTM
```

### Soluciones al data loss de UTMs

**Opción 1: Activar behavioral modeling en GA4** (más fácil)
```
GA4 → Admin → Reporting → Reporting identity → Blended
Requiere: Consent Mode v2 correctamente implementado + suficientes conversiones
```

**Opción 2: Server-side tagging** (más robusto — ver sección siguiente)

**Opción 3: First-party data collection**
```
Guardar utm_source, utm_medium, utm_campaign en DB propia al hacer login/signup
→ Datos 100% tuyos, no dependen de cookies ni de GA4
→ Esencial para SaaS con signup/login
```

**Cómo reportarlo al cliente:**
```
"Los datos UTM de GA4 representan el X% del tráfico real. El Y% 
restante son usuarios en EU que no aceptaron cookies. GA4 estima 
su comportamiento mediante modeling — los datos son orientativos, 
no exactos para ese segmento."
```

---

## Server-Side Tagging para UTMs

El server-side tagging mueve el tracking de GA4 del navegador del usuario a un servidor intermediario. Soluciona el data loss de Consent Mode y los bloqueadores de anuncios.

### Por qué importa para UTMs

```
Client-side (normal):
URL con UTM → Browser → GA4 tag en browser → GA4
Problema: adblocks bloquean, Consent Mode limita

Server-side:
URL con UTM → Tu servidor (Google Tag Manager Server) → GA4
Ventaja: el adblocker no ve el request, Consent Mode puede configurarse server-side
```

### Implementación básica (GTM Server-side)

```
1. Crear Container server-side en tagmanager.google.com
2. Desplegar Cloud Run container (Google Cloud, ~$10-50/mes)
3. Mover GA4 tag de client-side a server-side en GTM
4. Configurar custom domain (analytics.tudominio.com)
   → Aparece como first-party, no bloqueado por Safari ITP ni Firefox ETP
5. Los UTMs llegan al servidor antes de que el browser los bloquee
```

**Cuándo recomendar server-side tagging:**
```
✅ Cliente con > 30% de tráfico EU (Consent Mode data loss significativo)
✅ Cliente con audiencia técnica (developers, marketers) → alto uso de adblock
✅ SaaS B2B donde la atribución de cada lead vale mucho
✅ E-commerce con revenue attribution crítico
❌ Blog pequeño sin conversiones → no vale la complejidad
```

---

## UTMs para Canales Emergentes (2025-2026)

Los patrones de UTM originales no cubren AI search, Threads ni WhatsApp — canales con tráfico creciente.

### AI Search (ChatGPT / Perplexity / Gemini)

**Problema:** El tráfico de usuarios que hacen clic en links dentro de ChatGPT, Perplexity, etc. llega como "Direct" o "Referral" en GA4. No hay UTMs automáticos.

**Cómo trackear:**
```
Si incluyes links en respuestas AI (ej: en tu llms.txt, en training data):
→ Añadir UTMs a las URLs mencionadas en llms.txt:
   utm_source=chatgpt  |  utm_source=perplexity  |  utm_source=gemini
   utm_medium=ai-search
   utm_campaign=ai-visibility-2025

Si el tráfico llega como referral desde perplexity.ai:
→ GA4 ya lo captura como referral source = perplexity.ai
→ Configurar channel group custom en GA4 para agrupar todos los AI referrals
```

**Custom channel group para AI search en GA4:**
```
GA4 → Admin → Channel groups → Create custom channel group
Nombre: "AI Search"
Regla: Session source matches regex:
  (chat\.openai\.com|perplexity\.ai|gemini\.google\.com|bing\.com/chat|claude\.ai)
```

### Threads (Meta)

```
utm_source=threads
utm_medium=social
utm_campaign=[nombre-campaña]
utm_content=[tipo-post: texto|imagen|reel|link]

Nota: Threads no tiene UTMs automáticos — añadirlos manualmente en todos los links.
```

### WhatsApp (links compartidos)

**Caso frecuente:** links compartidos en grupos de WhatsApp llegan como "Direct" — no hay forma de saber que vienen de WhatsApp.

```
Estrategia: crear URLs con UTM para contenido que se va a compartir en WhatsApp

utm_source=whatsapp
utm_medium=social
utm_campaign=[campaña o contexto]
utm_content=[tipo: grupo-clientes|broadcast|historia]

Ejemplo: newsletter que se comparte en WA:
?utm_source=whatsapp&utm_medium=social&utm_campaign=2025-04-oferta&utm_content=grupo-vip
```

> **Para LATAM:** WhatsApp es el canal de referencia. Muchos clientes envían tráfico sin saberlo desde WhatsApp que aparece como Direct. Crear UTMs específicos para todo contenido compartible.

### Comparativa de canales emergentes

| Canal | utm_source | utm_medium | Notas |
|-------|-----------|-----------|-------|
| ChatGPT links | `chatgpt` | `ai-search` | Manual, en llms.txt |
| Perplexity | `perplexity` | `ai-search` | Manual |
| Gemini | `gemini` | `ai-search` | Manual |
| Threads | `threads` | `social` | Manual en todos los posts |
| WhatsApp broadcast | `whatsapp` | `social` | Manual en links compartidos |
| WhatsApp Status | `whatsapp-status` | `social` | Para links en estados |
| Podcast (show notes) | `[nombre-podcast]` | `podcast` | En description links |
| Newsletter de terceros | `[nombre-newsletter]` | `newsletter-referral` | Para links en otros newsletters |

---

## UTM Governance — Gestión en Equipo

Sin governance, múltiples personas generan UTMs diferentes para el mismo canal. El resultado es fragmentación de datos en GA4.

### Documento de governance (mínimo viable)

```markdown
# UTM Naming Guide — [Cliente/Empresa]

## Valores aprobados para utm_source:
newsletter | welcome-email | linkedin | twitter | instagram | facebook | 
threads | whatsapp | youtube | reddit | google | chatgpt | perplexity | [nombre-publicacion]

## Valores aprobados para utm_medium:
email | social | paid-social | cpc | display | link-building | digital-pr | 
guest-post | referral | ai-search | podcast | qr | partnership

## Formato utm_campaign:
[año]-[mes]-[nombre] (ej: 2025-04-lanzamiento-producto)
Siempre minúsculas, guiones, sin tildes.

## Quién aprueba nuevos valores: [nombre responsable]
## Dónde están los UTMs activos: [link a hoja de tracking]
```

### UTM Spreadsheet de equipo (columnas mínimas)

| Fecha | Campaña | URL destino | utm_source | utm_medium | utm_campaign | utm_content | URL con UTM | Responsable | Estado |
|-------|---------|------------|-----------|-----------|-------------|------------|------------|------------|-------|
| 2025-04-01 | Newsletter abril | /blog/post | newsletter | email | 2025-04-newsletter | cta-principal | [URL] | Lau | ✅ activo |

### Checklist antes de publicar un UTM nuevo

```
✅ Está en minúsculas
✅ Sin espacios (usar guiones)
✅ Sin caracteres especiales ni tildes
✅ utm_source y utm_medium incluidos
✅ El canonical de la landing page apunta a URL sin UTM
✅ No es un link interno del sitio
✅ El UTM está registrado en la hoja de tracking del equipo
✅ Se ha testeado en GA4 DebugView antes de lanzar
```

---

## Output Format

### UTM Audit Report

**Dominio:** [dominio]
**Período analizado:** [fechas]

#### Estado del Tracking

| Canal | UTMs implementados | Naming convention | Datos en GA4 |
|-------|------------------|------------------|-------------|
| Email | ✅/❌ | ✅ consistente / ⚠️ mezclado | ✅/❌ |
| Social | ✅/❌ | ✅/⚠️ | ✅/❌ |
| Link building | ✅/❌ | ✅/⚠️ | ✅/❌ |
| Ads | ✅/❌ | ✅/⚠️ | ✅/❌ |

#### Problemas Detectados

| Problema | Impacto | Fix |
|----------|---------|-----|
| UTMs en links internos | 🔴 Sesiones contaminadas | Eliminar UTMs de todos los links internos |
| Mayúsculas inconsistentes | 🟡 Datos fragmentados en GA4 | Estandarizar en minúsculas |

#### UTMs Generados

Para cada URL solicitada:

| Destino | UTM completo | URL final |
|---------|-------------|----------|
| [página] | source=X medium=Y campaign=Z | [URL completa] |

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Estrategia de link building | `/seo link-building [dominio]` | Decidir qué links construir y con qué anchors |
| Email marketing y UTMs | `/seo email [dominio]` | UTMs específicos para secuencias de email |
| Redes sociales y UTMs | `/seo smo [dominio]` | UTMs para posts sociales orgánicos y paid |
| Reportes con datos UTM | `/seo reporting [dominio]` | Dashboard con desglose por canal UTM |
| Datos de tráfico real por canal | `/seo google analytics [dominio]` | GA4 API para automatizar reporte de UTMs |
