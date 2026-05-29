# seo-prioritization-framework

## Purpose

This skill converts project context into a concrete prioritization model.

Its job is to decide what matters most for this specific project, given:
- business model
- project stage
- technical stack
- conversion goals
- migration risk
- content risk
- measurement reliability
- resource constraints

The output of this skill is the shared priority logic used by downstream skills.

## Why this skill matters

Not all SEO issues deserve the same weight.

Examples:
- A noindex problem on a demo page matters more than a minor copy tweak.
- A migration redirect failure matters more than a small schema improvement.
- A content decay issue on a top landing page matters more than a low-value blog cleanup.
- A tracking break on a revenue-driving page matters more than a cosmetic UX issue.

This skill ensures the system ranks work based on actual impact rather than generic SEO checklists.

## When to use this skill

Use this skill when:
- project context has been collected
- the system needs project-specific weights
- multiple findings must be ranked against each other
- the project has tradeoffs between SEO, UX, conversion, migration, content, and measurement
- the downstream skills need a consistent decision framework

## Required input

This skill consumes:
- `project_context`

It may also use:
- `initial_risk_flags`
- `missing_information_list`

## Required output

This skill must produce:
- `priority_rules`

It may also produce:
- `scoring_adjustments`
- `priority_notes`
- `decision_constraints`

## Core responsibilities

This skill must:
- assign weights by project type
- define the dominant business objective
- identify which risks matter most
- adjust priorities for migration, content, or technical conditions
- establish how downstream skills should rank findings

## Default weighting logic

Use weighted logic, not a flat checklist.

Suggested factors:
- SEO impact
- business impact
- technical risk
- dependency weight
- effort

## Project-type adjustments

### 1. B2B SaaS
Increase weight for:
- conversion
- lead quality
- demo flow
- landing page clarity
- measurement reliability
- product and feature pages
- migration protection if launch is involved

### 2. Ecommerce
Increase weight for:
- category and product page performance
- indexation of revenue pages
- structured data
- conversion clarity
- internal linking
- crawl efficiency
- faceted navigation risks

### 3. Content/media
Increase weight for:
- content performance
- topic clustering
- freshness
- internal linking
- CTR
- search demand capture
- article-level quality

### 4. Local/service business
Increase weight for:
- location pages
- conversion
- local relevance
- trust signals
- measurement quality
- call/form tracking
- service intent matching

### 5. Migration project
Increase weight for:
- redirects
- canonical integrity
- traffic preservation
- backlink protection
- indexation continuity
- measurement continuity
- launch monitoring

### 6. Technically stable growth project
Increase weight for:
- content expansion
- authority
- GEO
- internal linking
- content freshness
- distribution support
- conversion improvements

## Priority decision rules

### Rule 1: Blocking issues come first
Any issue that blocks indexing, rendering, redirects, measurement, or the main conversion path should usually become P1.

### Rule 2: Business-critical pages outrank low-value pages
A problem on a revenue page or lead page matters more than the same problem on a low-value page.

### Rule 3: Migration risk outranks cosmetic optimization
If a migration is in progress or recently completed, preservation and validation outrank refinement work.

### Rule 4: Poor measurement lowers confidence
If measurement is unreliable, performance findings must be downweighted or labeled with lower confidence.

### Rule 5: High dependency raises priority
If many other fixes depend on a task being completed first, raise its priority.

### Rule 6: Low effort does not automatically mean high priority
A quick fix is only high priority if it produces meaningful business or SEO value.

## Priority bands

### P1
Use for issues that:
- block indexing
- break crawling or rendering
- break primary conversion paths
- destroy measurement
- compromise migration stability
- cause major authority loss

### P2
Use for issues that:
- strongly reduce growth
- suppress traffic or conversion on strategic assets
- create major content performance weakness
- create serious technical or structural inefficiency

### P3
Use for issues that:
- improve quality
- increase clarity
- support growth
- fix medium-impact weaknesses

### P4
Use for issues that:
- are minor
- are cosmetic
- are experimental
- are nice-to-have refinements

## Adjustment logic by stage

### Existing site with problems
Prioritize:
- blockers
- performance loss
- data integrity
- core conversions
- high-value content damage

### New site pre-launch
Prioritize:
- technical readiness
- page structure
- content readiness
- measurement setup
- migration readiness if applicable

### Post-launch growth
Prioritize:
- content performance
- internal linking
- authority building
- GEO readiness
- conversion optimization

### Migration
Prioritize:
- URL mapping
- redirect validation
- canonical consistency
- indexation continuity
- traffic preservation
- monitoring

## Output structure

`priority_rules` should include:
- project_type
- dominant_objective
- weight_model
- priority_bands
- blocking_conditions
- stage_adjustments
- business_constraints
- notes

## Example

### Example 1
Project:
- B2B SaaS
- new site launching
- goal is demo requests
- React stack
- GSC and GA4 available

Priority logic:
- conversion pages and measurement carry high weight
- rendering issues become severe
- content quality matters, but only after technical readiness and main funnel clarity

### Example 2
Project:
- site migration for a content-heavy site
- traffic preservation is the main goal

Priority logic:
- redirects, canonical consistency, and traffic preservation become P1
- content refreshes and minor UX improvements are deferred until stabilization

## Tone and behavior

This skill should be decisive, structured, and explicit.
It should not merely repeat the project context; it should transform it into a ranking system.

## Maintenance rule

If repeated project patterns appear, update the weight model so future prioritization remains consistent.
