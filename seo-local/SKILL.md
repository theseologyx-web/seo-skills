---
name: seo-local
description: >
  Local SEO analysis covering Google Business Profile optimization, NAP
  consistency, citation health, review signals, local schema markup,
  location page quality, multi-location SEO, and industry-specific
  recommendations. Detects business type (brick-and-mortar, SAB, hybrid)
  and industry vertical (restaurant, healthcare, legal, home services,
  real estate, automotive). Use when user says "local SEO", "Google
  Business Profile", "GBP", "map pack", "local pack", "citations",
  "NAP consistency", "local rankings", "service area", "multi-location",
  or "local search".
user-invokable: true
argument-hint: "[url]"
license: MIT
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write
metadata:
  author: AgriciDaniel
  version: "1.7.0"
  category: seo
---

# Local SEO Analysis (March 2026)

## Key Statistics

| Metric | Value | Source |
|--------|-------|--------|
| GBP signals share of local pack weight | 32% | Whitespark 2026 |
| Proximity share of ranking variance | 55.2% | Search Atlas ML study |
| Review signals share (up from 16%) | ~20% | Whitespark 2026 |
| Google searches seeking local info | 46% | Industry data |
| Mobile "near me" searches leading to visit in 24h | 76% | Google confirmed |
| ChatGPT/AI usage for local recommendations | 45% (up from 6%) | BrightLocal LCRS 2026 |
| ChatGPT local conversion rate | 15.9% | Seer Interactive |
| Google organic local conversion rate | 1.76% | Seer Interactive |
| Local pack ads growth (Jan 2025 to Jan 2026) | 1% to 22% | Sterling Sky |

---

## Business Type Detection

Detect from page signals before analysis. This determines which checks apply.

### Brick-and-Mortar
- Physical street address visible in page content or footer
- Google Maps embed with pin/directions
- "Visit us at", "Located at", "Come see us"
- Structured address in LocalBusiness schema

### Service Area Business (SAB)
- No visible physical address
- Service area mentions: "serving [city/region]", "service area includes"
- "We come to you", "On-site service", "Mobile [service]"
- `areaServed` in schema without `address.streetAddress`

### Hybrid
- Both physical address AND service area language present
- "Visit our showroom" combined with "We also serve [areas]"

**Impact on checks**: SABs skip embedded map verification and physical address consistency. Brick-and-mortar gets full NAP + map checks.

---

## Industry Vertical Detection

Detect from page signals and GBP category patterns. Routes to industry-specific checks from `references/local-schema-types.md`.

| Vertical | Detection Signals |
|----------|------------------|
| **Restaurant** | /menu, menu items, reservations, cuisine types, food ordering, "dine-in", "takeout" |
| **Healthcare** | insurance accepted, patients, appointments, NPI, medical terms, "Dr.", HIPAA notice |
| **Legal** | attorney, lawyer, practice areas, bar admission, case results, "free consultation" |
| **Home Services** | service area, emergency service, "free estimate", licensed/insured/bonded, "24/7" |
| **Real Estate** | listings, MLS, properties for sale/rent, agent bio, brokerage, "open house" |
| **Automotive** | inventory, VIN, test drive, dealership, service department, "new/used/certified" |

If no vertical detected, use generic `LocalBusiness` analysis path.

---

## Analysis Dimensions

### 1. GBP Signals (25%)

Primary category is the **single most important local pack factor** (Whitespark #1, score: 193). Incorrect primary category is the **#1 negative factor** (score: 176).

**Check for:**
- GBP embed or reference detectable on page (Maps iframe, place ID, reviews widget)
- Primary category appropriateness (infer from page content vs visible GBP data)
- Evidence of secondary categories (optimal: 4 additional per BrightLocal)
- GBP posts presence (no direct ranking impact per WebFX, but triggers Post Justifications)
- Photos/video evidence (45% more direction requests with photos, Agency Jet)
- Q&A content (deprecated Dec 2025, replaced by Ask Maps Gemini AI -- recommend recreating Q&A content as FAQ sections on website; GBP removed existing Q&A with no export available)
- Google Verified badge eligibility (replaced Guaranteed/Screened in Oct 2025)
- GBP link URL strategy: do NOT link to strongest website page (Sterling Sky Diversity Update -- risks suppressing organic rankings)
- Business hours visibility on page (businesses open at search time rank higher, factor #5)

**Scoring guide:**
- Full: GBP embed present, category signals align, posts active, photos present
- Partial: Some GBP signals present but incomplete
- Low: No visible GBP integration on website

### 2. Reviews & Reputation (20%)

Review velocity matters more than total count. The **18-day rule** (Sterling Sky): rankings cliff if no new reviews for 3 weeks.

**Check for:**
- Total Google review count visible on page or schema (magic threshold: 10, Sterling Sky)
- Star rating (31% of consumers only use 4.5+, 68% only use 4+, BrightLocal 2026)
- Review recency indicators (74% only care about reviews in last 3 months)
- `aggregateRating` in schema (ratingValue, reviewCount, bestRating)
- Third-party review presence (consumers use average of 6 review sites, BrightLocal 2026)
- Owner response patterns (88% would use business that responds, BrightLocal)
- Review gating detection: any pre-screening of satisfaction before directing to review platform is prohibited by Google (fake engagement policy) and FTC ($53,088/violation)

**Industry-specific:**
- Healthcare: HIPAA prohibits confirming/denying reviewer is a patient in responses
- Legal: attorney-client privilege considerations in review responses

**Scoring guide:**
- Full: 10+ reviews, 4.5+ stars, recent activity, owner responses, multi-platform presence
- Partial: Some reviews but gaps in recency, rating, or response rate
- Low: <10 reviews, no recent activity, no responses, single platform only

### 3. Local On-Page SEO (20%)

Dedicated service pages = **#1 local organic factor AND #2 AI visibility factor** (Whitespark 2026).

**Check for:**
- Title tag contains city/service keywords
- H1 tag with local intent (city + service)
- NAP (Name, Address, Phone) visible in page HTML (footer, contact section, header)
- Dedicated service pages (one page per core service)
- Location page quality for multi-location sites:
  - **>60-70% unique content** minimum (industry consensus, no Google-confirmed threshold)
  - **Swap test**: if you can swap the city name and content still makes sense, it's a doorway page (RicketyRoo method). HVAC company lost 80% rankings + 63% traffic after March 2024 Core Update for this pattern
  - Local photos, area-specific testimonials, local FAQs
- Embedded Google Map (geographic signal reinforcement, not direct ranking factor -- lazy-load to mitigate speed impact)
- Click-to-call button (`tel:` link) and contact form above the fold
- Internal linking architecture: hub-and-spoke, every critical page within 3 clicks of homepage
- 2-5 contextual internal links per 1,000 words with descriptive anchor text

**Multi-location specific:**
- Store locator with individual crawlable URLs (SSR/SSG preferred over CSR)
- Subdirectory structure: `domain.com/locations/city-name/` (subdirectories consolidate link equity better, Bruce Clay: 50%+ traffic lift)
- Each location page has unique LocalBusiness schema with `@id`

**Scoring guide:**
- Full: City in title + H1, NAP visible, dedicated service pages, no doorway patterns, good internal linking
- Partial: Some local signals but missing service pages or doorway page risk
- Low: Generic title/H1, NAP not visible, thin location pages

### 4. NAP Consistency & Citations (15%)

Citations declining for traditional pack rankings but **3 of top 5 AI visibility factors are citation-related** (Whitespark 2026). Google's July 2025 documentation update removed "directories" from prominence definition.

**Check for:**
- NAP extraction: compare Name, Address, Phone from:
  1. Visible page HTML (footer, contact page)
  2. LocalBusiness JSON-LD schema
  3. Any visible GBP data
  - Flag any discrepancies between these three sources
- Citation presence on Tier 1 directories (check via WebFetch or site: search patterns):
  - Google Business Profile signals on page
  - Yelp: `site:yelp.com "Business Name"`
  - BBB: `site:bbb.org "Business Name"`
  - Facebook business page references
- Apple Business awareness (usage doubled to 27%, BrightLocal 2026 -- recommend claiming)
- Bing Places awareness (powers ChatGPT, Copilot, Alexa -- recommend claiming and optimizing)
- Industry-specific directory recommendations: load `references/local-schema-types.md` for per-vertical citation sources
- Data aggregator awareness: Data Axle, Foursquare, Neustar/TransUnion (recommend submission for downstream distribution)

**Scoring guide:**
- Full: Consistent NAP across page/schema, Tier 1 citations detected, industry directories present
- Partial: NAP present but inconsistencies, some citations missing
- Low: NAP discrepancies, no detectable citations, no schema address

### 5. Local Schema Markup (10%)

Schema is NOT a direct ranking factor (John Mueller confirmed). But enables rich results (43% CTR increase, Webstix case study) and helps AI systems parse business information.

**Check for:**
- LocalBusiness schema presence (extract JSON-LD blocks)
- Required properties: `name`, `address` with PostalAddress sub-properties
- Recommended properties: `geo` (minimum 5 decimal places, Confirmed), `openingHoursSpecification`, `telephone`, `url`, `priceRange` (<100 chars), `image`, `aggregateRating`
- **Correct subtype for industry** -- load `references/local-schema-types.md`:
  - Restaurant using `Restaurant` not generic `LocalBusiness`
  - Legal using `LegalService` not deprecated `Attorney`
  - Auto dealer using `AutoDealer` not deprecated `VehicleListing`
  - Healthcare using `MedicalClinic`/`Hospital`/`Dentist` not generic `MedicalBusiness`
- SAB-specific: `areaServed` with named cities (recommended, not in Google's official list but Schema.org supported)
- Multi-location: each location page has own LocalBusiness with unique `@id`, linked via `branchOf` to Organization on homepage
- Industry-specific schema patterns (per `references/local-schema-types.md`):
  - Restaurant: Menu + MenuSection + MenuItem + ReserveAction
  - Healthcare: Physician (Person) + MedicalSpecialty + sameAs to NPI
  - Legal: LegalService + Person + Service (practice areas)
  - Home Services: Subtype + areaServed + Service
  - Real Estate: RealEstateAgent + Person + RealEstateListing
  - Automotive: AutoDealer + Car + Offer (separate dept schemas)

**Scoring guide:**
- Full: Correct subtype, all recommended properties, industry-specific patterns, valid JSON-LD
- Partial: LocalBusiness present but generic type or missing recommended properties
- Low: No local schema, or schema with errors/placeholder content

### 6. Local Link & Authority Signals (10%)

Links declining for local pack but remain **~26% of local organic ranking** (Whitespark 2026, #2 factor group). "Best of" list placements = **#1 AI visibility citation factor**.

**Check for:**
- Local backlink indicators detectable from page:
  - Chamber of Commerce mentions or links (high Trust Flow, ~80% more consumer visits, GlueUp)
  - BBB accreditation/badge (Google uses BBB for business verification)
  - Local news/press mentions
  - Community involvement signals (sponsorships, local events, partnerships)
- "Best of" list presence (top AI visibility factor per Whitespark 2026)
- Digital PR signals: 66.2% of PR practitioners now track AI citations as KPI (BuzzStream 2026)
- Brand mentions correlate **3x more strongly** with AI visibility than traditional backlinks (Ahrefs: 0.664 vs 0.218 correlation)
- Link velocity benchmark: 5-10 quality local links/month for small businesses (consensus)

**Scoring guide:**
- Full: Local authority signals visible (chamber, BBB, press), community involvement evident
- Partial: Some authority signals but limited local link indicators
- Low: No detectable local authority signals

---

## AI Search Impact on Local

**Do not duplicate seo-geo analysis.** Provide local-specific AI context and recommend `/seo geo <url>` for full analysis.

Key local AI facts:
- AI Overviews appear on up to 68% of local searches (Whitespark Q2 2025)
- ChatGPT converts at 15.9% vs Google organic at 1.76% (Seer Interactive)
- 3 of top 5 AI visibility factors are citation-related (Whitespark 2026)
- ChatGPT does NOT access GBP directly -- sources from Bing index, Yelp, TripAdvisor, BBB, Reddit
- Bing Places is critical: powers ChatGPT, Copilot, Alexa
- AI-powered local packs (mobile US) show only 1-2 businesses, 32% fewer shown (Sterling Sky)

**Recommendation**: Run `/seo geo <url>` for comprehensive AI search visibility analysis including citability scoring, llms.txt check, and brand mention audit.

---

## Reference Files

Load on-demand as needed:
- `references/local-seo-signals.md`: Ranking factors, review benchmarks, citation tiers, GBP feature status, algorithm updates
- `references/local-schema-types.md`: LocalBusiness subtypes by industry, schema patterns, citation sources per vertical

---

## Output

Generate `LOCAL-SEO-ANALYSIS-{domain}.md` with:

1. **Local SEO Score: XX/100** with dimension breakdown table
2. **Business type**: Brick-and-mortar / SAB / Hybrid
3. **Industry vertical detected** + industry-specific findings
4. **GBP optimization checklist** (detected signals vs missing)
5. **Review health snapshot** (rating, count, velocity indicators, response patterns)
6. **NAP consistency audit** (page vs schema discrepancies, cross-source comparison)
7. **Citation presence check** (Tier 1 directory status)
8. **Local schema status** (present/missing/malformed + ready-to-use fix)
9. **Location page quality** (if multi-location: unique content %, doorway risk, store locator)
10. **Top 10 prioritized actions** (Critical > High > Medium > Low)
11. **Limitations disclaimer**: What this analysis could NOT assess (geo-grid ranking, Domain Authority, comprehensive backlinks, GBP Insights data, real-time local pack position) and which paid tools can fill those gaps

---

## Quick Wins

1. Claim and optimize Apple Business (usage doubled to 27%)
2. Claim and optimize Bing Places (powers ChatGPT, Copilot, Alexa)
3. Fix any NAP discrepancies between page, schema, and GBP
4. Add LocalBusiness schema with correct industry subtype
5. Add `geo` coordinates with 5+ decimal precision
6. Ensure phone number uses `tel:` link for click-to-call
7. Add city + service keyword to title tag and H1

## Medium Effort

1. Create dedicated page for each core service (Whitespark: #1 local organic factor)
2. Build review generation strategy maintaining 18-day minimum cadence
3. Submit to three data aggregators (Data Axle, Foursquare, Neustar/TransUnion) for downstream distribution
4. Claim industry-specific directory listings (per vertical recommendations)
5. Add industry-specific schema patterns (Menu for restaurants, Physician for healthcare, etc.)
6. Implement hub-and-spoke internal linking for service/location pages

## High Impact

1. Build local digital PR strategy targeting "best of" lists (#1 AI visibility factor)
2. Develop unique, non-swappable content for each location page (>60% unique)
3. Establish presence on platforms ChatGPT sources from (Yelp, TripAdvisor, BBB, Reddit)
4. Pursue Chamber of Commerce and BBB membership (authority + verification signals)
5. Create community involvement content (sponsorships, local events, partnerships)

---

## Beyond Google: Map & Directory Platforms

### Why Other Platforms Matter
- ChatGPT does NOT use Google — sources from Bing, Yelp, TripAdvisor, BBB, Reddit
- Apple Maps usage doubled to 27% (BrightLocal 2026)
- Waze: 150M+ users, owned by Google but separate data pipeline
- Each platform feeds different AI assistants and voice search engines

### Platform Priority Matrix

| Platform | Monthly Users | Powers | Priority |
|----------|--------------|--------|----------|
| Google Business Profile | 1B+ | Google Search, Maps, AI Overviews | Critical |
| Apple Business | 500M+ devices | Apple Maps, Siri, Spotlight | High |
| Bing Places | 100M+ | ChatGPT, Copilot, Alexa, Bing Maps | High |
| Yelp | 178M | ChatGPT, Siri, Alexa, Apple Maps | High |
| Facebook Business | 3B+ | Facebook Search, Instagram, Messenger | High |
| Waze | 150M+ | Waze navigation, Google Maps (partial) | Medium-High |
| Foursquare | 100M+ | Uber, Samsung, Microsoft, 1000+ apps | Medium |
| HERE Maps | 200M+ | Amazon Alexa, Garmin, TomTom, car nav | Medium |
| TripAdvisor | 463M | ChatGPT, Alexa, many travel apps | Medium (hospitality) |
| Nextdoor | 45M | Hyperlocal recommendations | Medium (local services) |
| MapQuest | 25M | Older demographics, some voice assistants | Low-Medium |

---

### Google Business Profile (GBP)
Already covered in detail in Analysis Dimensions above. Key action items:
- Claim, verify, and fully complete the profile
- Add photos monthly (businesses with 100+ photos get 520% more calls)
- Post weekly updates (triggers Post Justifications in local pack)
- Respond to all reviews within 24h

---

### Apple Business (Apple Maps)
Apple Maps is the default on iPhone/iPad/Mac — 1B+ Apple devices.

**How to claim:**
1. Go to business.apple.com
2. Sign in with Apple ID
3. Search for your business and claim it
4. Verify via phone or postcard

**Optimization checklist:**
- [ ] Business name exact match with GBP and website
- [ ] Complete address with suite/unit if applicable
- [ ] Primary and secondary categories set
- [ ] Phone, website, hours complete
- [ ] Add photos (interior, exterior, logo, team)
- [ ] Add Showcases (Apple's version of posts) — seasonal offers, events
- [ ] Set up Action Links (booking, ordering, reservations)
- [ ] Verify "Place Card" looks correct from an iPhone

**Why it matters for SEO:**
- Siri uses Apple Maps for "near me" queries on iOS
- Spotlight Search on iPhone uses Apple Maps data
- iOS 17+ Travel Time and Routing prioritizes well-optimized profiles

---

### Bing Places for Business
Bing Places powers: ChatGPT web search, Microsoft Copilot, Alexa (Amazon), Cortana, and Bing Maps.

**How to claim:**
1. Go to bingplaces.com
2. Sign in with Microsoft account
3. Import from Google Business Profile (fastest option) OR create manually
4. Verify via phone or email

**Optimization checklist:**
- [ ] Import/sync from GBP (keeps data consistent)
- [ ] All categories filled (allows more than GBP)
- [ ] Photos uploaded (different set from GBP is fine)
- [ ] Business description: 200-500 characters, keyword-rich
- [ ] Hours and holidays accurate
- [ ] Verify listing via Bing's verification process

**Critical for AI:** ChatGPT's web search pulls local business data from Bing index. A missing or incomplete Bing Places listing = invisible to ChatGPT recommendations.

---

### Waze for Business
Waze is owned by Google but operates an independent data pipeline. 150M+ active drivers use it for navigation and local discovery.

**Waze Advertised Pins vs. Organic Listing:**
- **Organic listing**: Free, powered by data from Waze Map Editor (community-maintained) and Google Maps
- **Advertised Pins**: Paid product — Branded Pin, Nearby Arrow, Search Promotion

**How to get listed organically:**
1. Claim/edit via **Waze Map Editor** (editor.waze.com) — free, community platform
2. Or submit via **Google Maps** (Waze pulls Google data for some regions)
3. Ensure GBP is fully optimized (Waze syncs with Google data in many markets)

**Waze optimization:**
- Business name: exact match with legal name and GBP
- Address: precise, GPS-verified location pin
- Category: select the most specific category available
- Hours: accurate — Waze shows "open now" in discovery
- Phone: click-to-call from Waze app

**Waze Ads (paid, if relevant):**
- Branded Pins: your logo on the map as users approach
- Nearby Arrow: alert drivers within X km of your location
- Best for: restaurants, gas stations, retail, service stations, hotels

---

### Facebook & Instagram Business
Facebook Business profile ranks in Google for brand queries AND feeds Instagram location data.

**Facebook Business Page for Local SEO:**
- [ ] Page category matches GBP primary category
- [ ] Complete address with map pin
- [ ] Hours, phone, website filled
- [ ] "Check-ins" enabled (social proof signal)
- [ ] Regular posts (signals active business)
- [ ] Reviews/recommendations enabled and responded to

**Instagram Location:**
- Add business address to Instagram profile
- Tag your location in every post (Instagram location pages aggregate content)
- Location-tagged posts appear in Instagram's local explore

---

### Foursquare
Foursquare's database powers 1,000+ apps including Uber, Samsung SmartThings, Snapchat, Microsoft, Twitter, and many mapping APIs. It's a data backbone of the local web.

**How to claim:**
1. Go to business.foursquare.com
2. Claim your venue
3. Verify via phone

**Why it matters:**
- Even if you never visit Foursquare, your data gets distributed to apps using their API
- Incorrect data on Foursquare propagates to hundreds of downstream apps
- Snapchat uses Foursquare for location data — relevant for younger demographics

---

### HERE Maps
HERE Maps powers navigation for Garmin, TomTom, Amazon Alexa, BMW, Mercedes, Ford, and 3,000+ business applications. Dominant in automotive and European markets.

**How to update HERE data:**
1. Go to **HERE Map Creator** (mapcreator.here.com) — free, community editing
2. Or submit via **HERE Places** (places.here.com/places/pages/home) for business claims

**Priority:** Medium — most relevant if your audience uses car navigation, voice assistants in vehicles, or if you're targeting European markets.

---

### TripAdvisor (Hospitality & Tourism)
Critical for: restaurants, hotels, attractions, tours, spas.

**Why it matters for AI:**
- ChatGPT specifically sources from TripAdvisor for hospitality recommendations
- 463M monthly visitors
- Reviews feed into AI Overviews for travel queries

**Optimization:**
- Claim your listing at tripadvisor.com/owners
- Complete all profile sections (photos, description, hours, amenities)
- Respond to every review (positive and negative)
- Encourage reviews from customers (TripAdvisor's guidelines allow asking)
- Keep info consistent with GBP

---

### Nextdoor Business
45M+ hyperlocal users. Particularly effective for home services, local retail, and neighborhood businesses.

**How to list:**
- go to business.nextdoor.com
- Free Business Page available
- "Local Deal" posts visible to neighborhood members

**Best for:**
- Plumbers, electricians, HVAC, landscaping
- Local restaurants and cafes
- Pet services, tutoring, cleaning services
- Any business that relies on neighborhood word-of-mouth

---

### Industry-Specific Platforms

| Industry | Platform | Why |
|----------|---------|-----|
| Healthcare | Zocdoc, Healthgrades, Vitals, WebMD | Siri health queries, patient discovery |
| Legal | Avvo, FindLaw, Justia, Martindale | Legal query visibility |
| Home Services | Houzz, Angi (Angie's List), HomeAdvisor, Thumbtack | Service discovery |
| Restaurants | OpenTable, Zomato, Grubhub | Reservation + delivery |
| Automotive | Cars.com, AutoTrader, DealerRater | Vehicle purchase intent |
| Hotels | Booking.com, Expedia, Hotels.com | Travel search |
| Real Estate | Zillow, Realtor.com, Trulia | Property search |
| Beauty/Wellness | Booksy, StyleSeat, Mindbody | Appointment booking |

---

## Tools for Local SEO

### All-in-One Local SEO Platforms
| Tool | Best For | Pricing |
|------|----------|---------|
| BrightLocal | Citations, rank tracking, reviews, GBP audits | Paid |
| Whitespark | Citation building, local rank tracker, GBP audit | Paid |
| Yext | Listings management across 200+ directories | Paid |
| Moz Local | NAP consistency, citation distribution | Paid |
| Semrush Local | Map pack tracking, GBP management | Paid |
| Rio SEO | Enterprise multi-location management | Paid |

### Citation Management
| Tool | Purpose | Pricing |
|------|---------|---------|
| BrightLocal Citation Builder | Manual citation service | Paid per citation |
| Whitespark Citation Finder | Find citation opportunities | Free + Paid |
| Yext | Automated sync to 200+ directories | Paid |
| Data Axle (Infogroup) | Data aggregator submission | Paid |
| Neustar/TransUnion | Data aggregator submission | Paid |
| Foursquare for Business | Foursquare + downstream apps | Free |

### Review Management
| Tool | Purpose | Pricing |
|------|---------|---------|
| GatherUp | Review generation + monitoring | Paid |
| Podium | Reviews + messaging + payments | Paid |
| Birdeye | Multi-platform reviews + reputation | Paid |
| ReviewTrackers | Aggregates 100+ platforms | Paid |
| Grade.us | Agency white-label | Paid |
| Google Business Profile (native) | Google reviews management | Free |

### Rank Tracking (Local Pack)
| Tool | Purpose | Pricing |
|------|---------|---------|
| BrightLocal | Local rank tracker (Google + Bing + Maps) | Paid |
| Whitespark Local Rank Tracker | Hyperlocal grid tracking | Paid |
| Semrush | Map pack + organic local | Paid |
| Ahrefs | Organic local + SERP features | Paid |
| GeoRanker | Geo-specific rank tracking | Paid |

### GBP Management Tools
| Tool | Purpose | Pricing |
|------|---------|---------|
| GBP native dashboard | Primary management | Free |
| Synup | Multi-location GBP management | Paid |
| Vendasta | Agency-grade GBP management | Paid |
| LocaliQ | GBP + ads + reporting | Paid |

---

## DataForSEO Integration (Optional)

If DataForSEO MCP tools are available, use `local_business_data` for live GBP data extraction, `google_local_pack_serp` for real-time local pack positions, and `business_listings` for automated citation auditing across directories.

For maps-platform analysis (geo-grid rank tracking, GBP profile via API, review velocity, cross-platform NAP), run `/seo maps <url>` — that skill handles API-based intelligence while this skill handles website-level analysis.

---

## Related Skills

| Need | Command | Why |
|------|---------|-----|
| Geo-grid rank tracking, GBP API audit, review intelligence | `/seo maps <url>` | seo-maps analyzes maps platforms via API; this skill analyzes the website |
| Schema validation and generation | `/seo schema <url>` | Full JSON-LD validation, rich result eligibility, structured data fixes |
| AI search visibility (Overviews, ChatGPT, Perplexity) | `/seo geo <url>` | llms.txt, citability scoring, AI Overview appearance, brand mention audit |
| Brand reputation and AI mention tracking | `/seo brand <url>` | Brand mentions correlate 3x more with AI visibility than traditional backlinks |
| Location page content quality and E-E-A-T | `/seo content <url>` | Unique content %, doorway page risk, thin content detection |

---

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable (DNS failure, connection refused) | Report the error clearly. Do not guess site content. Suggest the user verify the URL and try again. |
| No local signals detected on page | Report that no local business indicators were found. Suggest the user confirm this is a local business and provide the GBP listing URL if available. |
| NAP not found in page HTML | Check schema and meta tags. If still absent, flag as Critical issue. Recommend adding visible NAP to footer and contact page. |
| Industry vertical unclear | Present the top two detected verticals with supporting signals. Ask the user to confirm before applying industry-specific recommendations. |
| Multi-location with 50+ location pages | Apply the quality gates from seo orchestrator: WARNING at 30+ pages (enforce 60%+ unique), HARD STOP at 50+ pages (require user justification before continuing). |
