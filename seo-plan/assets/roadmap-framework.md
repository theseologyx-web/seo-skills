<!-- Updated: 2026-04-03 -->
# SEO Roadmap Framework — El Play Game Completo

## Cómo usar este archivo

Este framework conecta todos los elementos de la estrategia SEO en un flujo de decisión. Sirve para construir el roadmap de cualquier proyecto en 5 pasos.

---

## PASO 0: Intake (antes de analizar nada)

Preguntas obligatorias antes de cualquier acción:

| # | Pregunta | Por qué importa |
|---|----------|----------------|
| 1 | ¿Tienes acceso a GSC, GA4, Ahrefs WMT? | Sin datos no hay diagnóstico |
| 2 | ¿Cuál es la situación actual del sitio? (nuevo / estancado / cayendo / penalizado / migración / maduro) | Define el orden de acciones |
| 3 | ¿Qué tipo de negocio es? (SaaS / ecom / local / publisher / agencia) | Define arquitectura y contenido |
| 4 | ¿B2B, B2C o mixto? | Define keywords, timelines, KPIs y playbooks |
| 5 | ¿Cuál es el KPI principal para el cliente? (tráfico / leads / ventas / visibilidad) | Define qué optimizamos |
| 6 | ¿Qué recursos tiene? (presupuesto, capacidad de contenido, equipo técnico) | Define qué es ejecutable |
| 7 | ¿Hay historial relevante? (updates que afectaron, migraciones pasadas, penalizaciones anteriores) | Evita repetir errores |

---

## PASO 1: Diagnóstico → Situación + Playbooks

Cruzar el diagnóstico de datos con las matrices de situación y playbooks:

```
Diagnóstico (GSC + GA4 + Ahrefs) 
    ↓
situations.md → ¿Qué situación es?
    ↓
playbooks.md → ¿Qué táctica aplica dado el diagnóstico?
    ↓
Puede haber 1-3 playbooks activos en paralelo (priorizar por urgencia)
```

**Regla:** resolver primero lo que bloquea, luego lo que optimiza.

| Si encuentras... | Prioridad |
|-----------------|-----------|
| Penalización manual activa | 🔴 STOP — resolver antes de todo |
| Migración en curso sin redirects | 🔴 STOP — protocolo migración primero |
| Crawl waste masivo (> 40% URLs sin valor) | 🔴 Alta — crawl budget primero |
| Caída de tráfico reciente | 🔴 Alta — diagnosticar causa antes de actuar |
| Keywords en pos 4-20 con volumen | 🟡 Alta — quick wins en paralelo con lo demás |
| Contenido con tráfico cayendo | 🟡 Alta — content refresh |
| Todo OK pero sin crecimiento | 🟢 Media — gaps, topical authority, links |

---

## PASO 2: Construcción del Roadmap por Fases

### Estructura base (adaptar según situación)

```
FASE 1 — Fundación (semanas 1-4)
├── Técnico: indexación limpia, CWV pasando, redirects OK
├── On-page: title tags, H1s, meta descriptions en páginas core
├── Crawl: reducir waste si sitio > 500 páginas
└── GBP: si negocio local, optimizar antes de cualquier otra cosa

FASE 2 — Quick Wins (semanas 5-8)
├── Mangos bajitos: optimizar URLs en pos 4-20
├── Content refresh: actualizar páginas con tráfico cayendo
└── Internal linking: conectar páginas fuertes con débiles

FASE 3 — Expansión de Contenido (semanas 9-20)
├── Gaps de keywords: crear contenido para temas no cubiertos
├── Topical authority: completar clusters incompletos
└── GEO sprint: optimizar para AI citations si aplica

FASE 4 — Autoridad (mes 5+)
├── Link building: guest posts, digital PR, broken links
├── E-E-A-T: author pages, datos propios, credenciales
└── Programático: si hay patrones escalables

FASE 5 — Escala y Defensa (mes 9+)
├── Mantener y actualizar lo que funciona
├── Expandir a nuevos clusters o geografías
└── AI visibility: SAIV tracking y optimización continua
```

### Ajustes por situación

| Situación | Modificación al roadmap base |
|-----------|------------------------------|
| Sitio nuevo | Saltar Fase 2 quick wins (no hay posiciones que mejorar) → empezar por Fase 3 con long-tail |
| Penalización | Fase 0 adicional: limpiar penalización antes de empezar Fase 1 |
| Caída de tráfico | Fase 1 = diagnóstico de causa → content refresh antes que expansión |
| Migración en curso | Fase 0 = protocolo migración completo → luego Fase 1 |
| Maduro competitivo | Saltarse Fase 1 (ya está hecha) → Fases 3-5 inmediatas |

### Ajustes por modelo B2B vs B2C

| Dimensión | B2B | B2C |
|-----------|-----|-----|
| Fase 1 on-page | Páginas de soluciones + casos de uso | Páginas de producto + categorías |
| Fase 2 quick wins | Keywords de comparativa + BOFU | Keywords de producto + transaccionales |
| Fase 3 contenido | TOFU educativo + MOFU comparativas | Contenido de tendencia + UGC-friendly |
| Fase 4 autoridad | Trade press + G2/Capterra + LinkedIn | Consumer media + influencers + reviews |
| KPI Fase 2 | MQLs desde orgánico | Conversiones / transacciones |

---

## PASO 3: KPIs por Fase

Cada fase tiene sus propios indicadores de éxito. No medir todo con los mismos KPIs:

| Fase | KPIs principales | Fuente |
|------|-----------------|--------|
| Fase 1 | % páginas indexadas, CWV passing rate, 0 errores críticos en GSC | GSC, CrUX |
| Fase 2 | Mejora de posición en quick-win keywords, CTR en GSC | GSC |
| Fase 3 | Nuevas keywords en top 20, tráfico orgánico creciendo | GSC, GA4 |
| Fase 4 | Referring domains creciendo, DA/DR subiendo | Ahrefs |
| Fase 5 | Share of voice, SAIV, pipeline/revenue desde orgánico | GSC, GA4, CRM |

**KPIs adicionales B2B (a partir de Fase 2):**
- MQLs desde orgánico (GA4 + CRM attribution)
- SQL pipeline originado en SEO
- Demo requests desde páginas orgánicas

**KPIs a deprecar:** bounce rate (usar engagement rate GA4), DA/DR como KPI primario, posición promedio sin contexto de volumen.

---

## PASO 4: Cadencia de Revisión

El roadmap no es estático. Revisión periódica obligatoria:

### Mensual
- GSC: evolución de clics, impresiones, posiciones en keywords objetivo
- GA4: tráfico orgánico por landing page, conversiones, engagement rate
- Revisar si los playbooks activos están dando resultados
- Ajustar prioridades si hay cambio de situación

### Trimestral
- Auditoría de contenido ligera: ¿qué páginas cayeron? → candidates para refresh
- Competitive gap: ¿qué nuevas keywords cubren los competidores?
- Revisión de crawl budget: ¿nuevos crawl wastes generados?
- Evaluación de playbooks: ¿terminar alguno, activar uno nuevo?

### Semestral / Anual
- Auditoría técnica completa
- Revisión de arquitectura: ¿hay que reestructurar?
- Benchmark vs industria: ¿dónde estamos vs la media del sector?
- Revisión de objetivos: ¿los KPIs siguen alineados con el negocio?

---

## PASO 5: Entregables del Roadmap

| Documento | Contenido | Audiencia |
|-----------|-----------|-----------|
| `SEO-STRATEGY.md` | Diagnóstico + situación + modelo B2B/B2C + playbooks seleccionados + KPIs | Interno + cliente |
| `ROADMAP.md` | Fases con acciones, responsables, timelines y KPIs por fase | Cliente |
| `QUICK-WINS.md` | Lista priorizada de mangos bajitos con estimación de impacto | Equipo técnico |
| `CONTENT-PLAN.md` | Clusters de keywords + calendario de publicación 90 días | Redacción |
| `GEO-BASELINE.md` | SAIV inicial + queries monitorizadas para AI visibility | Cliente SaaS/B2B |

---

## Flujo completo en una vista

```
INTAKE (Paso 0)
B2B/B2C? → Situación? → Tipo negocio? → KPI? → Recursos?
        ↓
DIAGNÓSTICO (Paso 1)
situations.md + playbooks.md → priorizar playbooks según urgencia
        ↓
ROADMAP (Paso 2)
Fase 1: Fundación → Fase 2: Quick wins → Fase 3: Contenido → Fase 4: Autoridad → Fase 5: Escala
(ajustado por situación + B2B vs B2C)
        ↓
KPIs (Paso 3)
Métricas por fase, no todas iguales. B2B: +MQL/SQL/pipeline
        ↓
REVISIÓN (Paso 4)
Mensual → Trimestral → Semestral
        ↓
ENTREGABLES (Paso 5)
Strategy + Roadmap + Quick wins + Content plan + GEO baseline
```
