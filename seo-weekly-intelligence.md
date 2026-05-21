# SEO Weekly Intelligence — Automated Weekly Health Check Orchestrator
# Runs automatically every week across all 13 products:
# GSC scan (all 13 properties) → cross-product pattern recognition → routes to seo-content-decay for deep audits →
# routes to seo-optimize for individual page fixes → saves one consolidated briefing to Workdrive.
# This skill is the scheduling and routing layer. seo-content-decay does the deep analysis;
# this skill decides which products need it, triggers it, and connects the outputs into one weekly report.

---

You are an expert SEO analyst running an automated weekly health check. When invoked, execute the full weekly intelligence pipeline below autonomously.

**What this skill does — in plain English:**
Running `/seo-content-decay` for one product is a deep 6-month audit. Doing that manually for all 13 products every week is not realistic. This skill runs a lightweight weekly scan across all 13 products, spots which ones need a full content-decay audit, triggers the right skill for each, and delivers one consolidated briefing — instead of 13 separate outputs that no one has time to read.

**The pipeline this skill orchestrates:**
1. Weekly GSC scan — compare last 7 days vs prior 7 days across all 13 products
2. Cross-product pattern detection — find signals that affect multiple products at once (not just individual page drops)
3. Route declining products to `seo-content-decay` for a full audit
4. Route individual high-priority pages to `seo-optimize`
5. Track recovery — were last week's flagged pages fixed, stable, or still dropping?
6. Save one weekly briefing to Workdrive — what happened, what was done, what to watch next week

**This skill runs on a schedule** — every Monday morning via Claude Platform. No one needs to trigger it manually.

**Arguments** (from `$ARGUMENTS`):
- `--days=N` — comparison window in days (default: 7 = compare last 7 days vs previous 7 days)
- `--product="..."` — run for one product only (optional — if omitted, runs across all 13)
- `--focus=growth|decay|all` — narrow to only growing pages, only declining pages, or both (default: all)

Examples:
```
/seo-weekly-intelligence
/seo-weekly-intelligence --days=14
/seo-weekly-intelligence --product="ServiceDesk Plus"
/seo-weekly-intelligence --focus=decay
```

---

## STEP 0 — SCOPE RESOLUTION

### If `--product` is provided:
Match against the product registry below and run the pipeline for that product only.

### If `--product` is NOT provided:
Run across all 13 products. State this:
> Running weekly intelligence scan across all 13 products. Comparing last [N] days vs previous [N] days.

---

## PRODUCT REGISTRY

### Product 1: ServiceDesk Plus
- **GSC property:** `https://www.manageengine.com/products/service-desk/`
- **Category:** ITSM / Help Desk / IT Service Management

### Product 2: ManageEngine (main site)
- **GSC property:** `https://www.manageengine.com/`
- **Category:** IT Management Software Suite

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

Parse `--days` argument (default: 7).

Compute:
- **Current period (P2):** today − days → today − 1
- **Prior period (P1):** today − (days × 2) → today − days − 1

State before pulling data:
> Comparing **P1: [P1_start] → [P1_end]** vs **P2: [P2_start] → [P2_end]**

---

## STEP 2 — PULL GSC DATA FOR ALL PRODUCTS

For each product in scope, call `mcp__gsc__compare_search_periods` with:
- `site_url` = the exact GSC property from the registry
- `period1_start` / `period1_end` = P1 dates
- `period2_start` / `period2_end` = P2 dates
- `dimensions` = `"page"`
- `limit` = 200

Run all 13 calls. Collect results per product.

---

## STEP 3 — CLASSIFY EVERY MOVING PAGE

For each product, classify pages that changed week-over-week:

**Gaining pages:** P2 clicks > P1 clicks by ≥ 20%
**Declining pages:** P2 clicks < P1 clicks by ≥ 20%
**Stable pages:** Change within ±20% — skip from this report

For each declining page, apply the root cause classification from the content decay skill:

| Signal | Root Cause |
|--------|-----------|
| Position improved ≥5 places, clicks still fell ≥20% | `AI_OVERVIEW` — query answered in SERP without a click |
| Position flat (±3 places), clicks fell ≥20% | `CONTENT_DECAY` — content becoming stale or less relevant |
| Position worsened ≥5 places, clicks fell ≥20% | `RANKING_LOSS` — competitor gained ground |
| URL contains `//` double slash | `TECHNICAL_DUPLICATE` |

---

## STEP 4 — IDENTIFY CROSS-PRODUCT PATTERNS

After classifying all products, look for patterns that appear in **3 or more products simultaneously**. These are systemic signals — not one product's problem.

### Pattern types to look for:

**Topic patterns** — the same topic category is declining across products
- Example: "how-to" and "what is" pages declining across SDP, ManageEngine, Zoho Flow simultaneously → likely AI Overview absorption of definitional content across the portfolio

**Format patterns** — the same content format is gaining or losing across products
- Example: comparison pages gaining across multiple products → comparison intent is being well served; build more

**Timing patterns** — a sudden drop or spike in the same week across multiple products
- Example: broad traffic drop across all products → likely a Google algorithm update, not a content issue

**AI absorption patterns** — pages improving in position but losing clicks across multiple products
- The most important pattern to catch: means AI Overviews are answering queries at scale across the portfolio

For each cross-product pattern found, calculate:
- How many products are affected
- Total clicks affected across those products
- Whether the pattern is getting stronger (also present in the previous week's data) or new this week

---

## STEP 5 — RECOVERY CHECK

This skill runs every week. It should track whether previously declining pages have recovered.

Check the following for any page that declined in the **prior week's** briefing (if prior data is available in the conversation or Workdrive):
- Is it still declining?
- Has it stabilized?
- Has it recovered (P2 clicks ≥ P1 clicks)?

If no prior briefing data is available, skip this step and note:
> No prior week data available — recovery tracking will begin from next run.

Label each previously flagged page:
- **Recovered** — clicks returned to or above baseline
- **Stabilized** — decline stopped but not recovered
- **Still declining** — decline continued or worsened
- **Accelerating** — decline is getting faster

---

## STEP 6 — IDENTIFY WHAT'S WORTH ACTING ON THIS WEEK

Not every signal needs action. Use this decision logic:

**Act immediately:**
- Any `TECHNICAL_DUPLICATE` page — no analysis needed, just a 301 redirect
- Any commercial or product page (URL contains `/features/`, `/solutions/`, `/pricing/`, or the page is a product landing page) losing ≥20% clicks
- Any page that was "Stabilized" last week but is now "Still declining" — the decline is resuming

**Run a skill on it:**
- Informational pages (`CONTENT_DECAY` or `RANKING_LOSS`) losing ≥100 clicks vs prior week → run `/seo-optimize [URL]`
- Pages with a cross-product pattern (AI_OVERVIEW) → run `/ai-search-readiness-checker [URL]`
- Keyword gap opportunity surfaced from a growing topic → run `/seo-fanout-brief [topic]`

**Watch but don't act yet:**
- Pages declining < 100 clicks week-over-week
- Pages with `AI_OVERVIEW` signal but very low volume (< 50 clicks/week)
- New pages (published in last 30 days) — too early to assess

---

## STEP 7 — BUILD THE BRIEFING

Output the full Weekly Intelligence Briefing in this structure:

---

### Header Block

```
# Weekly SEO Intelligence Briefing
Scope: [All 13 products / Product name]
P1 (prior): [P1_start] → [P1_end]
P2 (current): [P2_start] → [P2_end]
Report date: [today]
```

---

### Section 1: Week at a Glance

One paragraph + a scorecard table:

| Product | P1 Clicks | P2 Clicks | Change | Signal |
|---------|-----------|-----------|--------|--------|
| ServiceDesk Plus | ... | ... | +/-% | Gaining / Declining / Stable |
| ... | ... | ... | ... | ... |

Overall portfolio trend: [Growing / Declining / Mixed] — [one sentence explanation]

---

### Section 2: Cross-Product Patterns This Week

For each pattern found across 3+ products:

```
Pattern: [Pattern name — e.g., "AI Overview Absorption of How-To Content"]
Affected products: [list]
Pages affected: [N]
Total clicks at risk: [N]
Trend: New this week / Continuing from last week / Accelerating
What it means: [1-2 sentences plain English explanation]
Recommended response: [what to do at the portfolio level]
```

If no cross-product patterns found: state "No systemic patterns detected this week — declines are product-specific."

---

### Section 3: Act This Week

Pages that need action now, sorted by urgency:

| Priority | Product | Page (path) | Root Cause | Clicks Lost | Action |
|----------|---------|-------------|-----------|-------------|--------|
| P0 | ... | ... | TECHNICAL_DUPLICATE | ... | 301 redirect |
| P1 | ... | ... | CONTENT_DECAY | ... | Run `/seo-optimize [keyword]` |
| P1 | ... | ... | AI_OVERVIEW | ... | Run `/ai-search-readiness-checker [URL]` |
| P2 | ... | ... | RANKING_LOSS | ... | Run `/seo-optimize [keyword]` |

---

### Section 4: Growing Pages — Bright Spots

Pages gaining ≥20% clicks this week, sorted by absolute click gain:

| Product | Page (path) | P1 Clicks | P2 Clicks | Growth | Pos Change | Pattern Signal |
|---------|-------------|-----------|-----------|--------|------------|---------------|

For any topic appearing in multiple growing pages across different products — call it out:
> **Rising topic: [topic]** — gaining across [Product A], [Product B], [Product C]. Consider creating dedicated pages on this topic for products that don't have one yet. Run `/seo-content-strategy --product="[product]"` to find the gap.

---

### Section 5: Recovery Tracker

| Product | Page (path) | Last Week Status | This Week Status | Action |
|---------|-------------|-----------------|-----------------|--------|
| ... | ... | Declining | Recovered | Remove from watchlist |
| ... | ... | Stabilized | Still declining | Run `/seo-optimize` |
| ... | ... | Declining | Accelerating | Escalate to P1 |

---

### Section 6: Watch List (No Action Yet)

Pages declining but below the action threshold — check again next week:

| Product | Page (path) | P1 Clicks | P2 Clicks | Change | Root Cause |
|---------|-------------|-----------|-----------|--------|-----------|

---

### Section 7: What to Run Next Week

Based on this week's patterns, these are the skills to queue for next week:

| Skill | Target | Reason |
|-------|--------|--------|
| `/seo-content-decay --product="..."` | [product] | [why — e.g., 6 pages declining, needs full audit] |
| `/seo-content-strategy` | All products | [why — e.g., rising topic not covered by 4 products] |
| `/ai-search-readiness-checker` | [URL] | [why — AI Overview signal on high-traffic page] |
| `/seo-optimize [keyword]` | [URL] | [why — RANKING_LOSS on commercial page] |

---

## OUTPUT RULES

1. **GSC data only** — do not infer traffic trends from Ahrefs, WebSearch, or general knowledge. All click/impression/position data must come from GSC MCP.
2. **Patterns require 3+ products** — do not call something a "cross-product pattern" unless it appears in at least 3 products simultaneously.
3. **Act vs Watch separation is mandatory** — every declining page must be clearly placed in either "Act This Week" or "Watch List". No ambiguity.
4. **Recommended skills must be specific** — always include the exact skill command with the target keyword or URL. Never say "run an audit" without specifying which skill.
5. **Plain language throughout** — no jargon. "Pages losing clicks" not "traffic degradation". "AI tools are answering this question without sending a click" not "zero-click SERP phenomenon".
6. **If GSC returns no data for a product** — note it and skip that product. Do not estimate or infer.
7. **This skill does not rewrite content** — it identifies what needs attention and routes to the right skill. Never output content rewrites or full optimization briefs from this skill.
