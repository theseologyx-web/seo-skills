# seo-migration-strategy

## Purpose

This skill plans and monitors SEO migrations before, during, and after launch.

Its purpose is to preserve traffic, indexation, authority, and conversion continuity when a site moves, restructures, redesigns, or changes platform.

## Why this skill matters

Migrations are high-risk events.

A migration can cause:
- traffic loss
- indexation loss
- redirect failures
- canonical conflicts
- backlink equity loss
- measurement breaks
- rendering issues
- conversion drops
- temporary or long-term instability

This skill ensures migrations are treated as controlled SEO operations instead of simple site launches.

## When to use this skill

Use this skill when:
- a migration is planned
- a migration is in progress
- a migration just launched
- traffic dropped after a move
- URLs changed
- the CMS or framework changed
- the domain changed
- the site redesign may affect SEO
- post-migration monitoring is needed

## Required input

This skill consumes:
- `project_context`
- `priority_rules`
- `content_clusters`
- backlink data if available
- old URL inventory
- new URL inventory
- migration mapping data if available

It may also use:
- crawl exports
- canonical reports
- redirect reports
- analytics data
- GSC data
- launch logs

## Required output

This skill must produce:
- `migration_risk_register`
- `migration_action_plan`

## Core responsibilities

This skill must:
- identify migration type
- assess migration risk
- map old URLs to new URLs
- protect content entity identity
- validate redirect logic
- validate canonical logic
- protect backlink equity
- monitor indexation continuity
- monitor traffic continuity
- detect post-launch failures
- define stabilization actions

## Migration phases

### Phase 1: Pre-migration
Plan and validate before launch.

Tasks:
- inventory old URLs
- inventory target URLs
- map content clusters
- map redirects
- validate canonical strategy
- identify high-value pages
- identify linked pages with backlink history
- define measurement plan
- define QA checklist
- define rollback path if needed

### Phase 2: Launch
Release carefully.

Tasks:
- confirm redirects
- verify status codes
- confirm preferred URLs are live
- confirm canonical behavior
- confirm sitemap behavior
- confirm main content renders
- confirm analytics and tracking
- confirm core templates are intact

### Phase 3: Post-launch
Monitor early performance.

Tasks:
- compare before and after
- detect traffic loss
- detect indexation gaps
- detect redirect failures
- detect canonical drift
- detect rendering problems
- detect measurement loss
- detect conversion loss

### Phase 4: Stabilization
Recover and optimize.

Tasks:
- fix redirect chains
- repair missing mappings
- update internal links
- recover authority
- re-evaluate cluster performance
- correct measurement gaps
- support ranking and traffic recovery

## Migration types

### 1. URL migration
Same content, new URL.

### 2. Domain migration
New domain, same or mostly same content.

### 3. Platform migration
New CMS, new stack, or new rendering model.

### 4. Redesign migration
Major template or UX changes.

### 5. Structural migration
New information architecture or new path logic.

### 6. Combined migration
More than one of the above at once.

## Required checks

### Redirect integrity
- one-hop redirects when possible
- correct destination relevance
- no redirect loops
- no large-scale missing redirects

### Canonical integrity
- canonical matches intended final URLs
- canonical does not conflict with redirects
- canonical does not point to obsolete versions

### Sitemap integrity
- final URLs only
- no obsolete URLs
- no mixed old and new versions

### Measurement continuity
- GSC still usable
- GA4 still usable
- events still firing
- conversions still tracked
- source naming still coherent

### Content continuity
- entity-level continuity preserved
- major content blocks retained when intended
- topic role remains stable unless replacement is intentional

### Authority continuity
- legacy backlinks still resolve effectively
- important referring URLs do not die without a good destination
- link equity is preserved as much as possible

## Risk levels

### Low
Minor move with clean mapping and limited traffic exposure.

### Medium
Moderate structural changes or partial content reorganization.

### High
Large URL shifts, important page changes, or mixed platform redesigns.

### Critical
Domain change, major replatforming, or high-volume pages at risk.

## Failure modes

- missing redirect mapping
- redirect chains
- irrelevant redirect targets
- canonical mismatch
- old URLs still indexed unexpectedly
- sitemap not updated
- internal links still pointing to old URLs
- tracking breakage
- measurement gaps
- content split across new and old versions
- backlink value not preserved

## Output fields

Every migration finding should include:
- risk_id or action_id
- affected asset or cluster
- migration_type
- risk_type
- description
- severity
- likelihood
- mitigation
- owner
- status
- recommended_action
- notes

## Example

### Example 1
A blog moves from `/blog/topic` to `/resources/topic`.

Interpretation:
- URL migration
- same content entity
- redirect and canonical validation required
- post-launch monitoring needed

### Example 2
A site moves from one domain to another while also changing CMS and templates.

Interpretation:
- combined migration
- high or critical risk
- requires detailed pre-launch, launch, and post-launch control

## Tone and behavior

This skill should be cautious, operational, and exact.
It should treat migrations as controlled systems, not casual launches.

## Maintenance rule

If new recurring migration patterns appear, update this file so the framework stays current.
