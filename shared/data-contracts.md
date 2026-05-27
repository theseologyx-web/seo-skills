# Data Contracts

## Purpose

This file defines the shared input and output objects used across the SEO skill system.  
Its goal is to make sure every skill can read, interpret, and pass data to other skills without ambiguity.

## Why this matters

Without shared contracts, one skill may describe a page one way, another may describe it differently, and a third may not know how to consume the output at all.  
Data contracts make the whole system interoperable.

## Contract principles

All shared objects must be:
- machine-readable
- human-readable
- reusable across skills
- stable in naming
- explicit in meaning

## Core shared objects

### `project_context`
Represents the business and technical context of the project.

Suggested structure:
- project_name
- business_model
- industry
- country
- target_market
- website_stage
- primary_goals
- primary_conversions
- secondary_conversions
- cms_or_framework
- rendering_model
- sitemap_status
- gsc_status
- ga4_status
- additional_data_sources
- audit_scope
- notes

### `priority_rules`
Represents the project-specific prioritization logic.

Suggested structure:
- project_type
- weighting_model
- conversion_weight
- migration_weight
- content_weight
- technical_weight
- authority_weight
- measurement_weight
- special_constraints
- notes

### `content_clusters`
Represents groups of URLs that map to the same content entity.

Suggested structure:
- content_cluster_id
- entity_name
- primary_topic
- canonical_url
- related_urls
- legacy_urls
- redirected_urls
- status_summary
- indexation_state
- rendering_state
- cluster_notes

### `sitewide_findings`
Represents findings from a full-site or broad audit.

Suggested structure:
- finding_id
- title
- description
- primary_dimension
- secondary_dimension
- affected_scope
- severity
- confidence
- score
- status
- priority_tier
- evidence
- recommended_action
- owner_skill
- related_content_cluster_ids

### `url_findings`
Represents findings tied to a specific URL.

Suggested structure:
- finding_id
- url
- content_cluster_id
- title
- description
- primary_dimension
- secondary_dimension
- severity
- confidence
- score
- status
- priority_tier
- evidence
- recommended_action

### `content_performance_findings`
Represents performance findings tied to a content entity or cluster.

Suggested structure:
- finding_id
- content_cluster_id
- entity_name
- traffic_trend
- impression_trend
- ctr_trend
- ranking_trend
- engagement_trend
- conversion_trend
- measurement_confidence
- primary_issue
- related_urls
- severity
- confidence
- score
- priority_tier
- recommended_action

### `content_quality_findings`
Represents editorial or content-quality findings.

Suggested structure:
- finding_id
- content_cluster_id
- url
- quality_status
- usefulness_assessment
- originality_assessment
- naturality_assessment
- expertise_signals
- trust_signals
- primary_issue
- severity
- confidence
- score
- recommended_action

### `migration_risk_register`
Represents migration-specific risks.

Suggested structure:
- risk_id
- affected_asset
- risk_type
- description
- severity
- likelihood
- mitigation
- owner
- status

### `migration_action_plan`
Represents the operational migration roadmap.

Suggested structure:
- action_id
- phase
- task
- reason
- owner
- dependency
- urgency
- status

### `growth_roadmap`
Represents post-audit or post-launch growth initiatives.

Suggested structure:
- initiative_id
- initiative_name
- objective
- related_dimension
- target_pages_or_clusters
- expected_impact
- effort
- priority_tier
- dependencies
- owner

### `geo_opportunities`
Represents opportunities to improve visibility in generative answer surfaces and AI-assisted search.

Suggested structure:
- opportunity_id
- content_cluster_id
- entity_name
- issue_or_opportunity
- generative_visibility_status
- entity_retrieval_status
- evidence_strength
- recommended_action
- expected_impact
- priority_tier

### `aeo_opportunities`
Represents opportunities to improve how directly and effectively the content answers user questions in answer engines.

Suggested structure:
- opportunity_id
- content_cluster_id
- entity_name
- question_match_status
- answer_directness_status
- answer_placement_status
- scannability_status
- faq_quality_status
- intent_fit_status
- recommended_action
- expected_impact
- priority_tier

### `llmo_opportunities`
Represents opportunities to improve entity clarity, passage quality, and citation readiness for language models.

Suggested structure:
- opportunity_id
- content_cluster_id
- entity_name
- entity_clarity_status
- passage_quality_status
- summarization_quality_status
- citation_readiness_status
- verifiability_status
- topic_framing_status
- recommended_action
- expected_impact
- priority_tier

### `ai_search_readiness_findings`
Represents umbrella readiness findings across GEO, AEO, and LLMO for a site, cluster, or URL set.

Suggested structure:
- finding_id
- content_cluster_id
- entity_name
- readiness_score
- readiness_status
- geo_readiness
- aeo_readiness
- llmo_readiness
- blocking_issues
- recommended_routing
- recommended_action
- priority_tier

## Shared field conventions

### IDs
All IDs should be stable, unique, and descriptive when possible.

Examples:
- `cluster_pricing_enterprise`
- `finding_rendering_homepage`
- `risk_redirect_loss_blog`

### Status fields
Use shared labels where possible:
- green
- yellow
- orange
- red

### Severity fields
Use:
- low
- medium
- high
- critical

### Confidence fields
Use:
- low
- medium
- high

## Relationship rules

### URL to content cluster
A URL may belong to one content cluster.  
A content cluster may contain multiple URLs.

### Finding to content cluster
A finding may optionally reference one or more content clusters.  
A URL finding should reference a `content_cluster_id` whenever possible.

### Skill handoff
If one skill produces an object that another skill depends on, the object name must remain unchanged.

Example:
- `seo-url-resolution-clustering` produces `content_clusters`
- `seo-content-performance-audit` consumes `content_clusters`
- `seo-growth-strategy` consumes `content_performance_findings`

## Required minimum fields

Every finding object should include:
- ID
- title or issue name
- description
- severity
- confidence
- score when applicable
- recommended action
- owner skill or owner role

## Flexibility rule

Skills may extend objects with additional fields if needed, but:
- shared field names must remain unchanged
- extensions must not break downstream consumption
- core objects must remain readable by all connected skills

## Output quality rule

Outputs should be structured enough for machine use but clear enough for human review.  
Avoid vague free text when a clear field can be defined.

## Example handoff

### Example 1
`seo-client-discovery` produces:
- `project_context`

### Example 2
`seo-prioritization-framework` consumes:
- `project_context`

and produces:
- `priority_rules`

### Example 3
`seo-url-resolution-clustering` consumes:
- `project_context`

and produces:
- `content_clusters`

### Example 4
`seo-content-performance-audit` consumes:
- `project_context`
- `priority_rules`
- `content_clusters`

and produces:
- `content_performance_findings`

## Maintenance rule

If a new skill needs a new shared object, add it here first before using it system-wide.