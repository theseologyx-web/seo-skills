# seo-ai-search-readiness

## Purpose

This skill evaluates whether a site, content cluster, or set of URLs is ready for AI Search.

Its purpose is to act as an umbrella assessment layer over GEO, AEO, and LLMO — deciding which of those skills should be activated and in what order.

## Why this skill matters

Not every site needs full GEO, AEO, and LLMO work at the same time.

Some sites have strong entity clarity but weak answerability.  
Others have good answer structure but poor machine-readable passage quality.  
Others have none of the above.

This skill prevents the system from defaulting to all three AI Search skills on every project.  
It evaluates readiness first and routes to the right specialized skill.

## When to use this skill

Use this skill when:
- the project has AI Search visibility as a goal
- the user wants to know if their site is ready for AI-assisted search
- GEO, AEO, or LLMO work is being considered but the starting point is unclear
- the system needs to decide which AI Search skills to activate
- a broad AI Search gap assessment is needed before specialized work begins

Do not use this skill to produce detailed GEO, AEO, or LLMO recommendations.  
Its job is to assess readiness and route, not to optimize.

## Required input

This skill consumes:
- `project_context`
- `sitewide_findings`
- `content_quality_findings`

It may also use:
- `content_clusters`
- `content_performance_findings`
- structured data context
- sample URLs or content clusters for evaluation

## Required output

This skill must produce:
- `ai_search_readiness_findings`

It may also produce:
- `recommended_routing` — which AI Search skills to activate next

## Core responsibilities

This skill must evaluate:
- overall AI Search readiness across GEO, AEO, and LLMO dimensions
- which layer is ready, partially ready, or not ready
- which blocking issues prevent AI Search visibility
- which skill should be activated next based on the readiness profile

## Readiness dimensions

### GEO readiness
Evaluate whether the content supports generative visibility.

Signals:
- entity clarity across key pages
- topical identity in generative contexts
- structured data alignment
- evidence quality
- content chunking for generative extraction

### AEO readiness
Evaluate whether the content directly answers likely questions.

Signals:
- question-to-answer match across key pages
- answer placement and directness
- FAQ coverage and quality
- scannability for quick extraction
- intent fit for conversational and voice queries

### LLMO readiness
Evaluate whether the content is suitable for language model processing and citation.

Signals:
- passage-level clarity
- entity framing consistency
- summarization quality
- citation readiness
- verifiability of claims
- topic framing for machine reading

## Readiness statuses

### Ready
The content in this dimension is well-structured for AI Search.  
No immediate blocking issues.

### Partially ready
The content has meaningful gaps but is not fully blocked.  
Targeted improvements would increase AI Search visibility.

### Not ready
The content has significant structural or editorial problems that prevent AI Search visibility in this dimension.  
Work in this dimension should be prioritized before others.

### Unclear
Not enough data to assess readiness in this dimension.  
More content samples or audit data are needed.

## Routing logic

### Route to `seo-geo`
When GEO readiness is partially ready or not ready and generative surface visibility is a project goal.

### Route to `seo-aeo`
When AEO readiness is partially ready or not ready and answer engine visibility is a project goal.

### Route to `seo-llmo`
When LLMO readiness is partially ready or not ready and language model citation quality is a project goal.

### Route to multiple skills
When readiness gaps exist across two or three dimensions, activate the relevant skills.  
Suggest an order based on which gap is most blocking.

## Output fields

Every AI Search readiness finding should include:
- finding_id
- content_cluster_id if applicable
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

## Example

### Example 1
A SaaS site has strong editorial content but vague entity framing and no FAQ coverage.

Interpretation:
- GEO readiness: partially ready
- AEO readiness: not ready
- LLMO readiness: partially ready
- Route to: `seo-aeo` first, then `seo-llmo`

### Example 2
A blog-heavy site has good answer structure but weak generative presence.

Interpretation:
- GEO readiness: not ready
- AEO readiness: ready
- LLMO readiness: partially ready
- Route to: `seo-geo`, then `seo-llmo`

## Tone and behavior

This skill should be diagnostic and decisive.  
It should not produce a list of generic AI Search tips.  
It should assess readiness per dimension and recommend a clear routing path.

## Maintenance rule

If new AI Search surfaces or evaluation criteria become relevant, update this file so the readiness model stays current.
