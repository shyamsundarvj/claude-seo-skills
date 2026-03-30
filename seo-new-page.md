# SEO New Page — Multi-Product Content Brief Generator for New Keywords
# Confirms no existing page → Ahrefs keyword data → SERP analysis → Full content brief → Internal linking plan
# Works with Google Search Console MCP and Ahrefs MCP integrations.

## WEBFETCH FALLBACK RULE (applies to ALL WebFetch calls in this skill)

When fetching any web page using WebFetch:
1. **First attempt:** Fetch the URL directly — `WebFetch URL: [URL]`
2. **If it fails or returns incomplete/empty content:** Retry using Jina Reader — `WebFetch URL: https://r.jina.ai/[URL]`
   - Jina Reader renders JavaScript, handles bot protection, and returns clean markdown
   - Free tier: 1,000 requests/day, no API key needed
3. **If both fail:** Note as "Could not fetch — analysis based on available data only"

This fallback applies to: competitor page fetches, sitemap fetches, and any other WebFetch call.

---

You are an expert SEO strategist with 15+ years of B2B experience. When invoked with a keyword that has no existing product page, generate a complete, production-ready content brief that a content writer or web team can execute directly.

**Arguments** (from `$ARGUMENTS`):
- First positional argument = target keyword (required). Example: `/seo-new-page agentic ai itsm`
- `--product="..."` — product path or name (optional — if omitted, ask the user)
- `--instructions="..."` — custom brand/content/tone instructions (optional)
- `--days=N` — GSC lookback window in days (default: 90)
- `--top=N` — number of SERP results to analyze (default: 10)
- `--format=brief|draft` — output format (default: brief). `brief` outputs a structured content brief. `draft` outputs a full content draft ready for review.

---

## STEP 0 — PRODUCT RESOLUTION

**Before doing anything else**, determine which product you are working on.

### If `--product` argument is provided:
Match it against the known product registry below and proceed.

### If `--product` is NOT provided:
Display this message to the user and wait for their response:

> **Which product is this new page for?**
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
>
> Reply with the number or product name. If not listed, provide the full URL path.

---

## PRODUCT REGISTRY

### Product 1: ServiceDesk Plus
- **GSC property:** `https://www.manageengine.com/products/service-desk/`
- **Ahrefs target:** `https://www.manageengine.com/products/service-desk/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `service-desk`
- **Site root:** `https://www.manageengine.com/products/service-desk/`
- **Category:** ITSM / Help Desk / IT Service Management
- **Primary competitors:** ServiceNow, Zendesk, Freshservice, SysAid, Ivanti, Jira Service Management, BMC Helix, TOPdesk, SolarWinds Service Desk, HaloITSM, InvGate
- **Known restrictions:** No vendor comparison tables on product pages. No pricing/cost claims in meta descriptions.

### Product 2: ManageEngine (main site)
- **GSC property:** `https://www.manageengine.com/`
- **Ahrefs target:** `https://www.manageengine.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `manageengine.com`
- **Site root:** `https://www.manageengine.com/`
- **Category:** IT Management Software Suite
- **Primary competitors:** SolarWinds, Freshworks, Atlassian, Ivanti, BMC, IBM, ServiceNow, OpenText

### Product 3: ManageEngine Academy
- **GSC property:** `https://www.manageengine.com/academy/`
- **Ahrefs target:** `https://www.manageengine.com/academy/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `academy`
- **Site root:** `https://www.manageengine.com/academy/`
- **Category:** IT Training / Certification / Learning
- **Primary competitors:** Udemy, Coursera, LinkedIn Learning, IT training providers

### Product 4: Zoho Bookings
- **GSC property:** `https://www.zoho.com/bookings/`
- **Ahrefs target:** `https://www.zoho.com/bookings/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `bookings`
- **Site root:** `https://www.zoho.com/bookings/`
- **Category:** Online Appointment Scheduling / Booking Software
- **Primary competitors:** Calendly, Acuity Scheduling, YouCanBookMe, Setmore, Square Appointments, Microsoft Bookings, Doddle, Simplybook.me, Reservio, Koalendar, Picktime

### Product 5: Zoho RPA
- **GSC property:** `https://www.zoho.com/rpa/`
- **Ahrefs target:** `https://www.zoho.com/rpa/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `rpa`
- **Site root:** `https://www.zoho.com/rpa/`
- **Category:** Robotic Process Automation (RPA)
- **Primary competitors:** UiPath, Automation Anywhere, Blue Prism, Microsoft Power Automate, IBM Robotic process automation, Appian RPA, Fortra, Pega RPA

### Product 6: Zoho Tables
- **GSC property:** `https://www.zoho.com/tables/`
- **Ahrefs target:** `https://www.zoho.com/tables/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `tables`
- **Site root:** `https://www.zoho.com/tables/`
- **Category:** No-code Database / Collaborative Spreadsheet
- **Primary competitors:** Airtable, Notion, Smartsheet, Monday.com, Coda, ClickUp, Rows

### Product 7: Zoho.com (main brand)
- **GSC property:** `https://www.zoho.com/`
- **Ahrefs target:** `https://www.zoho.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `zoho.com`
- **Site root:** `https://www.zoho.com/`
- **Category:** Business Software Suite
- **Primary competitors:** Salesforce, HubSpot, Microsoft 365, Google Workspace, Freshworks

### Product 8: Zoho Creator
- **GSC property:** `https://www.zoho.com/creator/`
- **Ahrefs target:** `https://www.zoho.com/creator/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `creator`
- **Site root:** `https://www.zoho.com/creator/`
- **Category:** Low-code / No-code Application Builder
- **Primary competitors:** OutSystems, Mendix, Microsoft Power Apps, Salesforce Platform, Bubble, Betty Blocks

### Product 9: Qntrl
- **GSC property:** `https://www.qntrl.com/`
- **Ahrefs target:** `https://www.qntrl.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `qntrl.com`
- **Site root:** `https://www.qntrl.com/`
- **Category:** Workflow Orchestration / BPM
- **Primary competitors:** Monday.com, Kissflow, Nintex, Pipefy, Appian, Camunda

### Product 10: ManageEngine Insights
- **GSC property:** `https://insights.manageengine.com/`
- **Ahrefs target:** `https://insights.manageengine.com/`
- **Ahrefs mode:** `domain`
- **URL path filter:** `insights.manageengine.com`
- **Site root:** `https://insights.manageengine.com/`
- **Category:** IT Thought Leadership / Content Hub
- **Primary competitors:** Spiceworks Insights, TechTarget, CIO.com, ComputerWeekly, BetaNews

### Product 11: Zoho Flow
- **GSC property:** `https://www.zoho.com/flow/`
- **Ahrefs target:** `https://www.zoho.com/flow/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `flow`
- **Site root:** `https://www.zoho.com/flow/`
- **Category:** Integration / Workflow Automation Platform
- **Primary competitors:** Zapier, Make (Integromat), Workato, Tray.io, n8n, Boomi

### Product 12: Zoho QEngine
- **GSC property:** `https://www.zoho.com/qengine/`
- **Ahrefs target:** `https://www.zoho.com/qengine/`
- **Ahrefs mode:** `prefix`
- **URL path filter:** `qengine`
- **Site root:** `https://www.zoho.com/qengine/`
- **Category:** Test Automation / QA Platform
- **Primary competitors:** Selenium, TestComplete, Katalon, Tricentis Tosca, Mabl, BrowserStack, LambdaTest

### Product 13: Unknown / Custom
- Ask user for: product category, top 3-5 competitors, key differentiators, content restrictions, site root URL.

---

## BRAND CONTEXT CHECK

For products 2-12 where brand context may be missing:
Before starting, ask:

> **Quick brand context check for [PRODUCT NAME]:**
> 1. What are the 2-3 key differentiators to highlight on this page?
> 2. Any content restrictions? (e.g., no competitor comparisons, no pricing claims)
> 3. Target audience and tone?
>
> Type "proceed" to skip — I'll use general best practices.

For **ServiceDesk Plus**, load context from CLAUDE.md. Do not ask unless context is missing.

---

## STEP 1 — CONFIRM NO EXISTING PAGE

### 1A. Check GSC for existing rankings
```
Tool: get_advanced_search_analytics
site_url: [GSC PROPERTY]
dimensions: query,page
filter_dimension: query
filter_operator: contains
filter_expression: [TARGET KEYWORD]
days: [--days value or 90]
row_limit: 50
sort_by: impressions
sort_direction: descending
```

### 1B. Check Ahrefs for existing rankings
```
Tool: site-explorer-organic-keywords
target: [AHREFS TARGET]
mode: [AHREFS MODE]
keyword_filter: [TARGET KEYWORD]
country: us
limit: 20
```

### 1C. Decision
- **If a page already ranks for this keyword:** Stop and inform the user:
  > **A page already exists for this keyword:**
  > - URL: [ranking URL]
  > - Position: [X]
  > - Clicks: [X] | Impressions: [X]
  >
  > Run `/seo-optimize [keyword] --product="[product]"` instead to optimise the existing page.
  > Or type "continue" if you want a new page brief anyway (e.g., targeting a different intent).

- **If no page ranks:** Proceed to Step 2.

---

## STEP 2 — KEYWORD INTELLIGENCE FROM AHREFS

### 2A. Keyword overview and search intent
```
Tool: keywords-explorer-overview
keywords: [TARGET KEYWORD]
select: keyword,volume,difficulty,cpc,traffic_potential,serp_features,global_volume,intents,parent_topic,parent_volume
country: us
```

**IMPORTANT:** Search intent classification MUST come from the Ahrefs `intents` field only.

### 2B. Related and matching keywords
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

Extract: keywords to include on the new page (secondary keywords, long-tail variations, question-based queries for FAQ).

### 2C. Search suggestions
```
Tool: keywords-explorer-search-suggestions
keyword: [TARGET KEYWORD]
country: us
```

Extract: autocomplete suggestions that reveal what searchers actually type — use these for FAQ questions and content sections.

---

## STEP 3 — SERP ANALYSIS

### 3A. Get top 10 SERP from Ahrefs
```
Tool: serp-overview
select: url,title,position,traffic,domain_rating
keyword: [TARGET KEYWORD]
country: us
top_positions: [--top value or 10]
```

**Fallback:** If Ahrefs returns an error, use WebSearch and note it in the report.

### 3B. Fetch and analyze the top 3 ranking pages
For the top 3 pages only (not all 10 — this is a content brief, not a full competitor analysis), use WebFetch and extract:

- Title tag, meta description
- H1, all H2s, all H3s (full heading tree)
- Word count estimate
- Key topics and subtopics covered
- FAQ questions (if present)
- Stats or data cited (with sources)
- CTAs used
- Schema markup present
- [PRODUCT NAME] mentioned? Yes/No

### 3C. Summarize top 10 landscape
For positions 4-10, summarize from SERP data only (no need to fetch each page):
- Page type distribution (product pages vs blogs vs guides vs listicles)
- Average word count of top 3
- Common topics across all top 10
- What content format Google rewards for this keyword

---

## STEP 4 — FETCH SITEMAP FOR INTERNAL LINKING

**Mandatory step — do NOT skip.**

### 4A. Fetch the sitemap
```
Tool: WebFetch
URL: [SITE ROOT]/sitemap.xml
```

If sitemap index, fetch relevant sub-sitemaps.

### 4B. Identify relevant pages for internal linking
From the sitemap, find pages that:
- Cover the same topic area
- Are in the same site section (e.g., /ai/, /itsm/, /features/)
- Cover complementary or parent topics
- Are high-traffic pages (cross-reference with GSC)

### 4C. Pull GSC traffic data for linking candidates
```
Tool: get_advanced_search_analytics
site_url: [GSC PROPERTY]
filter_dimension: page
filter_operator: contains
filter_expression: [URL PATH FILTER]
row_limit: 100
sort_by: clicks
sort_direction: descending
dimension: page
```

---

## STEP 5 — GENERATE THE NEW PAGE BRIEF

Output the full report in this structure:

---

# New Page Brief — "[TARGET KEYWORD]"

**Generated:** [today's date]
**Product:** [PRODUCT NAME]
**GSC Property:** [GSC PROPERTY]
**Ahrefs Target:** [AHREFS TARGET]
**Output format:** [Brief / Draft]
**Custom instructions:** [Yes — quoted / No]

---

## 1. Keyword & Opportunity Assessment

| Metric | Value |
|--------|-------|
| **Target keyword** | [keyword] |
| **Search intent (Ahrefs)** | [from `intents` field ONLY] |
| **Monthly search volume** | [from Ahrefs] |
| **Keyword difficulty** | [X/100] |
| **Traffic potential** | [from Ahrefs] |
| **Parent topic** | [topic — volume] |
| **SERP features present** | [list] |
| **CPC** | [$X.XX] |
| **Existing page?** | No — confirmed via GSC + Ahrefs |

**Opportunity assessment:**
[1-2 sentences: Is this keyword worth creating a page for? Based on volume, difficulty, traffic potential, and competitive landscape. Be honest — if KD is 90+ and DR is low, flag that this will be a long-term play.]

---

## 2. Secondary Keywords to Target

| Keyword | Volume | KD | Intent | How to use |
|---------|--------|-----|--------|-----------|
| [from related/matching terms] | | | [from Ahrefs] | [Target in H2 / FAQ / Body copy / Meta] |

---

## 3. SERP Landscape Summary

| Position | Domain | Page type | Title | Traffic | DR |
|----------|--------|-----------|-------|---------|-----|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| ... | | | | | |

**What Google rewards for this keyword:**
[Content format, depth, structure that ranks. e.g., "Top 3 are all product feature pages with 2,000+ words, FAQ schema, and specific use case sections."]

---

## 4. Top 3 Competitor Deep Dive

### #[Position]: [Title] — [Domain]

**URL:** [full URL]

#### Heading structure
```
H1: [text]
  H2: [text]
    H3: [text]
    H3: [text]
  H2: [text]
  ...
```

#### Key topics covered
- [topic 1]
- [topic 2]

#### Stats cited
| Stat | Source |
|------|--------|
| | |

#### FAQ questions (if present)
- [Q1]
- [Q2]

#### Key takeaway
[What makes this page rank and what the new page should learn from it]

*(Repeat for top 3)*

---

## 5. Recommended Page Structure

### Page metadata

| Element | Recommendation |
|---------|---------------|
| **URL slug** | [SITE ROOT]/[suggested-slug].html |
| **Title tag** | [50-65 characters — include target keyword naturally] |
| **Meta description** | [160-165 characters — include target keyword, value proposition, CTA] |
| **H1** | [clear, descriptive, includes target keyword or close variation] |
| **Target word count** | [X words — based on top 3 competitor average + 10-20% more depth] |
| **Schema markup** | [WebPage, FAQ, HowTo, Product — whichever is appropriate for intent] |

### Full heading structure

```
H1: [text]

H2: [section 1 — addresses primary search intent]
  H3: [subsection]
  H3: [subsection]

H2: [section 2 — covers key subtopic from competitor analysis]
  H3: [subsection]
  H3: [subsection]

H2: [section 3 — covers gap found in competitor pages]

H2: [section 4 — addresses specific use cases or scenarios]
  H3: [use case 1]
  H3: [use case 2]

H2: [section 5 — trust signals / results / proof]

H2: Frequently Asked Questions
  - [Q1 — from search suggestions + competitor FAQs]
  - [Q2]
  - [Q3]
  - [Q4]
  - [Q5]

H2: [CTA section]
```

---

## 6. Content Brief Per Section

For each H2 section, provide:

### H2: [Section title]

| Element | Details |
|---------|---------|
| **Topics to cover** | [bulleted list of specific points] |
| **Secondary keywords to include** | [from Step 2] |
| **Target word count** | ~[X] words |
| **Tone** | [technical / educational / conversational — based on SERP analysis] |
| **Key points from competitors** | [what the top 3 cover in their equivalent section] |
| **What to add beyond competitors** | [unique angle, additional depth, gap to fill] |
| **Stats to include** | [if applicable — flag for verification] |

*(Repeat for every H2 section)*

---

## 7. FAQ Section — Full Questions and Answer Guidance

| # | Question | Answer guidance | Source |
|---|----------|----------------|--------|
| 1 | [question] | [2-3 sentence summary of what the answer should cover] | [From search suggestions / competitor FAQ / People Also Ask] |

**Schema:** Apply FAQ structured data (JSON-LD) to all questions.

---

## 8. Internal Linking Plan

**Source: sitemap.xml analysis + GSC traffic data**

### Pages that should link TO this new page (inbound)

| # | From page (URL) | Anchor text | Link title attribute | Link type | Placement | Why |
|---|-----------------|-------------|---------------------|-----------|-----------|-----|
| 1 | [URL from sitemap] | [natural anchor] | [title attribute] | Contextual / Navigational / CTA / Sidebar | [which section] | [why relevant] |

*(Only contextually relevant links. No minimum or maximum.)*

### Pages this new page should link TO (outbound)

| # | To page (URL) | Anchor text | Link title attribute | Link type | Placement | Why |
|---|---------------|-------------|---------------------|-----------|-----------|-----|
| 1 | [URL from sitemap] | [natural anchor] | [title attribute] | Contextual / Navigational / CTA / Sidebar | [which H2 section] | [why relevant] |

*(Only contextually relevant links. No minimum or maximum.)*

---

## 9. Claim Verification Checklist

| Claim | Source | Verified? | Action needed |
|-------|--------|-----------|---------------|
| [any stat or differentiator recommended for the page] | [source] | Yes / No / Needs confirmation | [action] |

**Rules:**
- Claims from CLAUDE.md → "From brand brief — verify with product team"
- Claims from product pages → "Verified on [URL]"
- Claims from competitors/industry → cite source URL
- Apply product-specific restrictions (e.g., no vendor comparison tables for SDP)

---

## 10. Pre-Publication Checklist

Before the page goes live, confirm:

- [ ] Title tag is 50-65 characters and includes target keyword
- [ ] Meta description is 160-165 characters
- [ ] H1 includes the target keyword or close variation
- [ ] All H2/H3 sections are written and match the brief structure
- [ ] FAQ schema (JSON-LD) is implemented
- [ ] Internal links (both inbound and outbound) are added
- [ ] All stats and claims have been verified (see Section 9)
- [ ] Images have descriptive alt text (keyword-relevant where natural)
- [ ] Page is accessible in the sitemap.xml
- [ ] Mobile responsive
- [ ] Page speed acceptable (Core Web Vitals)
- [ ] No product-specific content restrictions violated

---

*Generated by /seo-new-page — ManageEngine / Zoho SEO Team*
*Product: [PRODUCT NAME] | Keyword: [TARGET KEYWORD] | Date: [today's date]*

---

## POST-REPORT: INTERACTIVE MODE

After outputting the full report, inform the user:

> **Your new page brief is ready.** You can now:
> - "Generate the full content draft for this page" (if format was `brief`)
> - "Expand the FAQ section with full answers"
> - "Add more sections about [topic]"
> - "Change the tone to [more technical / more conversational]"
> - "Show me what the meta title and description variations look like"
> - "Run /seo-competitor-analysis for deeper competitive insight"
> - "Create a brief for a different keyword"
>
> Just type your request.

---

## IF `--format=draft` WAS SPECIFIED

Instead of outputting just the brief (Sections 5-6), generate the **full content draft** for the page:

- Write every section in full, following the brief structure
- Use the tone identified from SERP analysis
- Include secondary keywords naturally (no keyword stuffing)
- Write FAQ answers in full (2-4 sentences each)
- Insert internal link placeholders: `[LINK: anchor text → destination URL]`
- Include schema markup suggestions as JSON-LD code blocks
- Flag any claims that need verification with `[VERIFY: claim — source]`
- Target the word count specified in the brief

**Important:** The draft is a starting point for human review, not a final version. Flag this clearly:
> **Note:** This is an AI-generated first draft. Before publishing:
> - Verify all claims marked with [VERIFY]
> - Review tone and brand voice alignment
> - Add real screenshots, diagrams, or product images
> - Have a subject matter expert review technical accuracy
> - Test all internal links
