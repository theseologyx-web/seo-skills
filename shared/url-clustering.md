# URL Clustering

## Purpose

This file defines how the SEO system groups multiple URLs into a single content entity.  
Its goal is to prevent duplicated analysis, fragmented performance tracking, and incorrect conclusions caused by URL variations.

## Why this matters

The same content can exist under multiple URLs for many reasons:
- migration history
- platform changes
- redirect chains
- trailing slash variants
- parameters
- canonical duplicates
- JS-rendered versions
- old and new paths
- localization variants
- temporary testing URLs

If the system analyzes each URL independently when they actually represent the same content, the results will be fragmented and misleading.

## Core rule

A content cluster is the primary unit of analysis.  
A URL is only one representation of that content cluster.

## What belongs in a cluster

Include URLs that represent the same underlying intent or content entity when they are materially equivalent.

Typical examples:
- old URL and new URL after migration
- URL variants with and without trailing slash
- parameter variants that do not change the core content
- canonical duplicate URLs
- alternate paths that resolve to the same page
- rendered and non-rendered versions of the same content when they are equivalent
- legacy URLs that redirect to a current canonical page

## What should not be clustered

Do not cluster URLs when they are only superficially similar but serve different intents.

Examples:
- category page vs article page
- product page vs comparison page
- language versions with different markets and content
- tag page vs editorial hub page
- pages with materially different search intent

## Clustering inputs

Use the following signals when deciding whether URLs belong together:

### 1. HTTP status
- 200
- 301
- 302
- 404
- 410
- other relevant states

### 2. Redirect behavior
- direct redirect target
- redirect chain length
- final destination
- redirect consistency

### 3. Canonical behavior
- self-referential canonical
- canonical to another URL
- canonical conflict
- missing canonical
- canonical inconsistent with redirects

### 4. Content equivalence
- same topic
- same intent
- same main content
- same conversion goal
- same structured data purpose

### 5. Rendering equivalence
- HTML response
- rendered JS response
- visible content match
- navigation and metadata match

### 6. Analytics history
- shared traffic history
- traffic migration from old to new URL
- similar query footprint
- same landing page role over time

### 7. Business role
- same conversion objective
- same funnel stage
- same product or service role

## Clustering process

### Step 1: Identify all candidate URLs
Collect all URLs that might refer to the same topic, page, or content entity.

### Step 2: Validate technical relations
Check status codes, redirects, canonicals, and rendering behavior.

### Step 3: Compare content meaning
Confirm whether the URLs actually serve the same intent and content purpose.

### Step 4: Assign a cluster ID
Create a stable identifier for the entity.

### Step 5: Pick a primary URL
Select the canonical or preferred current URL as the main representative of the cluster.

### Step 6: Record legacy and alternative URLs
Keep all related variants in the cluster for traceability.

## Cluster types

### Stable cluster
The entity has a clear primary URL and minimal historical ambiguity.

### Migrated cluster
The entity changed URLs but retained the same underlying content purpose.

### Fragmented cluster
The entity exists in multiple weakly connected URL versions and needs cleanup.

### Broken cluster
The URLs are inconsistent, partially lost, redirected incorrectly, or no longer fully represent the content.

### Unknown cluster
The system cannot yet determine cluster membership with confidence.

## Cluster fields

Every cluster should ideally include:
- content_cluster_id
- entity_name
- primary_topic
- primary_url
- canonical_url
- related_urls
- legacy_urls
- redirected_urls
- indexation_state
- rendering_state
- status_summary
- confidence
- notes

## How to assign confidence

### High confidence
Use when:
- redirects and canonicals align
- content equivalence is clear
- analytics history supports the match

### Medium confidence
Use when:
- most signals align
- one or two signals are unclear
- content similarity is strong but not perfect

### Low confidence
Use when:
- URLs are partially broken
- content differs in uncertain ways
- redirect or canonical logic is inconsistent

## Output rules

Every cluster output should clearly separate:
- confirmed members
- probable members
- excluded URLs
- reasons for inclusion or exclusion

## Example

### Example 1
A blog post originally lived at `/blog/seo-checklist`.  
It later moved to `/resources/seo-checklist`, and the old URL now redirects correctly.

Interpretation:
- same content cluster
- legacy URL should remain in the cluster
- new URL is primary

### Example 2
A product page and its comparison page share keywords but serve different user intents.

Interpretation:
- do not cluster
- treat as separate content entities

## Maintenance rule

If a recurring URL pattern appears across projects, document it here so future skills can recognize it consistently.
