---
name: seo-cro
description: >
  Conversion Rate Optimization a nivel de página: propuesta de valor, jerarquía de CTAs,
  fricción en formularios de la página, trust signals, presentación de oferta y conversion
  blockers. Scope: una URL o landing page específica. Para el journey completo post-click
  (onboarding, multi-step forms, 404s, microcopy) usar seo-cx. Para intent match SERP y
  pogo-sticking usar seo-sxo. Use cuando el usuario diga "CRO", "conversiones",
  "landing page optimization", "A/B test", "tasa de conversión", "CTA" o "bounce rate".
user-invokable: true
argument-hint: "[url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# UX & Conversion Rate Optimization (CRO) for SEO

> **Scope:** Este skill se enfoca en optimización de conversión a nivel de página — CTAs, layout above-fold, A/B testing, revenue. Para el journey completo del cliente (formularios, onboarding, fricción post-conversión) usar `seo-cx`. Para la capa SERP (intent match, pogo-sticking, featured snippets) usar `seo-sxo`.

Analyze and improve pages for both search engine rankings and user conversions. SEO brings traffic — CRO turns it into revenue.

## CRITICAL: Data Extraction

WebFetch misses `<head>` elements. Use `curl` for accurate data:
```bash
curl -sL [URL] | grep -i -E '(<title|meta name="description"|rel="canonical"|meta name="viewport")'
```

---

## 1. Above-the-Fold Analysis

The first screen a user sees determines whether they stay or leave. Google uses engagement signals (dwell time, pogo-sticking) as ranking factors.

### Checklist
- [ ] Clear value proposition visible without scrolling
- [ ] Primary CTA (call-to-action) above fold
- [ ] H1 matches user intent and ad/SERP promise
- [ ] No intrusive interstitials or popups on load (Google penalizes these)
- [ ] Page identifies what the site is within 3 seconds
- [ ] LCP element (hero image/text) loads in < 2.5s

### Common Above-Fold Issues
| Issue | SEO Impact | CRO Impact |
|-------|-----------|------------|
| Intrusive popup on load | ❌ Google penalty | ❌ High bounce |
| H1 doesn't match search intent | ❌ Relevance signal | ❌ Confusion |
| Slow hero image (LCP > 2.5s) | ❌ CWV failure | ❌ Abandonment |
| No clear CTA | ➖ Neutral | ❌ Lost conversions |
| Value prop buried below fold | ➖ Neutral | ❌ Pogo-sticking |

---

## 2. User Engagement Signals (SEO + CRO)

### Metrics That Affect Both Rankings and Revenue
| Signal | SEO Interpretation | CRO Interpretation |
|--------|-------------------|-------------------|
| Dwell time / time on page | Content quality signal | Engagement quality |
| Bounce rate (pogo-sticking) | Low relevance signal | High drop-off |
| Scroll depth | Content consumed | Interest level |
| Click-through rate (SERP) | Title/meta relevance | First impression |
| Return visits | Brand/authority signal | Loyalty indicator |

### How to Diagnose
- **GA4**: Engagement rate, average engagement time, scroll depth
- **Heatmaps** (Hotjar, Microsoft Clarity): Where users click and scroll
- **Session recordings**: Identify friction points
- **Search Console**: CTR and impressions by query

---

## 3. Page Layout & UX Audit

### Navigation & Information Architecture
- [ ] Main navigation clear and logical (< 7 items)
- [ ] Breadcrumbs present on deep pages (adds schema opportunity)
- [ ] Search functionality available on content-heavy sites
- [ ] Footer includes important links (About, Contact, Privacy)
- [ ] Mobile navigation usable with thumbs (hamburger menu, tap targets ≥ 48px)

### Content Hierarchy
- [ ] H1 → H2 → H3 logical nesting (no skipping levels)
- [ ] Paragraphs short (3-4 sentences max for web)
- [ ] Bullet points / numbered lists for scannable content
- [ ] Important information not buried in large text blocks
- [ ] Table of contents for long-form content (> 1500 words)

### Trust Signals
- [ ] SSL padlock visible
- [ ] Contact information easy to find
- [ ] Social proof (reviews, testimonials, logos) present
- [ ] Author information on blog/article pages
- [ ] Privacy policy and terms linked in footer
- [ ] Return/refund policy visible (e-commerce)

---

## 4. CTA Optimization

### CTA Best Practices
**Button copy:**

| ✅ High-converting | ❌ Low-converting |
|-------------------|------------------|
| "Start Free Trial" | "Submit" |
| "Get My Free Quote" | "Click Here" |
| "Download the Guide" | "Learn More" |
| "Book a Demo Today" | "Sign Up" |

**Visual:**
- High contrast button color (stands out from background)
- Sufficient white space around CTA
- Single primary CTA per page (avoid decision paralysis)
- Secondary CTA for users not ready to convert

**Placement:**
- Above fold (primary)
- After explaining value/benefits
- At end of content
- In sticky header/footer for long pages

---

## 5. Mobile UX Audit

Google uses mobile-first indexing — mobile UX directly impacts rankings.

### Mobile Checklist
- [ ] Viewport meta tag: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- [ ] No horizontal scrolling
- [ ] Text readable without zooming (≥ 16px body)
- [ ] Tap targets ≥ 48×48px (buttons, links)
- [ ] Forms usable on mobile (correct input types)
- [ ] No Flash or non-mobile-friendly plugins
- [ ] Images scaled correctly (no overflow)
- [ ] Popups/modals closeable on mobile

### Mobile-Specific Input Types
```html
<!-- Tel number keyboard on mobile -->
<input type="tel" name="phone">

<!-- Email keyboard on mobile -->
<input type="email" name="email">

<!-- Numeric keyboard on mobile -->
<input type="number" name="quantity">

<!-- Date picker on mobile -->
<input type="date" name="booking">
```

---

## 6. Core Web Vitals → CRO Connection

Poor CWV doesn't just hurt rankings — it directly kills conversions.

| Metric | Threshold | SEO Impact | CRO Impact (Research) |
|--------|-----------|-----------|----------------------|
| LCP > 4s | Poor | Rankings drop | ~24% fewer conversions |
| CLS > 0.25 | Poor | UX penalty | Users lose trust, abandon |
| INP > 500ms | Poor | Interactivity penalized | Frustration, drop-off |

### How to Diagnose CWV Impact on Revenue
1. Segment GA4 by page speed (fast vs. slow pages)
2. Compare conversion rates across speed segments
3. Calculate revenue impact: `(conversion rate delta) × (monthly visitors) × (avg order value)`

---

## 7. A/B Testing Opportunities

### High-Impact Elements to Test
| Element | Variation Ideas |
|---------|----------------|
| H1 / headline | Benefit-led vs. feature-led, question format |
| CTA button copy | "Get Started" vs. "Start Free" vs. "Try for Free" |
| CTA button color | High contrast vs. brand color |
| Hero image | People vs. product vs. abstract |
| Above-fold layout | Text left + image right vs. centered |
| Social proof placement | Above vs. below CTA |
| Form length | Short (3 fields) vs. long (7 fields) |

### A/B Testing Guidelines
- Test one element at a time
- Minimum sample: 1,000 visitors per variant
- Minimum duration: 2 weeks (capture weekly patterns)
- Statistical significance: ≥ 95% confidence

### A/B Testing Tools
| Tool | Best For | Pricing |
|------|----------|---------|
| VWO | Full-featured testing + heatmaps | Paid |
| Optimizely | Enterprise, multipage experiments | Paid |
| AB Tasty | Mid-market, easy setup | Paid |
| Convert.com | SEO-safe testing, no flicker | Paid |
| Google Optimize | Sunset March 2023 — do NOT use. Migrar a VWO, Convert.com, Statsig, o PostHog | - |
| Mutiny | AI personalization para B2B — adapta copy y CTAs por segmento de cuenta | Paid |
| Webflow Optimize | A/B testing nativo en Webflow sin código extra | Paid |

> **AI-Powered CRO (2026):** Dynamic CTAs que cambian en tiempo real según scroll depth, device, referral source y session history superan a static CTAs en **247%** en conversiones. PostHog ahora incluye A/B testing + feature flags en su plan gratuito. Microsoft Clarity integra Copilot AI para insights automáticos de heatmaps.

### SEO-Safe A/B Testing
- Use JavaScript-based tests (not server-side 301s)
- Do NOT cloak content from Googlebot
- Keep test duration short (< 30 days)
- Use `rel="canonical"` to point variants to original URL
- Don't test noindex pages

---

## 8. Landing Page Audit (Paid + Organic)

### Message Match
Does the page match what brought the user there?
- SERP title ↔ H1 ↔ content topic alignment
- Paid ad copy ↔ landing page headline
- Email CTA ↔ landing page offer

**Message mismatch is the #1 cause of high bounce rates.**

### Landing Page Speed
```bash
# Check Time to First Byte
curl -o /dev/null -s -w "TTFB: %{time_starttransfer}s\n" [URL]
```

Target: TTFB < 600ms

---

## 9. Tools by Function

### Heatmaps & Session Recordings
| Tool | Strengths | Pricing |
|------|-----------|---------|
| Microsoft Clarity | Free, unlimited sessions, heatmaps + recordings | Free |
| Hotjar | Heatmaps, recordings, surveys, funnels | Free + Paid |
| Crazy Egg | Scrollmaps, confetti clicks, A/B testing | Paid |
| FullStory | Enterprise session replay + analytics | Paid |
| Lucky Orange | Real-time recordings + chat integration | Paid |
| Mouseflow | Friction scoring, form analytics | Paid |

### A/B & Multivariate Testing
| Tool | Best For | Pricing |
|------|----------|---------|
| VWO | Full-stack testing + heatmaps + surveys | Paid |
| Optimizely | Enterprise, feature flags, multipage | Paid |
| AB Tasty | Mid-market, easy visual editor | Paid |
| Convert.com | SEO-safe (no flicker), privacy-friendly | Paid |
| Kameleoon | AI-powered personalization + testing | Paid |
| Statsig | Developer-friendly, feature flags | Free + Paid |

### Analytics & Funnel Analysis
| Tool | Purpose | Pricing |
|------|---------|---------|
| GA4 | Full funnel, engagement, conversions | Free |
| Mixpanel | Product analytics, retention, funnels | Free + Paid |
| Amplitude | Behavioral analytics, cohort analysis | Free + Paid |
| Heap | Auto-capture all clicks, retroactive analysis | Paid |
| PostHog | Open-source, self-hosted option | Free + Paid |

### Landing Page Builders (CRO-Optimized)
| Tool | Best For | Pricing |
|------|----------|---------|
| Unbounce | A/B tested landing pages | Paid |
| Instapage | Enterprise, personalization | Paid |
| Leadpages | SMB, fast setup | Paid |
| Webflow | Design-flexible, SEO-friendly | Paid |

### UX Research & Surveys
| Tool | Purpose | Pricing |
|------|---------|---------|
| Hotjar Surveys | On-page exit surveys | Free + Paid |
| Typeform | Beautiful forms, high completion | Free + Paid |
| UserTesting | Remote user testing, video feedback | Paid |
| Maze | Prototype testing, usability | Free + Paid |
| Lookback | Moderated user interviews | Paid |

---

---

## 10. WCAG 2.2 Accessibility (CRO Impact)

La accesibilidad no es solo compliance — los errores de accesibilidad son también errores de conversión. Un 15-20% de usuarios tienen alguna discapacidad; bloquearlos es perder conversiones.

### Criterios WCAG 2.2 con mayor impacto en CRO

| Criterio | Nivel | Impacto en conversión |
|----------|-------|----------------------|
| 1.4.3 Contraste de texto ≥ 4.5:1 | AA | Texto ilegible = abandono |
| 2.1.1 Navegación por teclado completa | A | Usuarios sin ratón no pueden completar forms |
| 2.4.7 Focus visible | AA | Sin focus visible → usuarios de teclado se pierden |
| 3.3.1 Identificación de errores en forms | A | Error genérico → usuario abandona el form |
| 3.3.2 Labels en campos de formulario | A | Sin label → usuario no sabe qué ingresar |
| 2.5.8 Tap target mínimo 24×24px | AA (nuevo 2.2) | Botones pequeños → fat finger errors en mobile |
| 1.4.4 Resize text hasta 200% | AA | Texto no escalable → abandono en usuarios con baja visión |

### Accessibility Quick Wins (alto impacto, bajo esfuerzo)
- [ ] Focus visible en todos los elementos interactivos (`:focus-visible` CSS)
- [ ] Labels explícitos en todos los campos de formulario
- [ ] Mensajes de error identificados con rol `alert` o `aria-live`
- [ ] Contraste de todos los CTAs ≥ 4.5:1
- [ ] Botones y links con texto descriptivo (no "clic aquí")
- [ ] Skip link al inicio del HTML (`<a href="#main">Ir al contenido</a>`)

---

## Output Format

### CRO + UX Audit Report

**URL:** [Page URL]
**Page Type:** [Homepage / Landing Page / Blog / Product / etc.]
**Audit Date:** [Date]

#### Executive Summary
[2-3 sentences: overall UX/CRO health, top conversion opportunity, main SEO-UX conflict]

#### UX Health Dashboard

| Category | Score | Issues Found | Status |
|----------|-------|-------------|--------|
| Above-fold clarity | X/10 | X issues | 🔴/🟡/🟢 |
| Mobile usability | X/10 | X issues | 🔴/🟡/🟢 |
| CTA effectiveness | X/10 | X issues | 🔴/🟡/🟢 |
| Trust signals | X/10 | X issues | 🔴/🟡/🟢 |
| Core Web Vitals | X/10 | X issues | 🔴/🟡/🟢 |
| Content scannability | X/10 | X issues | 🔴/🟡/🟢 |

#### Priority Actions

| Priority | Action | SEO Impact | CRO Impact | Effort |
|----------|--------|-----------|-----------|--------|
| 🔴 | [Critical] | [Impact] | [Impact] | [Low/Med/High] |
| 🟡 | [High] | [Impact] | [Impact] | [Low/Med/High] |
| 🟢 | [Quick win] | [Impact] | [Impact] | [Low/Med/High] |

#### A/B Test Recommendations
| Test | Hypothesis | Expected Lift | Priority |
|------|-----------|---------------|----------|
| [Element] | "Changing X to Y will improve Z because..." | +X% CVR | High/Med/Low |
