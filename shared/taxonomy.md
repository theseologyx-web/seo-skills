# Taxonomy

## Purpose

This file defines the shared language for all SEO skills in the system.  
Its job is to make sure every audit, content review, performance analysis, migration plan, and growth strategy uses the same categories, the same labels, and the same logic when describing problems and opportunities.

## Why this matters

Without a shared taxonomy, one skill may call something a technical issue, another may call it a content issue, and another may treat it as a UX problem.  
That creates duplicate findings, inconsistent priorities, and confusing handoffs between skills.  
A shared taxonomy avoids that by creating one common map for the entire system.

## Core dimensions

Every finding should map to one primary dimension and may optionally map to one secondary dimension.

### 1. Crawling
Use when the issue affects whether bots can reach or discover content.

Examples:
- blocked by robots.txt
- broken internal links
- excessive crawl depth
- infinite spaces or crawl traps
- weak discovery paths

### 2. Indexation
Use when the issue affects whether a URL or content entity can be indexed or kept indexed.

Examples:
- noindex
- canonical conflicts
- duplicate URLs
- soft 404
- excluded pages
- indexation loss after migration

### 3. Rendering
Use when the issue affects how search engines interpret content that depends on JavaScript or client-side rendering.

Examples:
- content missing from rendered HTML
- delayed JS rendering
- hidden critical content
- incomplete metadata in rendered output
- JS-only navigation not exposing links properly

### 4. Architecture
Use when the issue is about structure, hierarchy, and discoverability across the site.

Examples:
- poor category structure
- shallow or inconsistent hierarchy
- important content buried too deep
- unclear hub-and-spoke organization
- poor URL architecture

### 5. Internal linking
Use when the issue is about how the site distributes authority and discovery through links.

Examples:
- missing links to important pages
- weak anchor text
- orphan pages
- inconsistent navigation
- poor contextual linking

### 6. On-page
Use when the issue is page-level SEO optimization.

Examples:
- weak title tag
- poor meta description
- missing or duplicated H1
- heading hierarchy problems
- weak topical alignment
- thin content on a page

### 7. Content quality
Use when the issue is about usefulness, clarity, originality, depth, or trustworthiness of the content.

Examples:
- generic copy
- AI-like wording without value
- missing examples
- weak topical coverage
- low differentiation
- poor answer quality

### 8. Content performance
Use when the issue is about how the content performs in organic search and business outcomes.

Examples:
- traffic decline
- CTR drop
- position decay
- conversion weakness
- opportunity keywords not used
- content that ranks but does not convert

### 9. UX
Use when the issue affects user experience in a way that can influence SEO or conversion.

Examples:
- confusing navigation
- low clarity of purpose
- bad mobile experience
- weak CTA hierarchy
- friction in task completion
- poor readability

### 10. Accessibility
Use when the issue affects accessibility standards or screen-reader clarity.

Examples:
- missing labels
- poor contrast
- broken heading order
- keyboard traps
- alt text problems
- inaccessible controls

### 11. Conversion
Use when the issue affects the ability to turn traffic into leads, demos, signups, or revenue.

Examples:
- weak CTA
- poor form design
- unclear value proposition
- too much friction
- bad trust signals
- misaligned landing page intent

### 12. Structured data
Use when the issue affects schema or machine-readable markup.

Examples:
- missing schema
- invalid schema
- schema not matching visible content
- schema type mismatch
- rich result opportunity missing

### 13. Authority
Use when the issue affects trust, backlink equity, mention quality, or external credibility.

Examples:
- lost backlinks
- weak referring domains
- unclaimed legacy link equity
- poor topical authority
- brand weak in search ecosystem

### 14. Migration
Use when the issue is caused by a site move, redesign, domain change, URL change, or platform migration.

Examples:
- redirect problems
- canonical drift
- traffic loss after launch
- old URLs not preserved
- migration mapping gaps
- indexation instability after move

### 15. GEO
Use when the issue affects visibility in AI-assisted search, generative answer surfaces, or entity-based retrieval.

Examples:
- weak generative visibility
- content not appearing in AI overviews
- poor entity-level retrieval
- unclear topical identity in generative contexts
- weak semantic chunking for generative surfaces
- poor brand or entity presence in AI-generated answers

### 16. AEO
Use when the issue affects how directly and effectively the content answers user questions in answer engines and AI-assisted search.

Examples:
- question-to-answer match is weak
- answer is buried too deep in the content
- FAQ sections are vague or not aligned with real queries
- content is hard to scan for quick answer extraction
- intent fit is weak for voice or conversational search
- answer placement does not support direct extraction

### 17. LLMO
Use when the issue affects how language models understand, summarize, and cite the content.

Examples:
- entity framing is unclear or inconsistent
- passage-level clarity is low
- content is hard to summarize without distortion
- citations are unlikely because claims lack support
- topic framing is vague or ambiguous
- machine readability of key facts is poor

### 18. AI Search Readiness
Use when the assessment covers whether a site, cluster, or set of URLs is ready for AI Search overall — as an umbrella evaluation across GEO, AEO, and LLMO.

Examples:
- site has not been evaluated for any AI Search dimension
- readiness gaps across multiple AI Search layers
- blocking issues preventing GEO, AEO, or LLMO performance
- pre-assessment before routing to GEO, AEO, or LLMO skills

### 19. Measurement
Use when the issue is about analytics, tracking, attribution, or data reliability.

Examples:
- missing GSC data
- broken GA4 events
- inconsistent conversion tracking
- no content-level performance mapping
- broken tagging after migration
- incomplete source of truth

## Secondary dimensions

A finding may also include a secondary dimension when the issue crosses categories.

Examples:
- A JavaScript rendering issue may be both `Rendering` and `Indexation`.
- A content refresh that improves traffic and conversion may be both `Content performance` and `Conversion`.
- A redirect issue after a site move may be both `Migration` and `Measurement`.
- A weak title tag that hurts CTR may be both `On-page` and `Content performance`.

## How to assign dimensions

Use the dimension that best describes the root cause, not just the symptom.

Rules:
- If the main problem is discovery, choose `Crawling`.
- If the main problem is inclusion in the index, choose `Indexation`.
- If the main problem is JS interpretation, choose `Rendering`.
- If the main problem is structure or information architecture, choose `Architecture`.
- If the main problem is page content quality, choose `Content quality`.
- If the main problem is performance trend, choose `Content performance`.
- If the main problem is user friction, choose `UX` or `Conversion` depending on the outcome.
- If the main problem is data loss or broken measurement, choose `Measurement`.
- If the issue is connected to a move or launch, choose `Migration`.
- If the main problem is generative surface visibility or entity-level AI retrieval, choose `GEO`.
- If the main problem is answer directness, question match, or extractable answer quality, choose `AEO`.
- If the main problem is passage clarity, entity framing, or citation readiness for language models, choose `LLMO`.
- If the evaluation is a broad AI Search readiness check across GEO, AEO, and LLMO, choose `AI Search Readiness`.

## Required metadata per finding

Every finding should include:
- primary dimension
- optional secondary dimension
- severity
- confidence
- business impact
- recommended action
- owner skill
- related content cluster ID when applicable

## Example mapping

### Example 1
Problem: a page exists in HTML but Google is not reliably seeing content rendered by React.

Mapping:
- primary dimension: Rendering
- secondary dimension: Indexation

### Example 2
Problem: a useful article lost traffic after a domain migration.

Mapping:
- primary dimension: Migration
- secondary dimension: Content performance

### Example 3
Problem: a blog post ranks but users do not convert.

Mapping:
- primary dimension: Content performance
- secondary dimension: Conversion

### Example 4
Problem: multiple URLs show the same content and split signals.

Mapping:
- primary dimension: Indexation
- secondary dimension: Migration

## Output standard

When a skill writes findings, it should use the taxonomy terms exactly as defined here.  
Do not invent new top-level categories unless the shared taxonomy is updated first.

## Maintenance rule

If a recurring problem appears often enough across projects, it can be promoted to its own subcategory, but only if it does not duplicate an existing dimension.