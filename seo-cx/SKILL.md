---
name: seo-cx
description: >
  Customer Experience (CX) audit for SEO: user journey mapping, friction points,
  trust architecture, micro-copy, form UX, error pages, onboarding, and support accessibility.
  Use when user says "CX", "customer experience", "experiencia del cliente", "user journey",
  "micro-copy", "formularios", "página 404", "onboarding", "friction", "confianza",
  "trust signals", or "journey map".
user-invokable: true
argument-hint: "[url o dominio]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: Lau
  version: "1.0.0"
  category: seo
---

# Customer Experience (CX) Audit

> **Scope:** Este skill cubre el journey completo del cliente — formularios, microcopy, fricción post-landing, onboarding, 404s, confianza. Para optimización de conversión a nivel de página (CTAs, layout, A/B) usar `seo-cro`. Para la capa SERP pre-clic (intent match, pogo-sticking) usar `seo-sxo`.

El CX va más allá de la UX de una página individual: analiza el viaje completo del cliente desde el primer contacto hasta la conversión y retención. Un buen CX reduce la fricción, genera confianza y aumenta el LTV (Lifetime Value). Google infiere calidad de CX a través de señales de engagement, pogo-sticking, y reputación de marca.

## CRITICAL: Data Extraction

```bash
# Detectar páginas de error personalizadas
curl -sL [URL]/404 -o /dev/null -w "Status: %{http_code}\n"

# Verificar formularios y sus campos
curl -sL [URL] | grep -i -E '(<form|<input|<label|<button|type="submit")' | head -20

# Detectar chat y soporte
curl -sL [URL] | grep -i -E '(intercom|drift|zendesk|hubspot|crisp|tawk|livechat)' | head -5
```

---

## 1. Customer Journey Mapping

### Los 5 Entry Points principales
| Canal de entrada | Expectativa del usuario | Error CX común |
|-----------------|------------------------|----------------|
| Búsqueda orgánica | Encontrar respuesta o solución | Página sin relación con la query |
| Búsqueda de marca | Ir directo al sitio/producto | Homepage genérica sin contexto |
| Referido / recomendación | Ver qué hace el producto | Falta de propuesta de valor clara |
| Email / newsletter | Acción específica prometida | Link que lleva a homepage, no al recurso |
| Anuncio pago | Oferta o demo prometida | Landing genérica sin match con el ad |

### Micro-momentos (Google framework)
| Momento | Query type | Qué necesita el usuario | CX fail |
|---------|-----------|------------------------|---------|
| I want to know | "qué es X", "cómo funciona X" | Información clara, sin venta | Interstitial de registro antes de ver contenido |
| I want to go | "[marca] + ciudad/sitio" | Llegar rápido | Página de error o redirect inesperado |
| I want to do | "cómo hacer X" | Pasos accionables | Tutorial incompleto o detrás de paywall |
| I want to buy | "precio X", "comprar X" | Precio visible, CTA claro | Formulario largo, precio oculto |

### Entry Points en 2026 — incluir AI Answer Engines

| Canal | Comportamiento del usuario al llegar |
|-------|--------------------------------------|
| Google organic | Intent estándar, explorando |
| **AI Overview (Gemini)** | Ya leyó el resumen — llega buscando profundidad o acción directa |
| **ChatGPT / Perplexity** | Investigó con AI — expectativas altas, preguntas específicas |
| Email / Social | Conoce la marca, intent más cálido |
| Directo | Usuario recurrente o referido |

> El 60% de compradores usan AI para research antes de llegar al sitio. Los usuarios que vienen de AI answer engines llegan con intent más específico y toleran menos friction — optimizar las landing pages para este segmento.

### Checklist Journey Map
- [ ] Cada entry point lleva a una página relevante (no homepage siempre)
- [ ] El usuario entiende qué puede hacer en < 5 segundos en cualquier página
- [ ] Existe ruta clara de cada stage del funnel al siguiente
- [ ] Las páginas de entrada no piden demasiado demasiado pronto (registro, pago)
- [ ] Existe camino de vuelta (breadcrumbs, nav, back to top)

---

## 2. Trust Architecture

La confianza no se pide, se construye con evidencia. Para YMYL (Your Money Your Life) es crítico.

### Pirámide de confianza
```
          [Conversión]
              ↑
    [Social proof + garantías]
              ↑
  [Credenciales + transparencia]
              ↑
    [Seguridad + privacidad]
              ↑
     [Identidad del negocio]
```

### Checklist Trust Signals por nivel

**Identidad (base)**
- [ ] Logo visible en header
- [ ] Nombre de empresa/marca claro
- [ ] "Sobre nosotros" / "About" accesible desde nav principal
- [ ] Dirección física si es negocio con presencia (importante para YMYL)
- [ ] Email o teléfono de contacto visible en footer

**Seguridad y privacidad**
- [ ] HTTPS (candado verde en barra de dirección)
- [ ] Privacy Policy linkada en footer
- [ ] Terms of Service linkados en footer
- [ ] Cookie banner compliant (GDPR/CCPA según mercado)
- [ ] Sellos de seguridad en páginas de pago (SSL, PCI DSS)

**Credenciales y transparencia**
- [ ] Bio del autor en artículos (nombre, cargo, expertise)
- [ ] Fecha de publicación y actualización visible
- [ ] Fuentes citadas con links a origen
- [ ] Premios, certificaciones, membresías de industria
- [ ] Clientes o partners reconocibles (logos con permiso)

**Social proof**
- [ ] Testimoniales con nombre, empresa y foto (no anónimos)
- [ ] Casos de estudio con resultados específicos (no genéricos)
- [ ] Ratings y reviews de terceros (Google, G2, Capterra, Trustpilot)
- [ ] Métricas de uso: "10,000+ clientes", "4.8/5 en 500+ reviews"
- [ ] Presencia en medios reconocidos ("As seen in...")

**Garantías (BOFU)**
- [ ] Política de devolución / cancelación clara y visible
- [ ] Período de prueba gratuita comunicado claramente
- [ ] Garantía de dinero back si aplica
- [ ] SLA / uptime guarantee para SaaS

---

## 3. Micro-copy y UX Writing

El micro-copy son los textos pequeños que guían acciones: botones, labels, placeholders, mensajes de error. Tienen impacto desproporcionado en conversiones.

### Botones y CTAs
| ✅ Alta conversión | ❌ Baja conversión | Por qué |
|-------------------|-------------------|---------|
| "Iniciar prueba gratis" | "Submit" | Beneficio vs acción genérica |
| "Ver precios →" | "Más información" | Específico vs vago |
| "Descargar la guía (PDF, 15 páginas)" | "Descargar" | Contexto + expectativa |
| "Sí, quiero ahorrar tiempo" | "Aceptar" | Primera persona + beneficio |
| "Reservar demo — 30 min" | "Contáctanos" | Específico + tiempo comprometido |

### Labels de formulario
```html
<!-- ❌ MAL: Placeholder como único label (desaparece al escribir) -->
<input type="email" placeholder="Tu email" />

<!-- ✅ BIEN: Label visible + placeholder como hint -->
<label for="email">Email de trabajo</label>
<input type="email" id="email" placeholder="nombre@empresa.com" />
```

### Mensajes de error
| ❌ Error genérico | ✅ Error útil |
|------------------|--------------|
| "Error en el formulario" | "El email no tiene un formato válido. Ejemplo: nombre@empresa.com" |
| "Contraseña incorrecta" | "Contraseña incorrecta. ¿Olvidaste tu contraseña?" [+ link] |
| "Campo requerido" | "Necesitamos tu nombre para personalizar tu experiencia" |
| "Error 500" | "Algo salió mal de nuestro lado. Ya lo estamos revisando. [Intentar de nuevo]" |

### Empty States
Qué ve el usuario cuando no hay resultados (búsqueda interna, dashboard vacío, lista sin items):
- [ ] Imagen o icono ilustrativo (no pantalla en blanco)
- [ ] Explicación de por qué está vacío
- [ ] Acción para solucionarlo (+ CTA)
- [ ] Tono amigable, no técnico

```
Ejemplo: [Búsqueda sin resultados]
   🔍
"No encontramos resultados para 'marketing automation'"
¿Quizás buscabas? → automatización de marketing | email marketing
[Ver todos los artículos]
```

### Onboarding copy (primera visita / primer login)
- Mensaje de bienvenida personalizado si hay nombre
- Próximo paso claro: "Para empezar, haz X"
- Sin jerga técnica en el primer contacto
- Progress indicator si hay setup de múltiples pasos
- Opción de saltar (no forzar onboarding completo)

---

## 4. Form UX

Los formularios son el mayor punto de fricción en cualquier funnel. Cada campo extra reduce la conversión.

### Checklist Form UX

**Estructura**
- [ ] Mínimo de campos necesarios (preguntar: ¿necesitamos este dato ahora?)
- [ ] Labels sobre el campo (no dentro / no solo placeholder)
- [ ] Campos de una columna en mobile (no side-by-side)
- [ ] Orden lógico: fácil → difícil (nombre antes que NIT)
- [ ] Campos relacionados agrupados visualmente

**Tipos de input correctos**
```html
<input type="email" />    <!-- teclado @ en mobile -->
<input type="tel" />      <!-- teclado numérico en mobile -->
<input type="number" />   <!-- teclado numérico + flechas -->
<input type="date" />     <!-- date picker nativo en mobile -->
<input type="search" />   <!-- teclado con botón "Buscar" en mobile -->
```

**Validación**
- [ ] Validación inline (mientras escribe, no al submit)
- [ ] Mensaje de éxito inline (✅ "Email válido")
- [ ] Error específico, no genérico
- [ ] No borrar lo que el usuario escribió al mostrar error
- [ ] Autocompletado habilitado (`autocomplete="email"`, `autocomplete="name"`)

**Confianza en formularios**
- [ ] "¿Por qué pedimos esto?" cerca de campos sensibles (teléfono, empresa)
- [ ] "No spam. Solo te contactaremos para X" cerca del email
- [ ] Política de privacidad linkada cerca del submit
- [ ] Total de pasos visible en formularios multi-step (paso 1 de 3)

### Métricas clave de Form Analytics
| Métrica | Herramienta | Umbral saludable |
|---------|------------|-----------------|
| Form completion rate | Clarity, Hotjar, GA4 | > 70% para formularios cortos |
| Field drop-off | Clarity, Mouseflow | Identificar el campo que más abandona |
| Time to complete | Clarity, Hotjar | > 5 min = formulario muy largo |
| Error frequency by field | Clarity, Mouseflow | > 30% error en un campo = problema de UX |

---

## 5. Páginas de Error (404, 500, etc.)

### 404 personalizada — Checklist
- [ ] Reconoce que la página no existe (no en blanco)
- [ ] Mantiene header/footer de navegación
- [ ] Tiene buscador interno para que el usuario encuentre lo que buscaba
- [ ] Sugiere páginas populares o categorías principales
- [ ] Tono de marca (no solo "404 Not Found")
- [ ] CTA para ir a homepage o categoría principal
- [ ] NO hace redirect automático a homepage (Google lo ve como soft 404)

```html
<!-- Estructura mínima de una 404 efectiva -->
<h1>Esta página no existe</h1>
<p>Parece que la página que buscas fue movida o eliminada.</p>
<form action="/buscar">
  <input type="search" placeholder="¿Qué estabas buscando?">
  <button type="submit">Buscar</button>
</form>
<nav>
  <h2>O explora estas secciones</h2>
  <ul>
    <li><a href="/blog">Blog</a></li>
    <li><a href="/servicios">Servicios</a></li>
    <li><a href="/contacto">Contacto</a></li>
  </ul>
</nav>
```

### 500 y errores del servidor
- [ ] No expone información técnica al usuario (stack trace, rutas del servidor)
- [ ] Informa que el problema es del lado del servidor (no del usuario)
- [ ] Ofrece página de status o tiempo estimado de resolución si existe
- [ ] Contacto de soporte visible

### Impacto SEO de páginas de error
- 404s en Screaming Frog → pérdida de link equity si tienen inlinks
- Soft 404s (páginas que devuelven 200 pero están vacías) → peor que 404 real
- Cadenas de redirects desde páginas 404 → PageRank dilution

---

## 6. Soporte y Accesibilidad al Ayuda

### Canales de soporte — Checklist
- [ ] Chat en vivo o bot visible (sin obstruir contenido en mobile)
- [ ] Email de soporte en footer o página de contacto
- [ ] FAQ accesible desde páginas clave (pricing, producto)
- [ ] Centro de ayuda/documentación linkado desde el producto
- [ ] Tiempo de respuesta comunicado ("Respondemos en < 2h en horario laboral")
- [ ] Opciones múltiples: no solo un canal

### Posicionamiento del chat
```
❌ Chat widget que cubre el CTA principal en mobile
❌ Chat que aparece a los 2 segundos y hace scroll automático
✅ Chat en esquina inferior derecha, no sobre contenido crítico
✅ Chat que aparece solo después de X segundos o scroll del 50%
```

---

## 7. Accesibilidad como CX

La accesibilidad no es solo WCAG — es CX para el 15-20% de usuarios con alguna discapacidad.

### Quick wins de accesibilidad con impacto CX
- [ ] Skip navigation link (para usuarios de teclado / lectores de pantalla)
- [ ] Focus visible en todos los elementos interactivos
- [ ] Orden lógico de tab (sigue el flujo visual)
- [ ] ARIA labels en iconos sin texto (`aria-label="Cerrar menú"`)
- [ ] Videos con subtítulos o transcripción
- [ ] No depender de hover para mostrar información crítica (no funciona en mobile/touch)
- [ ] Animaciones respetan `prefers-reduced-motion`

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Output Format

### CX Score: XX/100

| Dimensión | Score | Issues | Estado |
|-----------|-------|--------|--------|
| Trust Architecture | XX/20 | X issues | 🔴/🟡/🟢 |
| Journey Clarity | XX/20 | X issues | 🔴/🟡/🟢 |
| Micro-copy Quality | XX/20 | X issues | 🔴/🟡/🟢 |
| Form UX | XX/20 | X issues | 🔴/🟡/🟢 |
| Error Handling | XX/10 | X issues | 🔴/🟡/🟢 |
| Support Accessibility | XX/10 | X issues | 🔴/🟡/🟢 |

### Customer Journey Map
```
[Entry point] → [Página de aterrizaje] → [Página de consideración] → [Conversión]
     ↓                  ↓                        ↓                      ↓
[Canal]            [Friction points]        [Trust gaps]           [CX wins/fails]
```

### Top Friction Points

| # | Punto de fricción | Etapa del journey | Impacto | Fix sugerido |
|---|------------------|------------------|---------|--------------|
| 1 | ... | TOFU/MOFU/BOFU | Alto/Medio/Bajo | ... |

### Micro-copy Issues

| Elemento | Texto actual | Problema | Texto sugerido |
|----------|-------------|---------|----------------|
| Botón CTA principal | "Submit" | Genérico, sin beneficio | "Obtener presupuesto gratis" |

---

## Skills Relacionados

| Necesidad | Skill | Por qué |
|-----------|-------|---------|
| Diseño visual e identidad | `/seo ux-visual [url]` | Jerarquía visual, tipografía, WCAG |
| Conversiones y A/B testing | `/seo cro [url]` | Heatmaps, engagement, funnels |
| Experiencia de búsqueda | `/seo sxo [url]` | Intent match, SERP → página |
| Schema para trust signals | `/seo schema [url]` | Review schema, Organization, Person |
| Calidad y E-E-A-T del contenido | `/seo content [url]` | Autoridad, credibilidad del contenido |

## Error Handling

| Escenario | Acción |
|-----------|--------|
| Sin acceso a formularios reales | Analizar HTML de los forms visibles. Indicar que form analytics requiere Clarity/Hotjar. |
| Sitio detrás de login para ver journey completo | Analizar las páginas públicas. Indicar explícitamente qué partes del journey no pudieron analizarse. |
| URL inaccesible | Reportar error. No inferir. |
