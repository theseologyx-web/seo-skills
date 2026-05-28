---
name: robots-txt
description: "Validate, generate, and audit robots.txt files for SEO. Use this skill whenever the user mentions robots.txt, crawler directives, bot blocking, crawl control, AI crawler management, or asks about controlling how search engines or AI bots crawl their website. Also trigger when the user wants to check if their robots.txt has errors, wants to generate a new robots.txt from scratch, needs to audit an existing robots.txt for SEO issues, wants to block or allow specific bots (like GPTBot, CCBot, ClaudeBot, Googlebot, Bingbot), or asks about crawl budget optimization. Trigger even if the user just pastes a robots.txt and asks 'is this ok?' or 'review this'. Works for any CMS or static site. Not for sitemap generation or validation — use seo-sitemap."
---

# robots.txt Skill — Validate, Generate & Audit

This skill handles three core workflows for robots.txt files: **validation** (syntax and logic checking), **generation** (creating new files from scratch based on site needs), and **auditing** (SEO-focused review of existing files for issues and improvements).

## Quick Reference: robots.txt Fundamentals

Before doing anything, internalize these rules — they're the foundation for every validation, generation, and audit task:

### Syntax Rules (RFC 9309)
- The file MUST be served at the root: `https://domain.com/robots.txt`
- Each record starts with one or more `User-agent:` lines
- Valid directives: `User-agent`, `Disallow`, `Allow`, `Sitemap`, `Crawl-delay` (unofficial but widely supported)
- Directives are case-insensitive (`disallow` = `Disallow`), but path values are case-sensitive (`/Admin/` ≠ `/admin/`)
- An empty `Disallow:` means "allow everything" for that user-agent
- A missing or empty robots.txt = everything is allowed
- Comments start with `#` and can appear on their own line or after a directive
- Blank lines separate records (groups of directives for a user-agent)
- `Sitemap:` is a standalone directive (not tied to any user-agent block) and must use an absolute URL
- Wildcards: `*` matches any sequence of characters; `$` anchors to end of URL (Google/Bing support these)
- Maximum recommended file size: 500 KiB (Google may stop processing beyond this)

### How Search Engines Pick Rules
- Google/Bing: the **most specific** (longest matching path) rule wins, regardless of order. In ties, `Allow` wins.
- Other crawlers: typically the **first matching rule** wins.
- Each crawler uses the **most specific user-agent block** that matches its name. If none matches, it falls back to `User-agent: *`.
- If a crawler has a specific block, it ignores the `*` block entirely — the rules are NOT merged (except Google, which merges multiple blocks for the same user-agent).

### Critical SEO Considerations
- `Disallow` does NOT mean "noindex" — Google can still index a URL if external links point to it. Use `noindex` meta tag or `X-Robots-Tag` header to prevent indexing.
- Blocking CSS/JS files prevents Google from rendering pages properly — avoid this.
- Blocking a URL with a canonical tag means Google can't see the canonical — bad for deduplication.
- Blocking parameterized URLs is good for crawl budget but bad if those pages have canonical tags.
- `Sitemap:` in robots.txt is supplementary; always submit sitemaps via Search Console too.
- `Crawl-delay` is ignored by Googlebot but respected by Bingbot, Yandex, and most AI bots. For sites being aggressively crawled by AI bots that respect Crawl-delay, use values like `Crawl-delay: 10` (seconds) as a throttle — it won't stop them but will reduce load.
- robots.txt is PUBLIC — never list sensitive paths in it (security through obscurity is a bad idea).
- **File size:** Maximum 500 KiB (Google stops processing beyond this). With the explosion of AI bot rules, some sites approach this limit. Write efficient rules: one `User-agent: *` block handles all unspecified bots; only add named blocks for bots that need different rules.
- **Page-level AI opt-out:** `<meta name="robots" content="noai, noimageai">` signals AI training opt-out at page level, complementing robots.txt-level blocking. Support varies by AI provider — treat as supplementary, not a replacement for robots.txt or WAF rules.

### AI Crawler Landscape (2025-2026)

**Adoption reality:** ClaudeBot is blocked by 69% of websites, GPTBot by 62% (2026 data). AI bot blocking has gone mainstream.

**Three-bot frameworks (2026):**

Anthropic (formalized February 2026):
- `ClaudeBot` — AI training crawl (block to protect training data)
- `Claude-SearchBot` — search index crawl (allow for AI search visibility)
- `Claude-User` — user-initiated fetch via Claude (allow for real-time citations)

OpenAI:
- `GPTBot` — AI training crawl (block to protect training data)
- `OAI-SearchBot` — search index crawl (allow for ChatGPT search visibility)
- `ChatGPT-User` — user-initiated browsing (allow for real-time citations)

**Known AI training bots (block to protect data):**
`GPTBot`, `Google-Extended`, `ClaudeBot`, `CCBot`, `FacebookBot`, `Bytespider`, `Applebot-Extended`, `Diffbot`, `Omgili`, `img2dataset`, `cohere-ai`, `AI2Bot`, `Meta-ExternalAgent`, `Amazonbot`, `Timpibot`, `Kangaroo Bot`

> **Note:** `anthropic-ai` is a deprecated user-agent. Use `ClaudeBot` (training), `Claude-SearchBot` (search), and `Claude-User` (browsing) for Anthropic bots.

**Known AI search/RAG bots (allow for AI search visibility):**
`Claude-SearchBot`, `Claude-User`, `OAI-SearchBot`, `ChatGPT-User`, `PerplexityBot`, `YouBot`

**AI crawler compliance reality:**
robots.txt is advisory — not enforced. Research shows Perplexity used undeclared crawlers with generic user-agent strings to bypass robots.txt blocking. **The only reliable method to block specific bots is network-level enforcement: WAF rules, Cloudflare AI bot management, or IP blocking.**

**Staying current:** The AI crawler landscape changes rapidly. Reference [knownagents.com](https://knownagents.com) (formerly Dark Visitors) for an up-to-date list of known AI crawlers and their declared user-agents.

**Cloudflare alternative:** Since July 2025, Cloudflare can generate and serve a managed `robots.txt` for AI bot management (Cloudflare Dashboard → Crawlers → AI Crawlers). Also supports Content Signals via DNS TXT records (`_cf-signals.yourdomain.com TXT "ai-train=deny"`) for more granular control than robots.txt.

---

## Workflow 1: VALIDATE a robots.txt

When the user provides a robots.txt (as text, file, or URL to fetch), run validation manually.

### Step 1: Analyze the file

Check for:
- Syntax errors (invalid directives, missing user-agent before rules, malformed lines)
- Logic errors (conflicting Allow/Disallow, overly broad blocks, duplicate rules)
- SEO warnings (blocking CSS/JS, missing Sitemap directive, blocking crawlers from canonical pages)
- AI crawler coverage (whether known AI training bots are addressed)
- Best practice violations (sensitive paths exposed, missing wildcard user-agent)

### Step 2: Interpret and present results

Group findings by severity:

1. **ERRORS** — Broken, causes unexpected crawler behavior
2. **WARNINGS** — Could hurt SEO or lead to unintended blocking
3. **INFO** — Suggestions

For each issue, explain **what** it is, **why** it matters, and **how** to fix it.

### Step 3: Offer a corrected version

Generate a corrected robots.txt and show it inline. Do NOT write to disk.

---

## Workflow 2: GENERATE a robots.txt

When the user wants to create a robots.txt from scratch, gather requirements first.

### Step 1: Gather context

Infer from context or ask only what's missing:
1. **CMS/platform** (WordPress, Shopify, Next.js, custom, etc.)
2. **Site type** (ecommerce, blog, SaaS, corporate, etc.)
3. **AI crawler policy** (block training only, block all AI, allow all)
4. **Sitemap URL**

### Step 2: Generate the file

**CRITICAL RULES:**
- **Only include Disallow rules for paths that actually exist on the site.** Do NOT invent or assume paths. If you haven't confirmed a path exists (via sitemap, crawl, or the user), do not include it.
- **No comments.** Do not add `#` comment lines. The file must be clean and minimal.
- **No header blocks.** No "last updated", no contact info, no purpose explanation.
- Group blocks logically: wildcard `*` first, then AI training bots (Disallow: /), then AI search bots (Allow: /).
- `Sitemap:` goes at the very bottom.
- Never block CSS, JS, or image files needed for rendering.

**Only add AI crawler blocks if the user requests an AI policy.** If they don't mention AI bots, omit those blocks entirely.

Present the generated robots.txt inline in the conversation. Do NOT write to disk.

### Step 3: Validate the generated file

Manually check the generated file for syntax errors and logic conflicts. Report any issues.

---

## Workflow 3: AUDIT a robots.txt (SEO Review)

When the user wants an in-depth SEO audit of an existing robots.txt.

### Step 1: Obtain the file

If the user provides a URL, fetch `{url}/robots.txt`. If they paste text, use that directly.

### Step 2: Run the validation script

Same as Workflow 1, Step 1.

### Step 3: Deep SEO analysis

Go beyond syntax validation. Analyze:

1. **Crawl Budget Impact**: Are there patterns that waste crawl budget? (e.g., not blocking faceted nav, internal search, paginated archives)
2. **Indexation Risk**: Are important pages accidentally blocked? Are canonical-tagged pages blocked?
3. **AI Bot Strategy**: Is there a coherent AI crawler strategy? Check:
   - Are three-bot frameworks covered? (Anthropic: ClaudeBot/Claude-SearchBot/Claude-User; OpenAI: GPTBot/OAI-SearchBot/ChatGPT-User)
   - Is `anthropic-ai` used? (deprecated — should be replaced with the three named bots)
   - Are they accidentally blocking AI search bots (Claude-SearchBot, OAI-SearchBot) that drive referral traffic?
   - Are newer bots missing (Meta-ExternalAgent, Amazonbot, Timpibot)?
   - Is WAF/Cloudflare bot management also in place? (robots.txt alone is not reliable against non-compliant crawlers)
4. **Competitive Analysis**: Based on the site type, what are they missing compared to industry best practices?
5. **Security Exposure**: Are sensitive paths being revealed in the robots.txt?
6. **Completeness**: Is the file missing common directives for the detected CMS/platform?
7. **Conflict Detection**: Do any Allow/Disallow rules conflict? Which one would win for Google vs other crawlers?

### Step 4: Generate audit report

Present findings as a structured analysis with:
- Executive summary (1-2 sentences on overall health)
- Critical issues (must-fix)
- Recommended improvements (should-fix)
- Nice-to-have optimizations
- A revised robots.txt with all recommendations applied

---

## Common Patterns Reference

These are **reference patterns only**. Only include a Disallow rule if the path has been confirmed to exist on the specific site being analyzed.

### WordPress (confirmed paths)
```
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php
Disallow: /wp-includes/
Disallow: /?s=

Sitemap: https://example.com/sitemap.xml
```

### Shopify (confirmed paths)
```
User-agent: *
Disallow: /admin
Disallow: /cart
Disallow: /checkout
Disallow: /account
Disallow: /*?*sort_by*
Disallow: /*?*q=*

Sitemap: https://example.com/sitemap.xml
```

### SaaS / Web Application (confirmed paths only)
```
User-agent: *
Disallow: /app/
Disallow: /dashboard/
Disallow: /api/
Disallow: /admin/
Disallow: /login
Disallow: /register

Sitemap: https://example.com/sitemap.xml
```

### AI Crawler Block (Training Only — Allow AI Search)

Block AI training bots; allow AI search/RAG bots for visibility in AI search results.

> **Note:** robots.txt is advisory. For reliable blocking, combine with WAF/Cloudflare AI bot management.

```
User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: FacebookBot
Disallow: /

User-agent: Meta-ExternalAgent
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: Applebot-Extended
Disallow: /

User-agent: Amazonbot
Disallow: /

User-agent: Diffbot
Disallow: /

User-agent: cohere-ai
Disallow: /

User-agent: AI2Bot
Disallow: /

User-agent: Timpibot
Disallow: /

User-agent: Omgili
Disallow: /

User-agent: img2dataset
Disallow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: Claude-SearchBot
Allow: /

User-agent: Claude-User
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: YouBot
Allow: /
```

### AI Crawler Block (All AI — Maximum Protection)

```
User-agent: GPTBot
Disallow: /

User-agent: OAI-SearchBot
Disallow: /

User-agent: ChatGPT-User
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: Claude-SearchBot
Disallow: /

User-agent: Claude-User
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: PerplexityBot
Disallow: /

User-agent: FacebookBot
Disallow: /

User-agent: Meta-ExternalAgent
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: Applebot-Extended
Disallow: /

User-agent: Amazonbot
Disallow: /

User-agent: Diffbot
Disallow: /

User-agent: cohere-ai
Disallow: /

User-agent: AI2Bot
Disallow: /
```

### AI Bot Throttle (Allow Crawl, Limit Rate)

For bots that respect `Crawl-delay` — doesn't stop them, reduces server load:
```
User-agent: ClaudeBot
Crawl-delay: 10

User-agent: GPTBot
Crawl-delay: 10

User-agent: CCBot
Crawl-delay: 30
```

---

## Important Reminders

- All generation and validation happens inline in the conversation. Do NOT write to disk — the user copy-pastes it themselves.
- **Never invent paths.** Only Disallow paths confirmed to exist on the specific site (found in sitemap, visible in nav, or confirmed by the user).
- **No comments in the output.** Clean file only.
- Be concise. A minimal robots.txt is better than a bloated one with guessed rules.
- If the user doesn't mention AI bots, don't add AI crawler blocks.