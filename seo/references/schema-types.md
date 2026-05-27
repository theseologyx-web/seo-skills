<!-- Updated: 2026-04-02 — Schema.org v29.4 + Google Rich Results Gallery -->
# Schema.org Types: Status, Rich Results & Recommendations

**Schema.org Version:** 29.4 (December 8, 2025)
**Source:** schema.org/docs/full.html + developers.google.com/search/docs/appearance/structured-data

---

## Format Preference
Always use **JSON-LD** (`<script type="application/ld+json">`).
Google's documentation explicitly recommends JSON-LD over Microdata and RDFa.

**AI Search Note:** Content with proper schema has ~2.5× higher chance of appearing in AI-generated answers (confirmed by Google and Microsoft, March 2025).

---

## Google-Supported Rich Result Types (Official Gallery)

These types generate **visual rich results in Google Search**. Priority for implementation.

| Schema Type | Rich Result Enables | Key Required Properties |
|-------------|-------------------|------------------------|
| `Article` / `NewsArticle` / `BlogPosting` | Enhanced article display with images | headline, image, datePublished, author |
| `BreadcrumbList` | Site hierarchy in SERP snippet | itemListElement with position, name, item |
| `Course` + `CourseInstance` | Course list carousel | name, description, provider, hasCourseInstance |
| `DiscussionForumPosting` | Forum/Q&A thread display | headline, author, datePublished, text |
| `Event` | Interactive event listings with date/place | name, startDate, location |
| `ImageObject` (via `image` property) | Google Images metadata | contentUrl, creator, copyrightHolder |
| `JobPosting` | Job search rich results | title, description, datePosted, hiringOrganization |
| `LocalBusiness` | Knowledge panel with hours, ratings, map | name, address |
| `Movie` | Movie carousel with details | name, director, image |
| `Organization` | Logo, name, contact in knowledge panels | name, url, logo |
| `Product` + `Offer` | Price, availability, ratings in SERP | name, offers (price, priceCurrency, availability) |
| `ProductGroup` | Product variants (color, size, etc.) | name, productGroupID, variesBy, hasVariant |
| `ProfilePage` + `Person` | Author/creator profile display | mainEntity (Person), name |
| `QAPage` | Question & answer page format | mainEntity with Question + acceptedAnswer |
| `Recipe` | Recipe rich card with ingredients | name, image, recipeIngredient, recipeInstructions |
| `Review` + `AggregateRating` | Star ratings in SERP snippet | ratingValue, reviewCount |
| `SoftwareApplication` / `WebApplication` | App rating and description | name, offers, (aggregateRating OR review) |
| `Speakable` | Text-to-speech for Google Assistant | cssSelector or xpath |
| `VideoObject` | Playable video with chapters/segments | name, description, thumbnailUrl, uploadDate, contentUrl |
| `WebSite` + `SearchAction` | Sitelinks search box in Google | name, url, potentialAction (SearchAction) |
| `VacationRental` | Property listings | name, image, location |
| `Event` (MathSolver) | Math problem solutions | — |
| `LoyaltyProgram` *(June 2025)* | Member pricing in Shopping | — |

---

## All Active Schema Types — By Category

### CreativeWork & Content
| Type | Use Case | Key Properties |
|------|----------|----------------|
| `Article` | Generic articles | headline, author, datePublished, image |
| `BlogPosting` | Blog posts | Same as Article |
| `NewsArticle` | News content | Same as Article + speakable |
| `AnalysisNewsArticle` | Analysis journalism | — |
| `ReportageNewsArticle` | News reporting | — |
| `Review` | Individual reviews | reviewRating, author, itemReviewed |
| `Book` | Books | name, author, isbn |
| `Movie` | Films | name, director, actor |
| `TVSeries` | TV shows | name, actor, numberOfEpisodes |
| `Episode` | Individual episodes | name, episodeNumber, partOfSeries |
| `MusicRecording` | Songs | name, byArtist, inAlbum |
| `Podcast` | Podcast shows | name, author, webFeed |
| `Recipe` | Food recipes | name, recipeIngredient, recipeInstructions, cookTime |
| `HowTo` | Step-by-step guides | name, step — **rich results deprecated Sept 2023, keep for semantics** |
| `Course` | Educational courses | name, description, provider, hasCourseInstance |
| `Dataset` | Data sets | name, description — **rich results retired late 2025** |
| `SoftwareApplication` | Desktop/mobile apps | name, operatingSystem, applicationCategory, offers |
| `WebApplication` | Browser-based SaaS | name, applicationCategory, browserRequirements, offers |
| `VideoObject` | Video content | name, thumbnailUrl, uploadDate, contentUrl, duration |
| `ImageObject` | Images | contentUrl, caption, creator |
| `WebSite` | Site-level entity | name, url, potentialAction |
| `WebPage` | Page-level | name, datePublished, breadcrumb |
| `AboutPage` | About pages | name, description |
| `ContactPage` | Contact pages | name, url |
| `ProfilePage` | Author/creator profiles | mainEntity (Person) |
| `FAQPage` | Q&A content | mainEntity (Question + acceptedAnswer) — **restricted (see below)** |
| `QAPage` | Community Q&A | — |
| `DiscussionForumPosting` | Forum threads | headline, author, datePublished |
| `Collection` | Curated collections | — |
| `Guide` | Comprehensive guides | — |
| `SheetMusic` | Musical scores | — |
| `ArchiveComponent` | Archival materials | — |

### Organizations & People
| Type | Use Case | Key Properties |
|------|----------|----------------|
| `Organization` | Any company/entity | name, url, logo, contactPoint, sameAs |
| `Corporation` | For-profit companies | name, legalName, tickerSymbol |
| `LocalBusiness` | Physical businesses | name, address, telephone, openingHours, geo |
| `EducationalOrganization` | Schools, universities | name, address, url |
| `GovernmentOrganization` | Government entities | name, url |
| `NGO` | Non-profits | name, url, nonprofitStatus |
| `NewsMediaOrganization` | News outlets | name, url, masthead |
| `MedicalBusiness` | Healthcare businesses | — |
| `PerformingGroup` | Bands, theater groups | name, member |
| `SportsOrganization` | Sports teams/orgs | name, sport |
| `Person` | Individuals | name, jobTitle, url, sameAs, image, worksFor |

### LocalBusiness Subtypes (for local SEO)
| Type | Specific Use |
|------|-------------|
| `Restaurant` | Food establishments |
| `FoodEstablishment` | All food businesses |
| `Hotel` / `LodgingBusiness` | Accommodation |
| `MedicalClinic` / `Hospital` | Healthcare |
| `DentistOffice` | Dental practices |
| `LegalService` / `Lawyer` | Legal services |
| `AccountingService` | Accounting firms |
| `RealEstateAgent` | Real estate |
| `FinancialService` | Financial services |
| `AutoDealer` | Car dealerships |
| `GroceryStore` | Grocery |
| `Pharmacy` | Pharmacies |
| `Gym` | Fitness centers |
| `Salon` / `HairSalon` | Beauty services |
| `Library` | Libraries |

### Products & Commerce
| Type | Use Case | Key Properties |
|------|----------|----------------|
| `Product` | Physical/digital products | name, image, description, sku, brand, offers |
| `ProductGroup` | Product variants | name, productGroupID, variesBy, hasVariant |
| `Offer` | Pricing & availability | price, priceCurrency, availability, validThrough |
| `AggregateOffer` | Price ranges | lowPrice, highPrice, priceCurrency |
| `Service` | Service offerings | name, provider, areaServed, description |
| `Demand` | Demand for products | — |

### Events
| Type | Use Case |
|------|---------|
| `Event` | Generic events |
| `BusinessEvent` | Business conferences |
| `ConferenceEvent` | Conferences *(added Dec 2025)* |
| `EducationEvent` | Educational events |
| `MusicEvent` | Concerts |
| `SportsEvent` | Sports events |
| `TheaterEvent` | Theater shows |
| `PerformingArtsEvent` | Performing arts *(added Dec 2025)* |
| `FoodEvent` | Food festivals |
| `Festival` | Festivals |
| `SaleEvent` | Sales events |
| `ExhibitionEvent` | Exhibitions |
| `Hackathon` | Hackathons |
| `CourseInstance` | Specific course run dates |
| `EventSeries` | Recurring events |

### Ratings & Reviews
| Type | Use Case | Key Properties |
|------|----------|----------------|
| `Review` | Individual review | reviewRating, author, itemReviewed, reviewBody |
| `AggregateRating` | Summary rating | ratingValue, reviewCount, bestRating, worstRating |
| `Rating` | Base rating | ratingValue |

### Places & Locations
| Type | Use Case |
|------|---------|
| `Place` | Generic location |
| `PostalAddress` | Mailing address |
| `GeoCoordinates` | Lat/Long coordinates |
| `AdministrativeArea` | City, state, country |
| `CivicStructure` | Public buildings |
| `Airport` | Airports |
| `LandmarksOrHistoricalBuildings` | Monuments |
| `Accommodation` | Lodging |
| `House` / `Apartment` | Residential |
| `VirtualLocation` | Online-only locations |

### Structured Data — Supporting Types
| Type | Use Case | Key Properties |
|------|----------|----------------|
| `BreadcrumbList` | Navigation path | itemListElement (position, name, item) |
| `ItemList` | Lists of items | itemListElement |
| `ListItem` | Individual list item | position, item, url |
| `SearchAction` | Sitelinks search box | target, query-input |
| `Brand` | Brand entity | name, logo, url |
| `OpeningHoursSpecification` | Business hours | dayOfWeek, opens, closes |
| `ContactPoint` | Contact information | telephone, contactType, availableLanguage |
| `JobPosting` | Job listings | title, description, hiringOrganization, jobLocation |
| `MonetaryAmount` | Price amounts | value, currency |
| `QuantitativeValue` | Numeric values with units | value, unitCode |
| `PropertyValue` | Generic name-value pairs | name, value |
| `EntryPoint` | API/action entry points | urlTemplate, actionPlatform |
| `Language` | Language entities | name, alternateName |
| `Audience` | Target audience | audienceType |
| `DefinedTerm` | Glossary terms | name, description, inDefinedTermSet |
| `MemberProgram` *(June 2025)* | Loyalty programs | — |
| `LoyaltyProgram` *(June 2025)* | Loyalty card programs | — |

### Medical (health-lifesci extension)
Use only for verified healthcare/medical content:
`MedicalCondition`, `Drug`, `MedicalProcedure`, `MedicalTest`, `AnatomicalStructure`, `Symptom`, `MedicalClinic`, `Physician`

---

## Restricted Types

| Type | Restriction | Nuance |
|------|------------|--------|
| `FAQPage` | Google rich results: **government and healthcare only** (since Aug 2023) | Still useful for AI/LLM citation (ChatGPT, Perplexity) on any site |

---

## Deprecated / Retired Types — Never Recommend for Rich Results

| Type | Status | Since | Alternative |
|------|--------|-------|-------------|
| `HowTo` | Rich results removed | Sept 2023 | Use `Article` for step-by-step content |
| `SpecialAnnouncement` | Deprecated | July 31, 2025 | Remove from sites |
| `CourseInfo` | Retired | June 2025 | Use `Course` |
| `EstimatedSalary` | Retired | June 2025 | Remove |
| `LearningVideo` | Retired | June 2025 | Use `VideoObject` |
| `ClaimReview` | Retired | June 2025 | Remove |
| `VehicleListing` | Retired | June 2025 | Remove |
| `Practice Problem` | Retired | Late 2025 | Use `Article` |
| `Dataset` (rich results) | Retired | Late 2025 | Keep schema for semantics only |

---

## Recent Additions (2024-2026)

| Type / Feature | Added | Notes |
|---------------|-------|-------|
| Product Certification markup | April 2025 | Energy ratings, safety certifications |
| `ProductGroup` | 2025 | E-commerce product variants |
| `ProfilePage` | 2025 | Author profiles for E-E-A-T |
| `DiscussionForumPosting` | 2024 | Forum/community content |
| `LoyaltyProgram` + `MemberProgram` | June 2025 | Loyalty card member pricing |
| Organization shipping/return policies | Nov 2025 | Via Search Console, no Merchant Center needed |
| `ConferenceEvent` | Dec 2025 | Schema.org v29.4 |
| `PerformingArtsEvent` | Dec 2025 | Schema.org v29.4 |

---

## SoftwareApplication / WebApplication — Full Properties

**Required (for rich results):**
- `name` — app name
- `offers.price` — set to `0` if free, include `priceCurrency` if paid
- `aggregateRating` OR `review` (at least one)

**Recommended:**
- `applicationCategory` — values: `BusinessApplication`, `EducationalApplication`, `GameApplication`, `HealthApplication`, `FinanceApplication`, `SocialNetworkingApplication`, `TravelApplication`, `ShoppingApplication`, `UtilitiesApplication`
- `operatingSystem` — e.g., "Windows", "macOS", "Web", "iOS", "Android"
- `description` — app description (must match page content)
- `screenshot` — ImageObject
- `featureList` — key features (WebApplication)
- `browserRequirements` — e.g., "Requires JavaScript" (WebApplication)

**Content consistency check:**
- `aggregateRating.ratingValue` → visible star rating on page
- `applicationCategory` → page text confirms this category
- `offers.price` → price or "Free" clearly visible on page

---

## LocalBusiness — Full Properties

**Required:**
- `name`
- `address` (PostalAddress with streetAddress, addressLocality, postalCode, addressCountry)

**Recommended:**
- `telephone`
- `url`
- `geo` (GeoCoordinates with latitude, longitude — minimum 5 decimal places)
- `openingHoursSpecification` (dayOfWeek, opens, closes)
- `priceRange` ($ to $$$$, max 100 chars)
- `aggregateRating` (for review sites)
- `department` (for multi-department businesses)
- `image`

---

## Validation Checklist

For any schema block:

1. ✅ `@context` is `"https://schema.org"` (not http)
2. ✅ `@type` is valid and not deprecated
3. ✅ All required properties present
4. ✅ Property values match expected data types
5. ✅ No placeholder text
6. ✅ URLs are absolute
7. ✅ Dates in ISO 8601 format (YYYY-MM-DD or YYYY-MM-DDTHH:MM:SS)
8. ✅ Images have valid, accessible URLs
9. ✅ **Every schema claim is verifiable in visible page content**
10. ✅ For React/Angular: JSON-LD is in server-rendered HTML, not JS-injected

## Testing Tools

- [Google Rich Results Test](https://search.google.com/test/rich-results)
- [Schema.org Validator](https://validator.schema.org/)
- [Google Search Console → Rich Results report](https://search.google.com/search-console)
