---
name: seo-link-building
description: >
  Proactive link acquisition strategy and execution. Covers guest posting (full workflow),
  Digital PR, HARO/Connectively, broken link building, skyscraper technique, niche edits,
  resource page outreach, outreach email sequences, link velocity, and campaign tracking.
  Use when user says "link building", "conseguir backlinks", "guest post", "outreach",
  "HARO", "digital PR", "broken link", "niche edit", "conseguir enlaces", or "link acquisition".
user-invokable: true
argument-hint: "[dominio o nicho] [--tactic guest-post|digital-pr|broken-link|skyscraper|niche-edit|resource-page]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# Link Building — Adquisición Proactiva de Backlinks

> **Skill relacionado:** `seo-backlinks` cubre el análisis del perfil existente (radiografía actual, tóxicos, anchor text, gap analysis). Este skill (`seo-link-building`) se enfoca en la adquisición proactiva — outreach, guest post, digital PR, broken link building. Úsalos en orden: primero `seo-backlinks` para entender el estado actual, luego `seo-link-building` para planificar la adquisición.

Estrategias y workflows para conseguir backlinks de calidad. El objetivo no es volumen sino autoridad, relevancia topical y naturalidad del perfil.

> Para **analizar** el perfil de backlinks existente: usar `seo-backlinks`.
> Este skill se enfoca en **conseguir** nuevos enlaces.

---

## Principios de Link Building 2025-2026

| Principio | Por qué importa |
|-----------|----------------|
| **Relevancia topical > Autoridad** | Un DR 40 de un sitio de tu nicho vale más que un DR 80 de un sitio irrelevante |
| **Editorial > Paid** | Google penaliza links pagados sin `rel="sponsored"`. Los links ganados editorialmente no tienen riesgo |
| **Velocidad natural** | 5-10 links nuevos/mes en un sitio nuevo es natural. 500 links de golpe = señal de spam |
| **Diversidad de fuentes** | Un solo dominio con 50 links = mínimo valor. 50 dominios con 1 link cada uno = mucho más valor |
| **Anchor text natural** | Máximo 10% exact-match. El 70%+ debe ser branded, URL, o genérico |

---

## Mapa de Tácticas por ROI y Esfuerzo

| Táctica | ROI | Esfuerzo | Tiempo | Mejor para |
|---------|-----|---------|--------|-----------|
| Reclamar menciones sin link | ⭐⭐⭐⭐⭐ | Bajo | 1-3 días | Marcas con presencia existente |
| Broken link building | ⭐⭐⭐⭐ | Medio | 1-2 semanas | Cualquier nicho con contenido competidor antiguo |
| Resource page links | ⭐⭐⭐⭐ | Medio | 1-2 semanas | Nichos con directorios y listas de recursos |
| Guest posting | ⭐⭐⭐ | Alto | 2-6 semanas/link | Nichos con blogs activos que aceptan contribuciones |
| Digital PR / HARO | ⭐⭐⭐⭐⭐ | Alto | Variable | Marcas con datos originales o expertos citables |
| Skyscraper | ⭐⭐⭐ | Muy alto | 4-8 semanas | Contenido que puede mejorarse significativamente |
| Niche edits | ⭐⭐⭐ | Bajo | 1 semana | Links contextuales en contenido existente |
| Link partnerships | ⭐⭐ | Bajo | — | Con cuidado: reciprocal links = señal spam |

---

## 1. Reclamar Menciones Sin Link (Quick Win)

La táctica con mejor ROI. La marca ya está mencionada — solo falta el link.

### Proceso
```bash
# 1. Buscar menciones sin link en Google
"nombre de marca" -site:tudominio.com

# 2. Buscar variantes del nombre
"Nombre Empresa" OR "NombreEmpresa" OR "nombre-empresa" -site:tudominio.com

# 3. Verificar si tienen link
curl -sL [URL_mencionando] | grep -i "tudominio.com"
# Si no aparece → no tiene link → oportunidad
```

### Email de reclamación (alta conversión)
```
Asunto: [Nombre de marca] — pequeña corrección en tu artículo

Hola [Nombre],

Encontré tu artículo "[título del artículo]" y me alegró ver que mencionas [Nombre de marca].

Vi que el nombre no está linkeado al sitio. ¿Podrías añadir el link a [URL específica]?
Facilita que tus lectores encuentren el recurso directamente.

Gracias,
[Tu nombre]
```

**Tasa de respuesta esperada:** 15-30% (la más alta de todas las tácticas porque no pides nada nuevo).

---

## 2. Guest Posting — Workflow Completo

> **Advertencia crítica (Oct 2025):** Google actualizó SpamBrain para detectar específicamente granjas de guest posts generados por IA sin supervisión editorial. Si usas IA para escribir guest posts en volumen sin revisión humana y sin aportar perspectiva real, el site receptor puede recibir una acción manual. Regla: el guest post debe leerlo y aprobarlo una persona real antes de publicarlo. La IA puede asistir, no reemplazar la voz y criterio del autor.

### Paso 1: Prospección (encontrar targets)

**Búsquedas en Google:**
```
[nicho] "escribe para nosotros"
[nicho] "guest post guidelines"
[nicho] "submit a guest post"
[nicho] "contributor guidelines"
[nicho] "write for us"
[nicho] intitle:"guest post by"
```

**Criterios de calificación del target:**

| Criterio | Mínimo | Óptimo | Descartar si |
|----------|--------|--------|-------------|
| Domain Rating (Ahrefs) / DA (Moz) | DR 20 | DR 40+ | DR < 15 |
| Tráfico orgánico estimado | 1,000 visitas/mes | 10,000+ | < 500 visitas/mes |
| Relevancia topical | Mismo nicho o relacionado | Misma audiencia exacta | Completamente irrelevante |
| Últimos posts publicados | < 6 meses | < 2 meses | Sin actividad 1+ año |
| Editorial standards | Editan el contenido | Tienen editor asignado | Publican todo sin filtro |
| Links dofollow en posts | Sí | Sí, contextuales | Solo nofollow en byline |

### Paso 2: Encontrar el contacto correcto

```bash
# Buscar email del editor
# Patrones comunes:
# editor@dominio.com
# contribute@dominio.com
# [nombre]@dominio.com

# Herramientas: Hunter.io (gratis 25 búsquedas/mes), Apollo, RocketReach
# Manual: página About/Team + LinkedIn del site
```

### Paso 3: Pitch inicial

**Regla crítica:** El pitch es sobre ELLOS, no sobre ti.

```
Asunto: Idea de artículo para [Nombre del Blog]: [Título propuesto]

Hola [Nombre],

Soy [Tu nombre], [breve credencial de 1 línea].

He leído [artículo específico de su blog] y noté que [observación relevante].
Creo que a tu audiencia le interesaría un artículo sobre [tema] porque [razón específica].

Aquí van 3 ángulos que podría desarrollar:
1. [Título 1] — [1 línea de descripción]
2. [Título 2] — [1 línea de descripción]
3. [Título 3] — [1 línea de descripción]

¿Alguno encaja con lo que buscas publicar próximamente?

[Tu nombre]
[Link a trabajos publicados anteriores o portfolio]
```

**Lo que NO hacer en el pitch:**
- No mencionar el link en el primer email
- No decir "esto beneficiará a tu SEO"
- No usar plantillas genéricas sin personalización
- No adjuntar el artículo en el primer contacto
- No enviar desde dominio de correo genérico (gmail/hotmail)

### Paso 4: Escribir el artículo

**Estándares de calidad para guest posts:**

| Elemento | Requisito |
|----------|-----------|
| Longitud | 1,000-2,000 palabras (salvo que pidan otra cosa) |
| Formato | H2/H3, bullets, imágenes, sin keyword stuffing |
| Enlace propio | 1-2 links máximo, contextuales, no en el primer párrafo |
| Anchor text | Branded o partial-match (NUNCA exact-match) |
| Author bio | Breve, con link a homepage o página de recursos |
| Valor real | El artículo debe publicarse sin el link y seguir siendo útil |

### Paso 5: Seguimiento post-publicación

```
Checklist post-publicación:
- [ ] El link es dofollow (verificar con: curl -sL URL | grep tudominio)
- [ ] El anchor text es el acordado
- [ ] El artículo está indexado (site:dominio.com "título del artículo")
- [ ] Registrado en hoja de seguimiento (ver sección Tracking)
```

---

## 3. Digital PR — Links Editoriales de Alta Autoridad

Links desde medios, periódicos digitales, y blogs de autoridad editorial. El tipo de link más valioso.

### HARO / Fuentes para periodistas (Help A Reporter Out)

Periodistas buscan fuentes expertas para sus artículos. Tú respondes y te citan con link.

> **Actualización 2025:** Connectively cerró el 9 de diciembre de 2024. HARO fue adquirido por Featured.com y relanzado en abril 2025 con filtros anti-spam y quality scoring de pitches.

**Fuentes activas:**
- **Featured.com** (nuevo HARO): featured.com — registrarse como experto
- **Source of Sources (SOS)**: creado por Peter Shankman (fundador original de HARO). Honor system, 3 emails/día, completamente gratis.
- **Qwoted**: qwoted.com
- **SourceBottle**: sourcebottle.com — fuerte fuera de USA
- **ProfNet** (pago): profnet.com
- **#JournoRequest** (Twitter/X): buscar #JournoRequest en X

**Proceso HARO:**
1. Suscribirse a emails de HARO (3 emails/día: mañana, tarde, noche)
2. Filtrar por categoría relevante al nicho
3. Responder RÁPIDO (< 2 horas — los periodistas usan las primeras respuestas)

**Plantilla de respuesta HARO:**

```
HARO RESPONSE — [Nombre de marca]

[Respuesta concisa y directa a la pregunta — 2-3 párrafos máximo]

Nombre completo: [Nombre]
Cargo / Empresa: [Cargo] en [Empresa]
URL para citar: [URL]
Email de contacto: [Email]
Foto (si aplica): [URL de foto de perfil profesional]
```

**Claves para que te elijan:**
- Responder en las primeras 2 horas
- Dar una perspectiva única (no opinión genérica)
- Incluir datos o ejemplos específicos
- Ser conciso — los periodistas no leen textos largos

### Estudios y Datos Originales (link bait)

Contenido con datos originales = fuente citable por periodistas y bloggers.

| Tipo de contenido | Cómo conseguir los datos | Dónde distribuir |
|------------------|------------------------|-----------------|
| Encuesta sectorial | Google Forms a tu lista email / LinkedIn | HARO como fuente + outreach directo a medios |
| Análisis de datos públicos | APIs públicas, datasets gobierno, GSC | Enviar a bloggers del nicho |
| Índice o ranking | Tu propia metodología | Press release + outreach medios especializados |
| Estudio de caso con números | Datos de tu propio producto/clientes | Guest post en publicaciones sectoriales |

### Press Release (comunicado de prensa)

Para lanzamientos, estudios, partnerships, premios.

```bash
# Distribuidoras gratuitas de press releases (DA alto):
# PRLog: prlog.org
# OpenPR: openpr.com
# PR.com: pr.com

# Distribuidoras de pago (links de mayor valor):
# PR Newswire, Business Wire, GlobeNewswire
```

---

## 4. Broken Link Building

### Proceso completo

**Paso 1: Encontrar páginas con links rotos en el nicho**
```bash
# Buscar páginas de recursos en tu nicho
"[nicho]" "recursos" OR "links útiles" OR "herramientas recomendadas"

# Una vez en la página, verificar links rotos con:
curl -o /dev/null -s -w "%{http_code}" [URL_del_link]
# 404 → link roto → oportunidad
```

**Paso 2: Verificar que tienes contenido alternativo**
El contenido que ofreces debe ser:
- Sobre el mismo tema que la página 404 eliminada
- De igual o mayor calidad que el original
- Accesible sin registro

**Paso 3: Contactar al webmaster**

```
Asunto: Link roto en tu página "[título de la página]"

Hola [Nombre],

Estaba revisando tu artículo "[título]" y noté que el link que apunta a
[URL rota] ya no funciona (devuelve 404).

Tenemos un recurso sobre [tema] que podría reemplazarlo:
[Tu URL]

¿Te sería útil actualizar el link? No hay obligación, solo pensé que te interesaría saberlo.

[Tu nombre]
```

**Clave:** Primero reportas el error (sin pedir nada), luego ofreces tu recurso.

---

## 5. Skyscraper Technique

Crear contenido mejor que el que ya recibe links, luego contactar a quien linkea al original.

> **Contexto 2026:** La técnica sigue funcionando (61% de SEOs confirman resultados positivos), pero la conversión bajó de ~11% a 3-5%. Los webmasters reconocen el formato y son más selectivos. "Más largo y bonito" ya no es suficiente — el contenido debe aportar valor genuinamente diferente. Usa el **método DUDO**: **D**atos propios, **U**nicidad real, **D**iseño superior, **O**bjetivo del usuario resuelto mejor.

### Proceso

**Paso 1: Encontrar contenido link-worthy en tu nicho**
```bash
# Via Ahrefs: Site Explorer → Top Pages → filtrar por backlinks
# Via BuzzSumo: contenido más compartido por tema
# Manual: búsqueda "best [tema]" + ver qué rankea con muchos links
```

**Paso 2: Crear contenido genuinamente diferente (método DUDO)**

| Lo que hacía el original | Lo que hace tu versión con DUDO |
|------------------------|---------------------------|
| 10 ejemplos con descripción | **Datos propios**: encuesta original, análisis de 1000+ casos reales |
| Artículo de texto plano | **Unicidad**: perspectiva que no existe en ningún otro artículo |
| Solo desktop | **Diseño**: herramienta interactiva, comparador, calculadora |
| Información genérica | **Objetivo del usuario**: cubre exactamente la intent + qué hacer después |

> Evitar: simplemente añadir más ejemplos, más palabras, o mejor diseño sin aportar datos o perspectiva nueva. Los webmasters ya detectan este patrón.

**Paso 3: Outreach a quien linkea el original**

```bash
# Exportar quién linkea al contenido original (Ahrefs backlinks del URL original)
# Filtrar: dofollow + DR > 20 + relevantes
```

```
Asunto: Actualización del recurso que enlazas en "[título de su artículo]"

Hola [Nombre],

Vi que en tu artículo "[título]" tienes un link a [URL original].
El artículo original tiene [problema específico: datos de 2019, links rotos, sin mobile].

Acabo de publicar [Tu URL] que cubre el mismo tema con [mejora específica].

¿Considerarías actualizar el link? Creo que tus lectores lo encontrarán más útil.

[Tu nombre]
```

---

## 6. Resource Page Link Building

### Encontrar resource pages

```bash
# Búsquedas específicas
intitle:"recursos de [nicho]" OR intitle:"links útiles [nicho]"
"[nicho]" inurl:recursos OR inurl:resources OR inurl:links
"[nicho]" "sitios recomendados" OR "herramientas recomendadas"
```

### Criterios de calidad de la resource page
- La página está activamente mantenida (links funcionando, posts recientes)
- Tu contenido encaja temáticamente con los recursos listados
- La página tiene un número razonable de links (< 100 — las listas de 500+ son spam)

### Pitch para resource pages

```
Asunto: Sugerencia de recurso para tu página de [tema]

Hola [Nombre],

Encontré tu página de recursos sobre [tema] y la encuentro muy útil.

Noté que no incluye [tipo de recurso que tú tienes]. Tenemos [Tu URL]
que cubre [qué hace y para quién sirve].

¿Lo considerarías añadir si encaja con lo que buscas?

[Tu nombre]
```

---

## 7. Niche Edits (Link Insertions)

Añadir un link contextual a un artículo publicado existente (no un artículo nuevo como guest post).

**Diferencia con guest post:** El artículo ya existe y tiene autoridad acumulada. El link en un artículo antiguo con tráfico suele ser más valioso que en un guest post nuevo.

**Proceso legítimo (sin pagar):**
1. Encontrar artículos del nicho que mencionan tu tema pero no te linkean
2. Ofrecer actualización del artículo con tu recurso añadido como valor

**Cómo encontrar targets:**
```bash
# Artículos que mencionan tu marca/producto sin linkear
"[nombre marca]" -site:tudominio.com

# Artículos sobre tu tema que podrías complementar
"[palabra clave principal]" site:[blog de nicho]
```

**⚠️ Niche edits pagadas** son técnicamente link schemes según Google. Si se detectan como patrón, pueden resultar en penalización manual. Usar con precaución y siempre añadir valor editorial real.

---

## 8. Outreach — Secuencia de Seguimiento

### Cadencia óptima (sin ser spam)

```
Email 1 (Día 0): Pitch inicial
Email 2 (Día 5): Seguimiento breve — "¿llegó mi email?"
Email 3 (Día 12): Seguimiento final — con ángulo diferente o recurso adicional
[Si no hay respuesta → archivar, no continuar]
```

**Email de seguimiento (Día 5):**
```
Asunto: Re: [asunto original]

Hola [Nombre],

Solo quería asegurarme de que mi email anterior llegó.

[Una línea resumiendo el valor que ofreces]

¿Tienes interés? Si no es momento adecuado, no hay problema.

[Tu nombre]
```

**Email de seguimiento final (Día 12):**
```
Asunto: Última consulta — [asunto original]

Hola [Nombre],

Te escribo por última vez sobre [tema].

Adicionalmente, [nuevo ángulo o recurso complementario que no mencionaste antes].

Si en algún momento resulta relevante: [Tu URL]

¡Saludos!
[Tu nombre]
```

---

## 9. Link Velocity — Ritmo Natural

Construir links demasiado rápido es tan peligroso como no construirlos.

### Velocidad natural por tipo de sitio

| Tipo de sitio | Links/mes seguros | Links/mes máximo | Señal de alarma |
|---------------|-----------------|-----------------|----------------|
| Sitio nuevo (< 6 meses) | 5-15 | 30 | > 50 de golpe |
| Sitio establecido (1-3 años) | 15-50 | 100 | > 200 de golpe |
| Sitio autoridad (3+ años) | 30-100 | 300 | > 500 de golpe |

### Diversificación del perfil (objetivo mensual)

| Tipo de link | % del total mensual |
|-------------|-------------------|
| Guest posts | 30-40% |
| Digital PR / editoriales | 20-30% |
| Directorios/citations relevantes | 15-20% |
| Broken link / resource page | 15-20% |
| Social / foros | 5-10% (nofollow, diversificación) |

---

## 10. Tracking de Campañas de Link Building

### Hoja de seguimiento (estructura mínima)

| Campo | Descripción |
|-------|-------------|
| Fecha | Cuándo se publicó el link |
| Dominio linking | URL del dominio que linkea |
| URL linking | URL específica del artículo |
| DR / DA | Autoridad del dominio al momento del link |
| Anchor text | Texto del enlace |
| URL de destino | Página del sitio destino |
| Táctica | Guest post / Digital PR / Broken link / etc. |
| Dofollow / Nofollow | Estado del link |
| UTM | Parámetros UTM añadidos a la URL destino |
| Tráfico enviado | Visitas desde ese link (GA4, 30 días) |
| Estado | Activo / Perdido / Redirigido |

### Monitoreo mensual de links activos

```bash
# Verificar que un link sigue activo y dofollow
curl -sL [URL_del_articulo] | grep "tudominio.com"

# Verificar estado HTTP de la URL linking
curl -o /dev/null -s -w "%{http_code}" [URL_del_articulo]
```

---

## 11. Lo Que NUNCA Hacer (Penalizaciones)

| Táctica | Riesgo | Consecuencia |
|---------|--------|-------------|
| Comprar links dofollow sin `rel="sponsored"` | Muy alto | Manual action — Google Search Console |
| PBN (Private Blog Network) | Muy alto | Penalización masiva del perfil |
| Links de footer sitewide | Alto | Pattern detectado por SpamBrain |
| Comment spam | Medio | Links ignorados + reputación negativa |
| Intercambio excesivo de links | Medio | Penguin risk |
| Servicios de "1000 links por $5" | Extremo | Penalización inmediata |
| Anchor text 100% exact-match | Alto | Penguin — over-optimization penalty |

---

## Output Format

### Plan de Link Building: [Dominio]

**Perfil actual:** [X RDs, DR X] ← datos de seo-backlinks
**Objetivo 90 días:** [+X RDs, DR objetivo]
**Industria:** [Nicho]

#### Análisis de Oportunidades

| Táctica | Oportunidades identificadas | Links potenciales/mes | Prioridad |
|---------|---------------------------|----------------------|-----------|
| Menciones sin link | X | X-X | 🔴 Primero |
| Resource pages | X targets | X-X | 🟡 |
| Broken links | X links rotos | X-X | 🟡 |
| Guest posts | X sitios calificados | X-X | 🟢 |
| Digital PR | X ángulos de dato | X-X | 🟢 |

#### Targets por Táctica

**Guest Posts identificados:**
| Sitio | DR | Tráfico est. | Contacto | Estado |
|-------|-----|------------|---------|--------|
| [dominio] | X | X | [email] | Por contactar |

#### Plan de Acción 30/60/90 días

**Días 1-30:** [Tácticas quick win: menciones, resource pages]
**Días 31-60:** [Guest posts en proceso + HARO activo]
**Días 61-90:** [Skyscraper si aplica + review de resultados]

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Analizar perfil existente | `/seo backlinks [dominio]` | Ver desde dónde empezamos |
| Competidor con más links | `/seo competitive [dominio]` | Identificar sus fuentes de links para replicar |
| Trackear tráfico de links | `/seo utm [url]` | UTMs para medir qué links envían tráfico real |
| Reportar a cliente | `/seo reporting [dominio]` | Integrar métricas de link building en reporte |
| Benchmarks de RDs por industria | `/seo benchmark [dominio]` | Saber cuántos RDs necesitas para tu sector |
