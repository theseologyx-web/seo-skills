---
name: seo-video
description: >
  Video and social video SEO across all platforms. Covers YouTube, TikTok, Instagram
  Reels, LinkedIn Video, Pinterest Video, Google Video Search, visual search (Google
  Lens), and multiformat content strategy. Use when user says "YouTube SEO", "TikTok
  SEO", "Instagram Reels", "video optimization", "visual search", "Google Lens",
  "video rankings", "short-form video", or "multimodal search".
user-invokable: true
argument-hint: "[youtube url, tiktok url, instagram url, channel url, or website url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Video SEO & Multimodal Search Optimization

Optimize video content for YouTube, Google Video Search, and visual/multimodal search experiences.

---

## 1. YouTube SEO Fundamentals

### Video Metadata Optimization

**Title:**
- 60 characters max (truncated beyond that in search)
- Primary keyword near the beginning
- Compelling, descriptive — not clickbait
- Format: `[Primary Keyword]: [Compelling Description] | [Brand]`

**Examples:**
| ✅ Good | ❌ Bad |
|---------|--------|
| "How to Fix Core Web Vitals in 2025 (Step-by-Step)" | "You NEED to see this SEO trick!! 🔥" |
| "Google Analytics 4 Tutorial for Beginners" | "GA4 Tutorial Part 3 Updated New Version" |

**Description:**
- First 2-3 lines visible without expanding — make them count
- Include primary keyword in first 100 characters
- 300-500 words minimum for important videos
- Add timestamps (chapters) — improves UX and appears in SERP
- Include relevant links (website, related videos)
- Add hashtags (3-5 max at end of description)

**Tags:**
- 5-10 specific, relevant tags
- **2026 note:** YouTube uses AI to understand speech, text, and visuals — tags are nearly irrelevant for ranking. Focus on title, description, and actual content quality over tag optimization.

### Chapters & Timestamps
```
00:00 Introduction
01:30 What is Core Web Vitals
03:45 How to Measure LCP
07:20 Fixing LCP Issues
12:10 CLS Explained
16:45 INP Optimization
22:00 Final Checklist
```
Chapters appear in Google SERP and YouTube search — major CTR improvement.

### Thumbnail Optimization
- Custom thumbnail (never auto-generated)
- High contrast, readable text at small size
- Face/emotion if applicable (increases CTR)
- Consistent brand style across channel
- 1280×720px minimum, 16:9 ratio
- File size < 2MB (JPG, PNG, GIF, WebP)

---

## 2. YouTube Channel Optimization

### Channel Authority Signals
- Channel name matches brand/topic consistently
- Channel description includes primary keywords
- Channel icon and banner professional and on-brand
- About section complete with relevant keywords
- Links to website and social profiles configured
- Featured video set for non-subscribers

### Playlists as SEO Asset
- Create playlists for topic clusters
- Playlist titles include target keywords
- Ordered logically (beginner → advanced)
- Add playlist descriptions with keywords

### Engagement Signals (Ranking Factors — Updated 2026)

**YouTube confirmed in 2026: Viewer Satisfaction surveys now weigh MORE than raw watch time.** A short video with high satisfaction outperforms a long video with mediocre retention.

| Signal | Priority 2026 | Why It Matters |
|--------|--------------|---------------|
| **Viewer Satisfaction score** | #1 (new) | Survey data directly from users post-watch |
| Watch time / Average view duration | High | Still important, but not the sole metric |
| Click-through rate (CTR) | High | Drives impressions to views |
| Likes, comments, shares | Medium | Engagement signals |
| Subscribers gained per video | Medium | Channel authority |
| Save to playlist | Medium | High-intent engagement |

**Target benchmarks (vary by niche):**
- Average view duration: > 40% of video length
- CTR: B2B average 2-4%, entertainment 6-10% (not a universal 4-5% target)
- Focus on satisfaction-optimizing: strong opening hook, clear value delivery, no padding

---

## 3. Video Structured Data (Schema)

Add `VideoObject` schema to pages embedding videos:

```json
{
  "@context": "https://schema.org",
  "@type": "VideoObject",
  "name": "How to Fix Core Web Vitals in 2025",
  "description": "Step-by-step guide to fixing LCP, INP, and CLS for better Google rankings.",
  "thumbnailUrl": "https://example.com/thumbnail.jpg",
  "uploadDate": "2025-03-15T08:00:00+00:00",
  "duration": "PT22M30S",
  "contentUrl": "https://www.youtube.com/watch?v=VIDEOID",
  "embedUrl": "https://www.youtube.com/embed/VIDEOID",
  "publisher": {
    "@type": "Organization",
    "name": "Brand Name",
    "logo": {
      "@type": "ImageObject",
      "url": "https://example.com/logo.png"
    }
  },
  "hasPart": [
    {
      "@type": "Clip",
      "name": "What is Core Web Vitals",
      "startOffset": 90,
      "endOffset": 225,
      "url": "https://www.youtube.com/watch?v=VIDEOID&t=90"
    }
  ]
}
```

**`hasPart` with Clip** enables Key Moments in Google SERP — shows timestamps directly in search results.

---

## 4. Google Video Search Optimization

### How Google Indexes Videos
- Google crawls and indexes videos embedded on pages
- Requires `VideoObject` schema OR video sitemap
- Prefers videos with transcripts (accessibility + indexability)
- YouTube videos have advantage (Google owns YouTube)

### Video Sitemap
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:video="http://www.google.com/schemas/sitemap-video/1.1">
  <url>
    <loc>https://example.com/blog/core-web-vitals-guide</loc>
    <video:video>
      <video:thumbnail_loc>https://example.com/thumbnail.jpg</video:thumbnail_loc>
      <video:title>How to Fix Core Web Vitals in 2025</video:title>
      <video:description>Step-by-step guide...</video:description>
      <video:content_loc>https://www.youtube.com/watch?v=VIDEOID</video:content_loc>
      <video:duration>1350</video:duration>
      <video:publication_date>2025-03-15T08:00:00+00:00</video:publication_date>
    </video:video>
  </url>
</urlset>
```

### Transcripts & Closed Captions
- Auto-captions are error-prone — upload manual SRT files
- Transcripts on page = additional keyword-rich text Google indexes
- Improves accessibility (WCAG compliance)
- Format: `.srt` or `.vtt` files

---

## 5. Visual Search Optimization (Google Lens & Pinterest)

### Google Lens
Google Lens identifies objects, text, and scenes in images. Optimize for it:

**Product images:**
- Clean, well-lit, white/neutral background
- Multiple angles
- High resolution (min 1000×1000px for products)
- Alt text describes the product specifically
- Structured data: `Product` schema with `image` property

**Informational images (infographics, charts):**
- Embed readable text in the image itself
- Descriptive file name (`core-web-vitals-checklist-2025.webp`)
- Alt text summarizes the key information
- Caption below image with relevant keywords

### Pinterest SEO
If Pinterest is relevant to your audience:
- Pin descriptions: 100-500 characters, keyword-rich
- Board names: descriptive, keyword-optimized
- Alt text on images (Pinterest reads it)
- Rich Pins enabled (requires schema markup on source page)
- Vertical format: 2:3 ratio (1000×1500px ideal)

### Image Search Optimization Checklist
- [ ] Descriptive, keyword-rich alt text on all images
- [ ] SEO-friendly file names (no IMG_1234.jpg)
- [ ] High-resolution images (Google Lens prefers quality)
- [ ] `Product` or `ImageObject` schema where appropriate
- [ ] Images submitted via sitemap (image sitemap)
- [ ] Open Graph `og:image` set correctly (controls social previews)

---

## 6. TikTok SEO

**49% of Americans use TikTok as a search engine** (2026). Google surfaces TikTok videos in its results and crawls them for AI Overviews. Optimize for TikTok search AND for Google (TikTok videos rank in Google).

**2026 ranking note:** TikTok now indexes complete captions, spoken words, and on-screen text — hashtags matter less. Semantic content of the video is what ranks, not hashtag count.

### TikTok Ranking Factors
| Factor | Weight | How to Optimize |
|--------|--------|----------------|
| Video completion rate | High | Hook in first 1-3 seconds, keep it tight |
| Rewatches | High | Loop-worthy endings |
| Shares | High | Shareable, relatable, surprising |
| Comments | High | Ask a question, spark debate |
| Saves | Medium | "Save this for later" content |
| Follows after viewing | Medium | Strong content promise |

### TikTok Keyword Optimization
- **Caption**: Include target keyword naturally in first line (TikTok indexes captions)
- **On-screen text**: Type keywords as text overlays — TikTok reads them
- **Spoken words**: TikTok auto-captions and indexes spoken content
- **Hashtags**: 3-5 hashtags max — mix niche + broad
  - 1 broad (`#marketing`)
  - 2 niche (`#seotips`, `#digitalmarketing2025`)
  - 1 trending (if relevant)

### TikTok Search Optimization
- Search the keyword in TikTok before creating — see what's ranking
- Use TikTok's autocomplete for keyword ideas
- Title/first line of caption = TikTok's "title tag"
- Profile bio: include primary niche keyword

### TikTok Technical Specs
- Vertical: 9:16 ratio, 1080×1920px
- Duration: 15s–10min (15-60s performs best for discovery)
- File format: MP4 or MOV
- Caption: 2,200 characters max (first 150 visible)

### TikTok → Google Spillover
TikTok videos increasingly appear in Google SERPs. For this:
- Ensure video caption includes target keyword (Google reads it)
- Strong thumbnail frame (Google shows the video in results)
- Link to your website in TikTok bio (traffic + brand signal)

---

## 7. Instagram Reels SEO

Instagram is not a traditional search engine but has growing search functionality and feeds Google Discover.

### Instagram Ranking Factors
| Factor | Impact |
|--------|--------|
| Watch time / replays | High |
| Shares (DMs + Stories) | High |
| Saves | High |
| Comments | Medium |
| Likes | Medium |
| Profile follows | Medium |

### Instagram Keyword Optimization
- **Caption first line**: Most important — treat like a title tag
- **Alt text**: Set manually on every post (`Advanced settings → Write alt text`)
- **Hashtags**: 3-10 highly relevant (Instagram deprioritizes hashtag spam)
- **Profile name field**: Searchable — add keyword (e.g., "Laura | SEO Specialist")
- **Bio**: Keyword-rich, describes who you help

### Instagram Reels Specs
- Vertical: 9:16, 1080×1920px
- Duration: 15s–90s (under 30s best for reach)
- Cover image: custom frame that shows well in grid

### Instagram → SEO Connection
- `og:image` on your website should match your Instagram visual style (brand consistency)
- Instagram profile often ranks for brand name searches
- High-authority backlink opportunity: link in bio → website

---

## 8. LinkedIn Video SEO

LinkedIn video gets 3× more engagement than text posts and ranks for brand + professional searches.

### LinkedIn Video Optimization
- **Native upload** always outperforms YouTube links (LinkedIn suppresses external links)
- **Captions/subtitles**: 85% of LinkedIn videos watched without sound — mandatory
- **First 3 seconds**: Must hook without sound
- **Optimal length**: 1-2 minutes for feed; up to 10 min for thought leadership
- **Description**: 150-200 characters, keyword-rich, visible before "see more"

### LinkedIn Video Ranking Signals
- Dwell time on video
- Reactions (especially "Insightful", "Love")
- Comments (especially from connections)
- Shares to feed or DMs
- Profile visits after viewing

### LinkedIn → SEO Value
- LinkedIn articles and posts **rank in Google** for brand + professional queries
- Embed YouTube video in LinkedIn article → backlink signal
- Company page videos can rank for `[brand] + [topic]` queries

---

## 9. Pinterest Video Pins

Pinterest is a visual search engine — videos get priority in feeds and search results.

### Pinterest Video SEO
- **Title**: 100 characters, keyword at start
- **Description**: 500 characters, natural keyword use, no hashtag stuffing
- **Cover image**: Eye-catching, vertical, text overlay with keyword
- **Board**: Place in keyword-optimized board with description
- **Link**: Always link to relevant page on your website

### Pinterest Video Specs
- Vertical: 2:3 ratio (1000×1500px) or square (1:1)
- Duration: 4 seconds–15 minutes (15s–1min performs best)
- File formats: MP4, MOV, M4V
- File size: < 2GB

### Rich Pins for Video
Enable Rich Pins on your website (requires schema markup) to pull metadata automatically into Pinterest pins — increases trust and SEO value.

---

## 10. Video en AI Search (2026)

### YouTube como fuente de AI Overviews

**29.5% de Google AI Overviews citan YouTube como fuente.** YouTube ya no es solo un canal de video — es fuente de citación para AI search.

**Optimizar video para ser citado por AI:**
- Incluir respuestas directas en la descripción (AI lee texto, no solo video)
- Añadir transcripciones completas en la descripción o como capítulos
- Usar VideoObject schema con `description` completa y `transcript`
- Títulos con preguntas directas: "How does X work?" → citable en AIO

**TikTok → Google pipeline:** Los TikToks aparecen en Google SERP y son rastreados para AI Overviews. Optimizar captions para ser seleccionados como source.

### YouTube Shorts SEO

**Shorts engagement pesa más en recomendaciones del canal entero desde 2026.**

| Factor | Shorts SEO |
|--------|-----------|
| Hook | Primeros 0-2 segundos críticos |
| Completion rate | > 60% es excelente (Shorts son cortos por definición) |
| Retention | Influye en RPM y monetización más que vistas brutas |
| Loop | Los Shorts hacen loop — optimizar para que el viewer vea 2-3x |
| Title | 40 chars max visibles — keyword al inicio |
| Hashtag #Shorts | Obligatorio para distribución en Shorts feed |

**Funnel Shorts → Long-form:** Usar Shorts para captar audiencia → CTA en Shorts apunta a video largo. Monitoreado en YouTube Analytics (Shorts → long-form bridge).

### AI Content Disclosure — Obligatorio

YouTube exige etiquetas de disclosure para contenido generado/alterado con AI:

- **Aplica a:** video generado con AI, voiceovers de AI, avatars sintéticos, footage modificado
- **Consecuencia de no etiquetar:** recomendaciones reducidas, posible penalización
- **Setup:** YouTube Studio → Edit Video → "Contains synthetic or AI-generated content"
- **TikTok y Meta** también requieren disclosure labels para AI-generated content

---

## 11. Tools by Platform

### YouTube Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| VidIQ | Keyword research, competitor analysis, optimization scores | Free + Paid |
| TubeBuddy | A/B thumbnail testing, bulk processing, SEO audit | Free + Paid |
| YouTube Analytics | Native performance data | Free |
| Keyword Tool for YouTube | Autocomplete-based keyword research | Free + Paid |
| Social Blade | Channel growth tracking, competitor benchmarking | Free + Paid |
| Morningfame | Small channel SEO, topic research | Paid |

### TikTok Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| TikTok Creative Center | Trending hashtags, sounds, top ads | Free |
| Kalodata | TikTok analytics + competitor research | Paid |
| Pentos | TikTok keyword tracking | Paid |
| Tokfluence | Influencer research + analytics | Paid |
| TikTok Analytics (native) | Performance data | Free |

### Instagram Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| Meta Business Suite | Native analytics, scheduling | Free |
| Later | Visual planning, hashtag research | Free + Paid |
| Iconosquare | Advanced analytics, competitor tracking | Paid |
| Flick | Hashtag research and management | Paid |
| Sprout Social | Multi-platform management + analytics | Paid |

### LinkedIn Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| LinkedIn Analytics (native) | Impressions, engagement, follower data | Free |
| Shield App | Advanced LinkedIn analytics | Paid |
| Taplio | Content scheduling + analytics | Paid |

### Cross-Platform Video Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| Descript | Video editing + auto-captions + transcripts | Free + Paid |
| Opus Clip | AI clips from long video → Shorts/Reels/TikTok | Paid |
| Repurpose.io | Auto-publish across platforms | Paid |
| Captions.ai | Auto-captions with styling | Free + Paid |
| Rev.com | Professional transcription + SRT files | Paid |

---

## 11. Multimodal Content Strategy

### Content Formats to Optimize
| Format | Search Surface | Optimization Focus |
|--------|---------------|-------------------|
| Video (YouTube) | YouTube + Google Video | Title, description, chapters, schema |
| Images | Google Images + Lens | Alt text, schema, file names |
| Infographics | Google Images | Alt text, surrounding text, links |
| Podcasts | YouTube Music / Spotify / Apple Podcasts (Google Podcasts discontinued 2024) | Show notes, transcripts, schema |
| Short-form video | Google Discover, Shorts | Reels/Shorts SEO, thumbnails |

### Cross-Format Strategy
- Repurpose: blog post → video → infographic → short clips
- Each format targets different intent/platform
- Internal link between formats (blog embeds video, video links to blog)
- Consistent keyword strategy across all formats

---

## 11. YouTube API — Datos en tiempo real

Para auditar un canal con datos reales (suscriptores, vistas, engagement por vídeo):

```bash
# Info del canal (Channel ID desde URL del canal):
python ~/.claude/skills/seo/scripts/youtube_search.py channel UC[CHANNEL_ID]

# Top vídeos por vistas:
python ~/.claude/skills/seo/scripts/youtube_search.py search "[brand]" --order viewCount --limit 20

# Detalle de un vídeo (ID desde URL youtube.com/watch?v=ID):
python ~/.claude/skills/seo/scripts/youtube_search.py video [VIDEO_ID]
```

**Setup requerido:** Habilitar YouTube Data API v3 en Google Cloud Console (misma API key que PSI/CrUX).
Ver setup completo en: `seo-smo` → Sección 1.1.

**Nota:** Para auditoría SMO completa (YouTube + TikTok + Instagram + LinkedIn), usar `seo-smo`.

---

## 12. Audit de Vídeos Existentes

Antes de recomendar qué crear, auditar qué existe y cómo está funcionando.

### 12.1 Inventario y clasificación de vídeos existentes

Para cada vídeo del canal, clasificar por tipo:

| Tipo de vídeo | Intención | Ejemplo |
|--------------|----------|---------|
| **Demo / Product tour** | Decisión (BOFU) | "Cómo funciona [Producto]" |
| **Tutorial / How-to** | Consideración (MOFU) | "Cómo crear una inspección en VLX" |
| **Explicativo (Explainer)** | Awareness (TOFU) | "Qué es la inspección digital" |
| **Caso de éxito / Testimonial** | Decisión (BOFU) | "[Cliente] usa [Producto] para..." |
| **Comparación / vs** | Consideración (MOFU) | "[Producto] vs [Competidor]" |
| **Sector / Industria** | Awareness (TOFU) | "Inspecciones en Oil & Gas" |
| **Onboarding / Feature** | Retención | "Nuevo: función de reportes automáticos" |
| **Webinar / Live** | Autoridad | "Q&A con el equipo de producto" |

### 12.2 Métricas por vídeo a evaluar

Obtener desde YouTube Studio → Analytics o vía API:

```bash
# Top vídeos por vistas (detectar los que más funcionan):
python ~/.claude/skills/seo/scripts/youtube_search.py search "[marca]" --order viewCount --limit 20

# Detalle de un vídeo específico (engagement, duración, tags):
python ~/.claude/skills/seo/scripts/youtube_search.py video [VIDEO_ID]
```

| Métrica | Señal | Acción si está bajo |
|---------|-------|-------------------|
| **Vistas totales** | Alcance histórico | Revisar título y thumbnail |
| **CTR desde impressiones** | Qué tan atractivo es el título/thumbnail | Mejorar thumbnail o reescribir título |
| **Watch time / % completado** | Qué tan relevante es el contenido | Mejor hook en primeros 30s o editar intro |
| **Likes/vistas ratio** | Engagement de calidad | Añadir CTA verbal para likes |
| **Comentarios** | Comunidad y relevancia | Añadir pregunta al final del vídeo |
| **Tráfico desde búsqueda YouTube** | SEO del vídeo | Optimizar título, descripción y tags |

### 12.3 Gaps de optimización en vídeos existentes

Para cada vídeo revisar:
- [ ] Título: ¿keyword al inicio? ¿< 60 chars? ¿Clickbait o descriptivo?
- [ ] Descripción: ¿keyword en primeras 2 líneas? ¿Link al sitio con UTM? ¿Capítulos/timestamps?
- [ ] Tags: ¿5-10 relevantes? ¿Incluye keyword exacta + variantes?
- [ ] Captions: ¿Manuales o auto? (manuales = mejor indexación)
- [ ] Thumbnail: ¿Custom? ¿Texto legible? ¿Consistencia de marca?
- [ ] Cards y pantalla final: ¿configuradas con links a landing y vídeos relacionados?
- [ ] Capítulos en descripción: ¿existen? (aparecen en SERP de Google)
- [ ] Vídeo embebido en la página web correspondiente

---

## 13. Gap Analysis — Qué Vídeos Crear

### 13.1 Identificar gaps por tipo de contenido + keyword

El punto de partida es el keyword mapping del sitio. Cada URL con tráfico relevante es candidata a tener un vídeo de soporte.

**Proceso:**
1. Listar páginas del sitio por prioridad (landings, blog posts con más tráfico, how-tos)
2. Para cada página: ¿existe un vídeo que la apoye? → si no → candidato a crear
3. Clasificar por tipo e intención:

```
Landing /digital-inspections-software/ → ¿hay demo/explainer? → si no → CREAR: Demo corto (2-3min)
Blog /blog/field-inspection-tips/ → ¿hay tutorial? → si no → CREAR: How-to (5-8min)
Industry /digital-inspections-software/oil-gas/ → ¿hay vídeo de sector? → CREAR: Explainer sector
Competidor keywords → ¿hay vídeo de comparación? → CREAR: [Marca] vs [Competidor]
```

### 13.2 Tipos de vídeos prioritarios por etapa del funnel

| Funnel | Tipo de vídeo | Keyword pattern | Duración ideal |
|--------|--------------|----------------|---------------|
| **TOFU** | Explainer del problema | "qué es X", "cómo funciona X" | 1-3 min |
| **TOFU** | Vídeo de sector/industria | "[sector] inspection", "[industria] digital" | 2-4 min |
| **MOFU** | Tutorial / How-to | "cómo hacer X con [producto]", "tutorial X" | 5-10 min |
| **MOFU** | Comparación | "[producto] vs [competidor]", "mejor app para X" | 5-8 min |
| **BOFU** | Demo del producto | "[producto] demo", "cómo usar [producto]" | 2-5 min |
| **BOFU** | Caso de éxito | "[cliente] usa [producto]", "[sector] success story" | 2-4 min |
| **Retención** | Feature updates / onboarding | "nueva función", "setup [producto]" | 1-3 min |

### 13.3 Template de planificación de vídeo

Para cada vídeo a crear:

```
VÍDEO: [Título provisional]
Tipo: [Demo / Tutorial / Explainer / Caso / Comparación]
Keyword objetivo: [keyword principal]
Intención: [TOFU / MOFU / BOFU]
Página web de destino (embed): [URL]
Página web que apoya (link en descripción): [URL]
Duración estimada: [X min]
CTA principal: [ir a la web / registrarse / descargar guía]
```

---

## 14. Estrategia de Embedding — Vídeos en el Sitio Web

El vídeo embebido en el sitio cumple tres funciones: aumenta el dwell time, genera señales de comportamiento positivas para SEO, y es elegible para Video Rich Results en Google.

### 14.1 Qué páginas deben tener vídeo embebido

| Tipo de página | Tipo de vídeo recomendado | Impacto |
|---------------|--------------------------|--------|
| **Homepage** | Explainer corto (60-90s) | Reducción del bounce rate |
| **Landing de producto** | Demo del producto (2-4min) | Aumenta tiempo en página + conversión |
| **Páginas de industria/sector** | Explainer de sector (2-3min) | Relevancia temática + dwell time |
| **Blog posts** | Tutorial o how-to relacionado | Aumenta tiempo en página hasta 2× |
| **How-to / Guías** | Tutorial paso a paso | Complementa el texto, rich result elegible |
| **Casos de éxito** | Testimonial en vídeo | Prueba social + tiempo en página |
| **Páginas de precios/comparación** | Demo o comparación (2-3min) | Reduce fricción antes de convertir |
| **FAQs** | Vídeos cortos de respuesta | Passage ranking + featured snippet |

### 14.2 Cómo implementar correctamente el embed

```html
<!-- Embed básico con lazy loading (no ralentiza la página) -->
<iframe
  src="https://www.youtube.com/embed/VIDEO_ID?rel=0"
  title="[Descripción con keyword]"
  loading="lazy"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>

<!-- Para rendimiento: usar youtube-nocookie.com y facade (carga bajo demanda) -->
<iframe src="https://www.youtube-nocookie.com/embed/VIDEO_ID" loading="lazy"></iframe>
```

**Importante para CWV:** Los iframes de YouTube penalizan LCP si se cargan de forma eager. Usar `loading="lazy"` o una fachada (lite-youtube-embed) que carga el iframe solo cuando el usuario hace clic.

### 14.3 VideoObject schema en páginas con vídeo embebido

Añadir `VideoObject` schema en CADA página donde el vídeo es el contenido principal o un complemento relevante:

```json
{
  "@context": "https://schema.org",
  "@type": "VideoObject",
  "name": "[Título del vídeo — con keyword]",
  "description": "[Descripción del vídeo — 150+ chars con keywords]",
  "thumbnailUrl": "https://img.youtube.com/vi/VIDEO_ID/maxresdefault.jpg",
  "uploadDate": "2025-04-01T00:00:00Z",
  "duration": "PT5M30S",
  "embedUrl": "https://www.youtube.com/embed/VIDEO_ID",
  "contentUrl": "https://www.youtube.com/watch?v=VIDEO_ID"
}
```

**Regla:** El schema va en la página web donde está embebido, no solo en YouTube.

### 14.4 Vídeo sitemap para Google

Para asegurar que Google indexa todos los vídeos embebidos:
```xml
<url>
  <loc>https://dominio.com/pagina-con-video/</loc>
  <video:video>
    <video:thumbnail_loc>https://img.youtube.com/vi/VIDEO_ID/maxresdefault.jpg</video:thumbnail_loc>
    <video:title>[Título con keyword]</video:title>
    <video:description>[Descripción]</video:description>
    <video:content_loc>https://www.youtube.com/watch?v=VIDEO_ID</video:content_loc>
    <video:duration>330</video:duration>
    <video:publication_date>2025-04-01T00:00:00Z</video:publication_date>
  </video:video>
</url>
```

---

## Output Format

### Video SEO Audit Report

**Channel/Page:** [URL]
**Audit Date:** [Date]

#### Summary
[2-3 sentences: overall video SEO health, top opportunity, primary gap]

#### YouTube Channel Health
| Factor | Status | Score | Action |
|--------|--------|-------|--------|
| Title optimization | ✅/⚠️/❌ | X/10 | ... |
| Description quality | ✅/⚠️/❌ | X/10 | ... |
| Chapters/timestamps | ✅/⚠️/❌ | X/10 | ... |
| Custom thumbnails | ✅/⚠️/❌ | X/10 | ... |
| VideoObject schema | ✅/⚠️/❌ | X/10 | ... |
| Transcripts | ✅/⚠️/❌ | X/10 | ... |

#### Priority Recommendations
| Priority | Action | Expected Impact | Effort |
|----------|--------|-----------------|--------|
| 🔴 | [Critical] | [Impact] | [Low/Med/High] |
| 🟡 | [High] | [Impact] | [Low/Med/High] |
| 🟢 | [Quick win] | [Impact] | [Low/Med/High] |
