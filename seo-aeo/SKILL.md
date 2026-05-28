# seo-aeo

## Purpose

This skill evaluates and improves how directly and effectively content answers user questions in answer engines and AI-assisted search.

Its focus is answerability: the ability of a page or content cluster to match a question, place the answer clearly, and be extracted reliably by answer-style systems.

> **Scope:** Answerability and direct-response content structure. Not for generative surface visibility → use `seo-geo`. Not for LLM passage chunking or citation anchors → use `seo-llmo`.

## Why this skill matters

Content that ranks well in classic SEO may still fail in answer engines.

Reasons include:
- the answer is present but buried too deep
- the question is implied but never explicitly addressed
- the FAQ section is vague or disconnected from real search queries
- the content is dense and hard to scan for a direct answer
- the intent fit is misaligned with conversational or voice queries

This skill focuses specifically on answerability — not on generative surface visibility (GEO) or language model citation quality (LLMO).

## When to use this skill

Use this skill when:
- the project wants to improve answer engine visibility
- content has poor question-to-answer match
- FAQ sections are weak, vague, or missing
- the content does not appear in featured snippets or AI answer boxes
- conversational or voice search is a traffic goal
- `seo-ai-search-readiness` identified AEO readiness as partially ready or not ready

Do not use this skill for generative surface issues unrelated to answerability.  
Those belong to `seo-geo`.

## Required input

This skill consumes:
- `project_context`
- `content_quality_findings`

It may also use:
- `content_clusters`
- `sitewide_findings`
- `content_performance_findings`
- raw page copy or URL samples
- search query data from GSC

## Required output

This skill must produce:
- `aeo_opportunities`

## Core responsibilities

This skill must evaluate:
- question-to-answer match
- answer directness
- answer placement
- content scannability
- FAQ usefulness
- intent fit for answer-style queries
- structure that supports direct extraction

## AEO dimensions

### 1. Question match
Does the content address the question the user likely asked?

Signals:
- heading or subheading that mirrors the query
- clear question stated near the top or in a FAQ
- content aligned with the searcher's information need

### 2. Answer directness
Does the content give the answer early and clearly?

Signals:
- answer appears in the first 1–2 sentences of the section
- no long preamble before the actual answer
- clear declarative structure

### 3. Answer placement
Is the answer positioned where answer engines can extract it?

Signals:
- answer appears close to the question or heading
- answer is not fragmented across multiple paragraphs
- answer is not hidden inside a long introduction

### 4. Scannability
Can the answer be located quickly by a machine or a human?

Signals:
- clear heading structure
- short paragraphs
- bullet points or numbered lists where appropriate
- no excessive filler between question and answer

### 5. FAQ quality
Are FAQ sections useful, specific, and aligned with real queries?

Signals:
- FAQs match questions people actually search
- answers are direct, not promotional
- FAQ schema is implemented where appropriate
- FAQs are not duplicates of the main content

### 6. Intent fit
Does the content match the way questions are phrased in conversational or voice search?

Signals:
- natural language phrasing in headings and answers
- content addresses the "who, what, when, where, why, how" structure when relevant
- content does not assume the user has background knowledge they may not have

## AEO improvement signals

The system should favor content that:
- places answers early and clearly
- uses question-style headings where appropriate
- has specific, direct FAQ sections
- avoids hedging language that weakens the answer
- uses structured formats that aid extraction
- matches the phrasing of real search queries

## Anti-signals

Flag content when it:
- buries answers in the middle or end of long sections
- uses vague answers like "it depends" without following up
- has FAQ sections that repeat marketing copy
- has no clear answer to the implied question in the heading
- is formatted as a wall of text with no scannable structure
- uses jargon without explanation

## Output fields

Every AEO opportunity should include:
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
- notes

## Example

### Example 1
A B2B SaaS page answers the question "what is X" somewhere in the third paragraph after two paragraphs of marketing context.

Interpretation:
- answer_placement_status: weak
- answer_directness_status: weak
- recommended_action: move the direct answer to the first sentence after the heading; remove the preamble

### Example 2
A help article has a FAQ section but all answers are two sentences long and promotional in tone.

Interpretation:
- faq_quality_status: weak
- recommended_action: rewrite FAQs with specific, direct answers; remove promotional framing; add FAQ schema

## Tone and behavior

This skill should be editorial and precise.  
It should focus on specific structural and placement problems, not on generic "write better content" advice.  
It should not attempt to solve GEO or LLMO problems — those belong to the dedicated skills.

## Maintenance rule

If answer engine behavior changes or new extraction patterns emerge, update this file so the AEO logic stays current.

```yaml
# aeo_output (parseable)
aeo_output:
  domain: ""
  answerability_score: "low|medium|high"
  qa_pairs_found: 0
  qa_pairs_missing: []
  featured_snippet_opportunities: []
  structured_answer_gaps: []
  findings: []
  priority_actions: []
  handoff_to_skills: []
```
