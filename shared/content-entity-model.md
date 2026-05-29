# Content Entity Model

## Purpose

This file defines what the SEO system means by a content entity.  
A content entity is the real object of analysis when multiple URLs, analytics records, and historical changes refer to the same underlying piece of content or topic.

## Why this matters

SEO systems often fail when they treat URLs as the final unit of analysis.  
In practice, one article, landing page, service page, or product page may have multiple URL versions over time.  
A content entity model prevents the system from misreading migrations, redirects, canonical changes, and performance changes as unrelated events.

## Core definition

A content entity is the logical content object behind one or more URLs, one or more tracking records, and one or more historical states.

Examples:
- one blog post across old and new URLs
- one service page with trailing slash variants
- one landing page before and after redesign
- one topic hub with updated content blocks over time
- one product or feature page with multiple legacy URL forms

## Content entity components

A content entity may include:

- entity_id
- entity_name
- primary_topic
- search_intent
- business_role
- funnel_stage
- primary_url
- canonical_url
- legacy_urls
- redirected_urls
- related_urls
- content_version_history
- technical_state
- indexation_state
- rendering_state
- analytics_history
- conversion_role
- notes

## Entity identity rules

### 1. Same intent, same entity
If multiple URLs represent the same user intent and the same content purpose, they belong to the same entity.

### 2. Same business role, same entity
If the page exists to serve the same business task and the same topic purpose, it is usually the same entity even if the URL changed.

### 3. Different intent, different entity
If two pages may share keywords but solve different problems or serve different stages of the funnel, they should not be merged into one entity.

### 4. Historical versions remain part of the entity
If a page moved, was redesigned, or changed URL, the old versions stay associated with the same entity when they still represent the same content lineage.

## Entity types

### Evergreen entity
A stable content asset with a long lifespan.

Examples:
- service page
- category page
- core landing page
- feature page

### Temporal entity
A content asset tied to a specific time or campaign window.

Examples:
- campaign page
- seasonal page
- event page
- announcement page

### Migrated entity
A content asset that changed URL, platform, or structure but remains the same business object.

### Clustered entity
A content object made of multiple interrelated URLs that should be analyzed together.

### Fragmented entity
A content object that is split across multiple URLs and needs cleanup or consolidation.

## Entity state fields

Every entity should include:
- entity_id
- current_state
- historical_state
- technical_risk
- performance_state
- content_quality_state
- conversion_state
- measurement_state

## State interpretation

### Current state
What the entity looks like now.

### Historical state
What the entity looked like before major changes.

### Technical risk
Risk related to indexing, redirects, rendering, or structure.

### Performance state
How the entity is performing in search and business outcomes.

### Content quality state
How useful, natural, or differentiated the content is.

### Conversion state
How well the entity supports leads, demos, signups, or sales.

### Measurement state
How reliable the data is for this entity.

## Relationship rules

### Entity to URL
One entity can map to many URLs.

### Entity to analytics
One entity can map to many analytics records over time.

### Entity to finding
One entity can have multiple findings across different skills.

### Entity to cluster
A cluster is the operational grouping of URLs.  
The content entity is the conceptual object behind that grouping.

## Analysis rules

The system should always ask:
- what is the entity?
- what is the canonical current representation?
- what historical URLs belong to it?
- what changed technically?
- what changed editorially?
- what changed in traffic or conversion?
- what changed in measurement?

## Example

### Example 1
A service page moved from `/seo-audit` to `/services/seo-audit`.

Interpretation:
- same content entity
- new primary URL
- old URL remains a legacy URL
- performance should be tracked at entity level

### Example 2
A blog article and a category landing page target similar keywords.

Interpretation:
- not the same entity
- separate content objects
- may be related but should not be merged

## Output standard

Whenever a skill references an entity, it should specify:
- entity_id
- entity_name
- primary_url
- cluster relation
- performance status
- technical status
- conversion role

## Maintenance rule

If a page type or content structure becomes common enough across projects, it can be defined as a named entity pattern in this file.
