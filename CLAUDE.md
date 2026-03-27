# SEO Project — Claude Instructions

## Project: ServiceDesk Plus (ManageEngine) SEO Strategy

This workspace contains SEO research and automation for **ServiceDesk Plus by ManageEngine** (Zoho Corp).

---

## Reddit Access — Current Status

| Method | Status | Notes |
|--------|--------|-------|
| Reddit OAuth API | Pending approval | Applied Nov 2025; 7-day review |
| Reddit public JSON (.json endpoints) | Blocked (HTTP 403) | Reddit blocks Python HTTP library |
| Claude WebFetch on Reddit | Blocked | Claude cannot fetch reddit.com |
| **Google Search for Reddit threads** | **Working** | Current method used by the skill |

**Current approach:** The `/seo-reddit-monitor` skill uses Google Search to find Reddit threads.
This is reliable and requires no credentials, though it depends on Google's index rather than Reddit's live hot feed.

**When Reddit API is approved:** Update `reddit_monitor.py` with credentials and run it directly to get real-time hot-sorted data.

---

## Key Files

| File | Purpose |
|------|---------|
| `ServiceDesk_Plus_AI_Keywords_SEO_Strategy_2026.md` | Full AI keyword gap analysis and content roadmap |
| `reddit_monitor.py` | Python script that fetches Reddit threads via API (called by `/seo-reddit-monitor` skill) |
| `~/.claude/commands/seo-reddit-monitor.md` | The `/seo-reddit-monitor` skill definition |

---

## Brand Context (for all AI tasks)

- **Product:** ServiceDesk Plus
- **Company:** ManageEngine (a division of Zoho Corp)
- **Category:** ITSM / Help Desk / Service Desk software
- **Primary competitors:** ServiceNow, Zendesk, Freshservice, SysAid, Ivanti, Jira Service Management
- **Key angle:** Affordable, AI-native ITSM with agentic automation — no per-agent AI fees

---

## Running the Reddit Monitor

```bash
# Full scan (last 7 days, hot sort, all subreddits)
/seo-reddit-monitor

# Extended window
/seo-reddit-monitor --days=14

# Focus on competitor threads
/seo-reddit-monitor --focus=competitors

# Focus on AI ITSM threads
/seo-reddit-monitor --focus=ai

# More threads per subreddit
/seo-reddit-monitor --limit=10
```

The skill automatically:
1. Runs `reddit_monitor.py` to fetch hot threads from target subreddits via Reddit API
2. Searches Reddit for ITSM-related queries (sorted by hot)
3. Deduplicates and filters results
4. Scores each thread (1–10 relevancy)
5. Generates engagement strategy + comment drafts for threads scoring 6+
6. Outputs a full structured report

---

## SEO Skill Rules (Team-Wide)

These rules apply to all SEO skills: `/seo-new-page`, `/seo-optimize`, `/seo-internal-links`, `/seo-competitor-analysis`, `/seo-tags`.

---

### 1. Internal Linking Quality Standards

**Applies to:** `/seo-new-page`, `/seo-optimize`, `/seo-internal-links`

#### Sitemap-first approach
- **Always fetch and parse the product's sitemap.xml first** before suggesting any internal links
- Analyze all pages listed in the sitemap to understand the full site structure
- Sitemap is the source of truth for what pages exist on the site — never rely solely on GSC data or memory

#### Anchor text from real page content
- **Use real on-page text as anchor text.** Never generate anchor text from breadcrumbs, page titles, or assumptions. Fetch each source/destination page and find the actual text on the page.
- **Never fabricate anchor text.** If a page can't be fetched, note it and skip rather than guess.

#### Inbound links (pages linking TO the target page)
- Fetch each source page
- Find the specific paragraph/sentence where the link fits contextually
- Quote the exact sentence from the page
- Suggest which words within that sentence become the anchor text
- Include a "Link title" attribute suggestion for each link

#### Outbound links (target page linking TO existing pages)
- Fetch each destination page to understand its actual content/H1/topic
- Suggest natural phrasing informed by what the destination page actually covers
- Include a "Link title" attribute suggestion for each link

#### Link placement rules
- **One link per unique page.** Each inbound link suggestion must come from a different source page. Do not suggest multiple links from the same page — pick the single best insertion point.
- **Never place links in any heading tag (H1, H2, H3, H4, H5, H6).** Headings should not carry link juice. Only suggest anchor text within body paragraph text, never from any heading or subheading.

#### Required fields for every link suggestion
Every internal link recommendation must include ALL of these fields:
- **From page** (source URL)
- **To page** (destination URL)
- **Anchor text** (the clickable text — from real on-page content)
- **Link title** (the title attribute for the `<a>` tag)
- **Link type** (contextual / navigational / footer / sidebar / CTA)
- **Where to place** (which section of the source page)

---

### 2. No Vendor Comparison Tables

**Applies to:** All SEO skills — **ServiceDesk Plus only** (does not apply to other products like Zoho Bookings, Zoho RPA, ManageEngine Academy, etc.)

ServiceDesk Plus has a company policy restriction that prevents adding comparison tables between SDP and other vendors (ServiceNow, Zendesk, SysAid, etc.) on their website pages.

- Do NOT suggest adding competitor comparison tables
- Instead, recommend differentiator-led content that highlights SDP's unique strengths (e.g., "no per-agent AI fees", "on-prem + cloud") without directly naming or comparing to competitors
- Alternative approaches: feature highlight sections, benefit-led content, use case breakdowns, ROI statistics

---

### 3. SERP Analysis & Competitor Deep Dive

**Applies to:** `/seo-new-page`, `/seo-optimize`, `/seo-competitor-analysis`

#### Unified top 10 format
- Analyze ALL top 10 SERP results in **one unified table** — do NOT split into separate "Competitor Pages" and "Review/Listicle Sites" categories
- What matters is who ranks in the top 10 and what they're doing, not what category they fall into
- Still flag whether SDP/ManageEngine is mentioned on each page (visibility gap check), but as part of the unified per-result analysis

#### Fetch and analyze all top 10 — not just top 3
- **Fetch and deep-dive ALL top 10 ranking pages**, not just the top 3. Every position in the top 10 may have content strategies worth analyzing.
- For each page, extract: title tag, meta description, H1, full heading tree (H2s/H3s), approximate word count, key topics, FAQ questions, stats cited, CTAs, schema markup, and whether the product is mentioned.

#### Direct competitor analysis beyond the top 10
- After analyzing the top 10 SERP, **separately check whether each direct competitor has a page targeting the same keyword** — even if they don't rank in the top 10.
- Use Ahrefs site-explorer or WebSearch to find competitor pages for the target keyword.
- If a direct competitor has a relevant page but doesn't rank in the top 10, fetch and analyze it too — note its current position and why it may not be ranking well.
- The product's primary competitors are defined in the "Brand Context" section of this file (or in the skill's product registry). Always check all of them.

---

### 4. Data Source Rules

**Applies to:** All SEO skills

1. **Search intent must come from Ahrefs Keyword Explorer only** — specifically the `intents` field (informational / navigational / commercial / transactional). Never infer or guess search intent from WebSearch results, GSC data, or general knowledge. Ahrefs is the single source of truth for intent classification.

2. **GSC lookback window defaults to 90 days** — not 28 days. The `--days` argument can override this, but the default is always 90.

3. **Ahrefs SERP overview (`serp-overview`) is the primary source** for top 10 ranking data (positions, traffic, domain rating). WebSearch is a secondary fallback only — if used, clearly note it as approximate data in the report.

---

### 5. Key Takeaways Section Restriction

**Applies to:** `/seo-new-page`, `/seo-optimize`

"Key takeaways" or "Summary" sections should ONLY be suggested for educational/informational content types:
- TechArticle (guides, explainers, how-tos)
- Article (blog posts, thought leadership)

Do NOT suggest for:
- Product feature pages
- Solution pages (WebPage - Solution)
- Webinar pages
- SoftwareApplication / landing pages
- Any commercial or transactional page type

---

### 7. Document Export Accuracy Rule

**Applies to:** All SEO skills that export `.docx` files (`/seo-tags`, `/seo-new-page`, `/seo-optimize`, etc.)

When generating a Word document (`.docx`) from a chat-based SEO report:

- **Every value in the exported document must exactly match what was output in the chat.** This includes section labels, alt text, titles, filenames, URLs, meta descriptions, SEO titles, breadcrumbs, schema markup, and internal link suggestions — no rephrasing, no paraphrasing, no substitution.
- **Never generate placeholder or inferred values** for fields that were explicitly stated in the chat output. Copy verbatim.
- **Image Tags count must match exactly.** If the chat output contains 1 image tag, the document must contain exactly 1. If it contains 5, the document must contain exactly 5. Never add or remove image entries.
- **Internal link counts must match exactly.** If the chat output lists 18 "From this page" links and 4 "From other pages" links, the document must contain all 18 and all 4 — not a subset.
- **If context is compressed** and the original chat values are no longer accessible, ask the user to paste the specific section rather than inventing or approximating values.

---

### 8. Preview Hook — Document Generation Tasks

The Claude Code preview system fires a "Preview Required" stop hook whenever files are edited, even for non-web tasks like `.docx` generation. This is expected and can be ignored for:
- Word document (`.docx`) generation
- Markdown/text file exports
- Any task that does not involve a web app, HTML, or dev server

This hook cannot be suppressed via config as it is part of the built-in Claude Code preview tools integration. The document is correctly saved regardless of the hook message.

---

### 6. SDP AI Page Structure Context

**Applies to:** All SEO skills targeting ServiceDesk Plus

- `/ai/` (hub page) is the better primary page for broad "AI ITSM" keywords. It serves as the top-level AI hub for ServiceDesk Plus with broader topical coverage.
- `/ai/ai-in-itsm-servicedesk-plus-features.html` focuses narrowly on hidden/embedded AI features within SDP. It is NOT a good primary page for the broad "AI ITSM" keyword.
- When recommending primary pages for AI-related keywords, prefer the hub page over feature-specific pages unless the keyword is specifically about a feature (e.g., "itsm virtual agent" → `/ai/virtual-agent.html`).
