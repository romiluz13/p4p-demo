# Tools & Integrations — Figma, PowerPoint, Google Slides, and More

> Precise answers to the tool questions PMs ask most. Every integration here has been verified against documented sources as of early 2026.

---

## Table of Contents

1. [Figma Integration](#figma-integration)
2. [PowerPoint — Native Limitations and Community Solutions](#powerpoint)
3. [Google Slides Import](#google-slides)
4. [Obsidian + Claude Code](#obsidian)
5. [NotebookLM Integration](#notebooklm)
6. [Calendar and Meeting Tools](#calendar-meeting)
7. [Data Tools with MCP Support](#data-tools)
8. [Connecting a Database Safely](#database-safety)
9. [Integration Quick Reference](#quick-reference)

---

## Figma Integration {#figma-integration}

### Free vs. Paid — The Definitive Answer

The most common question PMs ask: "Do I need a paid Figma seat to use this?" The answer depends on which connection method you use.

| Connection Method | Who Can Use | Cost |
|---|---|---|
| Remote MCP server | ALL Figma plans including free | FREE |
| Desktop MCP server | Dev mode or Full seat only | Paid (~$12–35/mo depending on tier) |
| Community plugin (arinspunk) | Any plan | Always free |

**Bottom line:** If you have a free Figma account, use the remote MCP server. If you have a paid Dev or Full seat, you can also use the desktop MCP server, which has slightly more capabilities.

### Setup for Free Users (Remote MCP Server)

This is the recommended path for PMs who do not have a Dev seat.

**Step 1: Get your Figma API key**

1. Open Figma in your browser
2. Click your profile picture (top-left)
3. Click "Account Settings"
4. Scroll to "Personal access tokens"
5. Click "Generate new token", give it a name like "claude-code"
6. Copy the token immediately — you cannot see it again

Total time: 2 minutes.

**Step 2: Add the MCP server to your Claude Code config**

Create or edit `.claude/settings.json` in your home directory or project directory:

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--figma-api-key=YOUR_KEY_HERE"]
    }
  }
}
```

Replace `YOUR_KEY_HERE` with the token you copied.

**Step 3: Restart Claude Code**

```bash
claude
```

On startup, Claude Code will initialize the Figma MCP server. You will see a confirmation message. If you get an error, double-check that Node.js is installed and the API key is pasted correctly.

**Step 4: Test it**

```
> What Figma files do I have access to?
```

Claude Code will list your accessible files through the API.

### Community Option (Any Plan, Always Free)

If you prefer not to configure JSON, the community plugin route works from any Figma plan:

```bash
/plugin marketplace add arinspunk/claude-talk-to-figma-mcp
```

This installs directly from the Claude Code plugin marketplace. It is maintained by the community and has fewer features than the official MCP, but it works reliably for the most common PM use cases.

### What PMs Actually Use Figma MCP For

These are the four highest-value use cases, not a theoretical feature list.

**1. Acceptance criteria from design**

Select a frame in Figma, then:

```
> Read the selected Figma frame "Checkout v3 - Empty Cart State".
  Write acceptance criteria for this screen.
  Format as a numbered list with clear pass/fail conditions.
  Save to specs/checkout-empty-state-ac.md
```

This replaces a 30-minute task (studying the design, writing acceptance criteria from scratch) with a 2-minute review task.

**2. Design tokens and component names as context for specs**

```
> Pull the design token names and component names from the "Design System"
  Figma file. I need to use the correct component names when writing
  engineering tickets so developers know exactly which component to use.
  Output as a reference table.
```

**3. FigJam diagrams as PRD input**

```
> Read the FigJam diagram titled "User Journey - Onboarding Redesign".
  Use it as the source of truth for the user flow.
  Write a PRD section describing the proposed flow step by step.
```

**4. Design versus implementation audit**

```
> I have a screenshot of our production checkout flow saved to
  screenshots/checkout-production-2026-02.png.
  Compare it against the Figma frame "Checkout v3 - Final".
  List every visual difference you can identify.
  Format as a table: Element | Figma | Production | Severity
```

### NEW — February 2026: Claude Code to Figma (Reverse Direction)

Anthropic and Figma announced a new capability in February 2026 that runs in the opposite direction: from your running application into Figma.

Source: figma.com/blog/introducing-claude-code-to-figma/ (published February 17, 2026)

**What it does:**

1. Take a screenshot of your running app — localhost, staging, or production
2. Claude Code pushes it to Figma as editable frames
3. Your team can annotate exactly what is different from the original design
4. No meeting required to run a "design vs. reality" audit

**Why it matters for PMs:**

The traditional workflow is: engineer ships feature → designer reviews it in staging → designer creates a long list of comments → PM triages → another sprint. This collapses that cycle. The pushed Figma frame becomes the artifact the whole team annotates, and the delta between original and shipped is visible in one view.

**How to use it:**

```
> Take a screenshot of localhost:3000/checkout
  and push it to Figma as a new frame in the
  "QA Audit - Q1 2026" file under the "Production vs Design" page.
  Label the frame "Checkout - Feb 22 2026"
```

### Code Connect — Keeping Generated Code in Your Component Library

Figma's Code Connect feature links Figma components to your actual codebase components. When Claude Code generates code suggestions from a Figma design, it uses your real component names and imports instead of generic HTML.

This is most relevant when your engineering team is using Claude Code to generate implementation from designs. For PM use cases, the main benefit is that acceptance criteria you write using component names will match what engineers actually see in their code.

To enable it, a developer needs to run `figma connect publish` once for your design system. After that, any Claude Code + Figma session gets component-accurate output automatically.

---

## PowerPoint — Native Limitations and Community Solutions {#powerpoint}

### The Situation (What Works Where)

This is the most common source of confusion for PMs, so the facts are stated directly:

- Anthropic shipped native PPTX editing in **Claude Desktop and Claude Web only** (the browser and desktop app)
- This native capability does **NOT** work in Claude Code CLI (the terminal version)
- Claude Code CLI requires a community solution to generate PPTX files

If you do most of your work in Claude.ai or Claude Desktop, native PowerPoint editing just works — paste your outline, ask for slides, download the file.

If you are using Claude Code CLI (the subject of this entire guide), read on.

### Best Solution: `tfriedel/claude-office-skills`

This is the most capable community solution for PPTX generation from Claude Code CLI.

**Install:**

```bash
/plugin marketplace add tfriedel/claude-office-skills
```

**How it works under the hood:**

1. Claude designs each slide in HTML/CSS
2. Playwright (a browser automation library) renders each slide as a screenshot
3. PptxGenJS converts the screenshots into a real `.pptx` file
4. Claude Code validates visually (it can see what the slide looks like)
5. You iterate on the output

This approach produces presentations that actually look like presentations — not just text-in-boxes — because the HTML/CSS rendering gives Claude full control over layout.

**Creating a presentation from your research files:**

```
Using office-skills, create a 10-slide executive presentation.

Source materials:
- research/q3-competitive-analysis.md
- specs/q4-roadmap.md

Audience: CPO and VP Engineering
Tone: Direct, data-forward, no fluff

Use our template: templates/company-deck-template.pptx
Save final output to: deliverables/q4-roadmap-exec-deck.pptx

Slide structure:
1. Situation (market and competitive context)
2-3. Key findings from competitive analysis
4. Our Q4 roadmap — 3 bets
5-7. One slide per bet: problem, solution, metrics
8. Resource requirements
9. Risks and mitigations
10. Ask and next steps
```

**Using a company template:**

The plugin extracts the structure of your existing `.pptx` template — fonts, colors, master layouts, logo placement — and replaces only the content while preserving all formatting. Your final deck looks like it was made in your company's template, not generated by a tool.

```
Using office-skills, update our template at templates/company-deck.pptx
with the following content. Preserve all existing formatting, fonts, and colors.
Only replace the text and placeholder content.

[your content here]
```

### Simpler Option: `dmytro-ustynov/pptx-generator-mcp`

If you just need to get Markdown into a `.pptx` file quickly and do not need design fidelity, this MCP is faster to set up:

```bash
/plugin marketplace add dmytro-ustynov/pptx-generator-mcp
```

You write your content in Markdown with `---` separators between slides:

```markdown
# Q4 Roadmap Overview

Three strategic bets for Q4 2026

---

# Bet 1: Search Overhaul

- Current NPS for search: 23
- Target NPS: 50+
- Key deliverable: semantic search

---

# Bet 2: Mobile Parity

...
```

Then run:

```
> Convert slides.md to a PowerPoint presentation.
  Save to deliverables/q4-roadmap.pptx
```

The output is functional but not visually polished. Use this for internal decks, not for CPO presentations.

### Full Prompt Template — Executive Deck from Scratch

```
Using office-skills, create a 10-slide exec presentation.

Sources:
- research/q3-competitive-analysis.md (competitive context)
- specs/q4-roadmap.md (our proposed roadmap)
- data/q3-metrics.csv (usage metrics to support claims)

Audience: CPO and VP Engineering — they know the product but not the detail.
Reading time available: 8 minutes. No reading material pre-read.

Template: templates/company-deck-template.pptx
Output: deliverables/q4-roadmap-exec-deck.pptx

Requirements:
- Every claim needs a data point from the source files
- Maximum 40 words of text per slide (excluding speaker notes)
- Include speaker notes with 3-5 sentences per slide
- Flag any assumption I should verify before presenting (prefix with [ASSUMPTION])
```

---

## Google Slides Import {#google-slides}

Claude Code does not have a native Google Slides MCP that creates slides directly in Google Drive. The practical options, in order of quality:

### Option 1: HTML Deck to PDF (Highest Fidelity)

```
> Create this presentation as a styled HTML file.
  Save to deliverables/q4-roadmap.html
```

Then:
1. Open the HTML file in Chrome
2. Press `Cmd+P` (Mac) or `Ctrl+P` (Windows)
3. Set destination to "Save as PDF"
4. In Google Slides, go to **File > Import slides**
5. Upload the PDF

Each PDF page becomes a slide. The visual fidelity is high because you are importing rendered output, not raw text.

### Option 2: PPTX to Google Slides (Easiest for Collaboration)

If you generated a PPTX using `office-skills`:

1. Upload the `.pptx` file to Google Drive
2. Right-click the file → "Open with" → "Google Slides"
3. Google Drive converts it automatically
4. All formatting is preserved reasonably well

### Option 3: Build in HTML and Present Directly

For presentations you will give live from your own machine, you can skip the import entirely:

```
> Create this as a Reveal.js HTML presentation.
  Save to presentations/q4-roadmap-deck.html
  It should be fully self-contained — no external CDN dependencies.
  Include keyboard navigation: arrow keys for next/previous slides.
```

Open the `.html` file in a browser and present from it directly. Works offline, no Google account needed, no conversion step.

---

## Obsidian + Claude Code {#obsidian}

Many PMs use Obsidian as their personal knowledge base — research notes, meeting notes, decision logs, interview synthesis. Claude Code integrates with it without any plugin.

### Setup

Point Claude Code at your Obsidian vault:

```bash
cd ~/Obsidian/ProductNotes
claude
```

Your entire vault is now readable by Claude Code. Every `.md` note is a file it can search, summarize, and synthesize.

### CLAUDE.md Reference to Notes Folder

Add a pointer in your CLAUDE.md:

```markdown
## Context Sources

- Personal research notes: ~/Obsidian/ProductNotes/research/
- Interview transcripts: ~/Obsidian/ProductNotes/user-interviews/
- Decision log: ~/Obsidian/ProductNotes/decisions/
- Competitor tracking: ~/Obsidian/ProductNotes/competitive/
```

Now when you ask Claude Code a question, it knows where to look:

```
> Search my research notes for everything related to the checkout abandonment
  problem. Synthesize the key themes and write a problem statement
  I can use to open the next sprint planning meeting.
```

### Useful Obsidian + Claude Code Workflows

**Synthesize interview notes before a PRD:**

```
> Read all markdown files in user-interviews/q3-2026/
  Extract the top 5 pain points mentioned across all interviews.
  Count how many interviewees mentioned each one.
  Format as a prioritized problem statement with supporting quotes.
```

**Build a decision log entry:**

```
> We decided today to use Server-Sent Events instead of WebSockets
  for the notification feature.
  Write a decision log entry.
  Reasoning: SSE is unidirectional (we don't need client-to-server streaming),
  simpler to implement, and works through most corporate proxies.
  Tradeoffs acknowledged: no bidirectional future use.
  Save to ~/Obsidian/ProductNotes/decisions/2026-02-22-sse-vs-websockets.md
```

---

## NotebookLM Integration {#notebooklm}

`jacob-bd/notebooklm-mcp-cli` connects your Google NotebookLM notebooks to Claude Code.

**Install:**

```bash
/plugin marketplace add jacob-bd/notebooklm-mcp-cli
```

**Why this is useful for PMs:**

NotebookLM is excellent for ingesting large documents — regulatory filings, lengthy research reports, product transcripts — and building a queryable corpus. If you have built notebooks with market research or competitive documents, this MCP lets Claude Code query them as part of a larger workflow.

**Example:**

```
> Query my NotebookLM notebook "Competitive Intelligence 2026"
  for everything about competitors' pricing models.
  Combine with the pricing analysis in research/our-pricing.md
  and write a recommendation on our pricing structure for Q3.
```

This is most useful for PMs who already have a substantial NotebookLM library and want to bring that knowledge into their Claude Code sessions.

---

## Calendar and Meeting Tools {#calendar-meeting}

There is no universally adopted calendar MCP for Claude Code as of early 2026. The practical approach for PMs is:

### Meeting Notes as Markdown Files

Export meeting notes from your tool of choice as Markdown (most tools support this), drop them into a folder, and point Claude Code at it:

```bash
mkdir meeting-notes
# Paste exported notes as .md files
```

Then use Claude Code for synthesis:

```
> Read all files in meeting-notes/q1-2026/
  Summarize the recurring themes and action items that keep coming up
  but have not been resolved. Format as a risk register.
```

### Zapier MCP for Broader Automation

The Zapier MCP (`zapier/mcp`) connects Claude Code to 6,000+ apps through Zapier's automation platform, including Google Calendar, Outlook, and meeting tools. This is the best current option for PMs who want calendar context without building custom integrations.

```bash
/plugin marketplace add zapier/mcp
```

Authentication requires a Zapier account and configuring the "Claude Code" action in your Zapier settings. Once connected:

```
> What meetings do I have this week that are related to the Q2 roadmap?
  Pull from my Google Calendar and cross-reference with the roadmap items
  in specs/q2-roadmap.md
```

---

## Data Tools with MCP Support {#data-tools}

| Tool | Source | Stars | Notes |
|---|---|---|---|
| MongoDB | Official (Anthropic) | — | Native integration, full CRUD, vector search |
| PostgreSQL | Community | — | Standard SQL interface |
| Metabase | Community | 42 | Business intelligence; read-only queries |
| Amplitude | Official (Amplitude) | — | Events, funnels, retention, cohorts |
| Google Analytics | Community | 177 | GA4 data access via API |
| Mixpanel | Community | 19 | Events and user profiles |
| Power BI | Community | 200 | Microsoft BI; report and dataset access |
| Snowflake | Community | — | Data warehouse; SQL queries |
| BigQuery | Community | — | Google data warehouse |

**How to find any of these:**

```bash
/plugin marketplace search [tool-name]
```

Or search the community registry at mcp.so.

### Installation Pattern (All Data MCPs Follow This)

```json
{
  "mcpServers": {
    "amplitude": {
      "command": "npx",
      "args": ["-y", "@amplitude/mcp-server"],
      "env": {
        "AMPLITUDE_API_KEY": "your-api-key",
        "AMPLITUDE_SECRET_KEY": "your-secret-key"
      }
    }
  }
}
```

Every data MCP follows the same pattern: `command` (how to launch it) + `env` (credentials). The only things that change are the package name and the environment variable names.

---

## Connecting a Database Safely {#database-safety}

If you connect Claude Code to a production database, read this section before you start.

### The Core Risk

Claude Code can execute queries against a database. If it connects with write credentials and makes a mistake — or if you accidentally approve a destructive operation — you can delete or corrupt data. This is not hypothetical: it has happened.

### The Safe Pattern: Read-Only Credentials

Create a dedicated read-only database user for Claude Code. Never use admin or application credentials.

**PostgreSQL:**

```sql
-- Create a read-only user for Claude Code
CREATE USER claude_code_readonly WITH PASSWORD 'choose-a-strong-password';
GRANT CONNECT ON DATABASE your_database TO claude_code_readonly;
GRANT USAGE ON SCHEMA public TO claude_code_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO claude_code_readonly;

-- Ensure future tables are also covered
ALTER DEFAULT PRIVILEGES IN SCHEMA public
  GRANT SELECT ON TABLES TO claude_code_readonly;
```

**MongoDB:**

```javascript
db.createUser({
  user: "claude_code_readonly",
  pwd: "choose-a-strong-password",
  roles: [{ role: "read", db: "your_database" }]
})
```

Then use this read-only connection string in your MCP configuration — never your application's connection string.

### Tell Claude Code Its Constraints Explicitly

In your CLAUDE.md or at the start of any data session:

```markdown
## Database Access Rules

- Connection: Read-only PostgreSQL user (claude_code_readonly)
- Permitted operations: SELECT queries only
- Forbidden: INSERT, UPDATE, DELETE, DROP, ALTER, TRUNCATE
- Before any query: state the query you plan to run and wait for my confirmation
```

Adding explicit constraints in CLAUDE.md is belt-and-suspenders. The read-only user credential is the actual technical protection. The CLAUDE.md instruction prevents confusion and makes your intent clear.

### Staging vs. Production

If you have a staging environment with a production-like dataset, always point Claude Code at staging first. Validate that the queries produce sensible output before running the same analysis against production.

---

## Integration Quick Reference {#quick-reference}

### Figma

```bash
# Free users — remote MCP
# Edit .claude/settings.json:
# "figma": { "command": "npx", "args": ["-y", "figma-developer-mcp", "--figma-api-key=KEY"] }

# Community alternative (any plan)
/plugin marketplace add arinspunk/claude-talk-to-figma-mcp
```

### PowerPoint (CLI)

```bash
# Full-featured (HTML→Playwright→PPTX)
/plugin marketplace add tfriedel/claude-office-skills

# Simple (Markdown→PPTX)
/plugin marketplace add dmytro-ustynov/pptx-generator-mcp
```

### Google Slides

No direct MCP. Workflow: Claude Code → HTML → PDF → Import to Google Slides, or Claude Code → PPTX → Upload to Drive.

### Data Tools

```bash
# Search for any data tool
/plugin marketplace search amplitude
/plugin marketplace search postgresql
/plugin marketplace search google-analytics
```

### Zapier (Connect Any Tool)

```bash
/plugin marketplace add zapier/mcp
```

### NotebookLM

```bash
/plugin marketplace add jacob-bd/notebooklm-mcp-cli
```

---

### The Two Rules for Tool Integrations

**Rule 1: Credentials are the only real security control.**
Instruction-based constraints (telling Claude Code not to do something) are helpful but not reliable. Always use the minimum-permission credential your tool supports.

**Rule 2: Start with one integration, not five.**
The most common reason PMs abandon MCP integrations is that they try to set up everything at once, get confused when one fails, and give up on all of them. Pick the one tool you use most — usually Jira or Figma — and use it for a week before adding another.

---

*Next: [08-real-stories.md](./08-real-stories.md) — The evidence base: PMs who are already doing this, what they built, and what they said about it.*
