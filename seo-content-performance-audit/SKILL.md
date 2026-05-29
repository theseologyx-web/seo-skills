# seo-content-performance-audit

## Purpose

This skill analyzes content performance at the content-cluster level rather than only at the URL level.

Its goal is to identify growth, decay, stagnation, recovery, and measurement anomalies using connected search and analytics data.

## Why this skill matters

Content performance is often misunderstood when the system only looks at a single URL.

A page may seem to lose traffic when:
- the URL changed
- traffic moved to a new URL version
- redirects were introduced
- canonical signals changed
- measurement broke
- search demand shifted
- content quality decayed
- internal links changed
- the page lost visibility for technical reasons instead of editorial reasons

This skill prevents those errors by analyzing performance through the content entity model.

## When to use this skill

Use this skill when:
- GSC data is available
- GA4 data is available
- other connected data sources are available
- page traffic changed and the cause is unclear
- content needs recovery or refresh decisions
- ranking, CTR, engagement, or conversion trends must be analyzed
- performance must be evaluated by content cluster, not raw URL

## Required input

This skill consumes:
- `project_context`
- `priority_rules`
- `content_clusters`
- GSC data
- GA4 data
- other MCP-connected sources when available

It may also use:
- backlink data
- migration logs
- content update history
- crawl snapshots
- conversion or CRM data

## Required output

This skill must produce:
- `content_performance_findings`

## Core responsibilities

This skill must:
- identify rising, declining, and stagnant content clusters
- compare performance across time windows
- distinguish URL loss from content loss
- detect migration-related distortions
- detect measurement issues
- highlight content refresh opportunities
- identify content pieces that rank but do not convert
- identify content pieces that convert but underperform in search visibility

## Primary metrics

### GSC
Use:
- impressions
- clicks
- CTR
- average position
- query coverage
- landing page performance

### GA4
Use:
- sessions
- engaged sessions
- engagement rate
- conversions
- revenue or lead value if available
- landing page behavior

### Other data
Use when available:
- CRM quality
- lead quality
- assisted conversions
- backlink changes
- content update timestamps
- crawl changes

## Performance categories

### Growth
The content cluster is improving meaningfully.

Signs:
- clicks increase
- impressions increase
- rankings improve
- CTR improves
- conversions improve

### Decay
The content cluster is losing meaningful visibility or value.

Signs:
- clicks fall
- impressions fall
- ranking position worsens
- CTR drops
- conversions weaken

### Stagnation
The content cluster is stable but not improving.

Signs:
- flat traffic
- flat rankings
- weak expansion
- no clear upward trend

### Recovery
The content cluster is rebounding after decline or after a fix.

Signs:
- traffic returns after migration repair
- rankings improve after content refresh
- CTR improves after title or meta improvements

### Anomaly
The data does not follow the expected pattern.

Signs:
- traffic drops but rankings do not
- impressions rise but clicks collapse
- GA4 and GSC disagree materially
- one URL version carries most data while the cluster is fragmented

## Analysis logic

### 1. Analyze by cluster
Do not stop at the URL layer if multiple URLs map to one content entity.

### 2. Use multiple windows
Compare short, medium, and longer time periods.

Suggested windows:
- 7 days
- 28 days
- 3 months
- 12 months
- pre-change vs post-change

### 3. Separate visibility from engagement
A page can gain visibility and lose conversion quality, or the reverse.

### 4. Check for technical distortions
Before calling content weak, check:
- redirect behavior
- canonical issues
- indexation loss
- rendering issues
- measurement gaps
- migration effects

### 5. Connect performance to business value
A small performance drop on a top revenue page may matter more than a larger drop on a low-value page.

## Findings types

### 1. Declining cluster
A cluster is losing visibility, engagement, or conversion value.

### 2. High-potential cluster
A cluster has positive signals and room to expand.

### 3. Near-rank cluster
A cluster is close to stronger visibility and may benefit from refinement.

### 4. Conversion-weak cluster
A cluster gets traffic but does not convert well.

### 5. Visibility-weak cluster
A cluster has good content value but poor search reach.

### 6. Measurement-risk cluster
The cluster's data is incomplete or untrustworthy.

## Output fields

Every finding should include:
- finding_id
- content_cluster_id
- entity_name
- trend_type
- primary_issue
- supporting_metrics
- severity
- confidence
- score
- priority_tier
- recommended_action
- notes

## Recommended interpretation rules

### If clicks fall and impressions also fall
Likely visibility decline, content decay, or migration loss.

### If impressions hold but CTR falls
Likely snippet issue, title issue, search intent mismatch, or competitive pressure.

### If traffic holds but conversions fall
Likely UX, offer, intent, or funnel issue.

### If GSC and GA4 disagree sharply
Likely measurement, attribution, or data-quality issue.

### If one cluster has multiple URL versions in play
Confirm clustering before drawing conclusions.

## Example

### Example 1
A blog cluster lost clicks after migration, but the old URL still appears in some data sources and the new URL now holds most of the traffic.

Interpretation:
- cluster may be recovering
- URL-level decline should not be treated as full content loss
- migration monitoring should continue

### Example 2
A SaaS landing page has stable impressions but lower CTR and lower demo submissions.

Interpretation:
- visibility is stable
- snippet or intent alignment may be weak
- conversion path should be reviewed

## Tone and behavior

This skill should be evidence-led, conservative, and business-aware.
It should avoid premature conclusions when URL lineage or measurement is uncertain.

## Maintenance rule

If new recurring performance patterns appear, update this file so future analysis stays consistent.
