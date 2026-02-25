# The PM MCP Toolkit — Every Tool, Verified

> MCP (Model Context Protocol) is how Claude Code connects to external tools. This file covers every MCP a Product Manager would realistically use, with installation commands, verified facts, and honest caveats.

---

## Table of Contents

1. [What MCP Actually Is](#what-mcp-is)
2. [How to Install an MCP Server](#how-to-install)
3. [Priority Install Order by PM Role](#priority-install-order)
4. [Project Management MCPs](#project-management)
5. [Design MCPs — Figma and Beyond](#design-mcps)
6. [Analytics MCPs](#analytics-mcps)
7. [Presentations and Documents](#presentations-documents)
8. [Research MCPs](#research-mcps)
9. [The Official PM Plugin](#official-pm-plugin)
10. [MCP Configuration Reference](#mcp-configuration-reference)

---

## What MCP Actually Is {#what-mcp-is}

MCP stands for Model Context Protocol. It is an open standard developed by Anthropic that lets Claude Code connect to external services — Jira, Figma, Notion, databases, analytics tools — and interact with them directly.

When you install an MCP server, Claude Code gains new tools. Instead of you copying data from Jira and pasting it into Claude Code, you say:

```
> Pull the top 10 open bugs labeled "P1" from Jira project ACME and
  organize them by component in a markdown table
```

And Claude Code fetches the data itself.

### Two Types of MCP Servers

**Remote servers** — Hosted by the vendor. You authenticate with an API token. No local install required. Examples: official Figma MCP, Amplitude MCP.

**Local servers** — Run on your machine. You install a package and configure a path. Examples: most community-built MCPs.

---

## How to Install an MCP Server {#how-to-install}

MCP servers are registered in Claude Code's configuration file: `~/.claude/config.json`.

### Basic installation pattern

```bash
# Step 1: Install the package (for npm-based servers)
npm install -g <package-name>

# Step 2: Register it in Claude Code
claude mcp add <name> -- <command> [args]
```

### Example: Adding a local npm-based MCP

```bash
# Install
npm install -g mcp-server-jira

# Register
claude mcp add jira -- mcp-server-jira --token YOUR_TOKEN --host your-company.atlassian.net
```

### Example: Adding a remote MCP

```bash
# No package install needed — just register with the remote URL
claude mcp add figma --transport sse https://mcp.figma.com/sse \
  --header "X-Figma-Token: YOUR_TOKEN"
```

### Verify it is working

```bash
# List registered MCP servers
claude mcp list

# Start a session and check tools
claude
> /tools
```

### The config.json directly

If you prefer to edit the config file directly:

```json
{
  "mcpServers": {
    "jira": {
      "command": "mcp-server-jira",
      "args": ["--token", "YOUR_TOKEN", "--host", "your-company.atlassian.net"]
    },
    "figma": {
      "transport": "sse",
      "url": "https://mcp.figma.com/sse",
      "headers": {
        "X-Figma-Token": "YOUR_TOKEN"
      }
    }
  }
}
```

---

## Priority Install Order by PM Role {#priority-install-order}

Not all MCPs are equally valuable for every PM. Here is the recommended install sequence by role:

| Order | MCP | B2B SaaS PM | Consumer PM | Platform PM | Growth PM |
|---|---|---|---|---|---|
| 1 | Official PM Plugin | Must | Must | Must | Must |
| 2 | Jira or Linear | Must | Optional | Must | Optional |
| 3 | GitHub Issues MCP | Recommended | Optional | Must | Optional |
| 4 | Figma MCP (free tier) | Must | Must | Recommended | Recommended |
| 5 | Notion MCP | Recommended | Recommended | Optional | Recommended |
| 6 | Amplitude or Mixpanel | Recommended | Must | Recommended | Must |
| 7 | Bright Data (research) | Recommended | Recommended | Recommended | Must |
| 8 | anyquery | Optional | Optional | Recommended | Recommended |
| 9 | PPTX MCP | Optional | Optional | Optional | Optional |

---

## Project Management MCPs {#project-management}

### Jira

**Option A — `nguyenvanduocit/jira-mcp`**
- Stars: 80
- Best for: Simple read/write of issues, sprints, epics

```bash
npm install -g @nguyenvanduocit/jira-mcp
claude mcp add jira -- jira-mcp \
  --token YOUR_JIRA_API_TOKEN \
  --host your-company.atlassian.net \
  --email your@email.com
```

What it enables:
- Pull all open tickets for a sprint into a markdown table
- Create a new Jira ticket from a spec Claude Code just drafted
- Update ticket descriptions in bulk
- Read epic hierarchies to understand feature groupings

**Option B — `aashari/mcp-server-atlassian-jira`**
- Stars: 56
- Best for: More granular Atlassian API access, including Confluence integration

```bash
npm install -g @aashari/mcp-server-atlassian-jira
claude mcp add atlassian-jira -- mcp-server-atlassian-jira \
  --host your-company.atlassian.net \
  --token YOUR_TOKEN \
  --email your@email.com
```

**Caveat:** Both Jira MCPs can be slow on large projects (1,000+ tickets) because they load all issues. Use specific JQL filters in your prompts to keep responses fast.

```
# Faster pattern — always filter in the prompt
> Pull only issues with label "Q2-roadmap" in project ACME that are unresolved
```

---

### Linear

**`tacticlaunch/mcp-linear`**
- Stars: 129
- Best for: Teams already using Linear for issue tracking

```bash
npm install -g mcp-linear
claude mcp add linear -- mcp-linear --token YOUR_LINEAR_API_KEY
```

Get your API key: Linear → Settings → API → Personal API keys

What it enables:
- Read and update Linear issues, cycles (sprints), and projects
- Create issues from Claude Code's spec output
- Query issues by team, label, priority, or status
- Move issues between cycles

```
> List all Linear issues in the "Mobile" team that are in the current cycle,
  group by priority, and flag any that have no assignee
```

**Why PMs prefer Linear over Jira MCP:** Linear's data model is cleaner and the MCP is more reliable. If your team uses Linear, start here instead of Jira.

---

### Notion

**`danhilse/notion_mcp`**
- Stars: 206
- Best for: Reading Notion pages and databases for reference

```bash
npm install -g notion-mcp-server
claude mcp add notion -- notion-mcp-server --token YOUR_NOTION_TOKEN
```

Get your Notion token: Notion → Settings → Integrations → New integration → Copy "Internal Integration Token"

Then share specific pages/databases with the integration inside Notion.

What it enables:
- Query Notion databases (product roadmap, customer feedback log, etc.)
- Read wiki pages and product docs
- Create new pages with Claude Code's output
- Update existing pages

**Known performance caveat:** The Notion MCP is slow and token-heavy for large workspaces. A workspace with 500+ pages will cause responses to time out or consume a large portion of your context window.

**Better pattern for Notion:**

Use the Notion MCP only for live queries and small databases. For large reference materials (product wiki, team handbook):

```bash
# Export from Notion as Markdown
# (Notion → Export → Markdown & CSV → Download)
# Then put the .md files in your project's context/ folder

# Reference them in CLAUDE.md:
# ## Context Loading Sequence
# - context/product-wiki.md — main product documentation
```

This is faster, more reliable, and does not consume your context window on every query.

---

### GitHub Issues (Official GitHub MCP)

- Source: Official GitHub MCP (Anthropic-maintained)
- Stars: Part of official GitHub integrations
- Best for: Teams using GitHub Issues as their PM tracking tool

```bash
# Install via GitHub's official MCP
claude mcp add github -- npx @modelcontextprotocol/server-github \
  --token YOUR_GITHUB_TOKEN
```

Get your token: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token. Scopes needed: `repo`, `read:org`

What it enables:
- Read and create issues, pull requests, and comments
- Query issues by label, milestone, assignee, or state
- Create issues directly from Claude Code's spec output
- Read PR descriptions to understand what shipped in a sprint

**Why this is increasingly PMs' favorite tracking MCP:**

GitHub Issues is markdown-native. Claude Code writes markdown natively. The two pair perfectly — Claude Code can create issues with perfectly formatted descriptions, code blocks, and checklists without any formatting translation layer.

```
> Read all closed pull requests merged in the last 14 days in repo acme/backend.
  Write a "What Shipped" summary in release-notes/sprint-42.md,
  grouping changes by feature area and flagging any changes to the API contract.
```

---

### Slack MCP

- Status: Limited public availability as of early 2026
- Official Slack MCP is in beta for enterprise customers
- Community options exist but have limited API access due to Slack's rate limits

**Current best option (community):**
```bash
npm install -g @modelcontextprotocol/server-slack
claude mcp add slack -- mcp-server-slack --token YOUR_SLACK_BOT_TOKEN
```

**Canvas note:** Slack Canvas is noted as limited in MCP access. The MCP provides message and channel read access, but Canvas documents are not reliably accessible.

**Honest caveat:** The Slack MCP is the weakest of the project management integrations as of early 2026. Use it for reading message history and specific channel queries. Do not rely on it for real-time notifications or Canvas access.

---

### anyquery

- Stars: 1,626
- Best for: Power users who want to query 40+ sources from one interface

```bash
npm install -g anyquery
claude mcp add anyquery -- anyquery mcp
```

What it enables: anyquery lets Claude Code run SQL-like queries against Notion databases, GitHub issues, Trello boards, Airtable, Spotify, and 35+ other sources simultaneously.

```
> Query my Notion "Customer Feedback" database for all entries with
  sentiment = "negative" and feature_area = "onboarding" from the last 30 days.
  Cross-reference with GitHub issues labeled "onboarding" that are still open.
```

**Setup note:** Each source in anyquery requires its own authentication. The initial setup takes 20-30 minutes. But once configured, it is the most powerful multi-source query tool available.

---

## Design MCPs — Figma and Beyond {#design-mcps}

### Figma MCP — The Full Breakdown

The Figma MCP situation is more nuanced than most documentation covers. Here is the accurate picture as of February 2026.

#### Option A — Official Figma Remote MCP (FREE for all plans including free tier)

Figma launched an official remote MCP server in early 2026. It is free for all Figma users including the free tier. You only need a personal access token.

```bash
# No package install needed — it is a remote server
# Just add it to your Claude Code config

claude mcp add figma --transport sse https://mcp.figma.com/sse \
  --header "X-Figma-Token: YOUR_FIGMA_TOKEN"
```

Get your Figma personal access token: Figma → Click your name (top left) → Settings → Account → Personal access tokens → Generate new token

What it enables:
- Read any Figma file by URL or file ID
- Get frame dimensions, component names, and text content
- Read design tokens and style guides
- Inspect layer hierarchy and auto-layout properties
- Read component documentation and variant properties

```
> Read the Figma file at [url] and list all the form components in the
  Design System page. For each component, describe its variants and states.
```

#### Option B — Official Figma Desktop MCP (Requires paid Dev or Full seat)

The Desktop MCP is part of Figma's "Dev Mode" feature set. It requires either:
- **Dev seat**: ~$12/month per seat
- **Full seat (Figma Professional or Organization)**: ~$35/month per seat

The Desktop MCP adds capabilities the remote server does not have:
- Export components as code (React, SwiftUI, Flutter)
- Access detailed typography measurements
- Read "Ready for Dev" annotations
- Link design decisions to code components

If your team already has Dev Mode seats, use this. If not, start with Option A (free remote MCP).

#### Option C — Community: `arinspunk/claude-talk-to-figma-mcp`

- Free, works on any Figma plan including free tier
- Maintained by the community
- Good fallback if the official remote MCP has downtime

```bash
git clone https://github.com/arinspunk/claude-talk-to-figma-mcp
cd claude-talk-to-figma-mcp
npm install
claude mcp add figma-community -- node index.js --token YOUR_FIGMA_TOKEN
```

#### NEW February 2026: Claude Code → Figma Direction

This is the most exciting recent development. Previously, the flow was Figma → Claude Code (read designs). The new direction is Claude Code → Figma (create or update designs).

How it works: Claude Code captures the live UI from a browser (via screenshot or DOM inspection), analyzes the layout, and creates editable Figma frames from it.

**Use cases for PMs:**
- Capture a competitor's UI and create a Figma frame for analysis
- Take a low-fidelity wireframe and generate a structured Figma layout
- Document an existing UI into Figma for a component library audit

**Source:** This capability is documented in the official Figma developer blog and MCP documentation, February 2026.

---

## Analytics MCPs {#analytics-mcps}

### Amplitude (Official Anthropic-partnered integration)

Amplitude is one of the official Anthropic partner integrations, which means it has enterprise-grade support and reliability.

```bash
claude mcp add amplitude --transport sse https://mcp.amplitude.com/sse \
  --header "Authorization: Bearer YOUR_AMPLITUDE_API_KEY"
```

Get your API key: Amplitude → Settings → Projects → [Your Project] → API key

What it enables:
- Query funnels, retention curves, and event counts directly in Claude Code
- Ask natural language questions about user behavior
- Pull cohort definitions and user segment data
- Generate insights reports from raw event data

```
> Pull the signup-to-first-value funnel for new users from the last 30 days.
  What are the three biggest drop-off points? What do users who convert
  in under 5 minutes have in common?
```

**PM tip:** Amplitude's MCP supports natural language queries, so you do not need to know Amplitude's query syntax. Describe what you want and let Claude Code translate it.

---

### Google Analytics MCP

- Stars: 177
- Best for: Teams using GA4 for web analytics

```bash
npm install -g @gal-jora/mcp-google-analytics
claude mcp add google-analytics -- mcp-google-analytics \
  --property-id YOUR_GA4_PROPERTY_ID \
  --credentials-path ~/path/to/service-account.json
```

Setup requires a Google Cloud service account with Analytics Read permissions.

```
> What are the top 10 landing pages by conversion rate over the last 7 days?
  Which pages have high traffic but low conversion — those are our optimization targets.
```

---

### Mixpanel MCP

- Stars: 19
- Best for: Teams using Mixpanel for product analytics

```bash
npm install -g mcp-mixpanel-server
claude mcp add mixpanel -- mcp-mixpanel-server \
  --token YOUR_MIXPANEL_TOKEN \
  --project-id YOUR_PROJECT_ID
```

**Note:** Lower star count reflects that this is a newer and less mature MCP. Test it on non-critical queries first before relying on it for important analysis.

---

### Power BI MCP

- Stars: 200
- Best for: Enterprise teams with data in Power BI

```bash
npm install -g mcp-powerbi
claude mcp add powerbi -- mcp-powerbi \
  --client-id YOUR_AZURE_CLIENT_ID \
  --tenant-id YOUR_AZURE_TENANT_ID
```

What it enables: Query Power BI datasets and reports directly. Useful when your company's business intelligence data lives in Power BI and you need to pull metrics into a spec or review.

---

### Metabase MCP

**`apiconn/metabase-mcp-server`**
- Stars: 42
- Best for: Teams using self-hosted or cloud Metabase for analytics

```bash
npm install -g metabase-mcp-server
claude mcp add metabase -- metabase-mcp-server \
  --url https://your-metabase.company.com \
  --username your@email.com \
  --password YOUR_PASSWORD
```

What it enables: Run saved Metabase questions and dashboards, query any database connected to Metabase, pull data into Claude Code for analysis.

```
> Run the "Weekly Active Users by Cohort" saved question in Metabase
  and tell me if the 30-day retention for users acquired in January
  is above or below our Q1 target of 40%.
```

---

## Presentations and Documents {#presentations-documents}

### The PPTX Situation in Claude Code CLI

**Important fact:** Native PPTX editing ships in Claude Desktop and Claude.ai web — not in the Claude Code CLI. If you open Claude Desktop (the GUI app), you can drag a PowerPoint file in and Claude will edit it natively.

In Claude Code CLI, you need a community MCP for PPTX work. Here are the options from best to simplest:

---

### `tfriedel/claude-office-skills` — Best Option for Claude Code CLI

This is the most capable PPTX solution available for Claude Code CLI as of early 2026. It works by converting your content to HTML, rendering it with Playwright (a browser automation tool), and generating a PPTX from the rendered output.

```bash
# Prerequisites
npm install -g playwright
npx playwright install chromium

# Install the MCP
git clone https://github.com/tfriedel/claude-office-skills
cd claude-office-skills
npm install
claude mcp add office-skills -- node index.js
```

What it enables:
- Create presentation decks from Claude Code's markdown output
- Apply proper styling, charts, and image layouts
- Export to .pptx format that opens in PowerPoint and Keynote

```
> Take the competitive analysis in research/competitive.md and
  create a 10-slide presentation. Use a clean business style.
  Save to presentations/competitive-review.pptx
```

**Caveat:** Requires Playwright and Chromium, which are ~150MB downloads. If you do not want these dependencies, use the Markdown option below.

---

### `dmytro-ustynov/pptx-generator-mcp` — Simple Markdown to PPTX

- Best for: Quick, no-frills decks from markdown

```bash
npm install -g pptx-generator-mcp
claude mcp add pptx -- pptx-generator-mcp
```

What it enables: Write a deck outline in markdown (with `## Slide Title` and `- Bullet point` syntax) and convert it directly to PPTX.

```
> Convert this markdown outline to a PPTX file:

## Q2 Roadmap Overview
- 3 themes: onboarding, collaboration, scale
- Target: 10% improvement in activation rate

## Theme 1: Onboarding
- Smart setup wizard
- Contextual tooltips
- Progress tracking

Save as presentations/q2-roadmap.pptx
```

---

### `GongRzhe/Office-PowerPoint-MCP-Server`

```bash
npm install -g office-powerpoint-mcp-server
claude mcp add powerpoint -- office-powerpoint-mcp-server
```

Alternative option for basic PowerPoint creation. Less feature-rich than tfriedel's option but simpler to install.

---

### `frontend-slides` — Presentations from Code

For engineering-facing presentations or when you want code snippets rendered properly:

```bash
npm install -g frontend-slides-mcp
claude mcp add frontend-slides -- frontend-slides-mcp
```

What it enables: Creates presentations rendered in HTML with proper syntax highlighting for code. Outputs to HTML that can be presented in browser or converted to PDF.

**Best for:** Architecture reviews, technical PRDs, API specification presentations.

---

## Research MCPs {#research-mcps}

### Bright Data MCP — Best for Competitive Intelligence

Bright Data is the most capable web scraping solution available as an MCP. It can unlock pages with bot detection, CAPTCHA challenges, and JavaScript-rendered content that simple scrapers cannot reach.

This is already installed in Claude Code for teams using the Bright Data plan (check with `/tools` to see if `scrape_as_markdown` appears).

If you need to install it manually:

```bash
claude mcp add brightdata --transport sse https://mcp.brightdata.com/sse \
  --header "Authorization: Bearer YOUR_BRIGHTDATA_TOKEN"
```

What it enables:
- Scrape any public web page and get clean markdown output
- Run batch scrapes on competitor pages simultaneously
- Search Google/Bing and get structured results
- Bypass anti-scraping protections on publicly accessible pages

**PM use cases:**

```
> Scrape the pricing pages for Figma, Sketch, and Adobe XD.
  Compare their pricing tiers, free plan limits, and team pricing.
  Output a comparison table to research/design-tool-pricing.md
```

```
> Search for "[competitor] product updates" on Google.
  Fetch the top 5 results and summarize what they have shipped
  in the last 6 months that overlaps with our roadmap.
```

**Important:** Bright Data only scrapes **public** pages. Do not use it for pages behind authentication.

---

### `jacob-bd/notebooklm-mcp-cli` — Connect Google NotebookLM

```bash
npm install -g notebooklm-mcp-cli
claude mcp add notebooklm -- notebooklm-mcp-cli \
  --google-account your@gmail.com
```

What it enables: Bridge Claude Code to Google NotebookLM. Upload research documents to NotebookLM and query them from Claude Code, or use NotebookLM's audio overview feature with documents Claude Code produces.

**PM use case:** Upload user research transcripts to NotebookLM via Claude Code, then query across all of them from a single session.

---

## The Official PM Plugin {#official-pm-plugin}

### `anthropics/knowledge-work-plugins/product-management`

- Stars: 7,500
- This is Anthropic's official plugin for product managers and knowledge workers

```bash
claude mcp add pm-plugin --transport sse \
  https://plugins.anthropic.com/product-management/sse
```

Or install via Claude Code's plugin registry:

```bash
claude plugin install product-management
```

### What It Includes

The PM plugin adds 6 slash commands that invoke specialized prompt chains built around PM best practices:

---

**`/pm:prd-create`** — Create a Product Requirements Document

```
> /pm:prd-create feature="Smart search with semantic matching"
```

Generates a full PRD including: executive summary, problem statement, user stories, functional requirements, non-functional requirements, success metrics, open questions, and out-of-scope items.

---

**`/pm:feature-spec`** — Create a detailed feature specification

```
> /pm:feature-spec feature="Bulk user import via CSV" epic="Admin Tools"
```

Generates: feature overview, user flow, edge cases, error handling, UI requirements, and acceptance criteria.

---

**`/pm:user-story`** — Convert a feature idea into properly formatted user stories

```
> /pm:user-story "users need to be able to export their data"
```

Generates: user stories in "As a [persona], I want [goal] so that [benefit]" format, with acceptance criteria in Given/When/Then format.

---

**`/pm:metrics-framework`** — Define measurement framework for a feature

```
> /pm:metrics-framework feature="New onboarding flow"
```

Generates: primary metric, secondary metrics, guardrail metrics, instrumentation requirements, and experiment design if A/B testing is relevant.

---

**`/pm:competitive`** — Run a structured competitive analysis

```
> /pm:competitive feature="AI writing assistant" competitors="Notion AI, Grammarly, Jasper"
```

If Bright Data MCP is also installed, this will scrape competitor pages automatically. Otherwise, it works from Claude Code's training data and any context files you have loaded.

---

**`/pm:roadmap-review`** — Review a roadmap for completeness and strategic alignment

```
> /pm:roadmap-review file="roadmap/q2-2026.md"
```

Reviews your roadmap file against your CLAUDE.md context and flags: missing success metrics, items without clear owners, items that conflict with stated OKRs, and sequencing issues.

---

## MCP Configuration Reference {#mcp-configuration-reference}

### Full config.json example for a typical PM setup

```json
{
  "mcpServers": {
    "product-management": {
      "transport": "sse",
      "url": "https://plugins.anthropic.com/product-management/sse"
    },
    "jira": {
      "command": "jira-mcp",
      "args": [
        "--token", "YOUR_JIRA_API_TOKEN",
        "--host", "your-company.atlassian.net",
        "--email", "your@email.com"
      ]
    },
    "github": {
      "command": "npx",
      "args": [
        "@modelcontextprotocol/server-github",
        "--token", "YOUR_GITHUB_TOKEN"
      ]
    },
    "figma": {
      "transport": "sse",
      "url": "https://mcp.figma.com/sse",
      "headers": {
        "X-Figma-Token": "YOUR_FIGMA_TOKEN"
      }
    },
    "notion": {
      "command": "notion-mcp-server",
      "args": ["--token", "YOUR_NOTION_TOKEN"]
    },
    "amplitude": {
      "transport": "sse",
      "url": "https://mcp.amplitude.com/sse",
      "headers": {
        "Authorization": "Bearer YOUR_AMPLITUDE_KEY"
      }
    },
    "brightdata": {
      "transport": "sse",
      "url": "https://mcp.brightdata.com/sse",
      "headers": {
        "Authorization": "Bearer YOUR_BRIGHTDATA_TOKEN"
      }
    }
  }
}
```

### Where is the config file?

```bash
# macOS / Linux
~/.claude/config.json

# Windows
%APPDATA%\Claude\config.json
```

### Checking which MCPs are active in a session

```
> /tools
```

This lists every tool available in the current session, including all MCP-provided tools.

### Troubleshooting a broken MCP

```bash
# Check MCP server status
claude mcp list

# Remove and re-add a broken MCP
claude mcp remove <name>
claude mcp add <name> -- <command> [args]

# Test in verbose mode
claude --verbose
```

---

## Quick Comparison Table

| MCP | Free Tier | Stars | Reliability | PM Value |
|---|---|---|---|---|
| Official PM Plugin | Yes (Claude sub) | 7,500 | High | Essential |
| Figma Remote MCP | Yes | Official | High | Essential |
| GitHub Issues MCP | Yes (public repos) | Official | High | High |
| Linear MCP | Yes | 129 | High | High |
| Jira MCP (nguyenvan) | Yes | 80 | Medium | High |
| Notion MCP | Yes | 206 | Medium | Medium |
| Amplitude MCP | Requires account | Official | High | High |
| Bright Data MCP | Paid | Official | High | High |
| anyquery | Free trial | 1,626 | High | Medium |
| tfriedel office-skills | Yes | Community | Medium | Medium |
| pptx-generator-mcp | Yes | Community | Medium | Low-Medium |
| Google Analytics MCP | Yes | 177 | Medium | Medium |
| Power BI MCP | Yes | 200 | Medium | Medium |
| Metabase MCP | Yes | 42 | Low-Medium | Medium |
| Mixpanel MCP | Yes | 19 | Low-Medium | Medium |

---

*Next: [03-claude-md-for-pms.md](./03-claude-md-for-pms.md) — How to write a CLAUDE.md that makes every session 10x more effective.*
