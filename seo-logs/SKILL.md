---
name: seo-logs
description: >
  Server log analysis for SEO. Covers Apache/Nginx/IIS log parsing, Googlebot
  crawl pattern identification, crawl budget optimization, bot inventory,
  redirect chain detection via logs, and log vs. GSC discrepancy analysis.
  Use when user says "log analysis", "server logs", "crawl logs", "Googlebot
  activity", "crawl budget", "bot traffic", or "access logs".
user-invokable: true
argument-hint: "[log file path or URL]"
allowed-tools: Read, Grep, Glob, Bash, WebFetch
---

# Server Log Analysis for SEO

Server logs are the ground truth of how search engines interact with your site. Unlike GSC (which samples), logs capture every single request — making them the most reliable source for crawl analysis.

---

## 1. Log Formats

### Apache Combined Log Format (default)
```
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /index.html HTTP/1.1" 200 2326 "http://referer.com" "Mozilla/5.0..."
```
Fields: `IP - user [timestamp] "METHOD /path HTTP/version" status bytes "referer" "user-agent"`

### Nginx Default Format
```
127.0.0.1 - - [10/Oct/2000:13:55:36 +0000] "GET /index.html HTTP/1.1" 200 2326 "http://referer.com" "Mozilla/5.0..."
```
Identical structure to Apache combined log.

### IIS W3C Format
```
2024-01-10 13:55:36 127.0.0.1 GET /index.html - 80 - 192.168.1.1 Mozilla/5.0 200 0 0 1234
```
Fields vary — check IIS log field header line (starts with `#Fields:`).

### How to Enable Full Logging
```nginx
# Nginx: ensure combined log format
access_log /var/log/nginx/access.log combined;

# Apache: ensure CustomLog is using combined format
CustomLog ${APACHE_LOG_DIR}/access.log combined
```

---

## 2. Quick Bash Analysis Commands

### Extract All Bot Traffic
```bash
# List all unique user agents in the log
awk -F'"' '{print $6}' access.log | sort | uniq -c | sort -rn | head -30

# Show only Googlebot requests
grep -i "googlebot" access.log | wc -l

# Show Googlebot requests by URL
grep -i "googlebot" access.log | awk '{print $7}' | sort | uniq -c | sort -rn | head -50
```

### HTTP Status Code Distribution
```bash
# Count all status codes
awk '{print $9}' access.log | sort | uniq -c | sort -rn

# Show only 4xx errors
awk '$9 ~ /^4/' access.log | awk '{print $9, $7}' | sort | uniq -c | sort -rn

# Show only 5xx errors
awk '$9 ~ /^5/' access.log | awk '{print $9, $7}' | sort | uniq -c | sort -rn
```

### Crawl Frequency Over Time
```bash
# Googlebot requests per hour
grep -i "googlebot" access.log | awk '{print $4}' | cut -d: -f1,2 | sort | uniq -c

# Googlebot requests per day
grep -i "googlebot" access.log | awk '{print $4}' | cut -d: -f1 | tr -d '[' | sort | uniq -c
```

### Redirect Chain Detection
```bash
# Find all 301/302 responses served to Googlebot
grep -i "googlebot" access.log | awk '$9 == "301" || $9 == "302" {print $7, $9}' | sort | uniq -c | sort -rn

# Find chains: URLs that appear as both source of redirect AND target of another redirect
grep -i "googlebot" access.log | awk '$9 == "301" {print $7}' | sort > redirected_urls.txt
```

### Response Time Analysis
```bash
# For Nginx with $request_time in log format
grep -i "googlebot" access.log | awk '{print $NF, $7}' | sort -n | tail -20
# Slowest pages crawled by Googlebot
```

---

## 3. Bot Identification

### Verified Search Engine Bots
| User-Agent String | Bot | Company | Verify DNS? |
|------------------|-----|---------|------------|
| `Googlebot` | Google Search | Google | Yes |
| `Googlebot-Image` | Google Images | Google | Yes |
| `AdsBot-Google` | Google Ads | Google | Yes |
| `Google-InspectionTool` | GSC URL Inspection | Google | Yes |
| `Bingbot` | Bing Search | Microsoft | Yes |
| `DuckDuckBot` | DuckDuckGo | DuckDuckGo | No |
| `Slurp` | Yahoo! Search | Yahoo/Oath | No |
| `Baiduspider` | Baidu | Baidu | No |
| `YandexBot` | Yandex | Yandex | No |

### Verify Googlebot Authenticity
Real Googlebot resolves to `*.googlebot.com` or `*.google.com`:
```bash
# Get IP from log, reverse DNS lookup
host 66.249.66.1
# Should return: 1.66.249.66.in-addr.arpa domain name pointer crawl-66-249-66-1.googlebot.com

# Forward DNS confirmation
host crawl-66-249-66-1.googlebot.com
# Should return original IP

# Quick script to verify all IPs claiming to be Googlebot
grep -i "googlebot" access.log | awk '{print $1}' | sort -u | while read ip; do
  rdns=$(host $ip | awk '{print $NF}')
  echo "$ip -> $rdns"
done
```

### AI Crawler Bots (2025-2026)

In 2026, AI bots represent a significant and growing portion of bot traffic. Identify them in logs:

| User-Agent String | Bot | Company | Purpose |
|------------------|-----|---------|---------|
| `GPTBot` | OpenAI GPTBot | OpenAI | AI training |
| `OAI-SearchBot` | OpenAI Search | OpenAI | Search indexing |
| `ChatGPT-User` | ChatGPT User | OpenAI | Real-time browsing |
| `ClaudeBot` | ClaudeBot | Anthropic | AI training |
| `Claude-SearchBot` | Claude Search | Anthropic | Search indexing |
| `Claude-User` | Claude User | Anthropic | User-initiated fetch |
| `PerplexityBot` | Perplexity | Perplexity | Search/RAG |
| `Bytespider` | TikTok/ByteDance | ByteDance | AI training |
| `CCBot` | Common Crawl | Common Crawl | Training datasets |
| `Meta-ExternalAgent` | Meta AI | Meta | AI training |
| `Amazonbot` | Amazon | Amazon | AI/Alexa |
| `Diffbot` | Diffbot | Diffbot | Knowledge graph |

```bash
# Extract all AI bot requests
grep -iE "(GPTBot|OAI-SearchBot|ChatGPT-User|ClaudeBot|Claude-SearchBot|Claude-User|PerplexityBot|Bytespider|CCBot|Meta-ExternalAgent|Amazonbot|Diffbot)" access.log | wc -l

# AI bot volume by user-agent
grep -iE "(GPTBot|ClaudeBot|PerplexityBot|Bytespider|CCBot)" access.log | \
  awk -F'"' '{print $6}' | grep -oE "(GPTBot|ClaudeBot|PerplexityBot|Bytespider|CCBot)" | \
  sort | uniq -c | sort -rn

# Detect potential fake user-agents (IPs claiming to be known bots but failing DNS verification)
# Check: does the IP resolve to a known AI provider's range?
grep -i "GPTBot" access.log | awk '{print $1}' | sort -u | while read ip; do
  rdns=$(host $ip 2>/dev/null | awk '{print $NF}')
  echo "$ip -> $rdns"
done
```

**AI bot crawl pattern analysis:**
```bash
# Pages targeted by AI bots (training data collection patterns)
grep -iE "(ClaudeBot|GPTBot|CCBot)" access.log | awk '{print $7}' | \
  sort | uniq -c | sort -rn | head -50

# Time distribution of AI bot crawls (do they respect Crawl-delay?)
grep -i "GPTBot" access.log | awk '{print $4}' | cut -d: -f1,2 | sort | uniq -c

# Check if they're respecting robots.txt (are they hitting Disallowed paths?)
# Compare crawled paths against your robots.txt Disallow rules
grep -i "ClaudeBot" access.log | awk '{print $7}' | grep -E "^/(api|wp-admin|private)" | wc -l
```

### Verify Googlebot with Published IP Ranges

The reverse DNS script works but is slow for large logs. Google publishes their IP ranges — faster for bulk verification:
```bash
# Download Googlebot IP ranges (official)
curl -s https://developers.google.com/static/search/apis/ipranges/googlebot.json | \
  python3 -c "import json,sys; [print(p['ipv4Prefix']) for p in json.load(sys.stdin)['prefixes'] if 'ipv4Prefix' in p]" > googlebot_ranges.txt

# Check if an IP is in Googlebot ranges (requires ipcalc or similar)
# For quick spot-check: use whois to confirm Google ownership
whois 66.249.66.1 | grep -i "google"
```

### SEO Crawler Bots (Not Googlebot)
| User-Agent | Tool | Purpose |
|-----------|------|---------|
| `AhrefsBot` | Ahrefs | Backlink crawling |
| `SemrushBot` | SEMrush | Site auditing |
| `DotBot` | Moz | Link analysis |
| `MJ12bot` | Majestic | Link analysis |
| `rogerbot` | Moz | Site auditing |
| `Screaming Frog SEO Spider` | Screaming Frog | Site auditing |

### Malicious Bots (Block in robots.txt or firewall)
```bash
# Detect suspicious high-volume IPs
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20
# IPs with thousands of requests that aren't known bots = likely scrapers
```

---

## 4. Crawl Budget Analysis

### What is Crawl Budget?
Google allocates a "crawl budget" — the number of pages it will crawl on your site in a given period. Wasted crawl budget = important pages not being crawled.

### Crawl Budget Wasters (Find in Logs)
```bash
# Faceted navigation / filter URLs (signs: ?, &, =)
grep -i "googlebot" access.log | awk '{print $7}' | grep "?" | sort | uniq -c | sort -rn | head -20

# Session IDs in URLs
grep -i "googlebot" access.log | awk '{print $7}' | grep -E "(PHPSESSID|sid=|sessionid)" | wc -l

# Infinite calendar / date pagination
grep -i "googlebot" access.log | awk '{print $7}' | grep -E "/[0-9]{4}/[0-9]{2}/" | wc -l

# Crawl of 404 pages (wasted budget)
grep -i "googlebot" access.log | awk '$9 == "404" {print $7}' | sort | uniq -c | sort -rn | head -20

# Already-noindex pages being crawled (wasted)
# Cross-reference with your noindex URL list
```

### Crawl Frequency Benchmarks
| Site Size | Expected Googlebot Requests/Day |
|-----------|--------------------------------|
| <1,000 pages | 50-500/day |
| 1,000-10,000 pages | 500-5,000/day |
| 10,000-100,000 pages | 5,000-50,000/day |
| >100,000 pages | 50,000+/day |

Low crawl frequency despite fresh content → crawl budget issue or site authority issue.

### Improve Crawl Budget
1. **Block waste in robots.txt**: faceted nav, session IDs, internal search results
2. **Fix 404s getting crawled**: 301 redirect or add to sitemap with correct URL
3. **Remove noindex pages from sitemap**: don't tell Google to crawl what you don't want indexed
4. **Improve site speed**: faster TTFB → Google crawls more pages per session
5. **Internal linking**: important pages should be 1-2 clicks from homepage

---

## 5. Log vs. GSC Discrepancy Analysis

### Common Discrepancies
| Logs Show | GSC Shows | Meaning |
|-----------|-----------|---------|
| Googlebot crawled URL | URL not in index | noindex tag, canonical mismatch, or thin content |
| URL getting 200 | URL shows as 404 in GSC | Googlebot using cached 404 from before fix |
| High crawl on page | Low impressions | Page indexed but not ranking |
| Redirect (301) in logs | GSC shows "Redirect error" | Redirect chain or loop |
| No Googlebot visits | GSC shows impressions | GSC data is cached / uses cached crawl |

### Export GSC Data for Comparison
```
GSC → Performance → Pages → Download CSV
GSC → Coverage → All tabs → Download CSV
Cross-reference with: grep -i "googlebot" access.log | awk '{print $7}' | sort | uniq > crawled_urls.txt
```

---

## 6. Structured Log Analysis Workflow

### Step 1: Filter for Search Bot Traffic
```bash
# Create a bot-only log
grep -iE "(googlebot|bingbot|slurp|duckduckbot|yandexbot|baiduspider)" access.log > bot_access.log
```

### Step 2: Status Code Summary for Bots
```bash
awk '{print $9}' bot_access.log | sort | uniq -c | sort -rn
```

### Step 3: Top Crawled URLs
```bash
awk '{print $7}' bot_access.log | sort | uniq -c | sort -rn | head -100 > top_crawled.txt
```

### Step 4: Top 404s Served to Bots
```bash
awk '$9 == "404" {print $7}' bot_access.log | sort | uniq -c | sort -rn | head -50
```

### Step 5: Crawl Frequency Trend
```bash
awk '{print $4}' bot_access.log | cut -d: -f1 | tr -d '[' | sort | uniq -c
```

### Step 6: Response Time Distribution (if logged)
```bash
# Requires %D (microseconds) or %T (seconds) in log format
awk '{print $NF}' bot_access.log | awk '{sum+=$1; count++} END {print "avg:", sum/count "ms"}'
```

---

## 7. Tools for Log Analysis

### Free / Open Source
| Tool | Description | How to Use |
|------|-------------|-----------|
| **GoAccess** | Real-time web log analyzer with visual reports | `goaccess access.log -o report.html --log-format=COMBINED` |
| **AWStats** | Classic log analyzer, generates HTML reports | Configure awstats.conf, run awstats.pl |
| **bash/awk/grep** | Built-in Linux tools for quick analysis | See commands above |
| **Python pandas** | For large log files + custom analysis | Parse CSV, filter, group by |

### Commercial
| Tool | Description | Pricing |
|------|-------------|---------|
| **Screaming Frog Log Analyzer** | Purpose-built SEO log tool, bot detection, crawl audit | Paid (separate from spider) |
| **SEMrush Log Analyzer** | Integrated with SEMrush, crawl vs. ranking comparison | Paid (with SEMrush) |
| **Botify** | Enterprise log analysis + crawl intelligence (absorbed OnCrawl) | Enterprise |
| **Lumar** (formerly DeepCrawl) | Crawl + log analysis + GSC integration | Enterprise |
| **JetOctopus** | Log analysis + crawler + GSC integration | Paid |
| **Splunk** | Enterprise log management (not SEO-specific) | Enterprise |
| **ELK Stack** | Elasticsearch + Logstash + Kibana — self-hosted log platform | Free (self-hosted) |

### Cloud-Native (for high-volume sites)
| Tool | Description |
|------|-------------|
| **Google BigQuery** | SQL queries on terabytes of log data in seconds (via Cloudflare Logpush or export) |
| **AWS Athena** | Query CloudFront/S3 logs with SQL, pay-per-query |
| **Cloudflare Logpush** | Stream HTTP request logs to R2/S3/BigQuery/Splunk in real-time (Enterprise+) |
| **Cloudflare Workers Analytics Engine** | Custom metrics from edge functions, accessible via SQL API |

---

## 8. CDN Log Access — Critical Gap

**Problem:** When traffic goes through a CDN (Cloudflare, CloudFront, Fastly), origin server logs show the **CDN's IP address** — not the real client IP or bot user-agent. Bot analysis from origin logs is meaningless when behind a CDN.

**Solution: Access CDN-level logs:**

| CDN | Log Access Method |
|-----|------------------|
| **Cloudflare** | Analytics → Security → Bots (dashboard) OR Cloudflare Logpush (Enterprise) → streams HTTP logs to R2/S3/BigQuery |
| **AWS CloudFront** | CloudFront standard logging → S3 bucket → query with Athena |
| **Fastly** | Real-time log streaming → S3, BigQuery, or Splunk |
| **Akamai** | DataStream 2 → streams to S3 or custom endpoint |

```bash
# On origin server behind Cloudflare: restore real IP from CF-Connecting-IP header
# Nginx — log real client IP (not CDN IP)
log_format combined_with_cf '$http_cf_connecting_ip - $remote_user [$time_local] '
                            '"$request" $status $body_bytes_sent '
                            '"$http_referer" "$http_user_agent"';
```

**GSC Crawl Stats (when no log access):**
Settings → Crawl Stats shows (sampled, not full logs):
- Crawl requests per day by Googlebot type (Desktop/Mobile/Image/Video)
- Response codes breakdown
- File type breakdown
- Average response time trend
Use for trend analysis when origin/CDN logs aren't available.

## 9. Cloud-Based Log Analysis

For large-scale log analysis (millions of requests/day), bash/awk approaches become slow. Cloud SQL-based approaches process terabytes in seconds:

### Google BigQuery
```sql
-- Cloudflare Logpush → BigQuery: find top AI bot URLs
SELECT
  cs_uri_stem AS url,
  COUNT(*) AS requests,
  REGEXP_EXTRACT(cs_user_agent, r'(GPTBot|ClaudeBot|PerplexityBot|Bytespider)') AS ai_bot
FROM `project.dataset.cloudflare_logs`
WHERE
  REGEXP_CONTAINS(cs_user_agent, r'(GPTBot|ClaudeBot|PerplexityBot|Bytespider|CCBot)')
  AND DATE(timestamp) >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
GROUP BY 1, 3
ORDER BY 2 DESC
LIMIT 100;
```

### AWS Athena (CloudFront logs in S3)
```sql
-- Athena: Googlebot crawl rate by day
SELECT
  date_parse(date || ' ' || time, '%Y-%m-%d %H:%i:%s') AS request_time,
  uri_stem,
  sc_status
FROM cloudfront_logs
WHERE
  cs_user_agent LIKE '%Googlebot%'
  AND date >= '2026-03-01'
ORDER BY request_time DESC;
```

### Log Retention Guidance

**Minimum 90 days** for meaningful crawl analysis:
- Seasonal crawl patterns require comparing same period year-over-year
- Google can take 3+ months to fully process a site migration
- AI bot trends need baseline data before/after blocking changes

```bash
# Nginx log rotation — keep 90 days of logs
# /etc/logrotate.d/nginx
/var/log/nginx/*.log {
    daily
    rotate 90          # keep 90 days
    compress
    delaycompress
    missingok
    notifempty
}
```

## 10. Reading Logs When You Don't Have SSH Access

If you only have FTP/cPanel access:
1. **cPanel → Logs → Raw Access** → download compressed logs
2. **Hosting control panel** → usually has an "Errors" section with 404/500 logs
3. **Cloudflare** → Analytics → Security → Bots (if behind Cloudflare)
4. **Google Search Console** → Coverage + Crawl Stats (sampled, not full logs)

---

## Output Format

### Log Analysis Report

**Site:** [domain]
**Log Period:** [date range]
**Log Size:** [lines / MB]

#### Bot Traffic Summary
| Bot | Requests | % of Total | Avg Response Time |
|-----|----------|-----------|------------------|
| Googlebot | X | X% | Xms |
| Bingbot | X | X% | Xms |
| SEO Crawlers | X | X% | Xms |
| Unknown/Suspicious | X | X% | - |

#### HTTP Status Distribution (Googlebot)
| Status | Count | % | Action Needed |
|--------|-------|---|--------------|
| 200 | X | X% | ✅ |
| 301 | X | X% | Review chains |
| 404 | X | X% | Fix or redirect |
| 500 | X | X% | ⚠️ Urgent |

#### Top 10 Crawl Budget Wasters
| URL Pattern | Requests | Issue | Fix |
|-------------|----------|-------|-----|
| [pattern] | X | [type] | [action] |

#### Key Findings
1. [Finding + recommendation]
2. [Finding + recommendation]
