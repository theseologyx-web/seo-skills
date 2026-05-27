# seo-ai-content-quality

## Purpose

This skill evaluates whether content is useful, natural, differentiated, and appropriate for publication.

Its goal is to check editorial quality beyond technical SEO and beyond raw keyword targeting.

## Why this skill matters

Content can be technically correct and still be weak.

Examples:
- content may be generic
- content may sound machine-generated
- content may repeat obvious points without adding value
- content may match keywords but not user intent
- content may lack expertise or trust
- content may read smoothly but still say very little

This skill helps the system identify content that is acceptable, strong, weak, or in need of human refinement.

## When to use this skill

Use this skill when:
- content was drafted with AI support
- content feels generic or unnatural
- page quality needs editorial review
- content performance is weak and the writing may be part of the problem
- the user wants a strategy for publishing AI-assisted content safely
- a page, cluster, or content program needs humanization and quality control

## Required input

This skill consumes:
- `project_context`
- URLs or content samples

It may also use:
- `content_clusters`
- `content_performance_findings`
- raw page copy
- editorial guidelines
- business or product context

## Required output

This skill must produce:
- `content_quality_findings`

## Core responsibilities

This skill must evaluate:
- usefulness
- naturality
- originality
- specificity
- completeness
- expertise signals
- trust signals
- intent match
- readability
- topical depth
- actionability

## Quality dimensions

### 1. Usefulness
Does the content actually help the user solve the problem?

### 2. Naturality
Does it read like a real human wrote it for a real audience?

### 3. Originality
Does it add something beyond common generic information?

### 4. Specificity
Does it speak to the actual business, product, audience, or use case?

### 5. Completeness
Does it cover the important angles without being thin?

### 6. Expertise signals
Does it show real knowledge, experience, or credible perspective?

### 7. Trust signals
Does it sound reliable, careful, and grounded?

### 8. Intent match
Does it answer what the searcher likely wants?

### 9. Readability
Is it easy to follow, scan, and understand?

### 10. Actionability
Does it give the reader something concrete to do or learn?

### 11. Summarization readiness
Can the content be accurately summarized by a language model without distorting the main point?

Signals:
- main claim is stated clearly and early
- supporting content reinforces rather than contradicts the main point
- structure reflects the logical order of the argument or explanation
- content does not require extensive inference to understand

### 12. Passage-level clarity
Can individual passages stand on their own and be extracted without losing meaning?

Signals:
- each section or paragraph has a clear, focused purpose
- key ideas are not split across disconnected parts
- transitions support coherence rather than just flow
- passages do not require the surrounding context to make sense

### 13. Entity framing
Is it clear what entity — brand, product, service, concept — the content is about?

Signals:
- entity named consistently throughout
- entity's role, category, or relationship to the topic is stated explicitly
- entity is not referred to only as "it", "the solution", or vague pronouns
- entity framing is consistent across headings, body, and metadata

### 14. AI-search reuse quality
Is the content structured in a way that AI search systems can reuse, cite, or surface it?

Signals:
- specific claims with support that can be extracted and cited
- content does not depend on visual or layout context to be understood
- key information is accessible without reading the full piece
- structured formats (lists, definitions, step-by-step) are used where appropriate

## AI-content risk signals

Flag content when it shows patterns like:
- overly generic phrasing
- repetitive structure with little substance
- vague claims without support
- filler sections that repeat the same idea
- excessive polish with no real specificity
- keyword stuffing disguised as helpful writing
- awkward or unnatural transitions
- content that reads fine but does not actually answer anything deeply

## Evaluation logic

### Rule 1: AI use is not the problem
AI-assisted drafting is acceptable if the final content is useful, accurate, natural, and differentiated.

### Rule 2: Human oversight matters
The skill should check whether the content appears reviewed, edited, and improved by a person with real context.

### Rule 3: Business context matters
Content should not be judged in a vacuum.  
It should be judged against the actual audience, offer, and topic.

### Rule 4: Natural does not mean casual
Natural content can still be formal, technical, or structured.  
The key is whether it feels purposeful and specific rather than generic.

### Rule 5: Technical quality does not guarantee content quality
A page can be technically perfect and still fail editorially.

## Quality statuses

### Strong
The content is useful, natural, and differentiated.

### Acceptable
The content is publishable but has some weak points.

### Weak
The content needs significant improvement before it should be trusted.

### Risky
The content may be technically fine but editorially too generic or thin to be safe as-is.

### Unclear
The system does not have enough context or content to judge confidently.

## Output fields

Every content-quality finding should include:
- finding_id
- content_cluster_id if applicable
- url
- quality_status
- usefulness_assessment
- naturality_assessment
- originality_assessment
- expertise_signals
- trust_signals
- summarization_readiness_assessment
- passage_clarity_assessment
- entity_framing_assessment
- ai_search_reuse_quality_assessment
- primary_issue
- severity
- confidence
- score
- recommended_action

## Recommended actions

Possible actions include:
- rewrite with more specificity
- add examples or use cases
- add expert context
- reduce generic filler
- strengthen trust signals
- simplify wording
- improve structure and flow
- add evidence or support
- human-review AI draft before publishing

## Example

### Example 1
A blog post explains a common SEO topic but uses generic phrasing, repetitive sections, and no original insight.

Interpretation:
- quality_status: weak
- issue: low originality and low specificity

### Example 2
A SaaS landing page drafted with AI is refined by a human editor, includes product-specific language, and clearly addresses the target audience.

Interpretation:
- quality_status: strong
- issue: none or minor refinement opportunities

## Tone and behavior

This skill should be editorial, careful, and practical.  
It should avoid blanket anti-AI bias and focus on actual content quality.

## Maintenance rule

If recurring content-quality patterns appear across projects, update this file so future content reviews remain consistent.