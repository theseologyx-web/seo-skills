# Migration Framework

## Purpose

This file defines the shared framework for handling site migrations, URL changes, redesigns, replatforming, and launch events across the SEO system.  
Its purpose is to preserve traffic, indexation, authority, and conversion continuity while the site changes.

## Why this matters

Migrations are one of the highest-risk events in SEO.  
A good migration is not only about redirects.  
It also requires canonical consistency, content mapping, crawl continuity, indexation stability, measurement continuity, and post-launch monitoring.

## Migration types

### 1. URL migration
The content stays the same or mostly the same, but the URL changes.

### 2. Domain migration
The site moves from one domain to another.

### 3. Platform migration
The CMS or technical stack changes.

### 4. Redesign migration
The site changes templates, layout, or UX significantly.

### 5. Replatforming with content changes
The site changes platform and content structure at the same time.

### 6. Structural migration
The information architecture, paths, or content hierarchy changes.

## Core migration phases

### Phase 1: Pre-migration
Plan the move before it happens.

Goals:
- map old URLs to new URLs
- identify content clusters
- protect backlinks
- document canonical targets
- validate tracking
- estimate risk
- define rollback logic
- prepare launch checklist

### Phase 2: Launch
Release the new version carefully.

Goals:
- confirm redirects
- verify canonical behavior
- confirm sitemap updates
- validate robots directives
- verify core templates
- confirm main content renders properly
- confirm tracking still works

### Phase 3: Post-migration
Monitor what happened after launch.

Goals:
- detect traffic loss
- detect indexation issues
- identify redirect failures
- monitor ranking and CTR changes
- confirm Google is finding the right pages
- catch soft 404 behavior
- detect canonical drift

### Phase 4: Stabilization
Recover missed signals and optimize the new state.

Goals:
- fix broken mappings
- repair redirect chains
- recover legacy backlinks
- update internal links
- clean up orphan pages
- recheck indexing
- strengthen content clusters
- verify measurement continuity

## Core migration controls

### Redirect mapping
Every old URL should map to the most relevant new URL.

Rules:
- use one-to-one mapping when possible
- avoid chains
- avoid irrelevant destination pages
- avoid redirecting many old URLs to a generic homepage unless absolutely necessary

### Canonical validation
Canonical tags must match the intended post-migration structure.

Rules:
- canonical should point to the preferred final URL
- canonical should not conflict with redirects
- canonical should not point to obsolete URLs

### Sitemap validation
The sitemap must reflect the live, preferred, indexable URLs.

Rules:
- remove obsolete URLs
- include key new URLs
- keep only final canonical URLs
- avoid mixing old and new versions

### Internal link cleanup
Internal links should point to the new preferred URLs.

Rules:
- update navigation
- update contextual links
- update footer and hub links
- reduce dependence on redirects

### Measurement continuity
Analytics and search measurement must remain stable.

Rules:
- confirm GSC property behavior
- confirm GA4 tracking
- confirm conversion events
- confirm tag firing
- preserve naming consistency where possible

### Backlink preservation
Legacy external links must not be wasted.

Rules:
- redirect old linked URLs to the best matching destination
- recover or update important backlinks when possible
- document high-value linking domains
- prioritize pages with strong authority history

## Required migration checks

Before launch:
- URL inventory complete
- URL mapping complete
- content cluster mapping complete
- redirect plan validated
- canonical plan validated
- sitemap plan validated
- tracking plan validated
- QA checklist prepared

At launch:
- redirects working
- key pages returning expected status codes
- rendered templates validated
- indexing directives correct
- analytics active
- no critical 404s on core assets

After launch:
- crawl and indexing checks
- redirect chain checks
- GSC performance checks
- GA4 behavior checks
- ranking trend checks
- conversion trend checks
- content cluster validation
- legacy backlink checks

## Risk categories

### Low risk
Minor URL adjustments with strong mapping and little traffic history.

### Medium risk
Moderate structural changes, template adjustments, or partial content moves.

### High risk
Large URL shifts, significant content restructuring, or important traffic pages affected.

### Critical risk
Domain moves, major replatforming, broken redirects, or key indexable pages at risk.

## Common failure modes

- redirect chains
- redirected pages to irrelevant destinations
- missing redirects
- canonical conflicts
- old URLs still indexed
- sitemap not updated
- internal links still pointing to old URLs
- tracking breakage
- measurement gaps
- content split across new and old versions
- backlink value not preserved

## Maintenance rule

If recurring migration patterns appear across projects, update this file to keep the framework current.
