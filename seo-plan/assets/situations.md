<!-- Updated: 2026-04-02 -->
# SEO Strategy by Site Situation

## Por qué la situación importa más que el tipo de negocio

El tipo de negocio (SaaS, local, ecommerce...) define la arquitectura y los contenidos.
La situación del sitio define el orden de las acciones y las prioridades estratégicas.

Un SaaS con penalización activa no puede hacer lo mismo que un SaaS nuevo. La situación va primero.

---

## Matriz de situaciones

### 1. Sitio nuevo (0 tráfico, recién lanzado o por lanzar)

**Señales:**
- Sin datos en GSC o GSC recién conectado
- DA = 0 o muy bajo (< 5)
- Pocas o ninguna página indexada

**Enfoque estratégico:**
- Fundación técnica primero: indexación limpia, CWV pasando, sitemap enviado
- Arquitectura y URL structure antes de crear contenido
- Atacar long-tail y keywords de baja dificultad (D < 30) en primeros 6 meses
- Construir páginas core primero (home, servicios/productos, contacto) antes de blog
- El blog no es prioridad hasta que las páginas principales estén optimizadas
- Primeros backlinks: directorios de nicho, menciones de marca, guest posts básicos

**KPI principal:** páginas indexadas + primeras posiciones en long-tail

**Lo que NO hacer:**
- Ir a por head terms con D > 50 desde el inicio
- Publicar mucho contenido fino para "llenar" el sitio
- Crear estructura de blog antes de tener las páginas de conversión listas

---

### 2. Sitio estancado (tráfico plano sin crecer)

**Señales:**
- Tráfico plano en GSC los últimos 3-6 meses
- Keywords en posiciones 5-20 sin moverse
- Sin penalización visible, sin caída brusca

**Enfoque estratégico:**
- Diagnóstico de por qué no crece antes de crear contenido nuevo
- Revisar canibalización: ¿varias URLs peleando por las mismas KW?
- Auditar contenido existente: ¿hay páginas con potencial que no están optimizadas?
- Internal linking: ¿las páginas clave reciben suficiente autoridad interna?
- Revisar intención de búsqueda: ¿el contenido responde lo que Google espera para esa KW?
- Optimizar lo existente antes de crear cosas nuevas (ratio impacto/esfuerzo mucho mejor)
- Backlinks hacia páginas específicas que están en posición 5-15 (quick wins)

**KPI principal:** mejora de posiciones en keywords existentes + CTR en GSC

**Lo que NO hacer:**
- Crear contenido nuevo sin antes diagnosticar por qué lo existente no funciona
- Ignorar el contenido que ya tiene tráfico mínimo — es el más fácil de mejorar

---

### 3. Caída de tráfico (bajada visible en GSC)

**Señales:**
- Caída de clics e impresiones en GSC vs período anterior
- Pérdida de posiciones en keywords que antes rankeaban
- Puede coincidir con un Google Core Update

**Enfoque estratégico:**
- Identificar CUÁNDO empezó la caída (fecha exacta en GSC)
- Cruzar la fecha con Google Updates (ver Google Search Status Dashboard)
- Identificar QUÉ páginas y KWs perdieron posiciones (GSC → Rendimiento → comparar períodos)
- Posibles causas: cambio de intención de búsqueda, contenido desactualizado, competidor mejoró, problema técnico, E-E-A-T débil
- Si es post-update: revisar qué tipo de contenido penalizó el update (thin, affiliate, UGC, etc.)
- Recuperación: actualizar contenido afectado, no crear nuevo hasta entender la causa

**KPI principal:** recuperación de clics en páginas afectadas

**Lo que NO hacer:**
- Crear contenido nuevo inmediatamente sin entender la causa
- Asumir que es penalización sin verificarlo en GSC primero
- Hacer cambios masivos en todas las páginas al mismo tiempo

---

### 4. Penalización (Manual Action en GSC)

**Señales:**
- Manual Action visible en GSC → Seguridad y acciones manuales
- Caída brusca y severa de tráfico (no gradual)
- Puede ser: spam de links, thin content, contenido engañoso, hacked site, cloaking

**Enfoque estratégico:**
- STOP: no hacer nada más hasta resolver la penalización
- Identificar el tipo de Manual Action (link spam, thin content, etc.)
- Limpiar la causa raíz según el tipo:
  - Link spam: disavow file en GSC + contactar sitios para eliminar links
  - Thin content: mejorar o eliminar páginas afectadas
  - Hacked site: limpiar malware, cambiar credenciales, revisar .htaccess
- Enviar reconsideración en GSC solo cuando la limpieza esté 100% hecha
- La reconsideración puede tardar semanas — planificar con el cliente

**KPI principal:** levantamiento de la Manual Action

**Lo que NO hacer:**
- Ignorar la Manual Action y seguir creando contenido
- Enviar reconsideración antes de limpiar completamente
- Crear backlinks nuevos mientras hay una penalización de links activa

---

### 5. Migración en curso (cambio de dominio, CMS o arquitectura URL)

**Señales:**
- Cambio de dominio (ej: http → https, dominio viejo → nuevo)
- Cambio de CMS (WordPress → React, Shopify → otro)
- Reestructuración de URLs (cambio de slugs, eliminación de categorías)
- Rediseño web con cambios en estructura

**Enfoque estratégico:**
- Inventario completo de URLs que rankean ANTES de migrar (GSC + Screaming Frog)
- Mapa de redirecciones 301: cada URL que tiene tráfico o backlinks necesita su redirect
- No eliminar nada que tenga tráfico sin redirect primero
- Preservar: title tags, meta descriptions, H1s, contenido en páginas que rankean
- Verificar el nuevo sitio en GSC inmediatamente tras el lanzamiento
- Monitoreo intensivo los primeros 30-60 días post-migración
→ Ver skill `/seo migrations` para protocolo completo

**KPI principal:** preservación del tráfico orgánico pre-migración (objetivo: no perder > 10%)

**Lo que NO hacer:**
- Migrar sin mapa de redirects completo
- Cambiar contenido Y estructura URL al mismo tiempo (aislar variables)
- Lanzar sin GSC verificado en el nuevo dominio

---

### 6. Sitio competitivo maduro (tráfico estable, necesita escala o diferenciación)

**Señales:**
- Tráfico orgánico estable o crecimiento lento
- Posiciones consolidadas en keywords principales
- DA > 30, perfil de backlinks razonable
- El cliente quiere crecer o defender posiciones frente a competencia más fuerte

**Enfoque estratégico:**
- Auditar qué keywords están en posición 5-15 con volumen: ahí están los quick wins
- Identificar gaps de contenido que la competencia cubre y el cliente no
- E-E-A-T: en mercados maduros, la calidad percibida diferencia — invertir en autor pages, caso estudios, datos propios
- Clusters temáticos: profundidad en el tema > amplitud superficial
- Link building cualitativo: pocos links de sitios de autoridad del nicho > muchos links genéricos
- GEO: optimizar para aparecer en AI Overviews, ChatGPT, Perplexity
- Contenido diferencial: datos propios, encuestas, herramientas interactivas — lo que no tiene la competencia

**KPI principal:** share of voice en keywords del sector + DA creciendo

**Lo que NO hacer:**
- Publicar contenido genérico que ya existe en 50 sitios del nicho
- Ignorar las keywords en posición 5-15 — son las más fáciles de mover
- Construir backlinks en masa — penalización de SpamBrain

---

## Combinaciones frecuentes

| Situación + Tipo de negocio | Prioridad estratégica |
|-----------------------------|----------------------|
| Nuevo + SaaS | Arquitectura de features/soluciones + long-tail BOFU |
| Nuevo + Local | GBP primero + páginas de servicio + ciudad principal |
| Estancado + Ecommerce | Canibalización de categorías + optimización de fichas de producto |
| Caída + Publisher | Identificar qué update afectó + revisar E-E-A-T de autores |
| Migración + cualquiera | Protocolo de migración tiene prioridad absoluta sobre todo lo demás |
| Maduro + SaaS | Comparison pages + clusters de casos de uso + GEO |
