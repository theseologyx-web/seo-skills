# seo-audit-general

## Purpose

This skill performs a broad, exhaustive SEO audit of the website at the sitewide level.

Its purpose is to identify technical, structural, content, UX, accessibility, schema, and conversion issues that affect organic performance and business outcomes.

## Why this skill matters

A sitewide audit is the system's broadest diagnostic layer.

It should reveal:
- what blocks discovery
- what blocks indexation
- what blocks rendering
- what weakens architecture
- what weakens internal linking
- what weakens content quality
- what weakens conversion
- what weakens accessibility
- what weakens structured data
- what weakens measurement reliability

This skill is not a replacement for URL-level review or performance analysis.
It is the broad map that identifies the main system-level issues.

## When to use this skill

Use this skill when:
- the user wants an exhaustive audit
- the site has broad SEO or UX problems
- the project is in recovery mode
- the site has technical complexity
- the site is React, Angular, Next.js, WordPress, or another stack with potential rendering or architecture issues
- the user wants a holistic diagnosis before action planning

## Required input

This skill consumes:
- `project_context`
- `priority_rules`
- optional `content_performance_findings`
- optional `content_clusters`

It may also use:
- crawl exports
- rendered HTML
- sitemap data
- robots data
- schema data
- technical logs
- analytics data
- UX observations
- conversion flow data

## Required output

This skill must produce:
- `sitewide_findings`

## Core responsibilities

This skill must evaluate the site across all major SEO dimensions:
- crawling
- indexation
- rendering
- architecture
- internal linking
- on-page quality
- content quality
- UX
- accessibility
- conversion
- structured data
- authority signals
- measurement reliability

## Audit areas

### 1. Crawling
Check:
- robots.txt behavior
- crawl depth
- discoverability of important pages
- internal link exposure
- orphan content
- crawl traps
- sitemap support

### 2. Indexation
Check:
- indexable vs non-indexable pages
- noindex usage
- canonical logic
- duplicate URL behavior
- excluded pages
- soft 404 behavior
- indexation consistency

### 3. Rendering
Check:
- whether critical content is visible in rendered output
- whether JS-dependent content is discovered properly
- whether metadata is rendered correctly
- whether links are accessible to crawlers
- whether navigation and main content are exposed properly

### 4. Architecture
Check:
- hierarchy
- URL structure
- topic organization
- hub and spoke logic
- depth of important pages
- inconsistent templates or path logic

### 5. Internal linking
Check:
- contextual links
- navigation links
- footer links
- anchor text quality
- link equity distribution
- orphaned or underlinked pages

### 6. On-page
Check:
- titles
- meta descriptions
- headings
- heading hierarchy
- topical alignment
- uniqueness
- page intent match

### 7. Content quality
Check:
- depth
- usefulness
- naturality
- specificity
- trust signals
- differentiation
- topic coverage

### 8. UX
Check:
- navigation clarity
- mobile usability
- content readability
- task completion
- friction
- clarity of page purpose
- CTA visibility

### 9. Accessibility
Check:
- labels
- contrast
- semantic structure
- keyboard accessibility
- alt text
- heading order
- interactive control clarity

### 10. Conversion
Check:
- CTA clarity
- funnel alignment
- lead flow
- form friction
- trust signals
- page-to-conversion fit

### 11. Structured data
Check:
- schema presence
- schema validity
- schema relevance
- schema consistency with visible content
- rich result opportunities

### 12. Authority
Check:
- backlink preservation on important assets
- legacy signals
- brand trust indicators
- external credibility markers

### 13. Measurement
Check:
- GSC presence
- GA4 presence
- event setup
- source reliability
- tracking continuity
- post-migration data continuity if relevant

## Audit methodology

### Step 1: Establish project context
Confirm the business, stage, stack, and main goals.

### Step 2: Determine the main risk profile
Decide whether the site is mainly:
- technically blocked
- performance-declining
- migration-exposed
- content-weak
- conversion-weak
- measurement-weak
- growth-ready but underdeveloped

### Step 3: Review broad site signals
Scan the sitewide systems first:
- crawl
- indexation
- rendering
- architecture
- linking
- templates
- measurement

### Step 4: Identify high-impact findings
Focus on issues that affect many pages, strategic pages, or core flows.

### Step 5: Assign severity and priority
Use the shared scoring and prioritization frameworks.

### Step 6: Group findings by root cause
Avoid repeating the same issue in multiple forms.

### Step 7: Connect to content clusters when relevant
If a sitewide issue affects a specific cluster, reference that cluster.

## Finding types

### Critical blocker
Issues that stop the site from being discovered, indexed, rendered, measured, or converted properly.

### Structural weakness
Issues that do not fully block performance but weaken the site systemically.

### Content weakness
Issues that reduce quality, intent alignment, depth, or search usefulness.

### UX weakness
Issues that reduce usability, clarity, or task completion.

### Conversion weakness
Issues that reduce business value from organic traffic.

### Opportunity
Areas where the site is not broken, but significant upside remains.

## Output fields

Every sitewide finding should include:
- finding_id
- title
- description
- primary_dimension
- secondary_dimension
- affected_scope
- severity
- confidence
- score
- priority_tier
- evidence
- recommended_action
- owner_skill
- related_content_cluster_ids

## Grouping rules

### Rule 1: One root cause, one finding
Do not create multiple findings for the same underlying issue unless the affected scopes are clearly different.

### Rule 2: Sitewide issues outrank isolated issues
If an issue affects many pages or critical templates, raise its priority.

### Rule 3: Strategic pages matter more
If an issue affects money pages, lead pages, or primary funnels, increase priority.

### Rule 4: Rendering and indexation issues are high urgency
Especially on JS-heavy sites or complex stacks.

### Rule 5: Measurement problems must be identified clearly
Do not interpret performance blindly if the data is unreliable.

## Example

### Example 1
A React site has key content visible to users but incomplete in rendered HTML, and some metadata is inconsistently exposed.

Interpretation:
- primary dimension: Rendering
- secondary dimension: Indexation
- severity: high or critical depending on scope

### Example 2
A WordPress site has thin content, weak internal linking, and inconsistent title patterns across many articles.

Interpretation:
- primary dimension: Content quality
- secondary dimension: On-page or Internal linking depending on root cause

## Tone and behavior

This skill should sound broad, disciplined, and systematic.
It should avoid overfitting to one page when the issue is actually sitewide.

## Maintenance rule

If recurring architecture, rendering, or content patterns appear across projects, update this skill to reflect them.
