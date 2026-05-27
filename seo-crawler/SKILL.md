---
name: cloudflare-crawl
description: >
  Descarga el HTML renderizado por Cloudflare de una o varias URLs.
  Esto representa exactamente lo que Google lee de un sitio web.
  Usar cuando el usuario quiera descargar páginas, crawlear un sitio,
  ver cómo ve Google una URL, o recopilar HTML de múltiples páginas.
---

# Cloudflare Crawler

> **Nota de arquitectura:** Este skill es un helper de captura de HTML — no es análisis SEO estratégico. Para crawl completo del sitio con mapeo de URLs usar `seo-firecrawl` (superior y más completo). Este skill se usa cuando se necesita el HTML exacto que ve Google de una URL específica via Cloudflare Browser Rendering.

## Al invocar este skill

Pregunta siempre al inicio:

> ¿Quieres descargar **1 URL** o **buscar y descargar varias**?

---

## Opción A — 1 URL

1. Pide la URL
2. Usa el endpoint `/content` de Cloudflare Browser Rendering para obtener el HTML renderizado
3. Guarda el resultado como archivo `.html` en `crawler/output/`
4. El archivo guardado es el HTML que Google realmente lee

### Cómo descargar 1 URL

```bash
curl -X POST "https://api.cloudflare.com/client/v4/accounts/{account_id}/browser-rendering/content" \
  -H "Authorization: Bearer {api_token}" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com", "rejectResourceTypes": ["image", "media", "font", "stylesheet"]}'
```

- Usa `render: true` (por defecto) para que Cloudflare ejecute JavaScript con Chrome headless antes de devolver el HTML — esto es exactamente lo que Google Caffeine indexa
- `rejectResourceTypes` excluye imágenes, fuentes y CSS para acelerar la descarga sin perder contenido indexable
- Guarda siempre como `.html`, nunca como `.md`

---

## Opción B — Buscar y descargar varias

1. Pide la URL de inicio (o patrón de URLs)
2. Pregunta cuántas páginas como máximo (default: 10)
3. Usa el endpoint `/crawl` con el flujo async: POST → poll → GET resultados
4. Muestra un resumen con todas las URLs descargadas

### Cómo crawlear varias URLs

Guarda cada página como `{slug}.html` en `crawler/output/`.

```bash
# 1. Iniciar el crawl
curl -X POST "https://api.cloudflare.com/client/v4/accounts/{account_id}/browser-rendering/crawl" \
  -H "Authorization: Bearer {api_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "limit": 10,
    "formats": ["html"],
    "render": true,
    "rejectResourceTypes": ["image", "media", "font", "stylesheet"]
  }'

# 2. Obtener el job_id de la respuesta y hacer polling
curl "https://api.cloudflare.com/client/v4/accounts/{account_id}/browser-rendering/crawl/{job_id}?limit=1" \
  -H "Authorization: Bearer {api_token}"

# 3. Cuando status = "completed", obtener todos los resultados
curl "https://api.cloudflare.com/client/v4/accounts/{account_id}/browser-rendering/crawl/{job_id}" \
  -H "Authorization: Bearer {api_token}"
```

El polling se repite cada 5 segundos hasta que `status` cambie de `"running"` a `"completed"`.

---

## Qué devuelve Cloudflare

Cloudflare ejecuta el JS de la página con Chrome headless y devuelve el **HTML final del DOM** — el mismo HTML que Google Caffeine recibe al indexar. Esto incluye:

- Contenido generado por JavaScript (React, Vue, etc.)
- Todo el `<head>`: meta description, canonical, OG tags, hreflang, structured data
- Texto visible en el DOM después del render
- Links presentes en el DOM final

**No incluye**: lo que está en `<noscript>`, iframes externos, ni contenido detrás de login/captchas.

**Formato de salida**: siempre `.html`. Nunca usar markdown — el HTML es la fuente de verdad.

**Importante — AI bots vs Googlebot:** El HTML que devuelve Cloudflare representa lo que ve un crawler con rendering completo de JS (como Googlebot Caffeine). Los AI bots (ClaudeBot, GPTBot, CCBot) en general **no ejecutan JavaScript** — solo descargan el HTML raw. Si el sitio es CSR puro, los AI bots ven una página casi vacía mientras Googlebot ve el contenido completo.

---

## Herramientas alternativas para comparar rendering

Cuando no hay acceso a Cloudflare Browser Rendering o se necesita validar lo que ve Googlebot específicamente:

| Herramienta | Qué muestra | Acceso |
|------------|-------------|--------|
| **Google URL Inspection API** (GSC) | Exactamente lo que Googlebot renderizó en su última visita | GSC → URL Inspection → Ver página renderizada |
| **Puppeteer / Playwright** | HTML renderizado por Chromium headless local | CLI/script local |
| **WebPageTest** | Screenshot + DOM después de render + filmstrip | Free online |
| **rendertron** | Proxy SSR open source (Google) | Self-hosted |

```bash
# URL Inspection API — ver rendered HTML via GSC API
# Requiere OAuth 2.0 con scope: https://www.googleapis.com/auth/webmasters.readonly
curl -X POST "https://searchconsole.googleapis.com/v1/urlInspection/index:inspect" \
  -H "Authorization: Bearer {oauth_token}" \
  -H "Content-Type: application/json" \
  -d '{"inspectionUrl": "https://example.com/page", "siteUrl": "https://example.com/"}'
# Response incluye: indexingState, crawledAs, lastCrawlTime, robotsTxtState, canonicalUrl
```

## Extracción de structured data del HTML renderizado

Después de descargar el HTML, extraer JSON-LD para auditar schema markup:
```bash
# Extraer todos los bloques JSON-LD del HTML descargado
grep -oP '(?<=<script type="application/ld\+json">)[\s\S]*?(?=</script>)' output/page.html | python3 -m json.tool

# O con Python para múltiples archivos
python3 -c "
import re, json, glob
for f in glob.glob('crawler/output/*.html'):
    html = open(f).read()
    schemas = re.findall(r'<script type=\"application/ld\+json\">(.*?)</script>', html, re.DOTALL)
    for s in schemas:
        try: print(f, json.loads(s).get('@type'))
        except: print(f, 'INVALID JSON-LD')
"
```

## Buenas prácticas de crawling (rate limiting)

Para no saturar el servidor al crawlear varias páginas:
- Usar `limit` en Opción B para no exceder 10-20 páginas en un solo crawl de auditoría
- Espaciar crawls en el tiempo (el plan gratuito tiene 5 crawls/día de todas formas)
- Si el sitio tiene WAF propio, crear una regla de excepción para la IP de Cloudflare Browser Rendering antes de crawlear

---

## Credenciales necesarias

El usuario debe tener:
- `account_id` de Cloudflare
- API token con permiso **"Browser Rendering – Edit"**

Si no los tiene, pide que los obtengan desde el dashboard de Cloudflare → My Profile → API Tokens.

---

## Límites importantes

- Plan gratuito: 5 crawls/día, máx 100 páginas por crawl
- Plan de pago ($5/mes): crawls ilimitados, hasta 100,000 páginas
- El crawler **no** puede bypassear protección bot de Cloudflare (Turnstile, WAF). Para crawlear tu propio sitio protegido, crear una regla WAF skip rule.
