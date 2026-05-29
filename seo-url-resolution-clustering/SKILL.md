# seo-url-resolution-clustering

## Purpose

This skill resolves URL-level records into content clusters.

Its job is to determine which URLs belong to the same underlying content entity so that audit, performance, and migration analysis can happen at the correct level.

## Why this skill matters

A single page may exist in multiple URL forms across time or across the technical stack.

Examples:
- old URL and new URL
- slash and non-slash variants
- canonical duplicates
- migrated versions
- rendered and non-rendered equivalents
- URL versions split by platform changes

If the system does not cluster URLs correctly, it will:
- misread performance changes
- duplicate findings
- undercount authority
- misdiagnose migration issues
- split analytics across the wrong pages

## When to use this skill

Use this skill when:
- URLs may represent the same content entity
- a site migration has occurred or is planned
- old URLs still exist
- canonical or redirect behavior is unclear
- content performance must be analyzed by page lineage
- analytics data may be fragmented across URL versions

## Required input

This skill consumes:
- `project_context`
- URL lists from crawls, GSC, GA4, or other sources

It may also use:
- raw redirects
- canonical signals
- rendered HTML
- JS rendering output
- historical URL records
- backlink destination records

## Required output

This skill must produce:
- `content_clusters`

## Core responsibilities

This skill must:
- identify candidate URL groups
- validate whether URLs represent the same content entity
- detect legacy and current URL relationships
- assign stable cluster IDs
- identify primary URLs
- preserve historical URL lineage
- flag uncertain or broken clusters

## Clustering signals

### 1. HTTP status
Check whether the URL returns:
- 200
- 301
- 302
- 404
- 410
- other relevant states

### 2. Redirect behavior
Check:
- destination URL
- number of hops
- redirect consistency
- redirect relevance

### 3. Canonical behavior
Check:
- self-referential canonical
- canonical to another URL
- canonical conflict
- missing canonical
- canonical consistency with redirect path

### 4. Content equivalence
Check whether the pages share:
- same intent
- same main topic
- same business role
- same conversion goal
- same overall content meaning

### 5. Rendering equivalence
Check whether:
- content appears in HTML
- content appears after JS rendering
- metadata is exposed properly
- navigation links are discoverable

### 6. Analytics history
Check:
- shared traffic history
- traffic migration over time
- query overlap
- page lineage consistency

### 7. Business role
Check:
- same funnel stage
- same offer or product role
- same conversion purpose

## Clustering rules

### Rule 1: Group by entity, not by superficial similarity
If pages share keywords but solve different problems, do not cluster them.

### Rule 2: Respect content intent
If the intent differs materially, treat the URLs as separate entities.

### Rule 3: Legacy URLs stay attached to the entity
A redirected or retired URL should remain associated with the cluster if it still belongs to that content lineage.

### Rule 4: Pick one primary URL
Each cluster should have one current preferred URL.

### Rule 5: Preserve traceability
Do not delete legacy URLs from the cluster history.

### Rule 6: Flag uncertainty
If the system cannot confidently decide, mark the cluster as uncertain instead of forcing a match.

## Cluster types

### Stable cluster
A clear entity with strong URL lineage and minimal ambiguity.

### Migrated cluster
A content entity that moved URLs but remains the same underlying object.

### Fragmented cluster
A content entity split across multiple weakly connected URLs.

### Broken cluster
A cluster with redirect, canonical, or indexation inconsistencies.

### Uncertain cluster
A cluster that needs human review or additional data.

## Cluster fields

Each cluster should include:
- content_cluster_id
- entity_name
- primary_topic
- primary_url
- canonical_url
- related_urls
- legacy_urls
- redirected_urls
- status_summary
- indexation_state
- rendering_state
- confidence
- notes

## Output logic

The output should clearly state:
- confirmed member URLs
- probable member URLs
- excluded URLs
- reasons for inclusion
- reasons for exclusion
- confidence level

## Example

### Example 1
A blog post moved from `/blog/seo-tips` to `/resources/seo-tips`.

Interpretation:
- same cluster
- new primary URL
- old URL becomes a legacy URL
- performance should be tracked at cluster level

### Example 2
A category page and a product page share similar keywords.

Interpretation:
- do not cluster
- different intent and different content entity

## Tone and behavior

This skill should be analytical and conservative.
It should not force URL groups together without enough evidence.

## Maintenance rule

If new recurring URL patterns appear in projects, document them here so future clustering remains consistent.
