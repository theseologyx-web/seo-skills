---
name: seo-ai-search-readiness
description: Evaluate overall readiness for AI Search across visibility, answerability, retrievability, trust, and technical support signals, then route work to seo-geo, seo-aeo, seo-llmo, or classic SEO skills. Not for deep execution in a single subdomain.
user-invokable: true
argument-hint: "<url or domain>"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Agent
metadata:
  author: AgriciDaniel
  version: "2.0.0"
  category: seo
---

# SEO AI Search Readiness

Use this skill as the umbrella audit for AI Search.

## Purpose

Determine whether the main blockers are:
- visibility in generative surfaces,
- answerability,
- LLM retrievability,
- technical support,
- trust / authority / entity clarity,
- or classic SEO foundations.

## Analyze

1. Generative visibility signals.
2. Answerability and direct-response patterns.
3. LLM retrievability and passage quality.
4. Entity/trust support.
5. Supporting technical factors.
6. Dependency on unresolved classic SEO issues.

## Output

Return `ai_search_readiness_findings`:

```yaml
ai_search_readiness_findings:
  domain: ""
  overall_state: "low|medium|high"
  primary_blockers:
    - blocker: ""
      severity: "high|medium|low"
      owned_by_skill: "seo-geo|seo-aeo|seo-llmo|seo-content|seo-entity|seo-schema|seo-technical"
  readiness_by_dimension:
    visibility: "low|medium|high"
    answerability: "low|medium|high"
    retrievability: "low|medium|high"
    trust_and_entities: "low|medium|high"
    technical_support: "low|medium|high"
  recommended_skill_sequence:
    - "seo-geo"
    - "seo-aeo"
    - "seo-llmo"
  first_priority_actions:
    - ""
  dependencies:
    - ""
```

## Decision rule

Do not go deep into execution unless the user explicitly requests it. This skill diagnoses and routes — it does not execute.
