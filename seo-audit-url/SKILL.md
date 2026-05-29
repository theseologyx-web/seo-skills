# seo-audit-url

## Purpose

This skill evaluates a single URL or a defined set of URLs at the page level.

Its purpose is to produce a precise, actionable diagnosis of what is happening on each page and what should be fixed.

## Why this skill matters

Sitewide audits are useful, but many problems are page-specific.

Examples:
- one landing page has a bad title tag
- one conversion page has a broken CTA
- one article has weak internal linking
- one URL has rendering problems
- one page is not aligned with search intent
- one page has poor accessibility
- one URL is part of a fragmented cluster

This skill lets the system examine a page in detail instead of only relying on broad site patterns.

## When to use this skill

Use this skill when:
- the user wants page-by-page analysis
- a shortlist of URLs needs review
- one page is underperforming
- the sitewide audit found page-specific issues
- performance or content quality must be analyzed at URL level
- a cluster needs page-specific diagnostics

## Required input

This skill consumes:
- `project_context`
- `priority_rules`
- optional `content_cluster_id`
- optional URL list

It may also use:
- rendered HTML
- raw HTML
- screenshots
- GSC data
- GA4 data
- schema data
- crawl data
- backlink context

## Required output

This skill must produce:
- `url_findings`

## Core responsibilities

This skill must evaluate each URL against the shared audit dimensions:
- crawling exposure
- indexability
- rendering
- on-page quality
- content quality
- internal linking
- UX
- accessibility
- conversion
- structured data
- measurement relevance

## URL evaluation checklist

### 1. URL identity
Check:
- final URL
- canonical URL
- redirect chain
- status code
- relation to content cluster

### 2. Indexability
Check:
- noindex
- canonical consistency
- duplicates
- soft 404 behavior
- exclusion signals

### 3. Rendering
Check:
- main content visibility
- metadata visibility
- JS dependency
- hidden or delayed content
- link discoverability

### 4. On-page
Check:
- title tag
- meta description
- H1
- heading hierarchy
- topical focus
- duplication

### 5. Content quality
Check:
- usefulness
- originality
- naturality
- depth
- clarity
- trust signals
- topic completeness

### 6. Internal linking
Check:
- inbound links
- outbound links
- anchor quality
- navigation support
- orphan risk

### 7. UX
Check:
- layout clarity
- readability
- mobile friendliness
- visual hierarchy
- task completion support

### 8. Accessibility
Check:
- semantic structure
- labels
- contrast
- heading order
- alt text
- keyboard accessibility

### 9. Conversion
Check:
- CTA clarity
- offer relevance
- form friction
- trust signals
- funnel fit

### 10. Structured data
Check:
- schema presence
- schema validity
- schema relevance
- schema consistency with content

### 11. Performance context
Check:
- GSC clicks, impressions, CTR, position if available
- GA4 sessions, engagement, conversions if available
- whether the URL is part of a larger content cluster

## Page-level logic

### Rule 1: Evaluate the page in context
Do not judge a URL in isolation if it belongs to a broader content cluster.

### Rule 2: Separate technical from editorial issues
A page can have strong content but weak rendering, or strong technical setup but weak content.

### Rule 3: Separate visibility from conversion
A page can rank but fail to convert, or convert well but need more visibility.

### Rule 4: Watch for URL lineage
If the page is a migrated or legacy URL, confirm that the URL history is not distorting the diagnosis.

### Rule 5: Be explicit about uncertainty
If data is incomplete, say so.

## Output fields

Every URL finding should include:
- finding_id
- url
- content_cluster_id if applicable
- title
- description
- primary_dimension
- secondary_dimension
- severity
- confidence
- score
- priority_tier
- evidence
- recommended_action

## Finding categories

### Healthy page
The page is functioning well with only minor tuning opportunities.

### Weak page
The page has meaningful issues but is not blocked.

### Blocked page
The page has one or more severe issues that limit discovery, indexation, rendering, or conversion.

### Opportunity page
The page is healthy but can be improved for higher performance.

### Uncertain page
Data is incomplete or contradictory.

## Example

### Example 1
A service page has a strong offer but the title tag is generic and the CTA is buried.

Interpretation:
- primary dimension: On-page
- secondary dimension: Conversion

### Example 2
A blog article is indexed but its key content is not visible in rendered HTML.

Interpretation:
- primary dimension: Rendering
- secondary dimension: Indexation

## Tone and behavior

This skill should be precise, page-specific, and evidence-based.
It should not overgeneralize sitewide patterns to the page unless there is strong support.

## Maintenance rule

If repeated page-level patterns appear across projects, update this skill so the page audit remains consistent.
