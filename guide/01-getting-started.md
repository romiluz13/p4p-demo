# Getting Started — Installation, First Session, Plan Mode

> A practical guide for Product Managers using Claude Code. No engineering background required.

---

## Table of Contents

1. [Why Claude Code is Different from Claude.ai](#why-different)
2. [Installation](#installation)
3. [Authentication](#authentication)
4. [First 10 Minutes](#first-10-minutes)
5. [Plan Mode — Safe Codebase Exploration](#plan-mode)
6. [CLAUDE.md Basics](#claudemd-basics)
7. [Your First PM Session](#your-first-pm-session)
8. [Common First-Week Mistakes](#common-first-week-mistakes)
9. [Claude Code 2.0 Features for PMs](#claude-code-20-features)

---

## Why Claude Code is Different from Claude.ai {#why-different}

Claude.ai is a chat interface. Claude Code is an **agentic coding environment** that runs in your terminal and has direct access to your machine. For PMs, this difference is everything.

| Capability | Claude.ai | Claude Code |
|---|---|---|
| Read your codebase | No — you paste snippets | Yes — reads any file you point it at |
| Persistent context | No — new chat = blank slate | Yes — CLAUDE.md loads automatically every session |
| Write files | No | Yes — creates PRDs, specs, tickets directly |
| Run commands | No | Yes — can run git, search tools, APIs |
| Connect to Jira / Figma / Notion | No | Yes — via MCP servers |
| Plan Mode (read-only) | No | Yes — explore without changing anything |
| Parallel agents | No | Yes — multiple tasks simultaneously |

### The Four PM Superpowers

**1. File system access**
Claude Code can read every file in your repository — changelogs, architecture docs, test files, past PRDs, API specs. Ask it "what changed in the last 3 sprints?" and it will actually read the git history.

**2. Persistent context via CLAUDE.md**
Instead of pasting your product context at the start of every chat, you write it once in a file called `CLAUDE.md`. Claude Code reads it automatically every session. It is the equivalent of briefing a consultant who never forgets and never leaves.

**3. Tool integrations (MCP servers)**
Claude Code connects to external tools through a protocol called MCP. You install a Jira MCP and Claude Code can read and write Jira tickets. You install a Figma MCP and it can read your design files. See `02-mcp-toolkit.md` for the full list.

**4. Plan Mode**
Before Claude Code does anything to your files, you can enter Plan Mode. In this mode, Claude Code reads everything but writes nothing. It is the safest way for PMs to explore a codebase without risking any changes.

---

## Installation {#installation}

### Prerequisites

- Node.js 18 or higher (check with `node --version`)
- A terminal application (Terminal on macOS, Windows Terminal, any Linux terminal)
- 5 minutes

### macOS

**Option A — Homebrew (recommended)**
```bash
brew install claude
```

**Option B — npm**
```bash
npm install -g @anthropic-ai/claude-code
```

**Verify installation**
```bash
claude --version
```

### Windows

**Option A — npm (recommended)**

Open PowerShell or Windows Terminal, then:
```powershell
npm install -g @anthropic-ai/claude-code
```

**Option B — Direct download**

Download the installer from [claude.ai/code](https://claude.ai/code) and run it.

**Verify installation**
```powershell
claude --version
```

### Linux

**Option A — npm**
```bash
npm install -g @anthropic-ai/claude-code
```

**Option B — Snap (Ubuntu/Debian)**
```bash
sudo snap install claude-code
```

**Verify installation**
```bash
claude --version
```

---

## Authentication {#authentication}

You need one of two things: an Anthropic API key, or a Claude.ai Pro/Team/Enterprise subscription.

### Option A — Claude.ai Subscription (Easier)

If you already have Claude Pro ($20/mo), Team, or Enterprise:

```bash
claude
```

On first run, Claude Code will open your browser and ask you to log in with your Claude.ai account. Click "Authorize" and you're done.

### Option B — Anthropic API Key

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Click "API Keys" in the left sidebar
3. Click "Create Key", name it "claude-code", copy the key
4. Set it in your terminal:

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

To make this permanent (macOS/Linux):
```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-..."' >> ~/.zshrc
source ~/.zshrc
```

For Windows PowerShell:
```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "sk-ant-...", "User")
```

**Which option should PMs choose?** If your company has a Claude Team or Enterprise subscription, use Option A — it is already paid for and the billing is consolidated. Use Option B only if you need a personal API key for personal projects.

---

## First 10 Minutes {#first-10-minutes}

### Minute 1-2: Start Claude Code

Navigate to a project folder (your product's repository, or any folder with documents you want to work with):

```bash
cd ~/Projects/my-product
claude
```

You will see:
```
Claude Code v2.x.x
Type /help for commands, or start typing to chat.
>
```

### Minute 3-4: Ask Something Safe

Start with a question that does not require Claude Code to write anything:

```
> What is this project?
```

Claude Code will read your repository and describe what it finds. If you are in an empty folder, it will say so.

### Minute 5-6: Explore in Plan Mode

```
> /plan
```

Now type:
```
> What are the main user flows in this product?
```

Claude Code will read your code and documentation without changing anything. Press `Escape` or type `/exit` to leave Plan Mode.

### Minute 7-8: Create Your First File

```
> Create a CLAUDE.md file with basic product context for this project
```

Claude Code will generate a starter CLAUDE.md. You will edit it later (see the CLAUDE.md section below).

### Minute 9-10: Save Output

```
> Write a one-page summary of what this codebase does, save it to docs/codebase-summary.md
```

Claude Code will create the file. Check it exists:
```bash
ls docs/
cat docs/codebase-summary.md
```

---

## Plan Mode — Safe Codebase Exploration {#plan-mode}

Plan Mode is the safest way for PMs to interact with a codebase. In Plan Mode, Claude Code can read files, search the codebase, and reason about architecture — but it cannot write, edit, or delete any files.

### How to Enter Plan Mode

**Method 1 — Command**
```bash
claude --plan
```

**Method 2 — During a session**
```
> /plan
```

**Method 3 — Keyboard shortcut**
Press `Shift+Tab` during any session to toggle Plan Mode on and off.

You will see a `[PLAN]` indicator in the prompt when Plan Mode is active.

### How to Exit Plan Mode

```
> /exit-plan
```
or press `Escape`.

### 5 Questions PMs Ask in Plan Mode

These are questions where you want analysis, not changes. Plan Mode is the right setting for all of them.

**Question 1: Understand architecture before writing a spec**
```
> Walk me through the architecture of the checkout flow. What services are involved,
  what are the main data models, and where are the integration points with third-party systems?
```

**Question 2: Find what changed in a sprint**
```
> Look at the git history for the last two weeks. What features were shipped?
  Were there any hotfixes? Summarize in plain language for a non-technical audience.
```

**Question 3: Assess technical debt before planning**
```
> Identify the three areas of the codebase with the most TODO comments and
  technical debt markers. What is the estimated impact if we do not address them
  in the next quarter?
```

**Question 4: Understand test coverage before a launch**
```
> What is the test coverage for the payment module?
  Are there any critical user paths that have no automated tests?
```

**Question 5: Reverse-engineer a feature**
```
> I need to write a spec for a new notification system.
  Can you describe how the existing notification system works,
  what it supports, and what would need to change to support push notifications?
```

---

## CLAUDE.md Basics {#claudemd-basics}

CLAUDE.md is a plain text file that Claude Code reads automatically at the start of every session. It is how you give Claude Code permanent context about your product without pasting it every time.

### Where It Lives

Claude Code looks for CLAUDE.md in this order:
1. `~/.claude/CLAUDE.md` — your global personal context (applies to all projects)
2. `./CLAUDE.md` — project-level context (the one you commit to the repo)
3. Any nested CLAUDE.md files in subdirectories (for monorepos)

For PMs, the most useful file is the **project-level** `./CLAUDE.md` in your product repository.

### Creating Your First CLAUDE.md

```bash
touch CLAUDE.md
```

Then open it in any text editor. Here is the minimum viable version — three sections that take 30 minutes to write and will save you hours every week:

```markdown
# [Product Name] — PM Context

## Product Vision
[One sentence: what we are building and for whom]
[Three sentences: what problem we solve and why we are the right solution]

## Core Users
- **[Persona 1]**: [Who they are, what they need, what frustrates them]
- **[Persona 2]**: [Who they are, what they need, what frustrates them]
- **[Persona 3]**: [Who they are, what they need, what frustrates them]

## Current Quarter (Q[X] [Year])
- OKR 1: [Objective — Key Result]
- OKR 2: [Objective — Key Result]
- OKR 3: [Objective — Key Result]
- Active sprint focus: [one line]
- Biggest open question: [one line]
```

This is enough to get started. The full Level 1 / Level 2 / Level 3 CLAUDE.md template is in `03-claude-md-for-pms.md`.

### What CLAUDE.md Does NOT Do

- It does not run code or execute commands
- It does not automatically update when your product changes — you update it
- It does not replace documentation — it supplements it
- It is not a secret — commit it to your repo and share it with your team

---

## Your First PM Session {#your-first-pm-session}

Here is a complete example session you can run today. It assumes you have a repository and a basic CLAUDE.md.

### Step 1: Start in the right directory

```bash
cd ~/Projects/my-product
```

### Step 2: Launch Claude Code

```bash
claude
```

### Step 3: Load context explicitly (optional but recommended)

```
> Load CLAUDE.md and confirm you have the product context
```

Claude Code will read the file and confirm what it understood. This is your chance to catch errors before you invest time in a session.

### Step 4: Ask your real question

```
> I need to write a one-page brief for the search improvements we discussed in
  planning. Based on the current search implementation in the codebase,
  what are the three biggest gaps between what we have and what a modern
  search experience looks like? Format the output as a problem statement
  with supporting evidence from the code.
```

### Step 5: Iterate

```
> Good. Now format this as a PRD executive summary — three paragraphs:
  problem, proposed solution, success metrics. Write it to specs/search-v2-brief.md
```

### Step 6: Check the output

```bash
cat specs/search-v2-brief.md
```

### Step 7: Save and exit

```
> /exit
```

The file is now on disk. You can open it in any editor, share it, or commit it to git.

### Full Example Transcript

```
$ cd ~/Projects/acme-app
$ claude

Claude Code v2.x.x
> Load CLAUDE.md and confirm you have the product context.

I've read CLAUDE.md. Here's what I understand:
- Product: Acme Task Manager for enterprise teams
- Core users: Project managers, individual contributors, team leads
- Q1 focus: Reduce time-to-first-task for new users (onboarding OKR)

> We are planning a "smart onboarding" feature for Q2. What does the current
  onboarding flow look like technically, and what are the three biggest
  friction points from an engineering standpoint?

[Claude Code reads onboarding-related files, auth flow, user model...]

The current onboarding flow has three steps:
1. Email verification (email_verification.ts) — no friction here
2. Team setup (team_setup_wizard.tsx) — requires 4 form fields before seeing
   any value; this is the biggest drop-off risk
3. First task creation (task_create_modal.tsx) — blocked behind team setup
   completion

Technical friction points:
1. Team setup is blocking — users cannot create a task until they complete
   team setup, even if they are solo users
2. No progressive disclosure — all onboarding fields shown at once
3. No "skip for now" escape hatch — confirmed by searching for skip/skip_onboarding
   across the codebase (zero results)

> Write this up as a one-page brief with recommended solutions.
  Save to specs/smart-onboarding-brief.md

[Creates file...]

I've saved the brief to specs/smart-onboarding-brief.md. It includes:
- Current state analysis with code references
- Three friction points with severity ratings
- Recommended solutions for each
- Suggested success metrics
```

---

## Common First-Week Mistakes {#common-first-week-mistakes}

### Mistake 1: Running Claude Code outside your project directory

**Symptom:** Claude Code does not know anything about your product. Answers are generic.

**Fix:** Always `cd` to your project directory before running `claude`. Claude Code reads files relative to where it is launched.

```bash
# Wrong — generic answers
cd ~
claude

# Right — context-aware answers
cd ~/Projects/my-product
claude
```

### Mistake 2: Not writing a CLAUDE.md before asking product questions

**Symptom:** You spend the first 5 minutes of every session explaining your product again.

**Fix:** Write a minimum viable CLAUDE.md (the three-section version above) before your first real session. 30 minutes of writing saves hours of re-explaining.

```bash
# Create it before your first session
echo "# My Product" > CLAUDE.md
# Then open it and fill in the three sections
```

### Mistake 3: Asking Claude Code to edit production files without reviewing first

**Symptom:** Claude Code helpfully edits a configuration file and breaks something.

**Fix:** Use Plan Mode for exploration. Ask Claude Code to write outputs to a `drafts/` or `specs/` folder, not directly to production files. Review before accepting.

```
# Safe pattern
> Analyze the API rate limiting config. Do not change anything.
  Write your recommendations to docs/rate-limit-analysis.md
```

### Mistake 4: Sessions that are too broad

**Symptom:** You spend an hour with Claude Code jumping between six different topics. Nothing gets finished.

**Fix:** One session = one task. Keep sessions focused. Use the CLAUDE.md "Session Scope Rule" from `03-claude-md-for-pms.md`.

```
# Unfocused (bad)
> Help me with my roadmap, write a PRD, and also analyze the competition

# Focused (good)
> Today I need one thing: a competitive analysis of our search feature
  vs Algolia, Elastic, and Typesense. Output to research/search-competitive.md
```

### Mistake 5: Treating Claude Code output as final

**Symptom:** You paste Claude Code's PRD draft directly into your wiki without reading it.

**Fix:** Claude Code output is a high-quality first draft, not a finished document. Always read it. It may reference files or make assumptions that need your judgment to validate.

A good habit: ask Claude Code to flag its own assumptions.

```
> Write the PRD. At the end, add a section called "Assumptions I Made"
  that lists every assumption you made that a human should validate.
```

---

## Claude Code 2.0 Features for PMs {#claude-code-20-features}

### Checkpoints

Claude Code 2.0 introduced checkpoints — snapshots of the state of your files before Claude Code makes changes. If a session goes sideways, you can roll back to the last checkpoint.

**How to use it:**
```bash
# List checkpoints
claude checkpoints list

# Restore a checkpoint
claude checkpoints restore <checkpoint-id>
```

**PM use case:** You asked Claude Code to refactor a set of spec files and do not like the result. Roll back to the checkpoint before the refactor and try a different approach.

### IDE Extension (VS Code — No Terminal Needed)

Claude Code now has a VS Code extension, which means you do not need to use the terminal at all. Install it from the VS Code marketplace:

1. Open VS Code
2. Go to Extensions (`Cmd+Shift+X`)
3. Search for "Claude Code"
4. Install and sign in

**PM use case:** You are already in VS Code editing a spec or a markdown file. You can run Claude Code commands from a sidebar without switching to the terminal. This is the lowest-friction entry point for PMs who are not comfortable in the terminal.

### Parallel Agents

Claude Code 2.0 supports running multiple agents simultaneously. You can kick off a competitive research task and a PRD draft at the same time, and both complete in parallel.

**How to use it (terminal)**
```bash
# Start two agents in parallel
claude --agent "research competitive landscape and save to research/competitive.md" &
claude --agent "draft PRD skeleton based on CLAUDE.md and save to specs/prd-draft.md" &
wait
```

**PM use case:** Use parallel agents at the start of a planning cycle — run research, discovery synthesis, and template scaffolding simultaneously instead of sequentially.

### Automation Hooks

Automation hooks let you trigger Claude Code actions automatically on specific events — for example, every time a new GitHub issue is labeled "needs-spec", Claude Code runs and creates a draft spec.

**Example hook configuration (`.claude/hooks.yaml`):**
```yaml
hooks:
  - name: auto-spec-on-label
    trigger: github_issue_labeled
    label: needs-spec
    action: |
      Read the issue title and body.
      Using CLAUDE.md for context, create a draft spec in specs/issues/{issue_number}.md.
      The spec should include: problem statement, proposed solution,
      acceptance criteria, and open questions.
```

**PM use case:** Remove the blank-page problem for spec writing. Every new ticket that gets labeled gets an automatic starter spec. You edit instead of starting from scratch.

---

## Quick Reference

```bash
# Start a session
claude

# Start in Plan Mode (read-only)
claude --plan

# Start on a specific file
claude README.md

# Run a one-off command without interactive session
claude -p "Summarize the main features of this codebase in 5 bullet points"

# Check version
claude --version

# Get help
claude /help
```

**In-session shortcuts:**
- `Shift+Tab` — toggle Plan Mode
- `/plan` — enter Plan Mode
- `/exit` — exit current mode or session
- `/help` — show all available commands
- `Ctrl+C` — interrupt current operation

---

*Next: [02-mcp-toolkit.md](./02-mcp-toolkit.md) — Connect Claude Code to Jira, Figma, Notion, and every other tool PMs use daily.*
