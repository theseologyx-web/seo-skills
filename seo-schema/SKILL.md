---
name: seo-schema
description: >
  Detect, validate, and generate Schema.org structured data. JSON-LD format
  preferred. Use when user says "schema", "structured data", "rich results",
  "JSON-LD", or "markup".
  Not for deciding which content types need schema (strategy) — use seo-content-types.
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Schema Markup Analysis & Generation

## CRITICAL RULE: Schema Must Match Page Content

**Schema is a claim. The page content must prove that claim.**

Before recommending or validating any schema, verify that the page text supports every schema property:

| Schema Property | Required Page Evidence |
|----------------|----------------------|
| `aggregateRating` | Visible star rating or review summary on page |
| `datePublished` / `dateModified` | Date visible to user (not hidden) |
| `author` | Author name or byline visible on page |
| `offers.price` | Price visible on page |
| `openingHours` | Hours visible somewhere on page or site |
| `telephone` | Phone number visible on page |
| `description` | Content matches the description claim |
| `applicationCategory` | Page content confirms the category claim |
| `image` | Image actually exists and loads on the page |
| `review.reviewBody` | Full review text is on the page |

**Google penalizes schema that misrepresents page content.** If a property is declared in schema but NOT visible on the page → flag as misrepresentation risk, not just a recommendation.

---

## Content-Type → Schema Mapping

When auditing a page, first identify its content type, then check which schemas apply and whether content supports them.

### SaaS / Software Product Pages

| Page | Primary Schema | Secondary Schema |
|------|---------------|-----------------|
| Homepage | `Organization` + `WebSite` + `SearchAction` | `SoftwareApplication` or `WebApplication` |
| Main product/feature page | `SoftwareApplication` or `WebApplication` | `Offer`, `AggregateRating` |
| Industry/use-case landing | `Service` | `Offer`, `BreadcrumbList` |
| Pricing page | `Offer` (multiple) | `SoftwareApplication` |
| Blog post | `Article` or `BlogPosting` | `BreadcrumbList`, `Person` (author) |
| About page | `Organization` + `Person` (team) | `ContactPage` |
| Contact page | `ContactPage` + `Organization` | LocalBusiness (if physical office) |
| FAQ page | `FAQPage` (GEO only — not Google rich results for commercial sites) | — |
| Checklist / template page | `HowTo` (deprecated for rich results, keep for semantics) or `Article` | — |
| Integration page | `SoftwareApplication` + `Service` | — |

### E-commerce / Product Sites

| Page | Primary Schema | Secondary Schema |
|------|---------------|-----------------|
| Product page | `Product` + `Offer` | `AggregateRating`, `Review`, `BreadcrumbList` |
| Product variants | `ProductGroup` + `Product` | `Offer` per variant |
| Category page | `ItemList` | `BreadcrumbList` |
| Review page | `Review` + `AggregateRating` | `Product` |

### Local Business / Services

| Page | Primary Schema | Secondary Schema |
|------|---------------|-----------------|
| Homepage | `LocalBusiness` + `Organization` | `WebSite`, `SearchAction` |
| Location page | `LocalBusiness` | `GeoCoordinates`, `OpeningHoursSpecification` |
| Service page | `Service` | `Offer`, `AggregateRating` |

### Content / Publisher Sites

| Page | Primary Schema | Secondary / Embedded Schema |
|------|---------------|------------------------------|
| Article / blog post | `Article` or `BlogPosting` | `BreadcrumbList`, `Person`, `VideoObject`*, `AudioObject`* |
| News article | `NewsArticle` | `BreadcrumbList`, `VideoObject`* |
| Author profile | `ProfilePage` + `Person` | — |
| Video page (video = contenido principal) | `VideoObject` | `BreadcrumbList` |
| Article/guía con video embebido | `Article` | `VideoObject` (vía propiedad `video`), `BreadcrumbList` |
| Course | `Course` + `CourseInstance` | `Offer`, `Organization`, `VideoObject`* |
| Event | `Event` | `Offer`, `Place`, `Organization`, `VideoObject`* |
| Recipe | `Recipe` | `AggregateRating`, `ImageObject`, `VideoObject`*, `HowToStep` (vía `recipeInstructions`) |
| Job posting | `JobPosting` | `Organization` |

> `*` = añadir solo si el contenido multimedia existe y es relevante en la página. `VideoObject` puede anidarse como propiedad `video` del schema principal o como nodo adicional en el `@graph`. **No limitarlo a "páginas de video"** — cualquier página con un video relevante debe tenerlo.

### Schema Opportunity Detection — What to Look for in Page Content

When analyzing a page, scan for these content signals and recommend the corresponding schema:

| If page content contains... | Recommend schema |
|-----------------------------|-----------------|
| Star ratings, review count | `AggregateRating` + `Review` |
| Author name + bio | `Person` + `ProfilePage` |
| Price information | `Offer` |
| Software/app features list | `SoftwareApplication` or `WebApplication` |
| "How to" steps or numbered process | `Article` (HowTo deprecated for rich results) |
| Video embed | `VideoObject` |
| Event date/location | `Event` |
| Q&A format | `DiscussionForumPosting` (QAPage deprecated Jan 2026) |
| Job title + requirements | `JobPosting` |
| Course / curriculum content | `Course` + `CourseInstance` |
| Business hours | `OpeningHoursSpecification` |
| Physical address | `PostalAddress` → `LocalBusiness` |
| Recipe ingredients + steps | `Recipe` |
| Breadcrumb navigation visible | `BreadcrumbList` |
| Multiple products with variants | `ProductGroup` + `Product` |
| Team member profiles | `Person` (multiple) + `Organization` |

---

## Detection

1. Scan page source for JSON-LD `<script type="application/ld+json">`
2. Check for Microdata (`itemscope`, `itemprop`)
3. Check for RDFa (`typeof`, `property`)
4. Always recommend JSON-LD as primary format (Google's stated preference)
5. **Cross-check every detected schema property against visible page content**

## Validation

- Check required properties per schema type
- Validate against Google's supported rich result types
- Test for common errors:
  - Missing @context
  - Invalid @type
  - Wrong data types
  - Placeholder text
  - Relative URLs (should be absolute)
  - Invalid date formats
- Flag deprecated types (see below)

## Schema Type Status (as of Feb 2026)

Read `references/schema-types.md` for the full list. Key rules:

### ACTIVE (recommend freely):
Organization, LocalBusiness, SoftwareApplication, WebApplication, Product (with Certification markup as of April 2025), ProductGroup, Offer, Service, Article, BlogPosting, NewsArticle, Review, AggregateRating, BreadcrumbList, WebSite, WebPage, Person, ProfilePage, ContactPage, VideoObject, ImageObject, Event, JobPosting, Course, DiscussionForumPosting

### VIDEO & SPECIALIZED (recommend freely):
BroadcastEvent, Clip, SeekToAction, SoftwareSourceCode

See `schema/templates.json` for ready-to-use JSON-LD templates for these types.

> **JSON-LD and JavaScript rendering:** Per Google's December 2025 JS SEO guidance, structured data injected via JavaScript may face delayed processing. For time-sensitive markup (especially Product, Offer), include JSON-LD in the initial server-rendered HTML.

### RESTRICTED (only for specific sites):
- **FAQ**: ONLY for government and healthcare authority sites in Google Search (restricted Aug 2023). **However, still recommended for all sites** for ChatGPT, Perplexity, and Bing Copilot — these AI platforms use FAQPage schema for Q&A parsing and content extraction. Dual value even without Google rich results.

### DEPRECATED — Google Jan 2026:
- **Sitelinks SearchBox** (`WebSite` with `potentialAction: SearchAction`): Deprecated January 2026. Remove from existing implementations. Google shows search in Knowledge Panel automatically without schema.
- **Q&A schema (QAPage)**: Deprecated January 2026. No replacement. Use `FAQPage` for AI platform value. Remove existing QAPage from sites.

### DEPRECATED — earlier:
- **HowTo**: Rich results removed September 2023 (keep schema for GEO/AI platforms only)
- **SpecialAnnouncement**: Deprecated July 31, 2025
- **CourseInfo, EstimatedSalary, LearningVideo**: Retired June 2025
- **ClaimReview**: Retired from rich results June 2025
- **VehicleListing**: Retired from rich results June 2025
- **Practice Problem**: Retired from rich results late 2025
- **Dataset**: Retired from rich results late 2025 (still works for Dataset Search, not rich results)
- **Book Actions**: Deprecated then reversed, still functional as of Feb 2026 (historical note)

## Generation

When generating schema for a page:
1. Identify page type from content analysis
2. Select appropriate schema type(s)
3. Generate valid JSON-LD with all required + recommended properties
4. Include only truthful, verifiable data. Use placeholders clearly marked for user to fill
5. Validate output before presenting

## Common Schema Templates

### Organization
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "[Company Name]",
  "url": "[Website URL]",
  "logo": "[Logo URL]",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "[Phone]",
    "contactType": "customer service"
  },
  "sameAs": [
    "[Facebook URL]",
    "[LinkedIn URL]",
    "[Twitter URL]"
  ]
}
```

### LocalBusiness
```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "[Business Name]",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "[Street]",
    "addressLocality": "[City]",
    "addressRegion": "[State]",
    "postalCode": "[ZIP]",
    "addressCountry": "US"
  },
  "telephone": "[Phone]",
  "openingHours": "Mo-Fr 09:00-17:00",
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "[Lat]",
    "longitude": "[Long]"
  }
}
```

### Article/BlogPosting
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "[Title]",
  "author": {
    "@type": "Person",
    "name": "[Author Name]"
  },
  "datePublished": "[YYYY-MM-DD]",
  "dateModified": "[YYYY-MM-DD]",
  "image": "[Image URL]",
  "publisher": {
    "@type": "Organization",
    "name": "[Publisher]",
    "logo": {
      "@type": "ImageObject",
      "url": "[Logo URL]"
    }
  }
}
```

## Additional Schema Templates

### ProfilePage + Person (Author Bio Page — Feb 2026)
```json
{
  "@context": "https://schema.org",
  "@type": "ProfilePage",
  "dateCreated": "[YYYY-MM-DD]",
  "dateModified": "[YYYY-MM-DD]",
  "mainEntity": {
    "@type": "Person",
    "@id": "https://example.com/author/[slug]/#persona",
    "name": "[Full Name]",
    "jobTitle": "[Role]",
    "description": "[Bio with relevant expertise]",
    "image": {"@type": "ImageObject", "url": "[Photo URL]"},
    "url": "https://example.com/author/[slug]/",
    "sameAs": ["[LinkedIn URL]", "[Twitter URL]"]
  }
}
```

### Product with Certification (hasCertification — April 2025)
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[Product Name]",
  "hasCertification": {
    "@type": "Certification",
    "issuedBy": {"@type": "Organization", "name": "[Certifying Body]"},
    "name": "[Certification Name]",
    "certificationIdentification": "[Cert ID if available]"
  }
}
```
Supports: USDA Organic, Energy Star, FDA, CE, ISO certifications.

### DiscussionForumPosting (Forum / Reddit-style content)
```json
{
  "@context": "https://schema.org",
  "@type": "DiscussionForumPosting",
  "headline": "[Thread Title]",
  "text": "[Post content]",
  "datePublished": "[YYYY-MM-DD]",
  "author": {"@type": "Person", "name": "[Username]"},
  "url": "https://example.com/forum/thread/[slug]",
  "interactionStatistic": {
    "@type": "InteractionCounter",
    "interactionType": "https://schema.org/CommentAction",
    "userInteractionCount": 42
  }
}
```

## Entity Graph via @id Connections

Schema on individual pages is valuable, but the real E-E-A-T and AI citation signal comes from **connecting schemas across pages via @id**. Each entity should have one canonical @id URI that appears consistently.

```json
// Organization (on every page that references the brand)
{
  "@type": "Organization",
  "@id": "https://example.com/#organization",
  "name": "Example Corp",
  "url": "https://example.com"
}

// Article (references Organization via @id)
{
  "@type": "Article",
  "publisher": {"@id": "https://example.com/#organization"},
  "author": {"@id": "https://example.com/author/maria/#persona"}
}

// Person/Author (on the author bio page)
{
  "@type": "Person",
  "@id": "https://example.com/author/maria/#persona",
  "name": "María López",
  "worksFor": {"@id": "https://example.com/#organization"}
}
```

**Pattern:** Every major entity on your site gets one permanent `@id` URI. Reference it by `@id` instead of re-declaring the full object everywhere. This builds a coherent entity graph that AI systems use for attribution.

**Standard @id patterns:**
- Organization: `https://domain.com/#organization`
- Person/Author: `https://domain.com/author/[slug]/#persona`
- Website: `https://domain.com/#website`
- Specific pages: `https://domain.com/[path]/#[type]`

## Schema Testing & Validation Workflow

### Tools
| Tool | Use for | URL |
|------|---------|-----|
| Google Rich Results Test | Verify eligibility for rich results | search.google.com/test/rich-results |
| Schema Markup Validator | Broad validation (non-Google schemas too) | validator.schema.org |
| Search Console Enhancements | Monitor errors in production (not pre-deployment) | GSC → Enhancements section |

### Pre-deployment validation process
1. Generate JSON-LD in the page
2. Paste raw HTML into Rich Results Test → check for errors
3. Also paste into Schema Markup Validator → catches schema.org issues Rich Results Test misses
4. Fix all errors (not just warnings) before deploying
5. After deployment: monitor Search Console Enhancements for 2-4 weeks

### Common validation errors and fixes
| Error | Cause | Fix |
|-------|-------|-----|
| Missing required property | Required field absent | Add field with visible page content |
| Value is not valid | Wrong data type (e.g., string instead of URL) | Check schema.org property type |
| Unparseable structured data | JSON syntax error | Validate JSON syntax (jsonlint.com) |
| Content mismatch | Schema claims property not visible on page | Add visible content or remove property |
| Blocked by robots.txt | Google can't crawl schema page | Unblock in robots.txt |

## Site-Level Schema Architecture

Every site type has a baseline schema layer. Apply across ALL pages, then add page-specific schema on top.

### Blog / Publisher
| Page | Schema |
|------|--------|
| All pages (global) | `Organization` + `WebSite` (in site template) |
| Homepage | + `WebPage` |
| Blog post | `Article`/`BlogPosting` + `BreadcrumbList` + `Person` (author) |
| Author bio | `ProfilePage` + `Person` |
| Category page | `CollectionPage` + `BreadcrumbList` |

### SaaS
| Page | Schema |
|------|--------|
| All pages (global) | `Organization` + `WebSite` |
| Homepage | + `SoftwareApplication` or `WebApplication` |
| Feature/product page | `SoftwareApplication` + `Offer` |
| Blog post | `Article` + `BreadcrumbList` |
| Pricing | `Offer` × N (per plan) |
| About | `Organization` + `Person` (team) |

### E-commerce
| Page | Schema |
|------|--------|
| All pages (global) | `Organization` + `WebSite` |
| Product page | `Product` + `Offer` + `AggregateRating` + `BreadcrumbList` |
| Category page | `CollectionPage` + `BreadcrumbList` |
| Homepage | `WebPage` |

## Output

- `SCHEMA-REPORT.md`: detection and validation results
- `generated-schema.json`: ready-to-use JSON-LD snippets

### Validation Results
| Schema | Type | Status | Issues |
|--------|------|--------|--------|
| ... | ... | ✅/⚠️/❌ | ... |

### Recommendations
- Missing schema opportunities
- Validation fixes needed
- Generated code for implementation

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable | Report connection error with status code. Suggest verifying URL and checking if the page requires authentication. |
| No schema markup found | Report that no JSON-LD, Microdata, or RDFa was detected. Recommend appropriate schema types based on page content analysis. |
| Invalid JSON-LD syntax | Parse and report specific syntax errors (missing brackets, trailing commas, unquoted keys). Provide corrected JSON-LD output. |
| Deprecated schema type detected | Flag the deprecated type with its retirement date. Recommend the current replacement type or advise removal if no replacement exists. |
