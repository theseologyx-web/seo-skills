---
name: seo-geo
description: >
  Optimize content for AI Overviews (formerly SGE), ChatGPT web search,
  Perplexity, and other AI-powered search experiences. Generative Engine
  Optimization (GEO) analysis including brand mention signals, AI crawler
  accessibility, llms.txt compliance, passage-level citability scoring, and
  platform-specific optimization. Use when user says "AI Overviews", "SGE",
  "GEO", "AI search", "LLM optimization", "Perplexity", "AI citations",
  "ChatGPT search", or "AI visibility".
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch
metadata:
  author: AgriciDaniel
  version: "2.0.0"
  category: seo
---

# AI Search / GEO Optimization (April 2026)

## Scope — qué cubre este skill y qué no

| Skill | Enfoque |
|---|---|
| `seo-geo` | Visibilidad en superficies generativas: AI Overviews, ChatGPT, Perplexity. Citabilidad de contenido, entidades, estructura para extracción por IA. |
| `seo-aeo` | Answerability: ser la fuente de la respuesta directa. Calidad de FAQs, respuestas directas, featured snippets. |
| `seo-llmo` | Passage quality, entity clarity y citation readiness para modelos de lenguaje. |
| `seo-ai-search-readiness` | Evaluación paraguas de preparación general antes de entrar a GEO, AEO o LLMO. |

**Cuándo escalar a otro skill:**
- Si el problema es que el contenido no responde preguntas directamente → `seo-aeo`
- Si el problema es que los modelos no citan ni procesan bien el contenido → `seo-llmo`
- Si no está claro cuál aplica → `seo-ai-search-readiness` primero

---

## Key Statistics (2026)

| Metric | Value | Source |
|--------|-------|--------|
| AI Overviews reach | 1.5 billion users/month across 200+ countries | Google |
| AI Overviews query coverage | ~13.14% of queries show AIO (Feb 2026); "AI Mode" experimental may push higher | Datos SEO / BrightEdge |
| CTR impact (AI Overviews) | -58% avg CTR when AIO present; but cited brands gain +35% organic clicks, +91% paid | Ahrefs / Seer Interactive |
| AI-referred sessions growth | 527% (Jan–May 2025) | SparkToro |
| ChatGPT search users | ~542M/month; 9% search market share (900M are total chatbot users, not search-specific) | OpenAI / Similarweb |
| Perplexity | 45M+ MAU; 370% YoY growth; $21.21B valuation | Perplexity 2026 |
| Gemini chatbot share | 21.5% chatbot traffic share (up from 5.7%) | SimilarWeb 2026 |
| Gartner 2026 forecast | Traditional search volume will fall 25% in 2026 | Gartner |
| AI Citation Decay | 50% of AI-cited content is <13 weeks old — freshness is a citability factor | Research 2026 |
| Entity density correlation | Content with 15+ connected entities: 4.8x more likely to be cited by AI (correlation 0.76) | SEO research 2026 |

## Critical Insight: Brand Mentions > Backlinks

**Brand mentions correlate 3x more strongly with AI visibility than backlinks.**
(Ahrefs December 2025 study of 75,000 brands)

| Signal | Correlation with AI Citations |
|--------|------------------------------|
| YouTube mentions | ~0.737 (strongest) |
| Reddit mentions | High |
| Wikipedia presence | High |
| LinkedIn presence | Moderate |
| Domain Rating (backlinks) | ~0.266 (weak) |

**Only 11% of domains** are cited by both ChatGPT and Google AI Overviews for the same query, so platform-specific optimization is essential.

---

## GEO Analysis Criteria (Updated)

### 1. Citability Score (25%)

**Optimal passage length: 134-167 words** for AI citation.

**Strong signals:**
- Clear, quotable sentences with specific facts/statistics
- Self-contained answer blocks (can be extracted without context)
- Direct answer in first 40-60 words of section
- Claims attributed with specific sources
- Definitions following "X is..." or "X refers to..." patterns
- Unique data points not found elsewhere

**Weak signals:**
- Vague, general statements
- Opinion without evidence
- Buried conclusions
- No specific data points

### 2. Structural Readability (20%)

**92% of AI Overview citations come from top-10 ranking pages**, but 47% come from pages ranking below position 5, demonstrating different selection logic.

**Strong signals:**
- Clean H1->H2->H3 heading hierarchy
- Question-based headings (matches query patterns)
- Short paragraphs (2-4 sentences)
- Tables for comparative data
- Ordered/unordered lists for step-by-step or multi-item content
- FAQ sections with clear Q&A format

**Weak signals:**
- Wall of text with no structure
- Inconsistent heading hierarchy
- No lists or tables
- Information buried in paragraphs

### 3. Multi-Modal Content (15%)

Content with multi-modal elements sees **156% higher selection rates**.

**Check for:**
- Text + relevant images
- Video content (embedded or linked)
- Infographics and charts
- Interactive elements (calculators, tools)
- Structured data supporting media

### 4. Authority & Brand Signals (20%)

**Strong signals:**
- Author byline with credentials
- Publication date and last-updated date
- Citations to primary sources (studies, official docs, data)
- Organization credentials and affiliations
- Expert quotes with attribution
- Entity presence in Wikipedia, Wikidata
- Mentions on Reddit, YouTube, LinkedIn

**Weak signals:**
- Anonymous authorship
- No dates
- No sources cited
- No brand presence across platforms

### 5. Technical Accessibility (20%)

**AI crawlers do NOT execute JavaScript.** Server-side rendering is critical.

**Check for:**
- Server-side rendering (SSR) vs client-only content
- AI crawler access in robots.txt
- llms.txt file presence and configuration
- RSL 1.0 licensing terms

---

## AI Crawler Detection

Check `robots.txt` for these AI crawlers. **Critical distinction: training crawlers vs search/retrieval crawlers.**

### Search/Retrieval Crawlers — ALLOW for AI visibility

| Crawler | Owner | Purpose |
|---------|-------|---------|
| OAI-SearchBot | OpenAI | ChatGPT web search (retrieval) |
| ChatGPT-User | OpenAI | ChatGPT live browsing |
| PerplexityBot | Perplexity | Perplexity AI search |
| GoogleOther | Google | Google AI features / Gemini |

### Training Crawlers — BLOCK if desired (no search benefit)

| Crawler | Owner | Purpose |
|---------|-------|---------|
| GPTBot | OpenAI | LLM training data (NOT search) |
| ClaudeBot | Anthropic | Claude training data (NOT search) |
| anthropic-ai | Anthropic | Claude training |
| CCBot | Common Crawl | Training datasets |
| Bytespider | ByteDance | TikTok/Douyin training |
| cohere-ai | Cohere | Cohere model training |

**Context:** 79% of top news sites block training bots. Websites block GPTBot/ClaudeBot 7x more than Googlebot. 400% growth in robots.txt bypass attempts Q2–Q4 2025 (Cloudflare data). Allowing training crawlers does NOT improve your AI search visibility — only retrieval crawlers matter.

---

## llms.txt Standard

The **llms.txt** standard provides AI crawlers with structured content guidance.

**Location:** `/llms.txt` (root of domain)

**Real adoption data (2026):**
- Only **7.4% of Fortune 500** companies have implemented it
- General web adoption: **5–15%**
- No major AI company (OpenAI, Anthropic, Google) officially uses it as a ranking signal
- Claims of "2.3x recall rate" are contested by independent studies
- No statistically significant correlation with more AI citations confirmed yet

**Verdict:** Implement it (low effort, no downside), but do not prioritize over content quality, entity presence, or structural optimization.

**Format:**
```
# Title of site
> Brief description

## Main sections
- [Page title](url): Description
- [Another page](url): Description

## Optional: Key facts
- Fact 1
- Fact 2
```

**Check for:**
- Presence of `/llms.txt`
- Structured content guidance
- Key page highlights
- Contact/authority information

---

## RSL 1.0 (Really Simple Licensing)

New standard (December 2025) for machine-readable AI licensing terms.

**Backed by:** Reddit, Yahoo, Medium, Quora, Cloudflare, Akamai, Creative Commons

**Check for:** RSL implementation and appropriate licensing terms.

---

## Platform-Specific Optimization

| Platform | Market Share | Key Citation Sources | Optimization Focus |
|----------|-------------|---------------------|-------------------|
| **Google AI Overviews** | 81.6% search share | Top-10 ranking pages (92%) | Traditional SEO + passage optimization; opt-out available (CMA UK, H1 2026) |
| **ChatGPT** | 9% search share, 542M/month | Wikipedia (47.9%), Reddit (11.3%) | Entity presence, authoritative sources; Atlas browser extension shifts interaction model |
| **Perplexity** | 2% search share, 45M MAU | Reddit (46.7%), Wikipedia | Community validation; Publishers' Program available (80% revenue share, $42.5M pool) |
| **Gemini** | 21.5% chatbot share | Google index, YouTube | Integrated into Google Search; YouTube citations (29.5% AIO cite video) |
| **Grok (xAI)** | Overtook Perplexity | X/Twitter data | X presence, real-time content |
| **Bing Copilot** | Microsoft ecosystem | Bing index, authoritative sites | IndexNow for fast indexing, Bing Webmaster Tools |
| **Apple Intelligence** | iOS ecosystem | Siri + ChatGPT/Gemini extensions | App store, Apple Maps, local structured data |

**Only 11% of domains** are cited by both ChatGPT and Google AI Overviews for the same query — platform-specific optimization is essential.

---

## Entity Density Framework

Content with **15+ connected entities** has 4.8x more probability of AI selection (correlation 0.76).

**How to optimize:**
- Name every relevant entity explicitly (people, organizations, places, concepts)
- Link entities to Wikipedia/Wikidata via `sameAs` in schema
- Use entity-rich context: "X, CEO of [Company], speaking at [Event]..."
- Avoid pronoun overuse — repeat entity names for machine readability
- Target: 15+ distinct entities per major content block

Schema markup with entity connections: **73% higher selection rate** in AI Overviews.

---

## Citation Decay & Freshness Strategy

**50% of AI-cited content is less than 13 weeks old.** Freshness is a direct citability factor.

**Actions:**
- Audit and update top-performing pages every 10–12 weeks
- Add "Last updated: [date]" prominently — AI systems use it as a signal
- Prioritize updating statistics, data tables, and "current" claims
- Republish with meaningful updates (not cosmetic changes)
- Track which pages are being cited: use Otterly.ai, Profound.ai, or Am I Cited

---

## Agentic Search Readiness

**OpenAI Operator** (launched Jan 2026) navigates, compares, and completes tasks for users (purchases, reservations). This is a new paradigm beyond informational responses.

**For e-commerce and transactional sites:**
- Structured product data (schema) that agents can parse
- Clear pricing, availability, and CTA signals in structured data
- API accessibility for agent-based interactions
- `Offer`, `Product`, `Service` schema with complete attributes

**ChatGPT Instant Checkout:** Direct purchases from chat. Optimize for AI-mediated transactions, not just information retrieval.

---

## Google AI Overviews Publisher Controls

Google announced granular opt-out controls for publishers (March 2026, CMA UK pressure):

- Opt out at page level: `<meta name="robots" content="nosnippet">` or `max-snippet:0`
- Opt out at site level: robots.txt `X-Robots-Tag: nosnippet`
- **Key:** Opting out of AIO does NOT affect organic ranking position
- Timeline: H1 2026

**When to opt out:** Paywalled content, proprietary research, or pages where AI summaries reduce conversion without citation benefit.

---

## Citation Economy Metrics

AI citations are replacing clicks as the primary value metric for many queries.

**Share of AI Voice (SAIV):** Track how often your brand is cited vs competitors in AI responses.

**Tracking tools:**
- Otterly.ai — brand monitoring across AI platforms
- Profound.ai — AI search visibility analytics
- Am I Cited — citation tracking
- DataForSEO: `ai_optimization_chat_gpt_scraper` + `ai_opt_llm_ment_search`

---

## Routing a otros skills AI Search

Cuando un hallazgo GEO tenga raíz fuera del scope de este skill, escalar así:

| Situación | Skill destino |
|---|---|
| El contenido no responde preguntas directamente o la estructura FAQ es débil | `seo-aeo` |
| Los pasajes son difíciles de extraer, la entidad es ambigua, o la citabilidad es baja | `seo-llmo` |
| No está claro si el sitio está preparado para AI Search en general | `seo-ai-search-readiness` |
| GEO y AEO se activan juntos | Ejecutar en paralelo, no fusionar hallazgos |

No resolver problemas de AEO o LLMO dentro de este skill. Producir `geo_opportunities` y escalar.

---

## Output

Generate `GEO-ANALYSIS.md` with:

1. **GEO Readiness Score: XX/100**
2. **Platform breakdown** (Google AIO, ChatGPT, Perplexity scores)
3. **AI Crawler Access Status** (which crawlers allowed/blocked)
4. **llms.txt Status** (present, missing, recommendations)
5. **Brand Mention Analysis** (presence on Wikipedia, Reddit, YouTube, LinkedIn)
6. **Passage-Level Citability** (optimal 134-167 word blocks identified)
7. **Server-Side Rendering Check** (JavaScript dependency analysis)
8. **Top 5 Highest-Impact Changes**
9. **Schema Recommendations** (for AI discoverability)
10. **Content Reformatting Suggestions** (specific passages to rewrite)

---

## Quick Wins

1. Add "What is [topic]?" definition in first 60 words
2. Create 134-167 word self-contained answer blocks
3. Add question-based H2/H3 headings
4. Include specific statistics with sources
5. Add publication/update dates
6. Implement Person schema for authors
7. Allow retrieval crawlers in robots.txt (OAI-SearchBot, PerplexityBot, ChatGPT-User)

## Medium Effort

1. Create `/llms.txt` file
2. Add author bio with credentials + Wikipedia/LinkedIn links
3. Ensure server-side rendering for key content
4. Build entity presence on Reddit, YouTube
5. Add comparison tables with data
6. Implement FAQ sections (structured, not schema for commercial sites)
7. Optimize entity density (target 15+ connected entities per page)

## High Impact

1. Create original research/surveys (unique citability)
2. Build Wikipedia presence for brand/key people
3. Establish YouTube channel with content mentions
4. Implement comprehensive entity linking (sameAs across platforms)
5. Develop unique tools or calculators
6. Update top pages every 10–12 weeks to maintain citation freshness

## DataForSEO Integration (Optional)

If DataForSEO MCP tools are available, use `ai_optimization_chat_gpt_scraper` to check what ChatGPT web search returns for target queries (real GEO visibility check) and `ai_opt_llm_ment_search` with `ai_opt_llm_ment_top_domains` for LLM mention tracking across AI platforms.

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable (DNS failure, connection refused) | Report the error clearly. Do not guess site content. Suggest the user verify the URL and try again. |
| AI crawlers blocked by robots.txt | Report exactly which crawlers are blocked and which are allowed. Distinguish training vs retrieval crawlers. Provide specific robots.txt directives. |
| No llms.txt found | Note the absence and provide a ready-to-use llms.txt template based on the site's content structure. |
| No structured data detected | Report the gap and provide specific schema recommendations (Article, Organization, Person) for improving AI discoverability. |
