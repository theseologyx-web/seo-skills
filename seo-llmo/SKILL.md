# seo-llmo

## Purpose

This skill evaluates and improves how clearly and reliably language models can understand, summarize, and cite the content.

Its focus is on entity clarity, passage quality, summarization readiness, citation readiness, and machine-readable topic framing.

## Why this skill matters

Language models process content differently from classic search crawlers.

They look for:
- clear entity framing — who or what is this about?
- well-structured passages — can this be extracted and used without distortion?
- verifiable claims — does this content support its statements?
- consistent topic framing — does the content stay coherent enough to summarize?
- citation-worthy content — is there a specific claim, insight, or fact worth citing?

Content that is well-written for humans may still be hard for language models to process if it is vague, inconsistent, or lacks verifiable substance.

This skill focuses specifically on machine readability for language models — not on generative surface visibility (GEO) or answer placement (AEO).

## When to use this skill

Use this skill when:
- the project wants content to be more citable by language models
- entity framing is unclear or inconsistent across pages
- the content is hard to summarize without distortion
- claims lack support or verifiability
- `seo-ai-search-readiness` identified LLMO readiness as partially ready or not ready
- the site's topical authority is strong but citation quality is weak

Do not use this skill for answer placement or conversational search optimization.  
Those belong to `seo-aeo`.

## Required input

This skill consumes:
- `project_context`
- `content_quality_findings`

It may also use:
- `content_clusters`
- `sitewide_findings`
- `geo_opportunities`
- raw page copy or URL samples
- structured data context

## Required output

This skill must produce:
- `llmo_opportunities`

## Core responsibilities

This skill must evaluate:
- entity clarity
- passage quality
- summarization readiness
- citation readiness
- claim verifiability
- topic framing consistency
- machine readability

## LLMO dimensions

### 1. Entity clarity
Is it obvious what entity — person, company, product, service, concept — the content is about?

Signals:
- entity name used consistently
- entity described clearly in early content
- entity role or category is explicit
- entity relationships are stated, not assumed

### 2. Passage quality
Can individual passages be extracted and used without distortion?

Signals:
- each section makes sense on its own
- paragraphs are focused and not fragmented
- key ideas are not split across disconnected sections
- transitions support coherence, not just flow

### 3. Summarization readiness
Can a language model summarize this content accurately?

Signals:
- the main point is stated clearly
- supporting detail does not obscure the main claim
- the content does not contradict itself
- the structure reflects the argument or explanation order

### 4. Citation readiness
Does the content contain specific claims, insights, or facts that a language model would consider worth citing?

Signals:
- specific data, statistics, or named examples
- original insights, not just rephrased common knowledge
- source attribution where applicable
- claims that are precise enough to be quoted

### 5. Verifiability
Are the content's claims supported or at least credible?

Signals:
- claims are grounded in evidence, experience, or explanation
- hedging language is used appropriately, not to avoid commitment
- vague superlatives are avoided ("the best", "the most")
- the content does not make claims that cannot be supported

### 6. Topic framing
Is the topic framed consistently throughout the content?

Signals:
- the primary topic is clear from the beginning
- subtopics support the main topic without pulling in unrelated directions
- the content does not drift into tangentially related subjects
- machine-readable headings reflect the actual content structure

## LLMO improvement signals

The system should favor content that:
- states entity and topic clearly in the first paragraph
- uses consistent terminology for key concepts
- organizes information so each section serves a clear purpose
- includes specific, verifiable claims
- avoids vague filler that adds length without adding meaning
- makes the main point extractable without reading the entire piece

## Anti-signals

Flag content when it:
- uses vague entity references ("the solution", "this approach")
- buries the main claim at the end
- contradicts itself across sections
- uses generic claims without support ("proven to increase results")
- is overly long with low information density
- switches terminology for the same concept without explanation

## Output fields

Every LLMO opportunity should include:
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
- notes

## Example

### Example 1
A product page describes the product in vague terms, uses multiple names for the same entity, and makes performance claims without evidence.

Interpretation:
- entity_clarity_status: weak
- verifiability_status: weak
- citation_readiness_status: weak
- recommended_action: define the entity clearly in the first paragraph; standardize terminology; replace vague claims with specific evidence

### Example 2
A blog article covers a technical topic but each section introduces a new angle without connecting back to the main argument.

Interpretation:
- topic_framing_status: weak
- summarization_quality_status: weak
- recommended_action: add a clear thesis early; restructure sections so each supports the central argument; improve passage-level coherence

## Tone and behavior

This skill should be analytical and precise.  
It should focus on machine readability problems that are specific and fixable.  
It should not overlap with AEO (answer placement) or GEO (generative surface visibility).  
It should complement both without duplicating their logic.

## Maintenance rule

If language model behavior or citation patterns evolve, update this file so the LLMO logic stays current.
