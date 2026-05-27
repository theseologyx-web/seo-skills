<!-- Updated: 2026-04-03 -->
# SEO Playbooks Tácticos — Estrategias por Momento

## Qué es un playbook vs una situación

- **Situación** (`situations.md`): el estado diagnóstico del sitio — nuevo, estancado, en caída, penalizado, en migración, maduro.
- **Playbook**: la táctica que se elige ejecutar dado ese diagnóstico, los recursos disponibles y el objetivo inmediato.

Un mismo sitio puede cambiar de playbook cada trimestre. No son excluyentes — pueden combinarse. La clave es saber **cuándo aplicar cuál**.

---

## Playbook 1: Mangos Bajitos (Quick Wins)

**Cuándo aplicar:**
- GSC muestra keywords en posiciones 4-20 con ≥ 100 impresiones/mes
- El sitio tiene contenido existente que ya está indexado y parcialmente posicionado
- El cliente necesita resultados rápidos (semanas, no meses)

**Lógica:** Google ya considera esa página relevante para esa keyword. El esfuerzo para pasar de pos 11 a pos 4 es mínimo comparado con crear una página nueva que empiece desde cero.

**Acciones:**
1. Exportar GSC → Rendimiento → filtrar posición 4-20, ordenar por impresiones
2. Para cada URL prioritaria: mejorar title tag, H1, estructura de la página
3. Verificar que la intención de búsqueda del contenido coincide con la SERP actual
4. Añadir internal links desde páginas con autoridad hacia la página target
5. Revisar CTR: si está por debajo del benchmark → reescribir title/meta para mejorar CTR
6. Si la keyword tiene 0 mención en el cuerpo → añadirla de forma natural en los primeros 100 palabras

**Timeline:** 4-8 semanas para ver movimiento en posiciones.

**Lo que NO hacer:**
- Crear una página nueva para esa keyword → canibalización garantizada
- Reescribir el contenido completamente → puede perder señales de posicionamiento ganadas

---

## Playbook 2: Reoptimización de Contenido Existente (Content Refresh)

**Cuándo aplicar:**
- Páginas que tuvieron tráfico pero han bajado en los últimos 6-12 meses (sin causa técnica aparente)
- Contenido publicado hace > 12 meses en nichos que cambian (tech, finanzas, salud, marketing)
- Comparativas, "mejor X", how-tos con herramientas — tipos de contenido con alto decay rate
- Sitio con mucho contenido existente → antes de crear nuevo, sacar partido de lo que ya hay

**Lógica:** Actualizar una página con posicionamiento previo es 3-5x más rápido que crear una nueva y rankearla desde cero. Google ya tiene señales positivas de esa URL.

**Acciones:**
1. GSC → filtrar páginas con impresiones bajando los últimos 6 meses → priorizar por volumen perdido
2. Identificar el tipo de decay: ¿cambió la intención? ¿el contenido está desactualizado? ¿competidor mejoró?
3. Actualizar datos, estadísticas y ejemplos con información 2025-2026
4. Revisar la SERP actual: ¿qué cubre el top 3 que el artículo no cubre? → añadir esas secciones
5. Mejorar estructura (H2s, listas, tablas) si la SERP favorece ese formato
6. Actualizar la fecha de publicación SOLO si los cambios son sustanciales (no como truco)
7. Añadir párrafos citables para AI (respuestas directas y concretas ≥ 40 palabras)

**Timeline:** 2-6 semanas para ver impacto. Páginas con posicionamiento previo responden más rápido.

**Lo que NO hacer:**
- Cambiar la URL (perder el historial de indexación y los backlinks)
- Reescribir sin leer primero la SERP actual — riesgo de cambiar lo que sí funciona
- Actualizar la fecha sin actualizar el contenido — señal negativa para Google

---

## Playbook 3: Contenido Nuevo para Gaps de Keywords

**Cuándo aplicar:**
- Keyword research revela temas relevantes que la competencia cubre y el cliente no
- Clusters temáticos incompletos — hay volumen en subtemas sin página asignada
- La marca quiere atacar nuevas audiencias o etapas del funnel no cubiertas
- Sitio técnicamente sólido y con autoridad mínima (DA > 15) para que el contenido rankee

**Lógica:** Llenar los gaps que la competencia cubre es la forma más directa de capturar tráfico incremental. Si nadie en tu sitio habla de "X + caso de uso Y", ese tráfico va al competidor.

**Acciones:**
1. Competitive gap analysis: exportar keywords de competidores que el cliente no tiene → `/seo competitive`
2. Priorizar por: volumen × intención comercial × KD aceptable para el DA actual
3. Agrupar keywords relacionadas en clusters → una URL por cluster (no por keyword)
4. Decidir formato según SERP: si top 3 son listicles → listicle; si son guías largas → guía
5. Para B2B: priorizar keywords con intención de research/comparativa + BOFU antes que TOFU de alto volumen
6. Para B2C: priorizar transaccional + comercial antes que informacional puro
7. Internal links hacia la nueva URL desde páginas existentes con autoridad

**Timeline:** 3-6 meses para ver rankings en keywords de KD medio. Long-tail: 4-8 semanas.

**Lo que NO hacer:**
- Una página por keyword (canibalización + thin content)
- Crear contenido sin revisar primero si ya existe una página propia que pueda optimizarse
- Ignorar el formato que Google está priorizando en esa SERP

---

## Playbook 4: SEO Programático

**Cuándo aplicar:**
- Sitio con datos estructurados que pueden generar páginas a escala: ciudades, integraciones, comparativas, templates de producto, combinaciones de atributos
- El patrón "X para [variable]" o "X vs Y" tiene volumen repetible y datos únicos por página
- Ecommerce con catálogo grande, directorios, plataformas de comparación, SaaS con integraciones

**Lógica:** Si hay 500 ciudades donde tu servicio opera y cada ciudad tiene búsquedas propias → 500 páginas individuales bien hechas > 1 página genérica. El volumen total de long-tail supera al head term.

**Tipos de páginas programáticas habituales:**
| Patrón | Ejemplo | Sector |
|--------|---------|--------|
| Servicio + ciudad | "abogado laboralista en Madrid" | Local / SAB |
| Herramienta + integración | "HubSpot + Salesforce integration" | SaaS |
| Producto + atributo | "zapatillas running talla 42 hombre" | Ecommerce |
| X vs Y | "Notion vs Asana comparativa" | SaaS / Software |
| Precio de X | "precio reforma baño 2026" | Local / Construcción |
| Alternativas a X | "alternativas a Monday.com" | SaaS |
| Casos de uso | "CRM para inmobiliarias" | SaaS B2B |

**Requisito crítico — evitar thin content:**
- Cada página debe tener datos únicos y relevantes para esa variable (no solo cambiar el nombre de ciudad)
- Mínimo: datos locales reales, reviews, precios específicos, o contenido contextualizado por variable
- Si todas las páginas tienen el mismo texto con el nombre cambiado → penalización por thin content

**Herramientas:** `/seo programmatic` para análisis y planificación.

**Timeline:** Técnicamente rápido de implementar; rankings en 2-4 meses si las páginas tienen sustancia.

**Lo que NO hacer:**
- Generar páginas con contenido idéntico salvo el nombre de la variable
- Indexar todas las páginas sin criterio → crawl budget waste si hay miles

---

## Playbook 5: Topical Authority Build

**Cuándo aplicar:**
- Sitio con 1-3 artículos sobre un tema importante pero sin profundidad real
- Competidores que rankean mejor tienen 20+ artículos sobre el mismo cluster temático
- Keyword difficulty del cluster es alta → la autoridad temática puede compensar el DA bajo
- Sitio que quiere ser la referencia en su nicho (publisher, SaaS, consultora)

**Lógica:** Google premia la profundidad temática. Un sitio con 20 artículos sobre "email marketing para ecommerce" puede superar a uno con DA 40 que tiene 2 artículos genéricos sobre el tema.

**Estructura:**
```
Pillar page (guía completa sobre el tema principal)
├── Supporting post 1 (subtema A)
├── Supporting post 2 (subtema B)
├── Supporting post 3 (subtema C)
├── Supporting post 4 (subtema D)
└── ... (hasta cubrir el 85%+ del cluster)
```

**Acciones:**
1. Mapear el cluster completo: todas las keywords del tema y sus variantes
2. Identificar qué subtemas tiene el cliente, cuáles le faltan
3. Crear la pillar page si no existe (2,000-4,000 palabras, cubre todo el tema)
4. Publicar supporting content en orden de volumen/intención (primero las más buscadas)
5. Internal links bidireccionales: pillar → supporting + supporting → pillar
6. Asegurar que cada supporting post linkea también a los otros posts del cluster

**Timeline:** 6-12 meses para que el cluster consolide autoridad. Los primeros posts rankean en 3-4 meses; el efecto cluster se nota hacia el mes 6-8.

**Lo que NO hacer:**
- Publicar todos los artículos del cluster a la vez sin estructura de internal linking
- Crear un pillar page superficial que no cubre el tema realmente
- Ignorar el cannibalization check antes de crear — puede que ya existan artículos que compiten internamente

---

## Playbook 6: Sprint de Link Building

**Cuándo aplicar:**
- Técnico OK, contenido bien optimizado, pero las keywords no suben de posición 8-15
- DR/DA significativamente menor que los competidores que rankean en el top 3
- Lanzamiento de una página clave que necesita autoridad rápida (landing de producto, comparativa)
- Sitio nuevo que ya tiene contenido pero no tiene backlinks suficientes

**Lógica:** Si el contenido responde bien la intención pero no sube, el cuello de botella es la autoridad. Los links son el voto de confianza que desbloquea las posiciones.

**Tácticas según objetivo:**

| Táctica | Cuando usarla | Velocidad |
|---------|--------------|-----------|
| Digital PR (datos + encuestas) | Autoridad rápida de sitios de alto DA | 4-8 semanas |
| Guest posts en nichos relevantes | Autoridad + relevancia temática | 6-12 semanas |
| Broken link building | Esfuerzo moderado, alta relevancia | 4-8 semanas |
| Niche edits (link insertions) | Velocidad si hay relaciones previas | 2-4 semanas |
| Skyscraper | Cuando hay contenido mejor que la competencia | 8-12 semanas |

**Lo que NO hacer:**
- Links masivos de directorios genéricos → SpamBrain penaliza
- Comprar links de PBN o sitios de baja calidad
- Hacer sprint de links sin asegurarse que el contenido de destino es bueno

→ Ver `/seo link-building` para ejecución táctica completa.

---

## Playbook 7: E-E-A-T Boost

**Cuándo aplicar:**
- Sector YMYL (salud, finanzas, legal, seguridad) con bajas posiciones a pesar de buen técnico y contenido
- Contenido anónimo sin autor identificado
- Post-update de Google que castigó E-E-A-T (Helpful Content, Core Updates 2023-2026)
- Sitio que no proyecta credibilidad vs competidores que tienen credentials claras

**Acciones:**
1. Crear author pages con bio, credentials, redes sociales, publicaciones externas
2. Añadir "Revisado por" en artículos YMYL (si aplica: médico, abogado, CFA...)
3. Citar fuentes primarias y datos verificables (con enlaces)
4. Añadir datos propios: encuestas, estudios, casos de uso reales con métricas
5. About page sólida: quiénes somos, historia, equipo, clientes, certificaciones
6. Conseguir menciones externas en publicaciones del sector (PR + link building combinados)

**Timeline:** 3-6 meses. Google necesita tiempo para reindexar y revaluar.

---

## Playbook 8: GEO Sprint (AI Visibility)

**Cuándo aplicar:**
- Sector con alta prevalencia de AI Overviews (informacional, health, finance, B2B research)
- SAIV = 0% — la marca no aparece en ninguna respuesta de AI para su nicho
- Cliente que quiere ser la fuente citada por ChatGPT/Perplexity/Google AIO
- Sitio SaaS USA donde los buyers usan ChatGPT para research de soluciones

**Acciones:**
1. Verificar que GPTBot, ClaudeBot, PerplexityBot, OAI-SearchBot están en ALLOW en robots.txt
2. Crear/actualizar `llms.txt` declarando preferencias de AI consumption
3. Añadir párrafos citables: respuestas directas y completas (≥ 40 palabras, self-contained)
4. Implementar schema: Organization, FAQPage (si aplica), Article, HowTo
5. Crear/optimizar contenido de marca: ¿qué es [marca], [marca] alternativas, [marca] vs X
6. Conseguir menciones en sitios de alta autoridad que AI cita frecuentemente en el nicho
7. Baseline: medir SAIV inicial con Otterly.ai / Peec AI antes de empezar

**Timeline:** 2-4 meses para primeras citations. SAIV > 10% suele tardar 6-9 meses en nichos competitivos.

→ Ver `/seo geo` para ejecución táctica completa.

---

## Playbook 9: Curación de Contenidos

**Cuándo aplicar:**
- Sitio con mucho contenido publicado a lo largo del tiempo, parte del cual ya no genera tráfico
- Post-auditoría revela páginas thin, duplicadas, desactualizadas o sin intención clara
- El sitio está estancado a pesar de tener buena cantidad de contenido — señal de que la calidad media es baja
- Antes de una migración o rediseño: limpiar antes de mover

**Lógica:** Google evalúa la calidad media del sitio, no solo las páginas individuales. Un sitio con 500 páginas de las cuales 300 no tienen valor arrastra hacia abajo a las 200 buenas. La curación eleva la calidad media percibida del dominio entero.

**Proceso de auditoría de contenido:**

1. **Inventario completo:** exportar todas las URLs indexadas (GSC → Cobertura + Screaming Frog)
2. **Cruzar con métricas:** para cada URL obtener clics, impresiones, posición (GSC) + sesiones (GA4) en los últimos 12 meses
3. **Clasificar cada URL en una de estas categorías:**

| Categoría | Criterio | Acción |
|-----------|----------|--------|
| **Keeper** | Tráfico estable o creciendo, posición buena | Mantener, refrescar si tiene > 12 meses |
| **Quick win** | Posición 4-20, pocas impresiones | Optimizar (Playbook 1) |
| **Refresh candidate** | Tuvo tráfico, ahora bajando | Reoptimizar (Playbook 2) |
| **Consolidate** | Varias URLs cubriendo el mismo tema | Mergear en la URL más fuerte + 301 de las otras |
| **Noindex** | Páginas de proceso (gracias, confirmación, filtros) sin valor SEO | Añadir `noindex` |
| **Delete + 301** | Sin tráfico, sin backlinks, sin valor → contenido thin o irrelevante | Eliminar, redirigir a URL relevante |
| **Delete sin redirect** | Sin tráfico, sin backlinks, sin ningún valor | Eliminar, dejar 404 o redirigir a categoría padre |

4. **Orden de ejecución:**
   - Primero: noindex (cambio en meta robots, sin riesgo de perder tráfico)
   - Segundo: consolidaciones (mergear contenido, crear 301)
   - Tercero: eliminaciones con 301
   - Último: eliminaciones sin redirect (confirmar que no hay backlinks)

**Lo que NO hacer:**
- Eliminar URLs sin verificar primero si tienen backlinks (Ahrefs / GSC → Links)
- Borrar contenido con tráfico aunque sea poco — si trae visitas, primero intenta optimizarlo
- Hacer todo de golpe — Google necesita tiempo para reindexar los cambios; procesar por lotes

**Timeline:** Los beneficios de curación se ven en 2-4 meses. El crawl budget mejora inmediatamente; el impacto en rankings tarda más.

→ Ver `/seo content` para evaluación de calidad individual de páginas.

---

## Playbook 10: Reducción de Crawl Budget

**Cuándo aplicar:**
- Sitio grande (> 500 páginas) donde Googlebot no recrawlea las páginas importantes con suficiente frecuencia
- Logs del servidor muestran que el crawler gasta tiempo en páginas sin valor (parámetros URL, duplicados, páginas de facetas)
- GSC → Cobertura muestra muchas URLs en "Descubierta, no indexada" o "Rastreada, no indexada"
- Ecommerce o publisher con miles de URLs generadas automáticamente
- Sitio con paginación profunda, filtros de búsqueda o parámetros de sesión en URLs

**Lógica:** Google asigna a cada sitio un "crawl budget" — un límite de tiempo que dedicará a rastrear las URLs del dominio. Si ese presupuesto se desperdicia en páginas sin valor, las páginas importantes se crawlean con menos frecuencia o no se recrawlean. Liberar crawl budget = Google dedica más tiempo a lo que importa.

**Fuentes principales de crawl waste:**

| Problema | Origen | Solución |
|---------|--------|----------|
| Parámetros URL (filtros, sesiones, tracking) | Ecommerce, CMS | Canonical a URL limpia + GSC Parameter Handling |
| Páginas de facetas (color+talla, precio+marca) | Ecommerce | Noindex o bloqueo en robots.txt si no tienen valor SEO |
| Paginación profunda (página 50, 100...) | Blog, ecommerce | Noindex en páginas > 3-4, rel=next/prev si aplica |
| Thin content autogenerado (tags, autores con 1 post) | WordPress/CMS | Noindex en taxonomías vacías o de bajo valor |
| URLs de sesión/usuario | CMS mal configurado | Canonical + bloqueo en robots.txt |
| Versiones duplicadas (www/no-www, http/https) | Configuración | Redirect 301 canónico + canonical en `<head>` |
| Páginas de búsqueda interna (`?s=`) | CMS | Bloquear en robots.txt (`Disallow: /?s=`) |
| URLs de impresión (`?print=1`) | CMS/plugins | Noindex o bloquear en robots.txt |

**Acciones por orden de impacto:**

1. **Auditar logs** del servidor para ver qué URLs crawlea más Googlebot → `/seo logs`
2. **robots.txt:** bloquear URLs de búsqueda interna, parámetros de sesión, páginas sin valor
3. **Canonical tags:** en todas las páginas con parámetros → apuntar a URL limpia
4. **Noindex:** facetas, paginación profunda, tags vacíos, autores con < 3 posts
5. **Eliminar redirects en cadena:** redirect A → B → C consume crawl; simplificar a A → C
6. **Sitemap limpio:** solo incluir URLs que quieres que Google indexe (no todas las URLs del sitio)
7. **GSC → Configuración de URL → Parameter Handling:** indicar a Google qué parámetros ignorar

**Métricas para medir mejora:**
- Frecuencia de recrawl de páginas clave (GSC → Inspeccionar URL → ver "última vez rastreada")
- Reducción de "Rastreada, no indexada" en GSC → Cobertura
- Logs: ratio de URLs útiles / total de URLs crawleadas

**Lo que NO hacer:**
- Bloquear en robots.txt URLs que tienen backlinks (los links no pasarán autoridad si la URL está bloqueada)
- Noindex + bloquear en robots.txt la misma URL (Google no puede ver el noindex si no puede rastrear)
- Usar `noindex` en lugar de `canonical` para duplicados — canonical es más semánticamente correcto

→ Ver `/seo logs` para análisis completo de crawl budget y comportamiento de Googlebot.

---

## Matriz de selección de playbook

| Diagnóstico detectado | Playbook recomendado | Urgencia |
|----------------------|---------------------|----------|
| Keywords en pos 4-20 con volumen | Mangos bajitos | Alta |
| Contenido con tráfico cayendo | Content Refresh | Alta |
| Competidores cubren temas que yo no | Gaps de keywords | Media |
| Datos estructurados escalables | SEO Programático | Media |
| Cluster temático incompleto | Topical Authority | Media-baja |
| Técnico OK + contenido OK pero no sube | Sprint link building | Alta |
| YMYL sin E-E-A-T claro | E-E-A-T Boost | Alta (si YMYL) |
| SAIV = 0% en nicho con AIO alto | GEO Sprint | Media |
| Mucho contenido sin tráfico / calidad media baja | Curación de contenidos | Alta |
| Sitio > 500 páginas con crawl ineficiente | Reducción crawl budget | Alta |

## Combinaciones habituales

| Situación del sitio | Playbooks recomendados |
|--------------------|----------------------|
| Sitio nuevo | Gaps de keywords → Topical Authority |
| Sitio estancado | Mangos bajitos + Content Refresh en paralelo |
| Caída post-update | Content Refresh + E-E-A-T Boost |
| Maduro competitivo | Topical Authority + GEO Sprint + Link Building |
| SaaS B2B con poco tráfico | Gaps de keywords (BOFU primero) + GEO Sprint |
| Ecommerce con catálogo grande | SEO Programático + Mangos bajitos en categorías |
