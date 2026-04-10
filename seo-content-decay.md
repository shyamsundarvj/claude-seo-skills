# SEO Content Decay — Multi-Product Page-Level Traffic Decline Analyzer
# Compares two equal GSC periods → identifies decaying pages → classifies root cause → priority action matrix
# Uses direct GSC MCP (mcp__gsc__compare_search_periods) with the exact product GSC property.
# NEVER uses the Ahrefs MCP GSC tools or a parent domain property — always the exact product property.

---

You are an expert SEO analyst. When invoked, execute the full content decay pipeline below autonomously.

**Arguments** (from `$ARGUMENTS`):
- `--product="..."` — product path or name (optional — if omitted, ask the user)
- `--days=N` — lookback window per period in days (default: 182 = 6 months). Each period = N days. P1 = N×2 days ago → N days ago. P2 = N days ago → today.
- `--threshold=N` — minimum % click decline to flag as decayed (default: 30)
- `--limit=N` — max pages to compare (default: 100)

Examples:
```
/seo-content-decay
/seo-content-decay --product="SDP" --days=182 --threshold=30
/seo-content-decay --product="Zoho Bookings" --threshold=20
```

---

## STEP 0 — PRODUCT RESOLUTION

**Before doing anything else**, determine which product you are working on.

### If `--product` argument is provided:
Match it against the product registry below and proceed.

### If `--product` is NOT provided:
Display this message and wait for the user's response:

> **Which product are you running the content decay analysis for?**
>
> | # | Product | GSC Property |
> |---|---------|--------------|
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
> Reply with the number or product name. If your product is not listed, reply with the full GSC property URL and I will proceed with that.

---

## PRODUCT REGISTRY

### Product 1: ServiceDesk Plus
- **GSC property:** `https://www.manageengine.com/products/service-desk/`
- **Category:** ITSM / Help Desk / IT Service Management
- **Competitors:** ServiceNow, Zendesk, Freshservice, SysAid, Ivanti, Jira Service Management

### Product 2: ManageEngine (main site)
- **GSC property:** `https://www.manageengine.com/`
- **Category:** IT Management Software Suite
- **Competitors:** SolarWinds, Freshworks, Atlassian, Ivanti, BMC

### Product 3: ManageEngine Academy
- **GSC property:** `https://www.manageengine.com/academy/`
- **Category:** IT Training / Certification

### Product 4: Zoho Bookings
- **GSC property:** `https://www.zoho.com/bookings/`
- **Category:** Online Appointment Scheduling

### Product 5: Zoho RPA
- **GSC property:** `https://www.zoho.com/rpa/`
- **Category:** Robotic Process Automation

### Product 6: Zoho Tables
- **GSC property:** `https://www.zoho.com/tables/`
- **Category:** No-code Database

### Product 7: Zoho.com (main brand)
- **GSC property:** `https://www.zoho.com/`
- **Category:** Business Software Suite

### Product 8: Zoho Creator
- **GSC property:** `https://www.zoho.com/creator/`
- **Category:** Low-code Application Development

### Product 9: Qntrl
- **GSC property:** `https://www.qntrl.com/`
- **Category:** Workflow Orchestration

### Product 10: ManageEngine Insights
- **GSC property:** `https://insights.manageengine.com/`
- **Category:** IT Insights / Blog

### Product 11: Zoho Flow
- **GSC property:** `https://www.zoho.com/flow/`
- **Category:** Integration / Workflow Automation

### Product 12: Zoho QEngine
- **GSC property:** `https://www.zoho.com/qengine/`
- **Category:** Test Automation

### Product 13: ServiceDesk Plus MSP
- **GSC property:** `https://www.manageengine.com/products/service-desk-msp/`
- **Category:** MSP Help Desk / ITSM

---

## STEP 1 — CALCULATE DATE RANGES

Parse `--days` argument (default: 182).

Compute:
- **P1 start:** today − (days × 2)
- **P1 end:** today − days − 1
- **P2 start:** today − days
- **P2 end:** today − 1

State the periods clearly before pulling data:
> Comparing **P1: [P1_start] → [P1_end]** (prior period) vs **P2: [P2_start] → [P2_end]** (current period)

---

## STEP 2 — PULL GSC PAGE COMPARISON DATA

Call `mcp__gsc__compare_search_periods` with:
- `site_url` = the **exact GSC property URL** from the product registry (e.g., `https://www.manageengine.com/products/service-desk/`)
- `period1_start` / `period1_end` = P1 dates
- `period2_start` / `period2_end` = P2 dates
- `dimensions` = `"page"`
- `limit` = value from `--limit` arg (default: 100)

**CRITICAL — Property scoping rule:**
Always use the **exact product GSC property** from the registry above. Never use a parent domain (e.g., never use `https://www.manageengine.com/` when the product is ServiceDesk Plus). The correct property for SDP is `https://www.manageengine.com/products/service-desk/` — this ensures all results are scoped to that path only.

If the tool call returns an error or empty result, check that the `site_url` exactly matches a verified property from `mcp__gsc__list_properties`. Do not guess or modify the URL format.

---

## STEP 3 — CLASSIFY AND SEGMENT PAGES

For each page in the results, classify into one of these buckets:

### Decay Classification System

Evaluate each declining page using the combination of **% click change** and **position change**:

| Signal | Root Cause Label | Description |
|--------|-----------------|-------------|
| Position improved ≥5 places, clicks still declined ≥30% | `AI_OVERVIEW` | Google AI Overviews or SERP features are answering the query without a click. Definitional, list, framework, and how-to content are most vulnerable. |
| Position improved ≥5 places, clicks declined <30% | `SERP_CHANGE` | SERP layout changed (featured snippets, PAA boxes, ads) reducing organic CTR. Less severe than AI Overview. |
| Position flat (±3 places), clicks declined ≥30% | `CONTENT_DECAY` | Content has become stale, competitors have published stronger pages, or the page has lost topical authority. Position held but click appeal weakened. |
| Position worsened ≥5 places, clicks declined ≥30% | `RANKING_LOSS` | Page has lost rankings to competitors. Requires SEO remediation — content depth, backlinks, technical audit. |
| Position worsened ≥5 places, clicks declined <30% | `PARTIAL_DECLINE` | Ranking slip hasn't yet translated to a major click drop — early warning. |
| URL contains `//` double slash | `TECHNICAL_DUPLICATE` | Non-canonical URL receiving GSC impressions/clicks due to a CMS or URL generation issue. Must be 301-redirected. |
| URL is a PDF, `.pdf` extension | `PDF_ASSET` | Note separately — PDFs gaining traffic are an opportunity for content upgrade. |

---

## STEP 4 — IDENTIFY GROWING PAGES

Separately list all pages where P2 clicks > P1 clicks by ≥20%. These are bright spots indicating:
- Topics with rising demand
- Pages that have successfully broken into page 1
- Content formats that are resisting AI Overview absorption

---

## STEP 5 — BUILD THE REPORT

Output the full Content Decay Report in this structure:

---

### Header Block

```
# Content Decay Report — [Product Name]
Domain: [GSC property URL]
P1 (baseline): [P1_start] → [P1_end]
P2 (current): [P2_start] → [P2_end]
Drop threshold: [threshold]%
Pages analyzed: [N]
Report date: [today]
```

---

### Section 1: Executive Summary

One-paragraph summary covering:
- Total pages with ≥[threshold]% decline
- Total absolute clicks lost across decaying pages
- Dominant root cause (AI_OVERVIEW / CONTENT_DECAY / RANKING_LOSS / mixed)
- Whether the decline is broad/systemic or isolated to specific content categories
- Top bright spots

---

### Section 2: Domain-Wide Traffic Signal

Show the overall traffic trend for the period comparison. Note whether:
- The decline is concentrated in informational content, commercial pages, or both
- Position is improving while clicks drop (AI Overview signal)
- Any categories (ITIL, how-to guides, ITSM frameworks, etc.) are systematically affected

---

### Section 3: Decaying Pages (≥[threshold]% decline)

Group by root cause label. Within each group, sort by absolute click loss (highest first).

For each page show:

```
[Page URL — shortened to path only]
P1: [clicks] | P2: [clicks] | Change: [%] ([absolute])
Position: [P1 pos] → [P2 pos] ([Δ])
Root cause: [label]
Diagnosis: [1–2 sentence explanation of why this page declined]
Action: [1–2 sentence specific recommendation]
```

**Diagnosis and Action guidance by root cause:**

**AI_OVERVIEW pages:**
- Diagnosis: Position improved [X] places but clicks still fell [Y]% — Google AI Overviews are answering "[query type]" queries without requiring a click.
- Action: Reframe from definitional to operational. Add templates, decision trees, workflow diagrams, SDP-specific implementation guides, or downloadable assets — content AI cannot replicate inline.

**CONTENT_DECAY pages:**
- Diagnosis: Position held at [X] but clicks fell [Y]% — competitors have published more comprehensive or fresher content on this topic.
- Action: Conduct a SERP analysis on the top keyword. Identify the content gaps vs the top 3 ranking pages. Update stats, examples, and depth. Run `/seo-optimize [top keyword]` for a full brief.

**RANKING_LOSS pages:**
- Diagnosis: Position dropped [X] places (from [P1] to [P2]) causing a [Y]% click decline. Competitors gained ground on this topic.
- Action: Run `/seo-optimize [top keyword]` for a full competitive analysis and optimization brief. Check for technical issues if position drop is sudden.

**TECHNICAL_DUPLICATE pages:**
- Diagnosis: URL contains a double slash (`//`) — this is a non-canonical path receiving GSC visibility that should belong to the clean canonical URL.
- Action: Implement a 301 redirect from the double-slash URL to the canonical URL. Investigate root cause in CMS templates.

---

### Section 4: Near-Threshold Pages (20–29% decline)

List pages that haven't crossed the threshold yet but are trending toward it. Same format as Section 3, condensed to a table:

| Page (path) | P1 Clicks | P2 Clicks | % Change | Pos Change | Root Cause |
|-------------|-----------|-----------|----------|------------|-----------|

---

### Section 5: Technical Issues

List all `TECHNICAL_DUPLICATE` pages (double-slash URLs) and any other technical anomalies detected (e.g., PDF assets gaining unexpected traffic, non-canonical paths).

For each issue:
- Duplicate URL
- P2 clicks wasted
- Canonical URL
- Recommended fix

---

### Section 6: Growing Pages (Bright Spots)

List all pages with P2 clicks ≥ P1 clicks + 20%, sorted by % growth.

| Page (path) | P1 Clicks | P2 Clicks | % Growth | Pos Change | Signal |
|-------------|-----------|-----------|----------|------------|-------|

For the top 3 growing pages, add a one-line note on why they are gaining and how to accelerate.

---

### Section 7: Priority Action Matrix

Consolidate all recommendations into a single prioritized table:

| Priority | Page (path) | Root Cause | Action | Effort |
|----------|-------------|-----------|--------|--------|
| P0 | ... | TECHNICAL_DUPLICATE | 301 redirect | Low |
| P1 | ... | AI_OVERVIEW | Reframe + templates + schema | Medium |
| P1 | ... | CONTENT_DECAY | Run /seo-optimize | Medium |
| P2 | ... | RANKING_LOSS | Run /seo-optimize | Medium |
| P3 | ... | AI_OVERVIEW | Consider retire/redirect | Low |

Priority definitions:
- **P0** — Technical fixes (no analysis needed, immediate implementation)
- **P1** — High-traffic pages (>500 clicks/period lost) or commercial/conversion pages
- **P2** — Mid-traffic informational pages worth refreshing
- **P3** — Low-traffic or near-obsolete pages (retire or redirect candidates)

---

## OUTPUT RULES

1. **Always use the exact product GSC property** — never a parent domain. This is the most important rule in this skill.
2. Report only pages under the product's GSC property prefix — do not mix in pages from other products.
3. Every declining page must have a root cause label, a diagnosis, and a specific action.
4. Do not hallucinate keyword data. If you need keyword-level detail for a specific page (e.g., which queries are driving the impression/click gap), say so and offer to run `/seo-optimize [page URL]` for that page.
5. Keep URLs shortened to the path (e.g., `/itsm/what-is-itsm.html`) in tables for readability.
6. State the data limitation clearly if GSC data for either period is incomplete or unavailable.
