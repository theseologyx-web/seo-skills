---
name: seo-privacy
description: >
  Privacy-first SEO strategy. Covers first-party data collection, GDPR/CCPA
  compliance for tracking, cookie consent impact on analytics, ethical tracking
  practices, GA4 consent mode, and maintaining SEO measurement accuracy in a
  privacy-first world. Use when user says "GDPR", "cookie consent", "first-party
  data", "privacy SEO", "consent mode", or "cookieless tracking".
user-invokable: true
argument-hint: "[url or site domain]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Privacy-First SEO

Navigate privacy regulations while maintaining SEO measurement accuracy and building first-party data assets.

---

## Privacy Landscape 2026 — Critical Updates

| Change | Status | Impact |
|--------|--------|--------|
| **Chrome third-party cookies** | **STILL ACTIVE** — Google reversed deprecation (April 2025). Safari + Firefox block by default; Chrome (65%+ market share) does NOT. | High — most analytics work normally in Chrome |
| **Privacy Sandbox** | **DEAD** — Google is retiring all 10 remaining APIs (Attribution Reporting, Topics API, Protected Audience) in Chrome and Android | High — no need to implement Privacy Sandbox alternatives |
| **Google Ads fingerprinting** | Now **permitted** (Feb 2025) — policies no longer prohibit fingerprinting for ad tracking, though GDPR consent still required | Medium — new fingerprinting options available |
| **GDPR enforcement** | €1.2B in fines in 2024 alone; €5.88B cumulative since GDPR launch | High — financial risk is real and growing |
| **One-click rejection mandate** | EU regulators standardizing: reject button must be as prominent as accept, one click max | High — many banners still non-compliant |

---

## 1. Privacy Regulations Overview

### Key Regulations Affecting SEO Tracking

| Regulation | Region | Applies To | Key Requirement |
|-----------|--------|-----------|----------------|
| GDPR | EU/EEA | Any site with EU visitors | Explicit consent before non-essential cookies |
| CCPA/CPRA | California, USA | Sites with CA residents | Opt-out right for data sale |
| PECR | UK | UK visitors | Consent for analytics cookies |
| LGPD | Brazil | Brazilian users | Similar to GDPR |
| PIPEDA | Canada | Canadian users | Meaningful consent |
| India DPDPA | India | Sites with Indian users | Consent Managers mandatory from Nov 2026 |
| EU AI Act | EU | AI systems (high-risk) | Risk assessments, human oversight, EU registration (Aug 2026) |
| China PIPL | China | Sites with Chinese users | Local data storage, consent requirements |
| US State Laws | 20+ US states | Varies | Growing patchwork: CT, CO, VA, TX, OR laws taking effect 2026-2027 |

**GDPR is the strictest** and effectively sets the global standard for most sites. **Financial risk:** €5.88B in cumulative GDPR fines (€1.2B in 2024 alone).

### What Requires Consent Under GDPR
- ✅ No consent needed: Strictly necessary cookies (session, cart, login)
- ❌ Consent required: Analytics (GA4), advertising, personalization, heatmaps
- ❌ Consent required: Social media embeds (YouTube, Facebook)
- ❌ Consent required: A/B testing tools, chatbots, marketing automation

---

## 2. Cookie Consent Audit

### Consent Banner Requirements (GDPR-Compliant)
- [ ] Banner appears before non-essential cookies fire
- [ ] Reject option as prominent as accept, **one click max** (EU one-click rejection mandate)
- [ ] No dark patterns (pre-ticked boxes, confusing language)
- [ ] Granular consent by category (analytics, marketing, preferences)
- [ ] Easy to withdraw consent at any time
- [ ] Privacy policy linked in banner
- [ ] Consent logged with timestamp and version
- [ ] CMP must be **Google-certified** + **IAB TCF 2.2 compliant** (required for Consent Mode v2 + Google Ads)

### Dark Patterns to Avoid (GDPR violations)
| Dark Pattern | Example | Status |
|--------------|---------|--------|
| Accept-only button | No reject option visible | ❌ Illegal |
| Pre-ticked boxes | Analytics pre-enabled | ❌ Illegal |
| Confusing toggle direction | "OFF" means active | ❌ Illegal |
| Hidden reject option | Buried in "settings" | ⚠️ Risky |
| Cookie wall | Block content unless accepting | ⚠️ Risky |

### How to Check Consent Implementation
```bash
# Check what cookies are set before consent
curl -sL [URL] -c /tmp/cookies.txt
cat /tmp/cookies.txt

# Check for third-party scripts loading before consent
curl -sL [URL] | grep -i -E '(google-analytics|googletagmanager|facebook|hotjar|clarity)'
```

---

## 3. GA4 Consent Mode v2

Google Consent Mode v2 (required for Google Ads + GA4 integration since March 2024) allows GA4 to model conversions even when users decline cookies.

### Implementation Check
```html
<!-- Must be BEFORE gtag.js loads -->
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}

  // Default: deny all before consent
  gtag('consent', 'default', {
    'analytics_storage': 'denied',
    'ad_storage': 'denied',
    'ad_user_data': 'denied',
    'ad_personalization': 'denied',
    'wait_for_update': 500
  });
</script>

<!-- Then load gtag.js -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXX"></script>
```

**When user accepts:**
```javascript
gtag('consent', 'update', {
  'analytics_storage': 'granted',
  'ad_storage': 'granted',
  'ad_user_data': 'granted',
  'ad_personalization': 'granted'
});
```

### Consent Mode Modes
| Mode | GA4 Behavior | Accuracy |
|------|-------------|---------|
| `analytics_storage: granted` | Full tracking | 100% |
| `analytics_storage: denied` + Consent Mode v2 | Modeled data | ~70-80% |
| No Consent Mode | No data for denied users | Significant data loss |

**Without Consent Mode v2:** EU sites may lose 30-50% of GA4 data. With it, Google models the missing conversions.

---

## 4. First-Party Data Strategy

### Why First-Party Data Matters for SEO
- Third-party cookies blocked in Safari + Firefox; **Chrome (65%+ market share) still allows them** (Google reversed deprecation April 2025)
- Privacy Sandbox is dead — do not build solutions around Topics API or Protected Audience
- Consent Mode reduces analytics accuracy for users who decline
- First-party data is owned, consent-based, and future-proof regardless of browser policy changes
- Enables personalization without privacy risk

### First-Party Data Collection Points
| Source | Data Collected | SEO Application |
|--------|---------------|----------------|
| Newsletter signup | Email, interests | Understand content demand |
| Lead forms | Contact info, needs | Intent signals |
| Account registration | Behavior, preferences | Personalization |
| Surveys / quizzes | Explicit preferences | Content strategy |
| Loyalty programs | Purchase history | Keyword clustering |
| Site search queries | On-site search terms | Gap analysis, new content ideas |

### Site Search as First-Party SEO Intelligence
Enable GA4 site search tracking:
```javascript
// GA4 site search parameter
gtag('config', 'G-XXXXXXXX', {
  'search_term_parameters': ['q', 's', 'query', 'search']
});
```

Analyze what users search on-site → reveals:
- Content gaps (searches with no results)
- High-intent topics for new content
- Product naming mismatches

---

## 5. Privacy-Compliant Analytics Alternatives

### Server-Side Tracking
Moves tracking to your server — bypasses browser-based blocking. Standard setup for 2026: **GTM Server-Side + Consent Mode v2**.

- **Tools:** GTM Server-Side (Stape for managed hosting), Segment, RudderStack
- **Benefits:** Not blocked by ad blockers, more accurate, GDPR-compliant with proper setup
- **Requires:** Server infrastructure (~$20-50/month managed), technical implementation

**Critical pitfall:** Consent lives in the browser, the tag server lives elsewhere. GA4/conversion tags can fire server-side before receiving browser consent — verify consent is forwarded correctly before tags execute.

**Also implement:**
- **Meta CAPI** (Conversions API): Server-side events for Meta Ads — bypass ITP/ad blockers
- **Google Enhanced Conversions**: Hashed first-party data (email, phone) for conversion matching without cookies

### Privacy-First Analytics Tools
| Tool | Privacy Model | Cookie-Free | GDPR-Compliant |
|------|-------------|-------------|---------------|
| Plausible | No cookies, no personal data | ✅ | ✅ |
| Fathom | No cookies, GDPR-native | ✅ | ✅ |
| Matomo (self-hosted) | You control data | Optional | ✅ |
| GA4 + Consent Mode v2 | Modeled data | ❌ | ✅ with proper setup |
| Cloudflare Analytics | Server-side, no cookies | ✅ | ✅ |

### Google Search Console as Privacy-Safe SEO Data
GSC provides ranking and click data **without needing user consent** (aggregate, non-personal data):
- Keyword rankings, impressions, CTR
- Index coverage and crawl issues
- Core Web Vitals (field data)
- Mobile usability errors

**Use GSC as primary SEO data source** — it's consent-free and directly from Google.

---

## 6. Privacy Policy & SEO

### What Google Looks For
Google (and users) evaluate trust via privacy signals. Missing or poor privacy documentation hurts E-E-A-T.

### Privacy Policy Requirements for SEO
- [ ] Privacy policy page exists and is linked in footer
- [ ] Uses HTTPS (same as rest of site)
- [ ] Lists all data collected and why
- [ ] Names all third-party tools (GA4, HubSpot, etc.)
- [ ] User rights explained (access, deletion, portability)
- [ ] Contact for privacy requests provided
- [ ] Last updated date visible

### Schema for Privacy Policy
```json
{
  "@context": "https://schema.org",
  "@type": "WebPage",
  "name": "Privacy Policy",
  "description": "Our privacy policy explains how we collect and use your data.",
  "url": "https://example.com/privacy-policy",
  "dateModified": "2025-03-01"
}
```

---

## 7. Impact Assessment: Consent Rate vs. Data Loss

### Calculating Data Loss
```
Estimated data loss = (1 - consent_rate) × monthly_sessions

Example:
- Monthly sessions: 50,000
- Consent rate: 60%
- Data loss: 40% × 50,000 = 20,000 sessions/month untracked
```

### Improving Consent Rate (Legally)
- Use clear, friendly language (not legal jargon)
- Explain the benefit to users ("Help us improve your experience")
- Reduce friction: minimize consent categories shown
- Test banner placement and timing
- Offer genuine value exchange for consent

**Benchmark consent rates:**
- Opt-in banner: 40-60% acceptance
- Opt-out banner (where legal): 80-95% acceptance

---

## 8. Tools by Function

### Consent Management Platforms (CMP)

**Requirement:** CMP must be **Google-certified** and **IAB TCF 2.2 compliant** for Consent Mode v2 + Google Ads integration. Check Google's certified CMP list.

| Tool | Strengths | Google-Certified | Pricing |
|------|-----------|-----------------|---------|
| Cookiebot (→ Usercentrics) | Auto-scans cookies, IAB TCF 2.2, merged with Usercentrics | ✅ | Paid |
| OneTrust | Enterprise, global compliance (GDPR + CCPA + 100+ laws) | ✅ | Paid |
| CookieYes | Easy setup, affordable, Consent Mode v2 | ✅ | Free + Paid |
| Termly | Affordable, auto-generates policy + banner | ✅ | Free + Paid |
| Quantcast Choice | Free CMP, IAB TCF compliant | ✅ | Free |
| TrustArc | Enterprise, legal + tech combined | ✅ | Paid |

### Privacy-First Analytics (No Consent Needed)
| Tool | Cookie-Free | Self-Hosted Option | GDPR Status |
|------|-------------|-------------------|-------------|
| Plausible | ✅ | ✅ | Fully compliant |
| Fathom | ✅ | ❌ | Fully compliant |
| Matomo | Optional | ✅ | Compliant (self-hosted) |
| Cloudflare Web Analytics | ✅ | ❌ | Fully compliant |
| Pirsch | ✅ | ✅ | Fully compliant |
| Simple Analytics | ✅ | ❌ | Fully compliant |
| Umami | ✅ | ✅ | Fully compliant (open-source) |
| PostHog | Optional | ✅ | Compliant (analytics + feature flags + session replay) |

### Cookie Scanning & Auditing
| Tool | Purpose | Pricing |
|------|---------|---------|
| Cookiebot Scanner | Detects all cookies + trackers on site | Free scan |
| OneTrust Cookie Audit | Enterprise cookie discovery | Paid |
| CookieMetrix | Free cookie scan + report | Free |
| Blacklight (by The Markup) | Privacy tracker detection | Free |
| webbkoll | Privacy check (cookies, referrers, CSP) | Free |

### GDPR Compliance Audit
| Tool | Purpose | Pricing |
|------|---------|---------|
| GDPR.eu checklist | Self-assessment guide | Free |
| DataGrail | Data subject request automation | Paid |
| Osano | Consent + privacy risk monitoring | Paid |
| Transcend | Data privacy infrastructure | Paid |

### Server-Side Tracking
| Tool | Best For | Pricing |
|------|----------|---------|
| GTM Server-Side | Existing GTM users | Free (needs server) |
| Segment | Multi-destination data routing | Free + Paid |
| RudderStack | Open-source, self-hosted | Free + Paid |
| Stape | Managed GTM server hosting | Paid |

---

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Datos SEO sin consent (rankings, impresiones, CTR) | `/seo google gsc <property>` | GSC no requiere consent del usuario — fuente primaria de datos SEO privacy-safe |
| Impacto del consent mode en reportes de tráfico | `/seo reporting [domain]` | Consent rate afecta directamente la precisión de GA4 y el cálculo de ROI |
| Privacy headers en servidor (Referrer-Policy, Permissions-Policy) | `/seo technical <url>` | Cat.3 Security audita headers de privacidad junto con HTTPS y CSP |
| Server-side tracking y cookie headers | `/seo server <url>` | Configuración de headers HTTP relacionados con cookies y privacidad |
| Schema para Privacy Policy page | `/seo schema <url>` | Validar WebPage schema en la página de privacidad |
| Privacy policy como señal de confianza E-E-A-T | `/seo content <url>` | Trustworthiness requiere privacy policy visible, fechada y enlazada en footer |

---

## Output Format

### Privacy-First SEO Audit Report

**Site:** [URL]
**Audit Date:** [Date]

#### Compliance Summary
| Regulation | Status | Risk Level |
|-----------|--------|-----------|
| GDPR | ✅/⚠️/❌ | Low/Medium/High |
| CCPA | ✅/⚠️/❌ | Low/Medium/High |
| Consent Mode v2 | ✅/⚠️/❌ | Low/Medium/High |

#### Data Collection Health
| Source | Status | Privacy Risk | Action |
|--------|--------|-------------|--------|
| Cookie consent banner | ✅/⚠️/❌ | Low/Med/High | ... |
| GA4 implementation | ✅/⚠️/❌ | Low/Med/High | ... |
| Consent Mode v2 | ✅/⚠️/❌ | Low/Med/High | ... |
| Third-party scripts | ✅/⚠️/❌ | Low/Med/High | ... |
| Privacy policy | ✅/⚠️/❌ | Low/Med/High | ... |

#### Priority Actions
| Priority | Action | Compliance Impact | SEO Impact | Effort |
|----------|--------|------------------|-----------|--------|
| 🔴 | [Critical compliance fix] | [Impact] | [Impact] | [Low/Med/High] |
| 🟡 | [High priority] | [Impact] | [Impact] | [Low/Med/High] |
| 🟢 | [Quick win] | [Impact] | [Impact] | [Low/Med/High] |

#### Estimated Data Loss
- Current consent rate: X%
- Estimated monthly sessions untracked: X,XXX
- Recommendation: [How to reduce data loss]
