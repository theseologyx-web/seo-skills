---
name: seo-smo
description: >
  Social Media Optimization (SMO) audit completo: ranking en buscadores internos de cada
  plataforma (TikTok, YouTube, Instagram, LinkedIn, Facebook, Reddit, Quora), estrategia de
  links social→sitio, contenido keyword-optimizado, optimización de perfiles, social commerce,
  social listening, UGC, Open Graph on-site, review platforms (G2, Capterra) y SMO→SEO/AI.
  Use cuando el usuario diga "SMO", "social SEO", "audit social", "aparecer en TikTok search",
  "YouTube SEO", "Instagram search", "llevar tráfico desde redes", "keywords en redes sociales",
  "estrategia de links en social", "optimizar perfil social", "social media optimization",
  "Reddit SEO", "Quora SEO", "review platforms", "G2 Capterra", "social commerce", "UGC".
user-invokable: true
argument-hint: "[URL del sitio, handle o nombre de plataforma]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# SMO — Social Media Optimization Completo

Cuatro objetivos integrados:
1. **Rankear en buscadores internos** de cada plataforma (YouTube, TikTok, Instagram, LinkedIn, Facebook, Reddit, Quora)
2. **Llevar tráfico cualificado al sitio web** con estrategia de links desde social
3. **Optimizar presencia y perfiles** para máxima visibilidad y engagement
4. **Ser citado en AI Overviews y LLMs** a través de reviews, UGC y menciones de marca

## Framework SMO 2025 — 4 Fases

| Fase | Objetivo | Canales principales |
|------|---------|-------------------|
| **1. Be Found** | Aparecer en búsquedas (plataforma + Google) | YouTube, TikTok, Reddit, Quora, Pinterest |
| **2. Be Cited** | Ser mencionado en AI Overviews y LLMs | Reviews (G2/Capterra), Reddit, Quora, Medium |
| **3. Be Scaled** | Amplificar con viral loops y social commerce | TikTok Shop, Instagram Shopping, UGC |
| **4. Be Chosen** | Convertir con prueba social y comunidad | Discord, reviews verificadas, testimonios |

---

## 0. API Connectivity — Datos automáticos disponibles

| Plataforma | API | Datos accesibles | Setup |
|-----------|-----|-----------------|-------|
| **YouTube** | ✅ YouTube Data API v3 | Suscriptores, vistas, vídeos, tags, engagement | API key en google-api.json |
| **TikTok** | ❌ Research API = académico | Solo manual + TikTok Creative Center | — |
| **Instagram** | ⚠️ Instagram Graph API | Posts, reach, followers (requiere Meta Business App) | Facebook App + review |
| **LinkedIn** | ❌ Company API = partners | Solo manual + LinkedIn Analytics nativo | — |
| **Facebook** | ⚠️ Graph API | Page likes, posts, reach, reviews | Facebook App + Page Token |
| **Pinterest** | ⚠️ Pinterest API | Pins, tableros, métricas básicas | Pinterest Developer App |

```bash
# Auditar canal YouTube con datos reales:
python ~/.claude/skills/seo/scripts/youtube_search.py channel UC[CHANNEL_ID]
python ~/.claude/skills/seo/scripts/youtube_search.py search "[marca]" --order viewCount --limit 20
python ~/.claude/skills/seo/scripts/youtube_search.py video [VIDEO_ID]

# Setup YouTube Data API v3:
# console.cloud.google.com → APIs & Services → Library → "YouTube Data API v3" → Enable
# Añadir API key en ~/.config/claude-seo/google-api.json → campo "api_key"
```

---

## 1. Keyword Research para Social Search

Cada plataforma tiene su propio buscador con intención y vocabulario diferente a Google. Las keywords no son las mismas.

### Cómo encontrar keywords por plataforma

| Plataforma | Fuente principal | Fuente secundaria |
|-----------|-----------------|------------------|
| **YouTube** | YouTube Autocomplete (barra de búsqueda) | YouTube Studio → Analytics → Search terms |
| **TikTok** | TikTok Autocomplete | TikTok Creative Center → Keyword Insights (trends.tiktok.com) |
| **Instagram** | Instagram Autocomplete + Explorar → Tags | Buscar hashtags y ver volumen |
| **LinkedIn** | LinkedIn Search Autocomplete | LinkedIn Pages → Analytics → Visitors → Organic search terms |
| **Facebook** | Facebook Search | Groups — ver nombres de grupos populares del nicho |
| **Pinterest** | Pinterest Autocomplete + guías de categoría | Pinterest Trends (trends.pinterest.com) |

### Tipos de queries por red (patrones de intención)

**YouTube** → informacional larga, comparación, how-to
```
"[software] tutorial"   "how to manage field inspections"   "[marca] vs [competidor]"   "best inspection app 2025"
```

**TikTok** → corta, coloquial, entretenimiento + aprendizaje rápido
```
"inspection app"   "field inspection tips"   "inspection software demo"
```

**Instagram** → hashtags como categorías visuales, nombres de cuenta
```
#inspectionsoftware   #fieldoperations   #digitaltransformation   #compliancemanagement
```

**LinkedIn** → profesional, cargo, sector, problema de negocio
```
"inspection management software"   "field operations technology"   "compliance audit software"
```

**Facebook** → páginas, grupos, posts de comunidad
```
"inspection professionals" (grupo)   "field service software" (página)
```

**Pinterest** → descriptivo visual, DIY, proceso, checklist
```
"inspection checklist template"   "field inspection workflow"
```

---

## 2. Social Search Optimization — Dónde van las keywords

### 2.1 YouTube

| Elemento | Importancia | Cómo |
|---------|------------|------|
| Título (primeros 60 chars) | 🔴 Crítica | Keyword exacta al inicio |
| Descripción (primeras 2-3 líneas) | 🔴 Crítica | Keyword + resumen + CTA con link |
| Tags | 🟡 Alta | Exacta + variantes + relacionadas (500 chars totales) |
| Capítulos/timestamps | 🟡 Alta | Nombrar cada sección con sub-keyword |
| Audio hablado | 🔴 Crítica | Decir la keyword — YouTube indexa captions automáticas |
| Nombre del archivo | 🟢 Media | `keyword-video.mp4` antes de subir |
| Playlists | 🟡 Alta | Título con keyword del cluster temático |

### 2.2 TikTok

| Elemento | Importancia | Cómo |
|---------|------------|------|
| Primera línea del caption | 🔴 Crítica | Keyword exacta antes del "ver más" |
| Texto en pantalla (overlay) | 🔴 Crítica | TikTok hace OCR — indexa el texto del vídeo |
| Audio hablado | 🔴 Crítica | Auto-captions se indexan — decir la keyword en primeros 5s |
| Nombre display de cuenta | 🟡 Alta | Incluir keyword de nicho (ej: "VLX | Inspection Software") |
| Hashtags | 🟡 Alta | 3-5: 1 broad + 2 niche + 1 trending si aplica |
| Bio | 🟡 Alta | Keyword principal en primeras palabras |

### 2.3 Instagram

| Elemento | Importancia | Cómo |
|---------|------------|------|
| Nombre display (campo Name) | 🔴 Crítica | Campo más indexado por búsqueda interna — incluir keyword |
| Bio | 🔴 Crítica | Keyword de nicho en primeras palabras (150 chars) |
| Alt text de imágenes | 🔴 Crítica | Configurar manualmente en Advanced Settings de cada post |
| Primera línea del caption | 🟡 Alta | Keyword antes de "ver más" |
| Hashtags | 🟡 Alta | 5-10 relevantes, mix niche + broad |
| Location tags | 🟢 Media | Para visibilidad en búsquedas locales |

**Ejemplo optimizado:**
- ❌ Nombre: "Visualogyx" (solo marca)
- ✅ Nombre: "Visualogyx | Inspection Software" (marca + keyword)

### 2.4 LinkedIn

| Elemento | Importancia | Cómo |
|---------|------------|------|
| Tagline de empresa | 🔴 Crítica | Keyword + propuesta de valor (120 chars) |
| About — primeros 156 chars | 🔴 Crítica | Snippet visible en búsqueda — keyword primaria aquí |
| Especialidades | 🔴 Crítica | Lista de keywords de producto/servicio/sector |
| Posts — primeras 2 líneas | 🟡 Alta | Keyword + hook antes de "ver más" |
| Hashtags de página | 🟡 Alta | 3 hashtags en Settings de la Company Page |

### 2.5 Facebook

| Elemento | Importancia | Cómo |
|---------|------------|------|
| Nombre de página | 🔴 Crítica | Nombre exacto de empresa (+ keyword si aplica) |
| Categoría | 🔴 Crítica | Más específica posible (ej: "Software Company") |
| Descripción corta | 🔴 Crítica | Keyword en primeras palabras — aparece en resultados FB |
| About completo | 🟡 Alta | Keywords naturales en descripción larga |
| Posts públicos | 🟡 Alta | Keywords en primeras líneas |

---

## 3. Estrategia de Links: Social → Sitio Web

Cada plataforma tiene reglas distintas. Ignorarlas = alcance suprimido.

### 3.1 Reglas de links por plataforma

| Plataforma | ¿Links en posts? | Solución | Penalización si se ignora |
|-----------|-----------------|---------|--------------------------|
| **Instagram** | ❌ No clickeable en posts | Link en bio / Link Sticker en Stories | No hay clics |
| **TikTok** | ❌ Solo cuentas Business | Link en bio (cuenta Business) | No hay clics |
| **LinkedIn** | ⚠️ Sí, pero penaliza alcance | Link en primer comentario (no en el post) | Hasta -60% alcance |
| **Facebook** | ⚠️ Sí, pero penaliza alcance | Post con imagen + link en primer comentario | Alcance reducido |
| **YouTube** | ✅ Sin penalización | Descripción + cards + pantalla final | — |
| **X/Twitter** | ✅ Leve penalización | Tweet con link (menor alcance que sin link) | Leve reducción |
| **Pinterest** | ✅ Sin penalización — es su función | Cada pin siempre lleva link | — |

### 3.2 Tácticas por plataforma

**Instagram:**
```
Bio → link a página propia de links (/links/) — mejor que Linktree, el tráfico queda en tu dominio
Stories → Link Sticker directo a URL específica (landing, blog, producto)
Actualizar bio link cuando se publica contenido nuevo relevante
Post: CTA verbal "link en bio" + referencia específica al recurso
```

**TikTok:**
```
Vídeo: CTA verbal en los primeros 5-10s: "Link en bio para [recurso específico]"
Bio: una URL (cuenta Business requerida) — usar página de links si hay múltiples destinos
Los usuarios no leen descripciones — el CTA debe ser hablado y/o en pantalla
```

**LinkedIn:**
```
1. Publicar post sin link (mayor alcance orgánico)
2. Inmediatamente comentar con: "Link completo aquí: [URL]"
3. Artículos LinkedIn Pulse: links internos sin penalización
4. Documentos PDF: URLs visibles (aunque no clickeables) + instrucción de buscarla
```

**Facebook:**
```
1. Post con imagen on-brand (sin URL en el cuerpo del post)
2. Primer comentario: link completo con UTM
3. Stories: swipe-up disponible en Pages (sin mínimo de seguidores)
4. Grupos propios: links funcionan mejor que en feed de Page
```

**YouTube:**
```
Descripción:
  Línea 1: resumen con keyword + CTA + link al sitio
  Línea 2-3: timestamps (capítulos)
  Resto: links a recursos mencionados en el vídeo (landing, blog, herramienta)
Cards (i): dirigir a landing o post del blog relacionado
Pantalla final: suscribe + video relacionado + link externo al sitio
```

**Pinterest:**
```
Cada pin linkea a una URL del sitio — SIEMPRE
Optimizar la URL de destino para que coincida con la keyword del pin
Tableros temáticos = keyword clusters visuales que rankean en Google Images
```

### 3.3 UTM tracking — medir tráfico social en GA4

Todos los links de social al sitio deben llevar UTMs:

```
https://dominio.com/blog/inspection-software-guide
  ?utm_source=instagram
  &utm_medium=social
  &utm_campaign=inspection-guide-abril
  &utm_content=link-bio

# utm_source por plataforma:
youtube | tiktok | instagram | linkedin | facebook | pinterest | twitter

# utm_content para distinguir tipo de link:
link-bio | story | comentario | descripcion-video | pin | tweet
```

GA4: Informes → Adquisición → Adquisición de tráfico → Session source/medium = instagram/social, tiktok/social, etc.

---

## 4. Contenido Keyword-Optimizado para Social

### 4.1 Qué contenidos de social rankean también en Google

| Plataforma | Rankea en Google | Qué tipo |
|-----------|-----------------|---------|
| **YouTube** | ✅ Fuertemente | How-to, reviews, tutoriales, comparaciones |
| **LinkedIn** | ✅ Sí (artículos + posts públicos) | Artículos con keyword en título |
| **Pinterest** | ✅ Sí (imágenes + tableros) | Pins con descripciones keyword-ricas |
| **TikTok** | ⚠️ Creciendo en 2025-2026 | Brand queries y términos coloquiales |
| **Instagram** | ❌ Muy limitado | Solo perfiles de marca para brand queries |
| **Facebook** | ❌ Muy limitado | Solo Pages públicas para brand queries |

### 4.2 Estructura de contenido por plataforma

**YouTube — título y descripción:**
```
TÍTULO: [Keyword primaria]: [Beneficio concreto] | [Marca]  ← primeros 60 chars
─────
DESCRIPCIÓN:
[Keyword] + resumen de valor + CTA + link al sitio       ← antes de "ver más"
[Link a landing o blog relacionado]

00:00 Intro
01:30 [Sub-keyword: sección 1]
04:00 [Sub-keyword: sección 2]

🔗 Recursos mencionados:
[Link 1 con UTM]
[Link 2 con UTM]

#hashtag1 #hashtag2 #hashtag3
```

**TikTok / Instagram Reels — caption:**
```
[Keyword] + hook — antes del "ver más" (primera línea = title tag)
Desarrollo en 2-4 líneas
CTA: "Link en bio para [recurso]" o "Comenta [X] para recibir [Y]"
─────
#keyword1exacta #keywordnicho #hashtagbroad
```

**LinkedIn — post sin link:**
```
[Hook con keyword] — antes de "ver más"            ← línea 1
[Problema que resuelve]                             ← líneas 2-3
[Solución o insight]                                ← líneas 4-6
[CTA] → "Link completo en comentarios 👇"           ← línea final
```

**Facebook — post con imagen:**
```
[Hook con keyword]
[Desarrollo breve — 2-3 líneas]
"Link en comentarios 👇"
─────
(Imagen on-brand, sin texto excesivo)
[Primer comentario: URL completa con UTM]
```

**Pinterest — descripción de pin:**
```
[Keyword primaria exacta] al inicio
Qué contiene el recurso al que linkea
A quién le sirve
CTA: "Visita el link para [acción concreta]"
#hashtag1 #hashtag2 #hashtag3
─────
URL de destino: página del sitio con misma keyword del pin
```

### 4.3 Keyword mapping social ↔ sitio web

El contenido social es top of funnel → el link lleva al usuario a la landing de conversión:

| Keyword en social | Plataforma | URL de destino en sitio |
|------------------|-----------|------------------------|
| "inspection software" | YouTube, LinkedIn | /digital-inspections-software/ |
| "field inspection tips" | TikTok, Instagram | /blog/field-inspection-tips/ |
| "[sector] inspection" | YouTube, LinkedIn | /digital-inspections-software/[sector]/ |
| "inspection checklist" | Pinterest, TikTok | /checklists/ |

---

## 5. Optimización de Perfiles — Audit Completo

### 5.1 YouTube Channel

- [ ] **Nombre del canal**: marca exacta + descriptor si aplica ("VLX – Inspection Software")
- [ ] **Descripción del canal**: keyword en primeras 100 chars, propuesta de valor, link al sitio
- [ ] **Links configurados**: sitio web + otras redes sociales en About
- [ ] **Banner y foto**: on-brand, profesionales
- [ ] **Featured video para no suscriptores**: el más representativo del canal
- [ ] **Playlists temáticas**: por cluster de keyword, con descripciones optimizadas
- [ ] **Trailer de canal**: CTA claro + keyword de posicionamiento
- [ ] **Handle**: @handle fácil de recordar, consistente con otras redes

### 5.2 TikTok

- [ ] **Nombre display**: marca + keyword de nicho
- [ ] **Bio**: keyword + propuesta de valor + CTA en 80 chars visibles
- [ ] **Link en bio**: activo, con UTM, apunta a URL relevante (requiere cuenta Business)
- [ ] **Cuenta Business**: activada (sin esto no hay analytics ni link clickeable)
- [ ] **Foto de perfil**: logo limpio, legible a 50px

### 5.3 Instagram

- [ ] **Nombre display** (campo Name): [Marca] | [Keyword de nicho]
- [ ] **Bio**: keyword + propuesta de valor + CTA (150 chars)
- [ ] **Link en bio**: página de links propia o URL directa con UTM
- [ ] **Highlights**: organizados con covers branded (qué es, casos de uso, testimonios)
- [ ] **Cuenta Business/Creator**: activada para analytics
- [ ] **Email/teléfono**: visible (señal E-E-A-T)
- [ ] **Alt text**: configurado manualmente en cada post (Advanced Settings)

### 5.4 LinkedIn Company Page

- [ ] **Tagline**: keyword + propuesta de valor (120 chars)
- [ ] **About**: keyword en primeras 156 chars (snippet visible en búsqueda)
- [ ] **Especialidades**: todas las keywords de producto/sector listadas
- [ ] **Website URL**: correcto y activo
- [ ] **Hashtags de página**: 3 configurados en Settings
- [ ] **Logo** (300×300px) y **banner** (1128×191px): profesionales
- [ ] **Featured post o artículo**: pinneado, con link al recurso más importante

### 5.5 Facebook Page

- [ ] **Categoría**: más específica posible
- [ ] **Descripción corta**: keyword en primeras palabras (aparece en resultados de búsqueda FB)
- [ ] **About completo**: keywords naturales, propuesta de valor
- [ ] **Website URL**: correcto
- [ ] **Email/teléfono/dirección**: completados
- [ ] **CTA Button**: configurado (Más información / Registrarse / Contactar)
- [ ] **Cover image/video**: 820×312px, on-brand, CTA visible
- [ ] **Post fijado (pinned)**: contenido más importante o última oferta

### 5.6 Consistencia cross-platform

| Elemento | Verificar |
|---------|----------|
| **Handle/username** | Igual en todas las plataformas |
| **Logo** | Misma versión en todas las redes |
| **Propuesta de valor** | Misma (adaptada al tono de cada red) |
| **Website URL** | Apunta al mismo sitio (UTMs diferenciados por red) |
| **Paleta de colores** | Coherente en covers, thumbnails, posts |
| **NAP** (local) | Nombre/Dirección/Tel idéntico en Facebook, GMB y sitio |

---

## 6. Engagement y Señales de Algoritmo

### Por plataforma — factores de distribución

**YouTube:**
- Watch time + % completado → ranking en search y sugeridos
- CTR desde resultados (thumbnail + título) → impresiones a clics
- Engagement primeras horas (likes, comentarios, saves) → boost inicial

**TikTok:**
- Completion rate (% que termina el vídeo) → distribución a más FYP
- Rewatches → señal de contenido de alta calidad
- Shares > Likes en importancia para el algoritmo
- Comentarios → engagement signal + oportunidad de keyword en respuesta

**Instagram:**
- Saves → KPI #1 (contenido de valor que la gente quiere volver a ver)
- Shares (DMs + Stories) → KPI #2 para viralidad
- Replies a Stories → señal de engagement de alta calidad
- Respuestas a comentarios en primeras 2 horas → boost de distribución

**LinkedIn:**
- Dwell time en el post (leer, no solo pasar)
- Comentarios (especialmente de conexiones de primer grado)
- Compartir con comentario propio > compartir directo
- Reacciones "Muy útil" e "Interesante" > "Me gusta"

**Facebook:**
- Comments > Likes en peso algorítmico
- Shares con comentario > shares directos
- Video retention (especialmente primeros 3 segundos)
- Respuestas del autor a comentarios (primeras 2 horas)

---

## 7. SMO → SEO Connections

### Plataformas que rankean en Google (datos 2025)

| Plataforma | % queries donde aparece | Tipo de contenido |
|-----------|------------------------|------------------|
| **Reddit** | 2º sitio más visible en Google (tras Wikipedia) | Discusiones, comparaciones, reviews, problemas — 1,900% crecimiento visibilidad, 603% tráfico orgánico |
| **YouTube** | 19.8% | How-to, tutoriales, reviews, demos |
| **Quora** | 8% | Respuestas a preguntas específicas |
| **LinkedIn** | 5% | Artículos, posts públicos, páginas de empresa |
| **Pinterest** | Alta en imágenes | Pins con descripciones keyword-ricas |
| **Instagram** | ⚠️ Creciendo | Posts indexados por Google desde 2026 — aparecen en SERPs, AI Overviews y voice search |
| **TikTok** | Presente en video carousels | Vídeos de TikTok aparecen regularmente en Google video carousels — doble visibilidad |
| **Medium** | Frecuente en AI Overviews | Artículos de autoridad |
| **G2/Capterra** | 17-26% en AI responses | Reviews de software — AI Overviews |

### Señales sociales → impacto SEO/AI

| Señal | Impacto |
|-------|---------|
| Perfiles sociales de marca | Rankean para brand queries — controlan la primera página |
| `sameAs` schema en el sitio | Conecta la entidad del sitio con perfiles en Knowledge Graph |
| Reviews en G2/Capterra/Google | 28% de citaciones en AI Overviews — alimentan LLMs |
| Reddit threads sobre la marca | Rankean en Google + son fuente para AI Overviews |
| Quora respuestas de expertos | Rankean en Google + E-E-A-T para el autor/empresa |
| YouTube embeds en el sitio | Aumentan dwell time + relación de autoridad YouTube↔sitio |
| LinkedIn articles públicos | Rankean en Google para queries de marca + sector |
| Pinterest tableros + pins | Rankean en Google Images para búsquedas visuales |
| UGC detallado (reviews, comentarios) | Alimenta LLMs con menciones de marca y casos de uso |
| Brand mentions sin link (social) | Señal de entidad para Knowledge Graph |
| Backlinks desde viral social | Contenido viral atrae links naturales de blogs/medios |
| NAP consistente cross-platform | Local SEO — refuerza autoridad GMB |

**Schema `sameAs` en el sitio** — conectar sitio con perfiles sociales:
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Visualogyx",
  "url": "https://visualogyx.com",
  "sameAs": [
    "https://www.youtube.com/@visualogyx",
    "https://www.linkedin.com/company/visualogyx",
    "https://www.instagram.com/visualogyx",
    "https://www.facebook.com/visualogyx",
    "https://www.tiktok.com/@visualogyx"
  ]
}
```

---

## 8. Plataformas Adicionales — Las que más se olvidan

### 8.1 Reddit — El 2º sitio más visible en Google

Reddit es el **2º sitio más visible en Google tras Wikipedia** (2026). Visibilidad orgánica creció 1,900% y el tráfico orgánico pasó de 175M a 1.23B visitas mensuales (+603%). Google firmó un acuerdo de $60M con Reddit para alimentar Gemini y AI Overviews — Reddit es ahora fuente preferencial para AI search.

**Por qué importa para SEO:**
- Respuestas en subreddits rankean directamente en Google para queries de comparación, reviews y problemas
- Google prioriza Reddit en "Discussions & Forums" del SERP
- AI Overviews cita Reddit frecuentemente como fuente de "experiencia real de usuarios"
- Los usuarios buscan activamente "[software] Reddit" para opiniones reales
- Partnership $60M con Google → Reddit = fuente primaria para Gemini + AI Overviews

> **Nota:** En enero 2025, Reddit sufrió una caída de visibilidad propia (cambios algorítmicos internos), pero su presencia EN Google sigue siendo la mayor de cualquier foro/red social.

**Estrategia de presencia en Reddit:**
- [ ] Identificar subreddits relevantes: r/SaaS, r/[industria], r/[competidor], r/[problema que resuelve el producto]
- [ ] Crear perfil de empresa (transparente, no spam) o perfil personal del fundador/experto
- [ ] Participar con valor antes de mencionar el producto (regla 9:1 — 9 aportes por 1 mención)
- [ ] Responder preguntas donde el producto es solución legítima — con link si el subreddit lo permite
- [ ] AMAs (Ask Me Anything) en subreddits de nicho — alto impacto de marca
- [ ] Monitorear menciones de marca y competidores con alertas

**Keyword research en Reddit:**
- Buscar `[keyword] site:reddit.com` en Google para ver qué se está discutiendo
- Usar Reddit Search para encontrar hilos relevantes
- Los títulos de posts populares = keywords reales que usa el target

### 8.2 Quora — Respuestas que rankean en Google

Quora aparece en el **8% de queries** en Google. Las respuestas bien escritas pueden rankear años.

**Estrategia:**
- [ ] Identificar preguntas relevantes sobre el problema que resuelve el producto
- [ ] Escribir respuestas completas y útiles (500-1500 palabras funciona mejor)
- [ ] Incluir link al sitio como recurso adicional (no como CTA directo)
- [ ] Crear perfil de autor con bio y credenciales completas (E-E-A-T)
- [ ] Usar Quora Spaces (grupos temáticos) para publicar contenido propio
- [ ] Keyword research: buscar preguntas en Quora con términos de tu nicho

**Qué rankea en Google desde Quora:**
- Preguntas con muchas vistas y respuestas consolidadas
- Respuestas largas y detalladas de autores con credibilidad verificada
- Contenido sobre comparaciones, alternativas, problemas específicos

### 8.3 Review Platforms — Autoridad para AI y B2B

Las review platforms generan el **28% de las citaciones en AI Overviews** para software B2B. Aunque pierden tráfico orgánico por zero-click, su presencia alimenta directamente a los LLMs.

| Plataforma | Para qué | Peso en AI |
|-----------|---------|-----------|
| **G2** | Software B2B — reviews verificadas | 23.1% citaciones AI |
| **Capterra** | Software en general | 17.8% citaciones AI |
| **Gartner Peer Insights** | Enterprise software | 26% citaciones AI |
| **Trustpilot** | B2C + SaaS | Alta |
| **Clutch** | Agencias + servicios B2B | Media |
| **Product Hunt** | Lanzamientos de productos | Media — muy visible en launch |
| **Yelp** | Local + servicios | 14.5% menciones AI |
| **App Store / Google Play** | Apps móviles | Alta para app queries |

**Optimización de review platforms:**
- [ ] Perfil completo en G2/Capterra: descripción con keywords, screenshots, vídeo demo
- [ ] Categorías y características correctamente listadas
- [ ] Proceso activo de solicitud de reseñas a clientes satisfechos
- [ ] Responder a TODAS las reseñas (positivas y negativas) — señal de E-E-A-T
- [ ] Mantener rating >4.0/5
- [ ] Verificar que la información de la empresa es consistente (NAP + URLs)

### 8.4 Discord — Comunidad y Social Listening B2B

Discord creció en B2B y SaaS como plataforma de comunidad oficial de productos.

**Usos para SMO:**
- [ ] Servidor propio de comunidad (si el producto tiene masa crítica de usuarios)
- [ ] Presencia en servidores de nicho relevantes (como usuario/aportador)
- [ ] Social listening: monitorear menciones de marca en servidores públicos
- [ ] Anuncios de nuevas features a la comunidad antes que en cualquier otro canal
- [ ] Feedback directo de usuarios = señal para mejorar producto y contenido

### 8.5 Medium / Substack — Contenido largo citado en AI

**Medium:**
- Artículos rankean en Google independientemente del dominio propio
- Son citados en AI Overviews como fuentes de autoridad
- Estrategia: publicar versiones ampliadas de posts del blog + link al sitio
- SEO: título con keyword, descripción con keyword, etiquetas relevantes

**Substack:**
- Newsletter que también es plataforma social (comments, notas, recommendations)
- Las "Notas" de Substack funcionan como posts cortos — visibles en la red
- Artículos indexados por Google
- Estrategia: newsletter de nicho posiciona al fundador/empresa como autoridad

### 8.6 Threads — Ya supera a X en usuarios activos

**141.5M DAU** en enero 2026 vs 125M de X/Twitter. Threads es ahora la segunda red de texto más grande tras LinkedIn.

- Conectado a Instagram (mismo handle, misma audiencia base)
- Posts indexados por Google (creciendo) — aparecen en SERPs para brand queries
- Sin algoritmo de pago — alcance orgánico muy superior a X
- Links en posts son clickeables (ventaja sobre Instagram)
- Estrategia: mismo contenido de LinkedIn/X adaptado al tono más casual; priorizar sobre X para audiencias B2C y lifestyle

### 8.6b Bluesky — Red en crecimiento con indexación completa

**30M+ usuarios**, crecimiento del 372% YoY a junio 2025. Protocolo AT Protocol (abierto, descentralizado).

**Por qué importa para SEO:**
- Perfiles de Bluesky indexados por Google
- Posts aparecen inconsistentemente en SERPs — mejorando con el tiempo
- API completamente abierta sin restricciones de acceso (ventaja sobre X)
- Audiencia: tech, periodistas, académicos, comunidad open source — alta autoridad de dominio media

**Estrategia:**
- Crear perfil verificado con dominio propio como handle (`tudominio.com` como username de Bluesky)
- Usar el mismo handle de dominio = señal de entidad para Knowledge Graph
- Publicar contenido de thought leadership, no solo reposts
- Prioridad: Medium para B2B tech, alta para marcas en sectores afectados por migración desde X

### 8.7 Spotify / Podcasts

Si el cliente tiene podcast o aparece como guest:
- [ ] Perfil de show optimizado con keywords en título y descripción
- [ ] Episodios con títulos keyword-optimizados (aparecen en Spotify Search)
- [ ] Show notes con links al sitio y recursos mencionados
- [ ] Transcripción del episodio publicada en el blog (contenido indexable)
- [ ] Distribuir en: Spotify, Apple Podcasts, Google Podcasts, Amazon Music

---

## 9. Open Graph On-Site — SMO desde el propio sitio

El sitio web debe prepararse para que cuando se comparte en social, el preview sea óptimo. Esto impacta directamente el CTR desde redes sociales.

### 9.1 Open Graph Tags (og:)

```html
<!-- Básicos — obligatorios -->
<meta property="og:title" content="[Título atractivo con keyword — max 60 chars]">
<meta property="og:description" content="[Descripción con beneficio claro — max 155 chars]">
<meta property="og:image" content="https://dominio.com/img/og-home.jpg">
<meta property="og:url" content="https://dominio.com/pagina/">
<meta property="og:type" content="website"> <!-- o "article" para blog posts -->

<!-- Imagen — crítica para CTR -->
<!-- Tamaño: 1200×630px mínimo — se muestra en Facebook, LinkedIn, WhatsApp -->
<!-- Peso: < 1MB — Facebook rechaza imágenes muy pesadas -->
<!-- Texto en imagen: < 20% de la superficie -->

<!-- Para artículos de blog -->
<meta property="og:type" content="article">
<meta property="article:published_time" content="2025-04-02T00:00:00Z">
<meta property="article:author" content="[URL perfil autor]">
```

### 9.2 Twitter / X Cards

```html
<!-- Twitter Card (también usada por LinkedIn, Slack, Discord al parsear) -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@handle">
<meta name="twitter:title" content="[Título — max 70 chars]">
<meta name="twitter:description" content="[Descripción — max 200 chars]">
<meta name="twitter:image" content="https://dominio.com/img/twitter-card.jpg">
<!-- Imagen Twitter: 1200×628px, max 5MB -->
```

### 9.3 WhatsApp / Telegram / Slack previews

- Usan og:title, og:description y og:image — los mismos tags
- WhatsApp: imagen cuadrada 1:1 también funciona bien
- Slack: usa og: tags + twitter: tags como fallback

### 9.4 Validación de OG tags

```bash
# Herramientas de validación:
# Facebook Sharing Debugger: developers.facebook.com/tools/debug
# LinkedIn Post Inspector: linkedin.com/post-inspector
# Twitter Card Validator: cards-dev.twitter.com/validator
# Open Graph Check: opengraph.xyz

# Verificar desde terminal:
curl -s "https://dominio.com/pagina/" | grep -i "og:"
```

### 9.5 OG Audit checklist

- [ ] `og:title` en todas las páginas (homepage, landings, blog posts)
- [ ] `og:description` diferente del meta description si aplica
- [ ] `og:image` custom en CADA página (no solo homepage) — 1200×630px
- [ ] Imágenes OG sin texto excesivo (< 20% del área)
- [ ] `og:url` canonical correcto
- [ ] Twitter Card `summary_large_image` en todas las páginas
- [ ] Validar con Facebook Debugger y LinkedIn Inspector
- [ ] OG image se carga correctamente (sin 404, sin redirect)

---

## 10. Social Commerce — Vender desde las Redes

Relevante cuando el cliente tiene e-commerce o vende directamente desde social.

### 10.1 TikTok Shop

TikTok Shop generó ~20% de todas las ventas por social commerce en 2025.

- [ ] Cuenta de vendedor configurada en TikTok Shop
- [ ] Catálogo de productos sincronizado
- [ ] Productos etiquetados en vídeos (Product Links)
- [ ] Showcase tab en el perfil con productos
- [ ] Afiliados TikTok activados (creadores que promocionan a comisión)
- [ ] LIVE Shopping events programados

### 10.2 Instagram Shopping

- [ ] Catálogo de Facebook/Meta conectado a Instagram
- [ ] Tienda de Instagram configurada y aprobada
- [ ] Productos etiquetados en posts y Reels
- [ ] Checkout habilitado (si disponible en el país)
- [ ] Shop tab visible en perfil

### 10.3 Pinterest Shopping

Pinterest tiene una tasa de conversión del **3.2%** — la más alta de social commerce.

- [ ] Catálogo de productos subido (feed de productos)
- [ ] Product Rich Pins activados (precio + disponibilidad en tiempo real)
- [ ] Pinterest Tag instalado en el sitio (para tracking de conversiones)
- [ ] Shop tab en perfil de Pinterest
- [ ] Boards de productos organizados por categoría/keyword

### 10.4 Facebook Shops

- [ ] Facebook Shop configurado con catálogo de Meta
- [ ] Productos taggeados en posts
- [ ] Checkout en Facebook si disponible
- [ ] Anuncios de catálogo (retargeting — aunque es paid, parte del ecosistema SMO)

---

## 11. Social Listening — Monitorear la Conversación

Social listening = saber qué se dice de la marca, competidores y sector EN TIEMPO REAL.

### 11.1 Qué monitorear

- Menciones del nombre de la marca (con y sin @)
- Nombre del producto / features específicos
- Nombres de competidores directos
- Keywords del sector (pain points, preguntas frecuentes)
- Hashtags de marca y de nicho

### 11.2 Herramientas (gratuitas primero)

| Herramienta | Plataformas | Precio |
|------------|------------|--------|
| Google Alerts | Web + blogs + noticias | Gratis |
| Reddit search + r/[nicho] | Reddit | Gratis |
| TikTok search por keyword | TikTok | Gratis |
| Mention (free tier) | Web + social | Gratis/limitado |
| Brand24 | Social + web | ~$79/mes |
| Sprout Social listening | Multi-plataforma | ~$249/mes |
| Hootsuite | Multi-plataforma | ~$99/mes |

### 11.3 Qué hacer con los datos

- **Oportunidades de respuesta**: alguien pregunta algo donde el producto es la solución → responder
- **UGC positivo**: alguien menciona la marca positivamente → amplificar (share, like, comentar)
- **Crisis temprana**: detectar quejas antes de que escalen → responder rápido
- **Ideas de contenido**: preguntas recurrentes = temas para crear contenido
- **Competitor gaps**: quejas sobre competidores = oportunidades de posicionamiento

---

## 12. UGC — User Generated Content como Señal de Autoridad

En 2025, el UGC (reseñas, testimonios, videos de usuarios) es tanto marketing como **infraestructura de datos para LLMs**.

### 12.1 Tipos de UGC para SMO

| Tipo | Canal | Impacto SEO/AI |
|------|-------|---------------|
| Reseñas verificadas | G2, Capterra, Google, Trustpilot | Alto — citadas en AI Overviews |
| Testimonios en video | YouTube, TikTok, Instagram | Alto — engagement + confianza |
| Posts de usuarios con el producto | Instagram, TikTok, LinkedIn | Medio — brand mentions |
| Comentarios detallados | Reddit, Quora | Alto — rankean en Google |
| Casos de éxito publicados por el cliente | Blog propio o LinkedIn | Muy alto — E-E-A-T |

### 12.2 Cómo activar UGC

- [ ] Email post-compra/onboarding solicitando reseña en G2/Capterra/Google
- [ ] Crear hashtag de marca propio para que usuarios etiqueten sus posts
- [ ] Concursos o incentivos para contenido de usuarios (con disclosure)
- [ ] Testimonios en video: guiar al cliente con preguntas clave (qué problema tenías → cómo lo resolviste → resultado)
- [ ] Curar y republicar el mejor UGC (con permiso) en canales propios
- [ ] Casos de éxito co-escritos con el cliente → publicar en blog + LinkedIn + enviárselos para que ellos también publiquen

### 12.3 UGC y AI Overviews

Google y los LLMs dan más peso a:
- Reseñas detalladas con casos de uso específicos (no "es muy bueno")
- UGC que menciona el nombre del producto + el problema que resuelve + el resultado
- Contenido de múltiples fuentes independientes diciendo lo mismo (consistencia)

**Template de solicitud de reseña para máximo impacto en AI:**
> "¿Puedes contarnos: qué problema tenías antes de usar [producto], cómo lo usas en tu día a día, y qué resultado has obtenido? Cuantos más detalles, mejor."

---

### Gratuitas
| Herramienta | Plataformas | Función |
|------------|------------|---------|
| YouTube Studio | YouTube | Analytics completo, search terms, CTR |
| TikTok Creative Center | TikTok | Keyword Insights, hashtags trending, top content |
| Meta Business Suite | Instagram + Facebook | Analytics, programación, bandeja unificada |
| LinkedIn Analytics | LinkedIn | Métricas de page + posts + search terms |
| Pinterest Trends | Pinterest | trends.pinterest.com — keywords en auge |
| Buffer Free | Multi | Programar 3 canales, analytics básico |

### De pago relevantes
| Herramienta | Para qué | Precio |
|------------|---------|--------|
| Metricool | Multi-plataforma: YouTube, TikTok, IG, LI, FB | ~$22/mes |
| Later | Instagram + TikTok, preview de grid + analytics | ~$18/mes |
| Iconosquare | Instagram + TikTok analytics avanzado | ~$49/mes |
| VidIQ / TubeBuddy | YouTube keyword research + SEO audit | Free + Paid |
| Shield App | LinkedIn analytics avanzado (personal + empresa) | ~$8/mes |

---

## 9. Output — Reporte SMO para Cliente

**Cliente:** [Nombre]
**Fecha:** [Fecha]
**Plataformas:** [Lista]

### Estado actual

| Plataforma | Social Search | Estrategia de links | Perfiles optimizados | Tráfico al sitio |
|-----------|--------------|--------------------|--------------------|-----------------|
| YouTube | ✅/⚠️/❌ | ✅/⚠️/❌ | ✅/⚠️/❌ | X sesiones/mes |
| TikTok | ✅/⚠️/❌ | ✅/⚠️/❌ | ✅/⚠️/❌ | X sesiones/mes |
| Instagram | ✅/⚠️/❌ | ✅/⚠️/❌ | ✅/⚠️/❌ | X sesiones/mes |
| LinkedIn | ✅/⚠️/❌ | ✅/⚠️/❌ | ✅/⚠️/❌ | X sesiones/mes |
| Facebook | ✅/⚠️/❌ | ✅/⚠️/❌ | ✅/⚠️/❌ | X sesiones/mes |

### Keywords objetivo por plataforma

| Plataforma | Keyword objetivo | Estado | Competencia |
|-----------|----------------|--------|-------------|
| YouTube | | Rankea/No rankea | |
| TikTok | | | |
| Instagram | | | |

### Plan de acción priorizado

| Prioridad | Plataforma | Acción | Impacto | Esfuerzo |
|-----------|-----------|--------|---------|---------|
| 🔴 Crítica | | | | |
| 🟡 Alta | | | | |
| 🟢 Quick win | | | | |

### KPIs de seguimiento mensual

| KPI | Fuente | Valor actual | Meta 90 días |
|-----|--------|-------------|-------------|
| Tráfico referido social total | GA4 | | |
| Sesiones desde YouTube | GA4 | | |
| Sesiones desde TikTok | GA4 | | |
| Sesiones desde Instagram | GA4 | | |
| Sesiones desde LinkedIn | GA4 | | |
| Impresiones desde YouTube Search | YouTube Studio | | |
| Link clicks (bio) | TikTok/IG Analytics | | |
