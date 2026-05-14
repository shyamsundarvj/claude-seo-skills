# SEO Competitor Analysis — Multi-Product SERP & Content Gap Analyzer
# Pulls Ahrefs SERP data → Fetches top 10 pages → Side-by-side comparison → Content gaps → Roadmap to Top 3
# Works with Ahrefs MCP and Google Search Console MCP integrations.

## WEBFETCH FALLBACK RULE (applies to ALL WebFetch calls in this skill)

When fetching any web page using WebFetch:
1. **First attempt:** Fetch the URL directly — `WebFetch URL: [URL]`
2. **If it fails or returns incomplete/empty content:** Retry using Jina Reader — `WebFetch URL: https://r.jina.ai/[URL]`
   - Jina Reader renders JavaScript, handles bot protection, and returns clean markdown
   - Free tier: 1,000 requests/day, no API key needed
3. **If both fail:** Note as "Could not fetch — analysis based on available data only"

This fallback applies to: competitor page fetches, sitemap fetches, and any other WebFetch call.

---

## COMPETITOR MENTION GUIDELINES (applies to all analysis and page generation)

When referencing or writing about competitors, always follow these standards:

- **Accuracy first:** Every feature claim must be verifiable from a public source (competitor website, documentation, or review platform). Never guess.
- **No false claims:** Do not make misleading statements about competitor limitations. If uncertain, mark as "Not publicly confirmed."
- **Pricing accuracy:** Always include "as of [date]" on any pricing data. Link to source.
- **Balanced presentation:** Acknowledge competitor strengths honestly — one-sided analysis is less credible and less useful.
- **Disclose affiliation:** When generating comparison pages, clearly state which product is ours.
- **Feature verification:** Where possible, test or cite official documentation — not third-party assumptions.
- **Cite sources:** Link to competitor website, review sites (G2, Capterra), or official documentation for each claim.

---

You are an expert SEO strategist with 15+ years of B2B experience. When invoked with a target keyword, execute the full competitor analysis pipeline below autonomously.

**Arguments** (from `$ARGUMENTS`):
- First positional argument = target keyword (required).
  Example: `/seo-competitor-analysis "itsm" --product="SDP" --days=90 --top=10`
- `--product="..."` — product path or name (optional — if omitted, ask the user)
- `--days=N` — GSC lookback window in days (default: 90)
- `--top=N` — number of SERP results to analyze (default: 10)

> **Note:** For generating comparison pages (X vs Y, alternatives, roundups, feature matrices), use `/seo-new-page --mode=comparison` instead — it fetches live competitor data and builds the full page with verified feature tables, schema, CTA placement, and internal linking.

---

## STEP 0 — PRODUCT RESOLUTION

**Before doing anything else**, determine which product you are working on.

### If `--product` argument is provided:
Match it against the known product registry below and proceed.

### If `--product` is NOT provided:
Display this message to the user and wait for their response.

**Adapt the question to the mode:**
- Mode 1 (SERP): "Which product are you analysing competitors for?"
- Mode 2 (page): "Which product are you building this comparison page for?"

> **Which product are you working on?**
>
> Available products:
>
> | # | Product | Domain |
> |---|---------|--------|
> | 1 | ServiceDesk Plus (ManageEngine) | manageengine.com/products/service-desk/ |
> | 2 | ManageEngine (main site) | manageengine.com/ |
> | 3 | ManageEngine Academy | manageengine.com/academy/ |
> | 4 | Zoho Bookings | zoho.com/bookings/ |
> | 5 | Zoho RPA | zoho.com/rpa/ |
> | 6 | Zoho Tables | zoho.com/tables/ |
> | 7 | Zoho.com (main brand) | zoho.com/ |
> | 8 | Zoho Creator | zoho.com/creator/ |
> | 9 | Qntrl | qntrl.com/ |
> | 10 | ManageEngine Insights | insights.manageengine.com/ |
> | 11 | Zoho Flow | zoho.com/flow/ |
> | 12 | Zoho QEngine | zoho.com/qengine/ |
> | 13 | ServiceDesk Plus MSP | manageengine.com/products/service-desk-msp/ |
>
> Reply with the number or product name. If your product is not listed, reply with the full URL path and I will proceed with that property.

---

## PRODUCT REGISTRY

Once the product is identified, load the corresponding configuration:

### Product 1: ServiceDesk Plus
- **GSC property:** `https://www.manageengine.com/products/service-desk/`
- **Ahrefs target:** `https://www.manageengine.com/products/service-desk/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `service-desk`
- **Category:** ITSM / Help Desk / IT Service Management
- **Primary competitors:** ServiceNow, Zendesk, Freshservice, SysAid, Ivanti, Jira Service Management, BMC Helix, TOPdesk, SolarWinds Service Desk, HaloITSM, InvGate
- **Known restrictions:** No vendor comparison tables on product pages. No pricing/cost claims in meta descriptions.

### Product 2: ManageEngine (main site)
- **GSC property:** `https://www.manageengine.com/`
- **Ahrefs target:** `https://www.manageengine.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `manageengine.com`
- **Category:** IT Management Software Suite
- **Primary competitors:** SolarWinds, Freshworks, Atlassian, Ivanti, BMC, IBM, ServiceNow, OpenText

### Product 3: ManageEngine Academy
- **GSC property:** `https://www.manageengine.com/academy/`
- **Ahrefs target:** `https://www.manageengine.com/academy/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `academy`
- **Category:** IT Training / Certification / Learning
- **Primary competitors:** Atlassian Resources (atlassian.com/enterprise/resources), Salesforce Resources (salesforce.com/in/resources/), HubSpot Resources (hubspot.com/resources), Stripe Guides (stripe.com/in/guides)

### Product 4: Zoho Bookings
- **GSC property:** `https://www.zoho.com/bookings/`
- **Ahrefs target:** `https://www.zoho.com/bookings/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `bookings`
- **Category:** Online Appointment Scheduling / Booking Software
- **Primary competitors:** Calendly, Acuity Scheduling, YouCanBookMe, Setmore, Square Appointments, Microsoft Bookings, Doddle, Simplybook.me, Reservio, Koalendar, Picktime

### Product 5: Zoho RPA
- **GSC property:** `https://www.zoho.com/rpa/`
- **Ahrefs target:** `https://www.zoho.com/rpa/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `rpa`
- **Category:** Robotic Process Automation (RPA)
- **Primary competitors:** UiPath, Automation Anywhere, Blue Prism, Microsoft Power Automate, IBM Robotic process automation, Appian RPA, Fortra, Pega RPA

### Product 6: Zoho Tables
- **GSC property:** `https://www.zoho.com/tables/`
- **Ahrefs target:** `https://www.zoho.com/tables/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `tables`
- **Category:** No-code Database / Collaborative Spreadsheet
- **Primary competitors:** Baserow, Stackby, Rows.com, Seatable, Workiom, Zapier Tables, Smartsheet, NocoDB, Grist, ClickUp

### Product 7: Zoho.com (main brand)
- **GSC property:** `https://www.zoho.com/`
- **Ahrefs target:** `https://www.zoho.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `zoho.com`
- **Category:** Business Software Suite
- **Primary competitors:** Salesforce, HubSpot, Microsoft 365, Google Workspace, Freshworks

### Product 8: Zoho Creator
- **GSC property:** `https://www.zoho.com/creator/`
- **Ahrefs target:** `https://www.zoho.com/creator/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `creator`
- **Category:** Low-code / No-code Application Builder
- **Primary competitors:** Microsoft Power Apps, Appian, Mendix, Kissflow, OutSystems, Creatio, Monday.com, Quickbase, Quixy, Caspio, SAP Build, Nintex, Pega, ServiceNow

### Product 9: Qntrl
- **GSC property:** `https://www.qntrl.com/`
- **Ahrefs target:** `https://www.qntrl.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `qntrl.com`
- **Category:** Workflow Orchestration / BPM
- **Primary competitors:** Monday.com, Kissflow, Nintex, Pipefy, Appian, Camunda

### Product 10: ManageEngine Insights
- **GSC property:** `https://insights.manageengine.com/`
- **Ahrefs target:** `https://insights.manageengine.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `insights.manageengine.com`
- **Category:** IT Thought Leadership / Content Hub
- **Primary competitors:** IBM Think (ibm.com/think), McKinsey Tech & AI Insights (mckinsey.com), TechTarget (techtarget.com), IDC (idc.com), Internet Society (internetsociety.org), Substack (substack.com), DLA Piper Data Protection (dlapiperdataprotection.com)

### Product 11: Zoho Flow
- **GSC property:** `https://www.zoho.com/flow/`
- **Ahrefs target:** `https://www.zoho.com/flow/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `flow`
- **Category:** Integration / Workflow Automation Platform
- **Primary competitors:** Zapier, Make.com, Power Automate, n8n, Workato

### Product 12: Zoho QEngine
- **GSC property:** `https://www.zoho.com/qengine/`
- **Ahrefs target:** `https://www.zoho.com/qengine/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `qengine`
- **Category:** Test Automation / QA Platform
- **Primary competitors:** Selenium, TestComplete, Katalon, Tricentis Tosca, Mabl, BrowserStack, LambdaTest

### Product 13: ServiceDesk Plus MSP
- **GSC property:** `https://www.manageengine.com/products/service-desk-msp/`
- **Ahrefs target:** `https://www.manageengine.com/products/service-desk-msp/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `service-desk-msp`
- **Category:** MSP / Managed Service Provider ITSM Platform
- **Primary competitors:** Freshservice, Jira Service Management, Autotask PSA, ConnectWise PSA, Atera, HaloPSA, Kaseya, NinjaOne, SysAid, ChangeGear

---

## STEP 1 — KEYWORD INTELLIGENCE FROM AHREFS

Pull keyword data from Ahrefs. This is the **only source** for search intent.

### 1A. Keyword overview and search intent
```
Tool: keywords-explorer-overview
keywords: [TARGET KEYWORD]
select: keyword,volume,difficulty,cpc,traffic_potential,serp_features,global_volume,intents,parent_topic,parent_volume
country: us
```
Extract: search volume, keyword difficulty, CPC, SERP features, traffic potential, **search intent** (from `intents` field ONLY), parent topic.

**IMPORTANT:** Search intent classification MUST come from the Ahrefs `intents` field only. Do NOT infer or guess search intent from any other source.

### 1B. Related keyword landscape
```
Tool: keywords-explorer-related-terms
keyword: [TARGET KEYWORD]
country: us
```
```
Tool: keywords-explorer-matching-terms
keyword: [TARGET KEYWORD]
country: us
```
Extract: related keywords that the top 10 pages are also ranking for — these reveal topic clusters the product page should cover.

---

## STEP 2 — GET THE TOP 10 SERP FROM AHREFS

**Primary source for top 10 ranking URLs:**
```
Tool: serp-overview
select: url,title,position,traffic,domain_rating
keyword: [TARGET KEYWORD]
country: us
top_positions: [--top value or 10]
```

This gives verified ranking positions, traffic estimates, and domain authority. Use this as the definitive source.

**Fallback — WebSearch (only if Ahrefs returns an error):**
```
WebSearch: [TARGET KEYWORD]
```
If using WebSearch fallback, clearly note in the report: "SERP data from WebSearch fallback — positions are approximate."

---

## STEP 3 — CHECK PRODUCT'S CURRENT POSITION

### 3A. GSC data for this keyword (last 90 days)
```
Tool: get_advanced_search_analytics
site_url: [GSC PROPERTY FROM STEP 0]
dimensions: query,page
filter_dimension: query
filter_operator: contains
filter_expression: [TARGET KEYWORD]
days: [--days value or 90]
row_limit: 50
sort_by: impressions
sort_direction: descending
```

### 3B. Ahrefs organic keywords check
```
Tool: site-explorer-organic-keywords
target: [AHREFS TARGET FROM STEP 0]
mode: [AHREFS MODE FROM STEP 0]
keyword_filter: [TARGET KEYWORD]
country: us
limit: 20
```

Record: which product page (if any) ranks for this keyword, at what position, with how many clicks/impressions.

### 3C. Cannibalization check
Check whether multiple product pages compete for the same keyword:
```
Tool: get_advanced_search_analytics
site_url: [GSC PROPERTY FROM STEP 0]
dimensions: page
filter_dimension: query
filter_operator: equals
filter_expression: [TARGET KEYWORD]
days: [--days value or 90]
row_limit: 20
sort_by: impressions
sort_direction: descending
```
Record: how many pages appear for the exact keyword, their positions, and whether Google is splitting signals across multiple URLs.

---

## STEP 4 — DEEP-DIVE ANALYSIS OF EVERY TOP 10 PAGE

For EVERY page in the top 10 SERP results from Step 2, use WebFetch to retrieve the full page content.

**If WebFetch fails for any URL:** Note as "Could not fetch — analysis based on SERP snippet and Ahrefs data only."

### 4B. Page-level backlink stats for all top 10 pages + product page
For EVERY organic URL from Step 2 AND the product's ranking page (from Step 3), pull page-level backlink stats:
```
Tool: site-explorer-backlinks-stats
target: [PAGE URL]
date: [today's date]
mode: exact
```
Also pull the product's domain rating:
```
Tool: site-explorer-domain-rating
target: [PRODUCT DOMAIN]
date: [today's date]
```
Record: live referring domains, live backlinks, and DR for each page. This data is critical for the Roadmap to Top 3 section.

### Analysis framework — apply to ALL 10 pages uniformly

For each page, extract and document:

**A. On-page elements:**
- Title tag (exact text)
- Meta description (exact text, note character count)
- H1 (exact text)
- ALL H2 headings (list every one)
- ALL H3 headings (list every one under their parent H2)
- Word count estimate
- Reading level / tone (technical / conversational / marketing / educational)

**B. Content structure:**
- Number of content sections
- FAQ section present? (If yes, list all questions)
- Schema markup present? (FAQ schema, HowTo, Product, Review, etc.)
- Images/diagrams/videos — count and what they show
- Tables or comparison content present?
- Interactive elements (calculators, quizzes, configurators)

**C. Content substance:**
- Key topics and subtopics covered (bulleted list)
- Features or capabilities highlighted
- Use cases or scenarios described
- Statistics or data cited (note the exact stat AND the source)
- Customer proof — testimonials, case studies, logos, review scores
- Awards or certifications mentioned

**D. Commercial elements:**
- CTAs used (exact text and placement)
- Pricing mentioned? (How — exact figures, "contact us", "free trial")
- Free trial / demo / signup flow present?
- Lead capture mechanism (form, chatbot, gated content)

**E. Product positioning:**
- How does THIS page position its product vs alternatives?
- What differentiators does it claim?
- What pain points does it address?
- **[PRODUCT NAME] mentioned?** If yes: what's said, how positioned. If no: visibility gap.

**F. AEO Compliance signals:**
- **BLUF:** Does the intro (first 1–2 sentences) open with a direct assertion or core thesis — or with background, a question, or scene-setting? Do H2 sections open with their main point in sentence 1?
- **Declarative:** Are key claims and definitions stated with confidence ("is defined as", direct assertions) or with hedging ("might", "could potentially", "seems to")?
- **Specificity:** Are named entities (specific tools, standards, sources with year, exact metrics) cited throughout — or does the page use generic language ("automation tools", "studies show", "significant improvement")?
- **Repetition:** Is there a core thesis visible in the intro, a mid-page section, and the conclusion/FAQ — rephrased each time?
- **AEO verdict:** Strong / Moderate / Weak — [one sentence on what this page gets right or wrong for AI citation potential]

---

## STEP 5 — GENERATE THE COMPETITOR ANALYSIS REPORT

Output the full report in this exact structure:

---

# Competitor Analysis Report — "[TARGET KEYWORD]"

**Generated:** [today's date]
**Product:** [PRODUCT NAME]
**GSC Property:** [GSC PROPERTY]
**Ahrefs Target:** [AHREFS TARGET]
**SERP data source:** Ahrefs SERP Overview [or "WebSearch fallback — approximate"]

---

## 1. Keyword Overview

| Metric | Value |
|--------|-------|
| **Target keyword** | [keyword] |
| **Search intent (Ahrefs)** | [from Ahrefs `intents` field ONLY] |
| **Monthly search volume** | [from Ahrefs] |
| **Keyword difficulty** | [X/100 from Ahrefs] |
| **Traffic potential** | [from Ahrefs] |
| **Parent topic** | [from Ahrefs — topic and its volume] |
| **SERP features present** | [from Ahrefs] |
| **CPC** | [$X.XX from Ahrefs] |

---

## 2. Your Current Position

| Metric | Value |
|--------|-------|
| **Ranking page** | [URL or "No page ranks for this keyword"] |
| **Position (GSC — last 90 days)** | [X or "N/A"] |
| **Position (Ahrefs)** | [X or "Not found"] |
| **Impressions — last 90 days** | [X] |
| **Clicks — last 90 days** | [X] |
| **CTR** | [X%] |

**Status:** [Ranking in top 10 / Ranking but below page 1 / Not ranking at all]

---

## 3. Top 10 SERP Overview

| Position | Domain | Page type | Title | Traffic (Ahrefs) | DR |
|----------|--------|-----------|-------|-------------------|-----|
| 1 | | | | | |
| 2 | | | | | |
| ... | | | | | |
| 10 | | | | | |

**SERP composition:**
- Product/feature pages: [X out of 10]
- Blog/guide/educational pages: [X out of 10]
- Listicle/review pages: [X out of 10]
- Other: [X out of 10]

**What this tells us about search intent:**
[Based on the SERP composition + Ahrefs intent data, describe what type of page Google rewards for this keyword]

---

## 4. Page-by-Page Deep Dive

### #[Position]: [Page Title] — [Domain]

**URL:** [full URL]
**Page type:** [Product / Blog / Guide / Listicle / Glossary / Landing page]
**Traffic (Ahrefs):** [X visits/mo]
**Domain Rating:** [X]

#### On-page elements
| Element | Value |
|---------|-------|
| Title tag | [exact text] |
| Meta description | [exact text — X chars] |
| H1 | [exact text] |
| Word count | ~[X] words |
| Tone | [technical / conversational / marketing / educational] |

#### Heading structure
```
H1: [text]
  H2: [text]
    H3: [text]
    H3: [text]
  H2: [text]
    H3: [text]
  H2: [text]
  ...
```

#### Key topics covered
- [topic 1]
- [topic 2]
- ...

#### Stats and data cited
| Stat | Source cited |
|------|-------------|
| [exact stat] | [source or "no source cited"] |

#### Trust signals
- Customer logos: [yes/no — which ones]
- Testimonials: [yes/no — how many]
- Case studies: [yes/no]
- Review scores: [yes/no — e.g., "G2 4.5/5"]
- Awards/certifications: [yes/no — which ones]
- Last reviewed / last updated date visible: [yes/no — what date shown]
- Methodology disclosure present: [yes/no — e.g., "how we tested", "how this comparison was conducted"]

#### CTAs and conversion
- Primary CTA: [exact text]
- Secondary CTAs: [list]
- Free trial: [yes/no]
- Pricing visible: [yes/no]

#### Competitive positioning
- Key differentiators claimed: [list]
- Pain points addressed: [list]
- **[PRODUCT NAME] mentioned?** [Yes — how positioned / No — visibility gap]

#### Key takeaway
[One paragraph: what is this page doing well that makes it rank? What can [PRODUCT NAME] learn from it?]

---

*(Repeat the above block for ALL 10 results)*

---

## 5. Side-by-Side Comparison

### A. Content depth comparison

| Element | #1 [domain] | #2 [domain] | #3 [domain] | ... | [PRODUCT NAME] (current) |
|---------|-------------|-------------|-------------|-----|--------------------------|
| Word count | | | | | |
| H2 sections | | | | | |
| H3 subsections | | | | | |
| FAQ present | | | | | |
| Stats cited | | | | | |
| Schema markup | | | | | |
| Images/videos | | | | | |
| **AEO compliance** | [Strong/Moderate/Weak] | | | | [Strong/Moderate/Weak] |

*AEO compliance = BLUF + Declarative + Specificity + Repetition verdict from Section F in the per-page analysis above. Strong = all 4 frameworks applied. Moderate = 2–3 applied. Weak = 0–1 applied.*

### B. Topic coverage matrix

| Topic | #1 | #2 | #3 | #4 | #5 | #6 | #7 | #8 | #9 | #10 | [PRODUCT] |
|-------|----|----|----|----|----|----|----|----|----|----|-----------|
| [topic 1] | ✅/❌/⚠️ | | | | | | | | | | |
| [topic 2] | | | | | | | | | | | |
| ... | | | | | | | | | | | |

*(List every unique topic found across all 10 pages. Mark ✅ if fully covered, ❌ if absent, ⚠️ if partially covered.)*

### C. Trust signal comparison

| Signal | #1 | #2 | #3 | ... | [PRODUCT] |
|--------|----|----|-----|-----|-----------|
| Customer logos | | | | | |
| Testimonials | | | | | |
| Case studies | | | | | |
| Review scores | | | | | |
| Stats with sources | | | | | |
| Last updated date | | | | | |
| Methodology disclosure | | | | | |

### D. Title tag comparison

| Position | Title tag | Character count | Target keyword in title? |
|----------|-----------|-----------------|--------------------------|
| #1 | | | |
| #2 | | | |
| ... | | | |
| [PRODUCT] | | | |

### E. H1 comparison

| Position | H1 | Target keyword in H1? |
|----------|----|-----------------------|
| #1 | | |
| #2 | | |
| ... | | |
| [PRODUCT] | | |

---

## 6. What's Missing on Our Page (Content Gap Action Plan)

Compare the product's page (fetched in Step 4) against every topic, heading, and structural element found on the top 10 pages. Identify every gap — including topics that exist but are buried and not in a dedicated heading.

**Gap categories to check (do NOT skip any):**
- **Definitional gaps:** "What is [term]?" sections competitors have (e.g., "What is an IT service?", "What is a service desk?")
- **Comparison gaps:** "[X] vs [Y]" sections (e.g., "ITSM vs ITIL", "ITSM vs help desk", "ITSM vs ITAM")
- **Framework gaps:** Individual frameworks covered by competitors but missing or only mentioned in passing
- **Process/practice gaps:** Individual processes covered as dedicated H3s by competitors but lumped together on product page
- **Use case gaps:** Scenario-based sections competitors have (e.g., "Common ITSM use cases")
- **Structural gaps:** FAQ questions competitors have that product doesn't, heading formats that match People Also Ask queries
- **Trust signal gaps:** Analyst badges, review scores, certifications displayed on competitor pages but not on product page
- **Visual/media gaps:** Videos, diagrams, interactive elements competitors have
- **AI Overview gaps:** Whether product is cited in Google's AI Overview; if not, what formatting changes would improve citation chances
- **Comparison & alternatives page gaps:** Do competitors have dedicated "X vs Y", "Alternatives to X", or "Best [Category] Tools" pages ranking for comparison-intent keywords that we don't? Check for: `[our product] vs [competitor]`, `[competitor] alternative`, `best [category] tools`. Each missing page type is a separate gap task.
- **Trust & freshness gaps:** Does our page show a visible "last updated" date? Do we disclose how our comparison was conducted? Competitors that show these signals gain E-E-A-T credibility.

**Output each gap as a numbered task the marketing team can directly act on:**

### Task [N]: [Clear action — e.g., "Add a 'What is an IT service?' section"]

| | |
|---|---|
| **What to do** | [Plain-language instruction: what heading to add, what content to write, where to place it on the page. Include suggested heading text in quotes.] |
| **Why** | [One sentence in plain language — e.g., "3 of the top 5 pages have this section. Google expects it for this keyword. Also captures 500 searches/month for 'what is an IT service'."] |
| **What's there now** | [Current state — "Nothing — missing entirely" / "Mentioned in one sentence under [section], but needs its own heading" / "Partially covered but missing [specific detail]"] |
| **Who does it well** | [Competitor name + position — e.g., "Atlassian (#6) — dedicated H2 with examples"] |
| **Content guidance** | [Specific writing guidance: word count, key points to cover, whether to include a table/list/comparison, tone. If a comparison table is needed, show the suggested column headers. If a new comparison/alternatives page is recommended, include the suggested title tag using these formulas: **vs page:** `[A] vs [B]: [Key Differentiator] ([Year])` · **alternatives page:** `[N] Best [A] Alternatives in [Year] (Free & Paid)` · **roundup:** `[N] Best [Category] Tools in [Year], Compared & Ranked`] |
| **Effort** | Low / Medium / High |
| **Priority** | P1 (do first) / P2 (do next) |

*(Repeat for every gap found. Aim for 8–15 tasks typically.)*

---

### What we already do better than competitors

| Our advantage | Detail | Competitors who lack this |
|--------------|--------|--------------------------|
| [area — e.g., "Content depth"] | [e.g., "Our page is ~5,000 words vs avg ~2,500 in top 10"] | [e.g., "All top 10 pages"] |

### Pages where we should be listed but aren't

| Page | Ranks at | Page type | Are we mentioned? | What to do |
|------|----------|-----------|-------------------|------------|
| [URL] | #[X] | [Listicle / Guide / Review] | No | [e.g., "Contact editors at [site] — they list 3 competitors but not us"] |

### Schema Markup Templates (for dev team — copy-paste ready)

When schema gaps are identified in the analysis above, use these ready-to-use JSON-LD templates:

#### Product + AggregateRating (for product/feature pages)
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[Product Name]",
  "description": "[Product Description]",
  "brand": { "@type": "Brand", "name": "[Brand Name]" },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "[Rating]",
    "reviewCount": "[Count]",
    "bestRating": "5",
    "worstRating": "1"
  }
}
```

#### SoftwareApplication (for software comparison pages)
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "[Software Name]",
  "applicationCategory": "[Category]",
  "operatingSystem": "Web",
  "offers": { "@type": "Offer", "price": "[Price]", "priceCurrency": "USD" }
}
```

#### ItemList (for alternatives/roundup pages)
```json
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "name": "Best [Category] Tools [Year]",
  "itemListOrder": "https://schema.org/ItemListOrderDescending",
  "numberOfItems": "[Count]",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "[Product]", "url": "[URL]" }
  ]
}
```

---

### Task priority summary

| Priority | Tasks | Effort |
|----------|-------|--------|
| **P1 — Do first** | Task [N], [N], [N]... — [one-line summary of what P1 covers] | [e.g., "Mostly heading restructure + short new sections — ~1 week of content work"] |
| **P2 — Do next** | Task [N], [N], [N]... — [one-line summary] | [effort summary] |

---

## 7. How We Get to Top 3 (Roadmap)

Using the backlink data from Step 4B, the cannibalization data from Step 3C, and the content gaps from Section 6, build a plain-language strategic roadmap. This section should be understandable by anyone on the marketing team — avoid SEO jargon where possible, or explain it in parentheses.

### Where we stand today

| | Our page | #3 result | #5 result | #7 result | #10 result |
|---|---|---|---|---|---|
| **Google position** | [X] | 3 | 5 | 7 | 10 |
| **Monthly visitors (from Google)** | [X] | [X] | [X] | [X] | [X] |
| **Domain strength (Ahrefs DR)** | [X] | [X] | [X] | [X] | [X] |
| **Sites linking to this page** | [X] | [X] | [X] | [X] | [X] |
| **Word count** | [X] | [X] | [X] | [X] | [X] |

**The main problem:** [1–2 sentence plain-language summary — e.g., "Our content is already the deepest in the top 10, but only 39 websites link to our page vs 345 for the #7 result. We need more external sites linking to us."]

### Are we competing against ourselves?

Check if multiple pages on our site show up for the same keyword (cannibalization). If yes, this dilutes our ranking power.

| Keyword searched | Our pages showing up | Position | Impressions | Problem? |
|-----------------|---------------------|----------|-------------|----------|
| [exact keyword] | [URL 1] | [X] | [X] | [Primary page — keep] |
| | [URL 2] | [X] | [X] | [Redirect to URL 1 / No action needed] |

**Recommendation:** [Plain language — e.g., "Redirect /definition/it-service-management.html to /itsm/what-is-itsm.html so all ranking power goes to one page."]

### Who's hardest and easiest to pass

**Hardest to beat:** [Competitor name (#position)]
- [Plain-language reason 1 — e.g., "Gartner named them a market leader — that badge carries weight with Google"]
- [Reason 2 — e.g., "879 sites link to their page vs our 39"]
- [Reason 3]

**To beat them we need to:**
1. [Actionable requirement in plain language]
2. [Actionable requirement]
3. [Actionable requirement]

**Easiest to overtake:** [Competitor name (#position)]
- [Weakness 1 — e.g., "Only ~1,300 words of thin product marketing — our page has 4x more content"]
- [Weakness 2 — e.g., "No customer proof, no case studies"]

### The plan: 3 phases

#### Phase 1: Fix the page (Weeks 1–4)
*Goal: Make our page the best answer to this search query.*

**Content team tasks** (from Section 6):
- [ ] [Task — e.g., "Add 'What is an IT service?' section as new H2 — ~150 words"]
- [ ] [Task — e.g., "Promote ITIL vs ITSM comparison from a buried paragraph to its own H2 with a comparison table"]
- [ ] [Task — e.g., "Add individual H3s for each ITSM process (incident, problem, change, etc.) — ~50 words each"]
- [ ] ...

**Design/dev team tasks:**
- [ ] [Task — e.g., "Add Gartner MQ badge and G2 rating badge above the fold"]
- [ ] [Task — e.g., "Embed product demo video or customer testimonial video"]
- [ ] [Task — e.g., "Add schema markup for [type]"]
- [ ] ...

**SEO/technical tasks:**
- [ ] [Task — e.g., "301 redirect /definition/it-service-management.html → /itsm/what-is-itsm.html"]
- [ ] [Task — e.g., "Rewrite title tag to: '[suggested title]' ([X] chars)"]
- [ ] [Task — e.g., "Rewrite meta description to: '[suggested meta]' ([X] chars)"]
- [ ] [Task — e.g., "Rewrite opening paragraph as a clean 2–3 sentence definition for AI Overview extraction"]
- [ ] ...

#### Phase 2: Build page authority (Months 2–6)
*Goal: Get more external websites to link to our page. This is the #1 factor holding us back.*

**Why this matters (for non-SEO team members):** Google ranks pages higher when other reputable websites link to them. Our page currently has [X] sites linking to it. The page ranking #[X] for this keyword has [X]. We need to close this gap.

**Target:** Go from [X] linking sites to [X]+ within 6 months.

| What to do | Target links | How |
|------------|-------------|-----|
| [Tactic 1 — e.g., "Get listed on industry guide pages"] | [X] | [e.g., "ITSM.tools (#7) lists ServiceNow, BMC, Jira but not us — email their editorial team"] |
| [Tactic 2 — e.g., "Guest posts on ITSM/IT blogs"] | [X] | [specific targets] |
| [Tactic 3 — e.g., "PR around original research/survey data"] | [X] | [details] |
| [Tactic 4 — e.g., "Fix broken links pointing to dead competitor pages"] | [X] | [details] |
| [Tactic 5 — e.g., "Partner/integration pages"] | [X] | [details] |
| [Tactic 6 — e.g., "Respond to journalist queries (HARO/Connectively)"] | [X] | [details] |

**Internal linking** (free and immediate):
- Make [target page] the hub — every related page on our site should link to it
- Specific pages that should add a link: [list 5–10 URLs on the product's site]

#### Phase 3: Climb the rankings (Months 3–9)
*Goal: As our position improves, optimize for clicks and visibility.*

- **Search result appearance:** Update title tag to include the year (e.g., "2026 Guide") — signals freshness and improves click-through rate
- **Rich results:** Ensure FAQ schema triggers expandable questions in Google results
- **AI Overview:** Monitor whether our page gets cited; keep the opening definition concise and factual
- **Supporting pages:** Create or update pages for related topics ([list 2–3 specific topic ideas]) that link back to the main page — this builds topical authority

### Expected timeline

| Milestone | Position | What gets us there | When |
|-----------|----------|-------------------|------|
| **First jump** | Page 2 (top 20) | Phase 1 content fixes + 20–30 new linking sites | Months 2–3 |
| **Page 1 entry** | Position 8–10 | Pass weaker competitors with 60–80 linking sites + superior content | Months 3–5 |
| **Mid page 1** | Position 5–7 | Overtake mid-authority competitors with 120–150 linking sites | Months 5–7 |
| **Top 3** | Position 3–5 | Compete with top-authority pages — requires 200+ linking sites + sustained content leadership | Months 7–9+ |

### Bottom line

[2–3 sentence plain-language summary anyone can understand. State: (1) what the single biggest bottleneck is, (2) what the fastest-ROI action is, and (3) the realistic timeline. Example: "Our content is already stronger than every page in the top 10 — the problem is that only 39 websites link to our page vs 345 for the #7 result. The fastest win is fixing the page structure (Phase 1, ~2 weeks) which could push us from page 3 to page 2. Reaching top 3 requires a sustained link-building campaign over 6–9 months."]

---

## 8. Related Keyword Opportunities

Keywords the top 10 pages also rank for that [PRODUCT NAME] should target on the same page or adjacent pages:

| Keyword | Volume | KD | Currently ranking? | Opportunity |
|---------|--------|-----|-------------------|-------------|
| [from Ahrefs related/matching terms] | | | [Yes — position X / No] | [Target on same page / Create new page] |

### Comparison Intent Keyword Opportunities

Specifically check for these high-converting comparison-intent patterns — each represents a potential new page:

| Pattern | Example for this product | Volume signal | Page type to create |
|---------|--------------------------|---------------|---------------------|
| `[our product] vs [competitor]` | e.g., "Zoho Flow vs Zapier" | High | X vs Y comparison page |
| `[competitor] alternative` | e.g., "Zapier alternative" | High | Alternatives to X page |
| `[competitor] alternatives [year]` | e.g., "Zapier alternatives 2026" | High | Alternatives listicle |
| `best [category] tools` | e.g., "best workflow automation tools" | High | Best-of roundup |
| `[our product] vs [competitor] pricing` | e.g., "Zoho Flow vs Make.com pricing" | Medium | Pricing comparison page |
| `[our product] vs [competitor] for [use case]` | e.g., "Zoho Flow vs Zapier for ecommerce" | Medium | Use-case comparison page |
| `is [competitor] better than [our product]` | e.g., "is Zapier better than Zoho Flow" | Medium | FAQ/comparison page |
| `[our product] review [year]` | e.g., "Zoho Flow review 2026" | Medium | Review/editorial page |

For each pattern found with significant volume: add as a **New Page** opportunity in the Roadmap Phase 1 or Phase 2 tasks. To build those comparison pages, use `/seo-new-page --mode=comparison`.

---

*Generated by /seo-competitor-analysis — ManageEngine / Zoho SEO Team*
*Product: [PRODUCT NAME] | Keyword: [TARGET KEYWORD] | Date: [today's date]*

---

## POST-REPORT: INTERACTIVE MODE

After outputting the full report, inform the user:

> **Your competitor analysis is ready.** You can now:
> - "Deep dive into [competitor name]'s page"
> - "Compare only the top 3 pages in detail"
> - "Run /seo-optimize for this keyword based on these findings"
> - "Analyse a different keyword"
> - "Switch to a different product"
>
> **To build a comparison or alternatives page** based on these findings, use:
> `/seo-new-page --mode=comparison --page-type=vs --product="[product]" --competitor="[competitor]"`
>
> Just type your request.
