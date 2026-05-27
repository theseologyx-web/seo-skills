# Recommendation Style

## Purpose

This file defines how the SEO system should write recommendations across all skills.  
Its purpose is to make every recommendation clear, consistent, actionable, and easy to prioritize.

## Why this matters

A recommendation is only useful if the reader can quickly understand:
- what was found
- why it matters
- what to do next
- how urgent it is
- who should own it

If recommendations are vague, the system becomes harder to execute.

## Required recommendation structure

Every recommendation should include:

1. What was found
2. Why it matters
3. Business risk
4. Technical explanation
5. Exact fix
6. Expected impact
7. Priority tier
8. Suggested owner

## Writing rules

### 1. Be specific
Avoid generic language.

Bad:
- improve SEO
- fix content
- optimize page
- work on UX

Better:
- update the title tag to better match the primary query intent
- fix the redirect target so the old blog URL resolves to the new canonical article
- expand the content cluster with supporting sections that answer common follow-up questions

### 2. Explain the reason
Do not give a fix without context.

Example:
- The canonical points to an old URL, which can split signals and prevent the preferred page from consolidating indexing strength.

### 3. Connect to business value
Make the business consequence obvious.

Example:
- This page drives demo requests, so weak CTA clarity is likely limiting conversions even if traffic remains stable.

### 4. Avoid overclaiming
Only recommend what the evidence supports.
If data is incomplete, say so.

### 5. Use direct language
Prefer short verbs and concrete nouns.
Avoid filler and marketing phrasing.

## Recommendation template

Use this format:

- finding: ...
- why_it_matters: ...
- business_risk: ...
- technical_explanation: ...
- exact_fix: ...
- expected_impact: ...
- priority_tier: ...
- owner: ...

## Tone guidelines

### Good tone
- clear
- firm
- practical
- specific
- balanced

### Avoid
- vague optimism
- dramatic warnings without evidence
- passive voice when direct language is possible
- generic consulting phrases

## Severity-to-action logic

### Critical
Must explain the blocking nature of the issue and why it should be fixed first.

### High
Must explain the strong negative effect and what will continue to suffer if delayed.

### Medium
Must explain the meaningful opportunity or quality gap.

### Low
Must explain the minor improvement and why it is still worth considering.

## Examples

### Example 1
- finding: The homepage title tag is duplicated across multiple strategic pages.
- why_it_matters: Search engines may struggle to distinguish the pages, reducing CTR and relevance clarity.
- business_risk: Key landing pages may cannibalize each other and lose organic opportunity.
- technical_explanation: The title pattern is too generic and does not differentiate page intent.
- exact_fix: Rewrite each strategic title tag to reflect the unique page purpose and primary query intent.
- expected_impact: Better relevance signals and cleaner CTR performance.
- priority_tier: P2
- owner: SEO/content

### Example 2
- finding: Old URLs continue to redirect through chains after migration.
- why_it_matters: Redirect chains slow crawling and may weaken signal consolidation.
- business_risk: Backlink equity and traffic continuity may be partially lost.
- technical_explanation: The old URL does not point directly to the final canonical destination.
- exact_fix: Update redirect rules so each old URL resolves in a single hop to the final preferred URL.
- expected_impact: Cleaner crawl paths and better preservation of authority.
- priority_tier: P1
- owner: SEO/engineering

## AI Search recommendation framing

When a recommendation comes from GEO, AEO, LLMO, or AI Search Readiness skills, the same required structure applies.  
The following framing rules help make AI Search recommendations specific and useful.

### GEO recommendations
- finding: describe the specific generative visibility gap
- technical_explanation: explain why the content fails in generative surfaces (entity clarity, chunking, evidence, etc.)
- exact_fix: describe a concrete structural or editorial change, not a vague "optimize for AI"
- owner: SEO/content or SEO/engineering depending on the fix type

### AEO recommendations
- finding: describe the specific answerability or question-match gap
- technical_explanation: explain where the answer breaks down (buried, vague, misaligned intent, missing FAQ, etc.)
- exact_fix: describe where to place the answer, how to restructure it, or what question to address
- owner: SEO/content

### LLMO recommendations
- finding: describe the specific entity, passage, or citation gap
- technical_explanation: explain why a language model would struggle (ambiguous entity, weak passage, no verifiable support, etc.)
- exact_fix: describe a specific improvement to entity framing, passage structure, or claim support
- owner: SEO/content

### AI Search Readiness recommendations
- finding: state whether the site or cluster is ready, partially ready, or not ready
- technical_explanation: explain which layer is blocking (GEO, AEO, LLMO, or all)
- exact_fix: recommend the routing path (which skill to activate next)
- owner: SEO lead

### Avoid in AI Search recommendations
- claiming content "will rank in AI" — no evidence supports that level of certainty
- recommending generic "AI optimization" without a specific fix
- treating GEO, AEO, and LLMO as interchangeable
- promising inclusion in AI overviews, which is not directly controllable

## Maintenance rule

If a project needs a specialized tone or output style, it may extend this file, but all recommendations must still include the required structure above.