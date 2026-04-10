# claude-seo-skills

Internal SEO automation skills for Claude Desktop — ManageEngine SEO team only.

---

## What's Inside

| File | Skill Command | What it does |
|------|--------------|-------------|
| seo-tags.md | /seo-tags | Generates SEO meta tags |
| seo-reddit-monitor.md | /seo-reddit-monitor | Reddit ITSM monitor |
| seo-optimize.md | /seo-optimize | On-page SEO optimization |
| seo-new-page.md | /seo-new-page | Content brief generator |
| seo-internal-links.md | /seo-internal-links | Internal linking analyzer |
| seo-competitor-analysis.md | /seo-competitor-analysis | Competitor analysis |
| ai-search-readiness-checker.md | /ai-search-readiness-checker | AI search audit |
| CLAUDE.md | Claude project context |
| CLAUDE-strategist.md | Claude strategist instructions |

---

## Teammate Setup Guide

### STEP 1 — Install Claude Desktop
Download and install from: https://claude.ai/download
Sign in with your Anthropic account.

---

### STEP 2 — Clone & Install Skills via Claude code

1. Open **Claude Code** --> Navigate to Code 
2. Type this as an example

"Clone this GitHub repo and install all skill files into
~/.claude/commands/ folder and copy CLAUDE.md and
CLAUDE-strategist.md into a folder called SEO on my Mac:
https://github.com/shyamsundarvj/claude-seo-skills"

3. Claude will automatically:
   - Clone the repo to your Mac ✅
   - Install all 7 skill files into the right folder ✅
   - Copy CLAUDE.md files into your SEO folder ✅

No Terminal, no GitHub account, no app downloads needed.

---

### STEP 3 — Connect Ahrefs MCP

1. Open Claude Code
2. Go to **Settings → MCP Connectors**
3. Search for **"Ahrefs"**
4. Click **Connect Ahrefs**
5. Authenticate with your Ahrefs account
6. Done ✅

---

### STEP 4 — Connect GSC MCP

This requires a one-time manual setup.
Full setup guide here: https://github.com/AminForou/mcp-gsc

Else, Here's you can follow the complete step-by-step guide based on exactly how you can set it up:

---

## How to Connect Google Search Console MCP Server to Claude

**Source repo:** `github.com/AminForou/mcp-gsc` — This is the MCP server we're using.

**Authentication method used:** Service Account (not OAuth)

---

### Step 1 — Prerequisites

Install the following:
- **Python 3.11+** — [python.org/downloads](https://python.org/downloads)
- **Node.js** — [nodejs.org](https://nodejs.org)
- **Claude Desktop** — [claude.ai/download](https://claude.ai/download)

---

### Step 2 — Set Up Google Cloud & Service Account

1. Go to [Google Cloud Console](https://console.cloud.google.com/) and create/select a project
2. Enable the **Search Console API**:
   `APIs & Services → Library → Search "Search Console API" → Enable`
3. Create a Service Account:
   `APIs & Services → Credentials → Create Credentials → Service Account`
4. Fill in the service account name and click **Create**
5. Go to the newly created service account → **Keys** tab → **Add Key → Create new key → JSON**
6. Download the key file and rename it to `service_account_credentials.json`
7. In [Google Search Console](https://search.google.com/search-console/), go to **Settings → Users and permissions** for your property and add the service account email as a **Full** user

---

### Step 3 — Download the MCP Server
Open Terminal and run:

git clone https://github.com/AminForou/mcp-gsc.git /Users/[your-username]/[your-folder]/mcp-gsc

Example:

git clone https://github.com/AminForou/mcp-gsc.git /Users/john/Documents/mcp-gsc

Note down the path you used — you'll need it in Steps 4 and 5.
---

### Step 4 — Install Dependencies

**In Terminal, run:**

cd /Users/[your-username]/[your-folder]/mcp-gsc

python3 -m venv .venv

source .venv/bin/activate

pip install -r requirements.txt

**Example**:

cd /Users/john/Documents/mcp-gsc

python3 -m venv .venv

source .venv/bin/activate

pip install -r requirements.txt
The `requirements.txt` installs:
- `google-api-python-client`
- `google-auth`
- `google-auth-oauthlib`
- `oauth2client`
- `mcp>=1.6.0`

---

### Step 5 — Place Your Credentials File
Move or copy the service_account_credentials.json you downloaded in Step 2 into your mcp-gsc folder:

/Users/[your-username]/[your-folder]/mcp-gsc/service_account_credentials.json

Example:

/Users/john/Documents/mcp-gsc/service_account_credentials.json

### Step 6 — Configure Claude Desktop

Open the Claude Desktop config file:

```bash
# Mac
nano ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

Add this configuration, replacing [your-username] and [your-folder] with your actual values:

{
  "mcpServers": {
    "gscServer": {
      "command": "/Users/[your-username]/[your-folder]/mcp-gsc/.venv/bin/python",
      "args": ["/Users/[your-username]/[your-folder]/mcp-gsc/gsc_server.py"],
      "env": {
        "GSC_CREDENTIALS_PATH": "/Users/[your-username]/[your-folder]/mcp-gsc/service_account_credentials.json",
        "GSC_SKIP_OAUTH": "true",
        "GSC_DATA_STATE": "all"
      }
    }
  }
}

**Example:**

{
  "mcpServers": {
    "gscServer": {
      "command": "/Users/john/Documents/mcp-gsc/.venv/bin/python",
      "args": ["/Users/john/Documents/mcp-gsc/gsc_server.py"],
      "env": {
        "GSC_CREDENTIALS_PATH": "/Users/john/Documents/mcp-gsc/service_account_credentials.json",
        "GSC_SKIP_OAUTH": "true",
        "GSC_DATA_STATE": "all"
      }
    }
  }
}

**Key environment variables explained:**

| Variable | Value | Purpose |
|---|---|---|
| `GSC_CREDENTIALS_PATH` | Path to your `.json` key | Points to the service account credentials |
| `GSC_SKIP_OAUTH` | `"true"` | Forces service account auth, skips browser OAuth |
| `GSC_DATA_STATE` | `"all"` | Returns fresh data matching GSC dashboard (vs `"final"` which has 2–3 day lag) |

Save the file: `Ctrl+O` → `Enter` → `Ctrl+X`

---

### Step 6 — Restart Claude Desktop

Fully quit and reopen Claude Desktop. You should now see GSC tools available.

---

### Step 7 — Verify It Works

In Claude, type:
> "List all my GSC properties"

If it returns your Search Console properties, the connection is working.

---

### Available Tools After Setup

| Tool | What it does |
|---|---|
| `list_properties` | Shows all your GSC properties |
| `get_search_analytics` | Top queries, impressions, clicks, CTR |
| `get_performance_overview` | Site performance summary |
| `check_indexing_issues` | Page indexing problems |
| `inspect_url_enhanced` | Detailed URL inspection |
| `get_sitemaps` | List all sitemaps |
| `submit_sitemap` | Submit new sitemap to Google |
| `compare_search_periods` | Period-over-period comparison |
| `get_advanced_search_analytics` | Multi-dimension filtered analytics |

*(19 tools total — ask Claude to "list tools" after setup for the full list)*

---

### Troubleshooting

- **"python not found"**: Use the full path to `.venv/bin/python` in the config
- **"Service account has no access"**: Make sure the service account email is added as a user in GSC
- **Config not loading**: Fully quit Claude Desktop (not just close the window) and reopen
- **Paths wrong on Windows**: Use `\\` separators and point to `.venv\Scripts\python.exe`

---

### STEP 5 — Test Everything

Open Claude code and type:
- `/seo-tags` → skills working ✅
- `Show my GSC properties` → GSC connected ✅
- `Show Ahrefs domain rating for manageengine.com` → Ahrefs connected ✅

---

## Getting Latest Updates

Whenever Shyam updates a skill, just type this in Claude code:

Pull the latest changes from my claude-seo-skills repo
and update all skill files in ~/.claude/commands/


Claude will update everything automatically ✅

---

## Need Help?
Contact Shyam on Cliq for:
- Any setup issues




