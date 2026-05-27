---
name: seo-entity
description: >
  Entity SEO and Knowledge Graph optimization. Covers entity identification,
  entity authority building, semantic relationships, schema for entity clarification,
  E-E-A-T entity signals, Wikipedia/Wikidata optimization, and AI search entity
  visibility. Use when user says "entity SEO", "Knowledge Graph", "entity authority",
  "semantic SEO", "entity recognition", "brand entity", "topical authority",
  or "entity disambiguation".
user-invokable: true
argument-hint: "[brand name, person name, or website url]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Entity SEO & Knowledge Graph Optimization

Entity SEO is about making Google understand WHO you are, WHAT you do, and HOW you relate to other entities — not just what keywords you use. It's the foundation of AI search visibility and E-E-A-T.

> **Skill relacionado:** `seo-brand` cubre la capa de reputación y presencia en SERPs (branded SERPs, knowledge panel management, menciones de marca, reviews, Google Business Profile). Este skill (`seo-entity`) se enfoca en la capa técnica y semántica (Knowledge Graph, Wikipedia/Wikidata, schema para entidades, relaciones semánticas). Úsalos juntos para una estrategia de autoridad de marca completa.

---

## 1. What Is an Entity?

In Google's Knowledge Graph, an **entity** is any uniquely identifiable thing:
- A **person** (Elon Musk, Marie Curie)
- An **organization** (Apple Inc., Red Cross)
- A **place** (Paris, Yellowstone National Park)
- A **concept** (machine learning, democracy)
- A **product** (iPhone 16, Tesla Model 3)
- A **creative work** (The Great Gatsby, Inception)

Google's goal: understand content the way humans do — by recognizing entities and their relationships, not just keywords.

### Why Entity SEO Matters Now
- Google's algorithm has shifted from **keywords** → **entities + intent**
- AI Overviews, ChatGPT, and Perplexity all reason about entities
- Sites with strong entity signals rank more stably through algorithm updates
- E-E-A-T is fundamentally about entity authority (is the author/brand a trusted entity?)
- **Gartner predicts traditional search volume will drop 25% by 2026** as buyers shift to AI assistants — entity visibility in AI systems is no longer optional
- Google's Knowledge Graph contains **800 billion facts about 8 billion entities** — being in it means being part of how AI answers questions
- **Feb 2026:** Google added a dedicated [Authors section](https://developers.google.com/search/docs/fundamentals/creating-helpful-content) to Search Central — the clearest signal yet that author entity transparency is a direct quality consideration

---

## 2. Entity Identification

### Identify Your Core Entities
Every website has at least 2-3 core entities to optimize:

**Organization entity:**
- Your company/brand name
- What it does (products, services)
- Where it operates
- Who leads it
- Its relationships to other entities

**Person entities (for personal brands or author-driven sites):**
- Author/founder names
- Their expertise areas
- Their credentials
- Their associations (companies, publications, organizations)

**Topic/concept entities (for content sites):**
- Your primary topical area
- Key sub-topics
- Related expert entities in your niche

### Entity Status Check
Determine if your entity already exists in Google's Knowledge Graph:

```bash
# Search Google for: "[Brand Name]" and look for:
# 1. Knowledge Panel on the right → entity exists, is it correct?
# 2. No panel → entity not yet recognized or not prominent enough
# 3. Wrong entity shown → disambiguation needed

# Also check:
# - en.wikipedia.org/wiki/[Brand_Name]
# - www.wikidata.org/wiki/ (search for your brand)
# - Google Knowledge Graph Search API (requires API key):
# https://kgsearch.googleapis.com/v1/entities:search?query=[BrandName]&key=[API_KEY]
```

---

## 3. Entity Authority Building

Google determines entity authority from **corroborating signals** across the web — not just your own website.

### The Entity Authority Pyramid

```
         ┌─────────────────────┐
         │    Wikipedia        │  (Highest authority signal)
         ├─────────────────────┤
         │    Wikidata         │  (Structured entity data)
         ├─────────────────────┤
         │  Authoritative      │  (Forbes, NYT, BBC, industry press)
         │  News Mentions      │
         ├─────────────────────┤
         │  Industry           │  (Trade publications, associations)
         │  Directories        │
         ├─────────────────────┤
         │  Social Profiles    │  (LinkedIn, Twitter, YouTube)
         │  (Verified)         │
         ├─────────────────────┤
         │  Your Website       │  (Foundational but not sufficient alone)
         └─────────────────────┘
```

**Key insight:** Google trusts external sources more than your own website to validate your entity. You can claim anything on your site — but third-party corroboration is what creates entity authority.

### Building Entity Corroboration

**Step 1: Foundational digital presence**
- [ ] Consistent brand name across ALL platforms (exact match)
- [ ] Complete social profiles: LinkedIn, Twitter/X, YouTube, Facebook, Instagram
- [ ] Verified profiles where possible (checkmarks, account verification)
- [ ] Each profile links to your website
- [ ] Each profile has consistent bio describing your entity

**Step 2: Structured data on your site**
(See Schema section below — `Organization`/`Person` with `sameAs`)

**Step 3: Wikipedia (if eligible)**
Notability requirements for Wikipedia:
- Covered in multiple independent, reliable, secondary sources
- Not just press releases or self-promotion — genuine editorial coverage
- Significant coverage (not just mentions)

If eligible:
1. Create/improve Wikipedia article
2. Must follow Wikipedia's neutral point of view policy
3. Link from Wikipedia article to your site
4. This is the strongest single entity signal you can get

**Step 4: Wikidata**
Wikidata is the structured data layer behind Wikipedia and is directly integrated into Google's Knowledge Graph.

How to create/improve a Wikidata entry:
1. Go to wikidata.org
2. Search for your entity
3. If it doesn't exist: create item (requires some Wikipedia coverage)
4. If it exists: add/improve properties:
   - **P31**: instance of (e.g., Q4830453 = business enterprise)
   - **P279**: subclass of (categorización jerárquica)
   - **P856**: official website ← critical
   - **P18**: image
   - **P159**: headquarters location
   - **P571**: inception (founding date)
   - **P112**: founder
   - **P452**: industry
   - **P2002**: Twitter username
   - **P2003**: Instagram username
   - **P4264**: LinkedIn company
   - **P1566**: GeoNames ID (for places/locations)
   - **P7859**: WorldCat Entities ID (for academic entities)
   - **P2671**: Google Knowledge Graph ID ← maps your Wikidata entry directly to Google's KG. Check it with: `curl "https://www.wikidata.org/wiki/[QID]"` and look for the Google KG ID property

**Step 5: Industry directory listings**
Get listed in authoritative directories in your vertical:
- Crunchbase (tech/startups)
- Bloomberg company profiles (business)
- LinkedIn Company Page (universal)
- Glassdoor (employer entity signals)
- BBB (trust entity signal)
- Chamber of Commerce (local business entity)
- Industry-specific associations

**Step 6: Press & media mentions**
- PR campaigns targeting authoritative publications
- Expert quotes in industry articles
- Podcast appearances (Google indexes podcast content)
- Author bylines in reputable publications

---

## 4. Schema Markup for Entity Clarification

Schema is how you formally declare your entity to Google in machine-readable format.

### Organization Entity Schema

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://example.com/#organization",
  "name": "Exact Brand Name As It Appears Everywhere",
  "url": "https://example.com",
  "logo": {
    "@type": "ImageObject",
    "url": "https://example.com/logo.png",
    "width": 512,
    "height": 512
  },
  "foundingDate": "2018",
  "description": "Concise description of what the organization does, 1-2 sentences.",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main St",
    "addressLocality": "Chicago",
    "addressRegion": "IL",
    "postalCode": "60601",
    "addressCountry": "US"
  },
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-312-555-0100",
    "contactType": "customer service",
    "availableLanguage": ["English", "Spanish"]
  },
  "sameAs": [
    "https://twitter.com/yourbrand",
    "https://linkedin.com/company/yourbrand",
    "https://facebook.com/yourbrand",
    "https://instagram.com/yourbrand",
    "https://youtube.com/@yourbrand",
    "https://en.wikipedia.org/wiki/YourBrand",
    "https://www.wikidata.org/wiki/Q[ID]",
    "https://www.crunchbase.com/organization/yourbrand"
  ]
}
```

**`sameAs` is the entity disambiguation property** — it tells Google "this entity on my website = these entities on other platforms." The more authoritative the `sameAs` URLs, the stronger the entity signal.

### Person Entity Schema (Author/Expert)

```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "@id": "https://example.com/#person-john-doe",
  "name": "John Doe",
  "jobTitle": "Chief SEO Strategist",
  "worksFor": {
    "@type": "Organization",
    "@id": "https://example.com/#organization"
  },
  "url": "https://example.com/about/john-doe",
  "image": "https://example.com/images/john-doe.jpg",
  "description": "John Doe is an SEO strategist with 12 years of experience in technical SEO and content strategy.",
  "alumniOf": {
    "@type": "Organization",
    "name": "Northwestern University"
  },
  "knowsAbout": ["SEO", "Technical SEO", "Content Strategy", "Link Building"],
  "sameAs": [
    "https://twitter.com/johndoe",
    "https://linkedin.com/in/johndoe",
    "https://en.wikipedia.org/wiki/John_Doe",
    "https://scholar.google.com/citations?user=XXXXX"
  ]
}
```

### WebSite Entity Schema (Sitelinks Search Box)

```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "@id": "https://example.com/#website",
  "url": "https://example.com",
  "name": "Brand Name",
  "potentialAction": {
    "@type": "SearchAction",
    "target": {
      "@type": "EntryPoint",
      "urlTemplate": "https://example.com/search?q={search_term_string}"
    },
    "query-input": "required name=search_term_string"
  }
}
```

---

## 5. Topical Authority as Entity Signaling

Google's **topical authority** system assigns entities expertise scores by topic based on content depth and coverage.

### Topical Authority Map

Build a comprehensive map of your entity's claimed expertise:

```
Core Entity: "YourBrand" (SEO Agency)
├── Primary Topic Authority: SEO
│   ├── Technical SEO (strong coverage)
│   ├── Local SEO (moderate coverage)
│   ├── Content SEO (moderate coverage)
│   ├── E-commerce SEO (gaps detected)  ← create content
│   └── International SEO (no coverage) ← create or avoid
├── Secondary Topic Authority:
│   ├── Content Marketing (peripheral)
│   └── Digital Analytics (peripheral)
└── Avoid: Finance, Health, Legal (outside expertise = dilutes authority)
```

**Rule:** It's better to be the definitive authority in a narrow topic than a mediocre authority in a broad one.

### NLP-Based Entity Optimization

Google's NLP (Natural Language Processing) reads your content and extracts entities. Help it:

**Co-occurrence signals:**
Mention your entity name near established entities in your niche:
```
❌ "We provide SEO services"
✅ "Our SEO methodology builds on frameworks developed by industry leaders
   like Rand Fishkin (Moz), Barry Schwartz (Search Engine Roundtable),
   and Google's John Mueller"
```

When Google sees your entity co-occurring with authoritative entities, it associates your entity with that topic cluster.

**Named Entity Recognition (NER) optimization:**
- Use your full formal name on first mention in articles
- Vary how you refer to the entity after that (pronouns, abbreviations are fine)
- Place the entity name in the first 100 words of key pages
- Include in title tags where natural

---

## 6. Author Entity & E-E-A-T

For content sites, author entities are critical for E-E-A-T signals — especially for YMYL (health, finance, legal).

### Author Entity Checklist
- [ ] Dedicated author bio page (URL: /about/author-name or /author/name)
- [ ] Professional headshot (real photo, not stock)
- [ ] Credentials listed with specifics (not just "expert")
- [ ] Years of experience mentioned
- [ ] Links to external validation (published articles, LinkedIn, credentials)
- [ ] `Person` schema with `sameAs` and `knowsAbout`
- [ ] Author byline on every article with link to bio page

### E-E-A-T Entity Signals by Type

| Signal | Experience | Expertise | Authoritativeness | Trustworthiness |
|--------|------------|-----------|------------------|----------------|
| First-hand case studies | ✅ | | | |
| Professional credentials | | ✅ | ✅ | |
| Backlinks from authorities | | | ✅ | |
| Wikipedia mention | | | ✅ | |
| Press mentions | | | ✅ | ✅ |
| Verified social profiles | | | ✅ | ✅ |
| Contact info visible | | | | ✅ |
| Privacy policy + ToS | | | | ✅ |
| HTTPS + security | | | | ✅ |
| Reviews/testimonials | ✅ | | | ✅ |

---

## 7. Entity Disambiguation

When Google confuses your entity with another entity of the same name:

### Signs of Disambiguation Problems
- Your Knowledge Panel shows wrong entity
- Google shows competitor in "branded" searches
- Your entity conflated with a famous person/company with same name

### Disambiguation Strategies

**Strengthen `sameAs` links**: More `sameAs` pointers = stronger disambiguation signal.

**Add disambiguating context in content:**
```html
<!-- Title tag example for disambiguation -->
<title>Acme Inc. | Software Company | Chicago, IL</title>

<!-- Not just: -->
<title>Acme Inc.</title>
```

**Wikidata disambiguation:**
If there are multiple entities with the same name, Wikidata uses disambiguation pages. Ensure your entity has a unique Wikidata identifier (Qxxxx).

**Local entity disambiguation:**
For local businesses sharing a name with a larger brand:
- Add geographic identifiers in schema and content
- `"name": "Acme Plumbing Chicago"` (not just "Acme Plumbing")
- Emphasize local signals (city, neighborhood, region)

---

## 8. AI Search & Entity Visibility

Entity optimization is the #1 factor for appearing in AI Overviews and AI chatbot answers.

### How AI Systems Use Entities

- **ChatGPT**: Sources from Bing index + Wikipedia + authoritative publications
- **Google AI Overviews**: Uses Knowledge Graph entities heavily for factual responses
- **Perplexity**: Real-time web + Wikipedia + authoritative sources
- **Gemini**: Deep Google Knowledge Graph integration

### AI Citation by Entity Presence

Authors and brands with presence across **Wikipedia, Reddit, and LinkedIn** are **2.8x more likely to be cited by AI systems**. AI systems use entity graphs to determine citation priority — an entity that is well-connected (multiple `sameAs` references, Wikidata entry, authoritative citations) is treated as more authoritative.

**mainEntityOfPage — tell AI systems what this page is primarily about:**
```json
// On your About page (or homepage):
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://example.com/#organization",
  "name": "Example Corp",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://example.com/about/"
  }
}
```
`mainEntityOfPage` tells Google "this page is primarily about THIS entity." Critical for disambiguation and AI parsing.

### Entity Optimization for AI Search

**For factual queries ("What is [Brand]?"):**
- Wikipedia article is the most cited source by AI
- Wikidata structured properties used for factual answers
- Consistent `sameAs` schema helps AI systems identify correct entity

**For recommendation queries ("Best [service type] in [city]"):**
- Reviews on authoritative platforms (Yelp, Trustpilot, G2)
- Mentions in "best of" lists on authoritative publications
- Entity co-occurrence with topic keywords in multiple sources

**For expertise queries ("Top experts in [field]"):**
- Author entity signals (credentials, publications, speaking)
- Consistent expertise attribution across external sources

### Knowledge Graph API — Practical Workflow
Check whether your entity exists and how Google understands it:
```bash
# Check entity existence in Google's KG
curl "https://kgsearch.googleapis.com/v1/entities:search?query=[BrandName]&key=[API_KEY]&limit=5"

# Interpret results:
# - "resultScore" > 1000 = strong entity presence
# - "@type" array = how Google classifies your entity
# - "detailedDescription.url" = which Wikipedia article Google uses
# - Empty results = entity not recognized by Google KG yet
```

**From "not in KG" to "in KG" — steps:**
1. Create/improve Wikidata entry (P2671 maps to Google KG ID)
2. Get Wikipedia article (or improve existing)
3. Add consistent `Organization`/`Person` schema with `sameAs` links
4. Build citations in authoritative publications (3-5 minimum)
5. Add `mainEntityOfPage` on your homepage/about page
6. Wait 4-8 weeks and re-check API

### Entity-Based Content Strategy
Content planning at the entity level (not just keyword level):
- Identify which entities your topic cluster should connect to
- If your entity graph lacks connections to certain topics, create content that establishes those connections
- Use Google's NLP API to identify which entities top-ranking pages associate with your niche
- Entity gap analysis: entities your competitors' content mentions that yours doesn't

### Entity Monitoring & Maintenance
| Signal to monitor | How | Frequency |
|-------------------|-----|-----------|
| Knowledge Panel accuracy | Search brand name in Google | Monthly |
| Wikidata edits | Watch your Qxxxx page | Monthly |
| Brand mentions across web | Google Alerts, Mention.com | Weekly |
| AI citation tracking | Manual sampling in ChatGPT/Perplexity for target queries | Monthly |
| GSC brand queries growth | GSC Performance → filter brand terms | Monthly |

**Disputing incorrect Knowledge Panel info:**
1. Claim the panel via "Suggest an edit" (requires verified Google account)
2. Update Wikidata (Google sources from it)
3. Update Wikipedia if applicable
4. File feedback via Google Business Profile if it's a local business panel

---

## 9. Entity Audit

### Full Entity Health Check

```bash
# 1. Check Knowledge Panel existence
# Search: "[Brand Name]" → note panel presence

# 2. Check Wikipedia
curl -sL "https://en.wikipedia.org/wiki/[BrandName]" | grep -i '<title>'

# 3. Check Wikidata
curl -sL "https://www.wikidata.org/wiki/Special:Search?search=[BrandName]" | grep -i 'class="wb-itemlink"' | head -5

# 4. Check schema on homepage
curl -sL https://yourdomain.com | grep -A 50 '"@type": "Organization"'

# 5. Check sameAs consistency
# For each social profile: does the username/URL match what's in schema?

# 6. Check name consistency
# Google Brand Name in GSC → is it the same as schema name → same as Wikipedia?
```

### Entity Consistency Matrix

| Platform | Brand Name | URL | Description Match | Logo Match |
|---------|-----------|-----|-------------------|-----------|
| Website | ✅/❌ | - | ✅/❌ | ✅/❌ |
| Schema | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Google Business Profile | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Wikipedia | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Wikidata | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| LinkedIn | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Twitter/X | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Crunchbase | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |

**Any inconsistency = entity confusion signal for Google.**

---

## 10. Tools for Entity SEO

| Tool | Purpose | Pricing |
|------|---------|---------|
| Google Knowledge Graph API | Check if entity exists in KG | Free (API key) |
| Wikidata (wikidata.org) | Create/edit entity structured data | Free |
| Wikipedia | Highest-authority entity signal | Free (editorial standards apply) |
| InLinks | Entity analysis + NLP optimization | Paid |
| WordLift | AI entity markup + knowledge graph | Paid |
| Kalicube Pro | Entity audit + Knowledge Panel optimization | Paid |
| Dixon Jones Entity SEO | Entity-based SEO analysis | Paid |
| Schema Markup Validator | Validate Organization/Person schema | Free |
| Google Rich Results Test | Test entity schema output | Free |

---

## Output Format

### Entity SEO Audit Report

**Entity Name:** [Name]
**Entity Type:** [Organization/Person/Concept]
**Audit Date:** [Date]

#### Entity Recognition Status
| Platform | Status | Notes |
|---------|--------|-------|
| Google Knowledge Panel | ✅ Exists / ❌ Missing / ⚠️ Incorrect | [Details] |
| Wikipedia | ✅/❌ | [URL or "not found"] |
| Wikidata | ✅/❌ | [ID or "not found"] |
| Schema on site | ✅/❌/⚠️ | [Type present, issues if any] |

#### Entity Authority Score
| Signal Type | Present | Quality | Priority to Improve |
|------------|---------|---------|---------------------|
| Wikipedia presence | ✅/❌ | Good/Fair/Poor | 🔴/🟡/🟢 |
| Wikidata entry | ✅/❌ | Good/Fair/Poor | 🔴/🟡/🟢 |
| Press mentions (10+) | ✅/❌ | Good/Fair/Poor | 🔴/🟡/🟢 |
| Social profiles verified | ✅/❌ | Good/Fair/Poor | 🔴/🟡/🟢 |
| sameAs schema | ✅/❌ | Good/Fair/Poor | 🔴/🟡/🟢 |
| Author entity pages | ✅/❌ | Good/Fair/Poor | 🔴/🟡/🟢 |

#### Inconsistencies Found
[List any name/URL/description mismatches across platforms]

#### Priority Actions
| Priority | Action | Expected Impact | Effort |
|----------|--------|----------------|--------|
| 🔴 | [Action] | [Impact] | [Low/Med/High] |
