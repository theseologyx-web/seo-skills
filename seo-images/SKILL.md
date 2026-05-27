---
name: seo-images
description: >
  Image optimization analysis for SEO and performance. Checks alt text, file
  sizes, formats, responsive images, lazy loading, and CLS prevention. Use when
  user says "image optimization", "alt text", "image SEO", "image size",
  "optimize images", "image performance", or "image audit".
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: AgriciDaniel + jorgejaramillo (merged)
  version: "1.7.1"
  category: seo
---

# Image Optimization Analysis

Analyze images on a webpage or site for SEO and performance optimization. Provide actionable recommendations sorted by impact.

## CRITICAL: Data Extraction Method

**WebFetch converts HTML to markdown and may lose `<img>` tag attributes** like `alt`, `loading`, `width`, `height`, `srcset`, and `sizes`. It also strips `<picture>` elements and `<source>` tags entirely.

### Correct approach:

**Use `curl` via Bash to extract image tags accurately:**
```bash
# Get all <img> tags with their attributes
curl -sL [URL] | grep -oP '<img[^>]+>' | head -20

# Check for <picture> elements and srcset
curl -sL [URL] | grep -i -E '(<picture|srcset|<source.*type=)'

# Check for lazy loading
curl -sL [URL] | grep -i 'loading=' | head -10
```

**Use WebFetch** only for understanding the general page layout and visible image context.

**NEVER report alt text as "missing" based solely on WebFetch output.** Always verify with `curl`.

---

## Input

Expects one of:
- URL to analyze (fetches and parses HTML)
- HTML file or content
- List of image URLs to audit

## REGLA CRÍTICA: Texto en imágenes → debe ser HTML

**Las imágenes NO deben contener texto quemado (burned text) salvo excepciones.**

Google no puede leer texto dentro de una imagen con la misma fiabilidad que el texto HTML. El texto en imagen:
- No es indexado como contenido del sitio
- No es accesible (lector de pantalla no lo lee)
- No se puede traducir/localizar automáticamente
- Penaliza los Core Web Vitals si la imagen es más pesada por el texto

### Regla por tipo de elemento

| Elemento | ¿Texto en imagen? | Formato correcto |
|---------|------------------|-----------------|
| **Título/H1 de artículo** | ❌ Nunca | `<h1>` HTML sobre o debajo de la imagen |
| **Título de banner/hero** | ❌ Nunca | `<h1>` o `<h2>` HTML con CSS overlay sobre la imagen |
| **CTA de banner** | ❌ Nunca | `<a>` o `<button>` HTML sobre la imagen |
| **Subtítulo de sección** | ❌ Nunca | `<h2>/<h3>` HTML, no en la imagen de fondo |
| **Texto decorativo corto** | ⚠️ Evitar | Si inevitable, describir en alt text |
| **Infografía** | ✅ Permitido | Imagen con texto visual — añadir alt descriptivo completo o transcripción en HTML debajo |
| **Diagrama / Gráfico** | ✅ Permitido | Añadir `<caption>` o párrafo explicativo en HTML |
| **Screenshot de UI** | ✅ Permitido | Es una captura real — describir en alt text |
| **Imagen de redes sociales (OG/thumbnail)** | ✅ Permitido | No se indexa como contenido del sitio |

### Patrón correcto para banners con texto

```html
<!-- ❌ MAL: título quemado en la imagen -->
<img src="banner-inspection-software.jpg" alt="Inspection Software">

<!-- ✅ BIEN: imagen como fondo, texto en HTML -->
<section style="position: relative;">
  <img src="banner-background.webp" alt="" role="presentation"
       width="1920" height="600" fetchpriority="high">
  <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%);">
    <h1>Inspection Software for Field Teams</h1>
    <a href="/demo/">Request a Demo</a>
  </div>
</section>

<!-- ✅ BIEN: CSS background-image + HTML overlay (patrón más común) -->
<section class="hero" style="background-image: url('banner-background.webp');">
  <h1>Inspection Software for Field Teams</h1>
  <a href="/demo/" class="btn">Request a Demo</a>
</section>
<!-- Nota: con background-image, Google puede no indexar la imagen. Usar <img> si la imagen tiene valor SEO -->
```

### Cuándo sí se puede usar texto en imagen

1. **Infografías**: el texto es parte del contenido visual → añadir transcripción en HTML debajo o en `aria-describedby`
2. **Capturas de pantalla**: muestran UI real → describir en alt text qué se ve
3. **Thumbnails de YouTube/OG images**: no son contenido indexable del sitio
4. **Logotipos**: texto del logo en imagen → alt text = nombre de la marca

### Alt text cuando la imagen SÍ tiene texto

Si una imagen inevitablemente incluye texto, el alt text debe transcribirlo:
```html
<!-- Infografía con datos -->
<img src="inspection-stats-2025.webp"
     alt="Infografía: Las empresas que digitalizan inspecciones reducen errores un 73%, ahorran 4.2h por inspector a la semana y mejoran el compliance en un 89%. Fuente: VLX Report 2025."
     width="800" height="1200">
<!-- Optionalmente añadir transcripción completa en <details> debajo -->
```

---

## Analysis Checklist

### 1. Alt Text Quality

Every `<img>` must have descriptive alt text (except decorative images with `role="presentation"` or `alt=""`).

**Quality criteria:**
- Describes image content, not filename
- Natural keyword inclusion (no stuffing)
- Length: 10-125 characters
- Context-appropriate

**Examples:**

| ✅ Good | ❌ Bad | Why |
|---------|--------|-----|
| "Chocolate labrador puppy playing with tennis ball in backyard" | "IMG_2847.jpg" | Descriptive vs filename |
| "Graph showing 40% increase in organic traffic from Q3 to Q4" | "graph chart data analytics metrics" | Natural vs keyword stuffing |
| "Black leather office chair with ergonomic lumbar support" | "Click here to see more" | Descriptive vs call-to-action |
| "Sunset over Golden Gate Bridge, San Francisco" | "Photo" | Specific vs generic |

**Decorative images** (dividers, backgrounds, spacers):
```html
<img src="divider.png" role="presentation" alt="">
```

### 2. File Size Optimization

**Tiered thresholds by image type:**

| Image Category | Target | Warning | Critical | Examples |
|----------------|--------|---------|----------|----------|
| Thumbnails / Icons | < 50KB | > 100KB | > 200KB | Product thumbnails, author avatars, category icons |
| Content images | < 100KB | > 200KB | > 500KB | Blog photos, product images, gallery items |
| Hero / Banner images | < 200KB | > 300KB | > 700KB | Above-fold hero images, full-width banners |
| Background images | < 150KB | > 250KB | > 600KB | Section backgrounds, pattern fills |

**Flag images exceeding thresholds and estimate compression savings.**

### 3. Modern Format Usage

**Recommended format hierarchy:**

| Format | Browser Support | Compression | Use Case | File Size Comparison |
|--------|-----------------|-------------|----------|---------------------|
| AVIF | ~96% (2026) | Best | Photos, complex images | 50% smaller than JPEG |
| WebP | 95.3% | Excellent | Default for most images | 25-35% smaller than JPEG |
| JPEG | 100% | Good | Fallback for photos | Baseline |
| PNG | 100% | Lossless | Graphics with transparency | Larger than WebP |
| SVG | 100% | Vector | Icons, logos, simple graphics | Scales without quality loss |

**Preferred `<picture>` element pattern:**

```html
<picture>
  <source srcset="hero-image.avif" type="image/avif">
  <source srcset="hero-image.webp" type="image/webp">
  <img src="hero-image.jpg" alt="Modern living room with minimalist furniture"
       width="1200" height="675" loading="lazy" decoding="async">
</picture>
```

**Flag:**
- JPEGs/PNGs that should be WebP/AVIF
- Missing fallback formats in `<picture>` elements
- Estimate conversion savings

#### JPEG XL: Emerging Format

In November 2025, Google's Chromium team reversed its 2022 decision and announced it will restore JPEG XL support in Chrome using a Rust-based decoder. The implementation is feature-complete but not yet in Chrome stable. JPEG XL offers lossless JPEG recompression (~20% savings with zero quality loss) and competitive lossy compression. **Not yet practical for web deployment**, but worth monitoring for future adoption.

### 4. Responsive Images Implementation

Check for proper `srcset` and `sizes` attributes to serve appropriately sized images.

**Example:**
```html
<img
  src="product-800.webp"
  srcset="product-400.webp 400w,
          product-800.webp 800w,
          product-1200.webp 1200w,
          product-1600.webp 1600w"
  sizes="(max-width: 640px) 100vw,
         (max-width: 1024px) 50vw,
         33vw"
  alt="Wireless noise-cancelling headphones in matte black"
  width="800"
  height="600"
  loading="lazy"
>
```

**Flag images without:**
- `srcset` for images > 400px wide
- `sizes` attribute matching layout breakpoints
- Device pixel ratio variants (1x, 2x for retina)

### 5. Lazy Loading Strategy

**Critical rules:**
- ✅ `loading="lazy"` on below-fold images
- ❌ NEVER lazy-load LCP (Largest Contentful Paint) images
- ❌ NEVER lazy-load above-fold images
- ✅ Native lazy loading preferred over JavaScript libraries

**Examples:**

```html
<!-- ❌ BAD - Lazy loading hero image hurts LCP -->
<img src="hero.webp" loading="lazy" alt="...">

<!-- ✅ GOOD - Hero image loads immediately -->
<img src="hero.webp" fetchpriority="high" alt="..." width="1200" height="630">

<!-- ✅ GOOD - Below-fold image lazy loads -->
<img src="testimonial.webp" loading="lazy" decoding="async" alt="..." width="400" height="400">
```

**Flag:**
- Hero/above-fold images with `loading="lazy"` (critical error)
- Below-fold images without `loading="lazy"` (missed optimization)

### 6. LCP Image Optimization

**For hero/banner images:**

```html
<img src="hero-banner.webp"
     fetchpriority="high"
     alt="Professional kitchen remodeling services in Seattle"
     width="1920"
     height="1080">
```

**Critical:**
- Add `fetchpriority="high"` to LCP image
- NO `loading="lazy"` on LCP image
- Use optimal format (WebP/AVIF with JPEG fallback)
- Include explicit dimensions

### 7. Async Decoding for Non-Critical Images

Prevent image decoding from blocking main thread:

```html
<img src="gallery-item-3.webp"
     alt="Finished basement renovation with home theater setup"
     width="600"
     height="400"
     loading="lazy"
     decoding="async">
```

Add `decoding="async"` to all images except LCP image.

### 8. CLS Prevention

**Every image needs dimensions to prevent layout shift:**

```html
<!-- ✅ GOOD - Explicit dimensions -->
<img src="author-photo.webp" width="80" height="80" alt="Sarah Chen, Content Director">

<!-- ✅ GOOD - CSS aspect ratio -->
<img src="featured-image.webp" style="aspect-ratio: 16/9" alt="...">

<!-- ✅ GOOD - Responsive with intrinsic aspect ratio -->
<img src="hero.webp"
     width="1200"
     height="675"
     style="width: 100%; height: auto;"
     alt="...">

<!-- ❌ BAD - No dimensions -->
<img src="blog-image.webp" alt="...">
```

**Flag all images without:**
- `width` and `height` attributes, OR
- CSS `aspect-ratio` property

### 9. SEO-Friendly File Naming — Reglas Completas

**Regla mnemónica: minúsculas · keywords · guiones · sin caracteres especiales · sin versiones**

#### Reglas obligatorias

| Regla | ✅ Correcto | ❌ Incorrecto |
|-------|-----------|-------------|
| **Solo minúsculas** | `inspection-software.webp` | `Inspection-Software.webp`, `INSPECTION.jpg` |
| **Guiones como separador** | `field-inspection-app.webp` | `field_inspection_app.webp`, `field inspection app.webp` |
| **Sin caracteres especiales** | `inspeccion-digital.webp` | `inspeccion#digital.webp`, `inspeccion&digital.webp` |
| **Sin tildes ni ñ** | `inspeccion-campo.webp` | `inspección-campo.webp`, `niño.webp` |
| **Sin espacios** | `oil-gas-inspection.webp` | `oil gas inspection.webp` |
| **Sin números de versión** | `homepage-hero.webp` | `homepage-hero-v2-final.webp` |
| **Sin fechas** | `inspection-checklist.webp` | `inspection-checklist-2024-01-15.webp` |
| **Sin nombres genéricos** | `digital-inspection-software-demo.webp` | `IMG_2847.jpg`, `image001.png`, `photo.jpg`, `screenshot.png` |
| **Sin nombres de cámara/pantalla** | `team-photo-office.webp` | `DSC_1234.jpg`, `Screen Shot 2025-04-01.png` |
| **Extensión en minúsculas** | `hero.webp` | `hero.WEBP`, `hero.JPG` |

#### Estructura recomendada del nombre

```
[keyword-principal]-[descriptor-adicional]-[contexto-si-aplica].[ext]

Ejemplos:
inspection-software-dashboard.webp         → producto + elemento visual
field-inspection-app-android.webp          → producto + variante
oil-gas-inspection-checklist.webp          → sector + tipo de contenido
visualogyx-team-office-barcelona.webp      → marca + contexto
digital-inspection-report-example.webp    → tipo de contenido + calificador
blog-hero-inspection-tips-2025.webp        → tipo de página + keyword
```

#### Longitud recomendada

- **Ideal:** 3-5 palabras separadas por guiones
- **Máximo:** 6-7 palabras (después pierde legibilidad y peso SEO)
- **Mínimo:** 2 palabras (nunca nombre de un solo término genérico)

#### Idioma del nombre

El nombre debe estar en el **mismo idioma que el contenido de la página**:
- Sitio en inglés → `field-inspection-software.webp`
- Sitio en español → `software-inspeccion-campo.webp`
- Sitio multiidioma → usar el idioma de la versión donde se sube primero, o versiones separadas por locale

#### Keywords en el nombre

- Incluir la **keyword objetivo de la página** donde se usa la imagen
- No repetir la misma keyword en todas las imágenes de la misma página (dilución)
- Para imágenes de producto: `[nombre-producto]-[color/variante/uso].webp`
- Para imágenes de blog: `[keyword-del-post]-[número-si-hay-varias].webp`

```
Página sobre: "inspection software for oil and gas"
Imágenes:
  oil-gas-inspection-software-dashboard.webp   ← imagen principal
  field-inspector-tablet-oilfield.webp         ← imagen de uso
  inspection-report-oil-gas-example.webp       ← imagen de contenido
  vlx-oil-gas-integration-chart.webp           ← diagrama
```

#### Casos especiales

| Caso | Nombre recomendado |
|------|------------------|
| Logo | `[marca]-logo.svg` / `[marca]-logo-white.svg` |
| Favicon | `favicon.ico` / `favicon-32x32.png` |
| OG Image | `og-[slug-pagina].webp` |
| Twitter Card | `twitter-card-[slug].webp` |
| Avatar/foto de autor | `[nombre-autor]-seo-specialist.webp` |
| Screenshot de UI | `[producto]-[feature]-screenshot.webp` |
| Infografía | `infografia-[tema-keyword].webp` |
- No special characters or spaces
- Extension matches actual format

**Examples:**

| ✅ Good | ❌ Bad |
|---------|--------|
| `blue-nike-running-shoes-mens.webp` | `IMG_1234.jpg` |
| `kitchen-remodel-before-after.webp` | `image-final-v2.png` |
| `seo-traffic-growth-chart-2024.webp` | `Screen Shot 2024-01-15.png` |
| `chocolate-cake-recipe-close-up.webp` | `DSC_0456.JPG` |

**Flag:**
- Generic names (IMG_, DSC_, image_, photo_)
- Uppercase extensions
- Dates or version numbers in filenames
- Spaces or special characters

### 10. CDN and Image Delivery Optimization

**Check for:**
- Images served from CDN (different domain)
- Cache headers (`Cache-Control: max-age=31536000, immutable` for hashed filenames)
- Automatic format negotiation (CDN serves AVIF to supporting browsers, WebP to others)

**Modern Image CDN providers (replace manual format conversion):**
| Provider | Key feature | Pricing |
|----------|------------|---------|
| Cloudflare Images | Automatic AVIF/WebP negotiation, resize, CDN | $5/month + per image |
| Cloudinary | Most features, transform-on-URL | Free tier + Paid |
| Imgix | Best for complex transforms, ecommerce | Paid |
| Fastly Image Optimizer | Edge-based, auto-format, auto-compress | Paid |

**Auto-format via CDN (eliminates manual `<picture>` srcset for format switching):**
```html
<!-- With Cloudinary: add f_auto,q_auto to URL -->
<img src="https://res.cloudinary.com/demo/image/upload/f_auto,q_auto/product.jpg" alt="...">

<!-- With Imgix: add auto=format -->
<img src="https://example.imgix.net/product.jpg?auto=format" alt="...">
```

**Compression quality targets by format:**
| Format | Photos quality | Graphics quality | Tool |
|--------|---------------|-----------------|------|
| WebP | 75-85 | 80-90 | cwebp, Squoosh, sharp |
| AVIF | 40-55 (different scale) | 60-70 | avifenc, sharp |
| JPEG | 75-85 | N/A | jpegoptim, mozjpeg |
| PNG | N/A | Use oxipng or pngquant (lossless) | oxipng, pngquant |

---

## 11. Visual Search Optimization (Google Lens / Circle to Search)

> Google Lens processes **12B+ queries/month** (+30% annually). Circle to Search is on **200M+ devices**. This is the fastest-growing image search surface, yet most sites don't optimize for it.

**How Lens works:** Computer vision — NOT alt text. Lens matches images based on visual content (shapes, colors, context, product characteristics), not HTML attributes.

### Product Images for Google Lens
| Requirement | Why | Implementation |
|-------------|-----|---------------|
| Clean background (white/neutral) | Lens isolates subject from background | Studio photos or background removal tools |
| Multiple angles (3+ views) | Increases recognition probability | Front, side, detail, in-use shots |
| High resolution (min 800px) | Low-res images lose visual detail | 1,200px+ recommended |
| Distinctive subject | Generic stock photos don't rank | Use product-specific, unique imagery |
| Consistent lighting | Flat/even lighting improves recognition | Avoid harsh shadows |

### February 2026 Gemini 3 Multi-Object Update
Google Lens can now recognize **multiple objects simultaneously** in a single image (Gemini 3 integration). Implications:
- Lifestyle/in-use product photos now trigger multi-object recognition
- Products shown in context (kitchen tool in kitchen) may rank for broader queries
- Optimize in-use photos to show product clearly alongside context objects

### Google Images as a Search Surface
Google Images has its own ranking signals beyond web search:
- **Image quality and relevance** — matches visual content to query
- **Page context** — the page title, surrounding text, and URL influence image ranking
- **Licensable badge** — requires `ImageObject` schema with `license` and `acquireLicensePage` properties
- **SafeSearch classification** — automatic, based on image content
- **Landing page quality** — a great image on a low-quality page ranks poorly

### Image Sitemap
Images not discoverable through normal crawling (JS-loaded, CDN-transformed, lazy-loaded) benefit from an image sitemap:
```xml
<url>
  <loc>https://example.com/product/red-chair</loc>
  <image:image>
    <image:loc>https://cdn.example.com/images/red-chair-front.webp</image:loc>
    <image:title>Red Ergonomic Office Chair — Front View</image:title>
    <image:caption>Red ergonomic office chair with lumbar support</image:caption>
  </image:image>
</url>
```

---

## 12. AI-Generated Image Requirements (IPTC — Mandatory)

Google requires IPTC DigitalSourceType metadata on ALL AI-generated images:
```bash
# Embed IPTC metadata with ExifTool
exiftool -IPTC:DigitalCreator="TrainedAlgorithmicMedia" image.jpg
```
- Required for: Google Images, Google Discover, Google Merchant Center
- Failure to include = risk of removal from Google surfaces
- See `seo-content-types` for full IPTC implementation guide

## Output Format

### Executive Summary

```
Image Audit Summary for: [URL/Page Name]
Analyzed: [XX] total images
Scan Date: [Date]

Critical Issues: X
Warnings: X
Estimated Savings: XXX KB (XX%)
```

### Metrics Dashboard

| Metric | Status | Count | Details |
|--------|--------|-------|---------|
| Total Images | ℹ️ | XX | - |
| Missing Alt Text | ❌ | XX | SEO issue |
| Poor Alt Quality | ⚠️ | XX | Generic/stuffed keywords |
| Oversized Images | ⚠️ | XX | > category threshold |
| Legacy Formats | ⚠️ | XX | JPEG/PNG → WebP/AVIF |
| Missing Dimensions | ❌ | XX | CLS risk |
| LCP Image Issues | ❌ | XX | Performance killer |
| Missing Lazy Loading | ⚠️ | XX | Below-fold images |
| Improper Lazy Loading | ❌ | XX | Above-fold images |
| No Responsive Images | ⚠️ | XX | Missing srcset |
| Poor File Names | ⚠️ | XX | Generic/non-descriptive |
| No CDN Usage | ⚠️ | XX | Delivery not optimized |

### Priority Optimization List

Sort by estimated impact (file size savings × page views):

| Priority | Image | Current | Issues | Recommendation | Est. Savings |
|----------|-------|---------|--------|----------------|--------------|
| 🔴 Critical | hero-banner.jpg (LCP) | 850KB JPEG | Lazy loaded, wrong format, oversized | Convert to WebP, add fetchpriority="high", remove lazy loading | 650KB + LCP improvement |
| 🔴 Critical | product-gallery-1.png | 1.2MB PNG | No dimensions, wrong format | Convert to WebP, add width/height | 900KB + prevent CLS |
| 🟡 High | testimonial-photos/*.jpg (12 images) | 200-400KB each | Legacy format, no lazy loading | Batch convert to WebP, add lazy loading | 2.1MB total |
| 🟡 High | blog-featured-*.jpg | 300-600KB JPEG | Missing srcset, oversized | Create responsive variants, optimize | 1.5MB total |
| 🟢 Medium | icon-*.png (24 images) | 15-50KB each | Could be SVG or smaller WebP | Convert to SVG or compress to WebP | 800KB total |

### Detailed Recommendations

**1. Critical: Fix LCP Image (Immediate Action)**

Current:
```html
<img src="hero-banner.jpg" loading="lazy" alt="Kitchen remodeling">
```

Optimized:
```html
<picture>
  <source srcset="hero-banner.avif" type="image/avif">
  <source srcset="hero-banner.webp" type="image/webp">
  <img src="hero-banner.jpg"
       fetchpriority="high"
       alt="Modern kitchen renovation with quartz countertops and stainless appliances"
       width="1920"
       height="1080">
</picture>
```

**Impact:** Improved LCP score, 76% smaller file (850KB → 200KB)

**2. Convert Legacy Formats (High Impact)**

- Convert XX JPEG images to WebP: ~XX% savings (~XXX KB)
- Convert XX PNG images to WebP: ~XX% savings (~XXX KB)
- Total estimated savings: XXX KB

**3. Implement Responsive Images**

Add `srcset` and `sizes` to XX large images:
- Prevents mobile users from downloading desktop-sized images
- Estimated mobile data savings: XXX KB per page load

**4. Fix Alt Text Issues**

XX images need alt text improvements:

| Image | Current Alt | Issue | Suggested Alt |
|-------|-------------|-------|---------------|
| product-1.jpg | "image" | Too generic | "Stainless steel espresso machine with built-in grinder" |
| team-photo.jpg | "our team teamwork collaboration" | Keyword stuffing | "Marketing team collaborating in conference room" |
| logo.png | "click here" | Not descriptive | "Acme Construction logo" |

**5. Add Lazy Loading**

XX below-fold images should lazy load:
```html
loading="lazy" decoding="async"
```

**6. Fix CLS Issues**

XX images missing dimensions:
```html
width="800" height="600"
```

**7. Rename Files**

XX images have poor filenames:
- `IMG_2847.jpg` → `customer-testimonial-sarah-chen.webp`
- `Screen-Shot-2024.png` → `quarterly-revenue-growth-chart.webp`

### Estimated Performance Impact

**Before optimization:**
- Total image weight: XXX KB
- LCP: X.X seconds
- CLS: 0.XX

**After optimization:**
- Total image weight: XXX KB (XX% reduction)
- Estimated LCP improvement: -X.X seconds
- Estimated CLS improvement: -0.XX

**Page Speed Impact:**
- Mobile: XX% faster
- Desktop: XX% faster
- First Contentful Paint: -XX ms
- Total Blocking Time: -XX ms

## Implementation Checklist

- [ ] Convert hero/LCP image to WebP/AVIF with `fetchpriority="high"`
- [ ] Remove `loading="lazy"` from above-fold images
- [ ] Convert remaining JPEGs/PNGs to WebP
- [ ] Add dimensions (width/height) to all images
- [ ] Implement `srcset` for images > 400px wide
- [ ] Add `loading="lazy"` to below-fold images
- [ ] Add `decoding="async"` to non-LCP images
- [ ] Fix all missing/poor alt text
- [ ] Nombres de archivo: minúsculas, guiones, keywords, sin tildes/ñ/especiales/versiones/fechas
- [ ] Configure CDN if not already in use
- [ ] **Texto quemado**: títulos, banners y CTAs usan HTML sobre imagen, no texto en la imagen
- [ ] **Infografías con texto**: tienen alt text completo o transcripción HTML debajo
- [ ] **Banners hero**: texto del heading es `<h1>/<h2>` HTML, la imagen es decorativa o background
- [ ] Set up automated image optimization pipeline

## Tools & Resources

**Recommended tools:**
- **Compression:** Squoosh.app, TinyPNG, ImageOptim
- **Format conversion:** cwebp (WebP), avifenc (AVIF)
- **Responsive images:** Cloudinary, Imgix, sharp (Node.js)
- **Testing:** Lighthouse, WebPageTest, Chrome DevTools
- **Bulk processing:** ImageMagick, sharp, Squoosh CLI

**Automation:**
- Set up build pipeline to auto-convert images
- Implement CDN with automatic format optimization
- Use responsive image generation in CMS
