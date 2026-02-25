# CLAUDE.md for Product Managers — From Minimal to Level 3

> CLAUDE.md is the highest-leverage 2 hours you will spend on Claude Code. This file shows you exactly what to write, level by level, with full copy-paste templates.
>
> Credit: patterns from [pm-studio (zoekdestep)](https://github.com/zoekdestep/pm-studio) and the Claude Code PM community.

---

## Table of Contents

1. [What CLAUDE.md Actually Does](#what-it-does)
2. [Level 1: Basic Context — 30 Minutes](#level-1)
3. [Level 2: Active Context — 1 Hour](#level-2)
4. [Level 3: Full Orchestration — 2 Hours](#level-3)
5. [The Level 3 Template](#level-3-template)
6. [Folder Structure That Works With Level 3](#folder-structure)
7. [Decision Log Template](#decision-log)
8. [Context Hygiene Rules](#context-hygiene)
9. [Team CLAUDE.md — Sharing With Engineering](#team-claude-md)
10. [Common CLAUDE.md Mistakes](#common-mistakes)

---

## What CLAUDE.md Actually Does {#what-it-does}

Claude Code reads your CLAUDE.md file automatically at the start of every session. It is loaded before your first message. You do not need to paste it, reference it, or remind Claude Code about it.

Think of it as briefing a consultant who:
- Never forgets what you told them
- Reads every brief fully before the meeting starts
- Has zero context from previous engagements unless you put it in writing
- Is equally productive on day 1 and day 300

Without CLAUDE.md, every Claude Code session starts from zero. You explain your product, your users, your priorities, your writing style — and then you do it again next session, and again the session after that.

With CLAUDE.md, every session starts at full context. Claude Code already knows your product, your users, your current quarter, your decisions, your anti-patterns, and how you want outputs formatted.

### What CLAUDE.md is not

- It is not a knowledge base — do not try to put everything in one file
- It is not a prompt — it is context
- It is not static — you update it as your product and priorities change
- It is not secret — commit it to your repository

### The loading sequence

When you run `claude` in a directory, Claude Code loads context in this order:

1. `~/.claude/CLAUDE.md` — your global personal context (applies to all projects)
2. `./CLAUDE.md` — the project CLAUDE.md in your current directory
3. Any CLAUDE.md files in subdirectories you navigate to

The project CLAUDE.md is the most important one. The rest of this guide focuses on it.

---

## Level 1: Basic Context — 30 Minutes {#level-1}

Level 1 is three sections: product vision, personas, and OKRs. It takes 30 minutes to write and immediately improves every session.

### Before Level 1

```
Session starts:
You: "Can you help me write a PRD for our new export feature?"
Claude Code: "I'd be happy to help! Could you tell me:
- What does your product do?
- Who are your users?
- What is the context for this feature?"

[You spend 5 minutes explaining your product]
[Next session: repeat]
```

### After Level 1

```
Session starts with CLAUDE.md loaded:
You: "Can you help me write a PRD for our new export feature?"
Claude Code: "Based on your product context — Acme Task Manager for
enterprise project managers — here is a PRD draft for the export feature,
including success metrics aligned with your Q2 activation OKR..."
```

### Level 1 Template

```markdown
# [Product Name] — PM Context

## Product Vision
[One sentence: what we are building and for whom]

Example: "Acme is a project management tool for enterprise operations teams
that replaces spreadsheet-based planning with structured, automated workflows."

We solve [core problem] for [core user] because [why now, why us].

## Core Users

### [Primary Persona Name]
- Role: [job title / function]
- Goal: [what they are trying to accomplish]
- Pain: [what frustrates them today]
- Success: [how they know things went well]

### [Secondary Persona Name]
- Role: [job title / function]
- Goal: [what they are trying to accomplish]
- Pain: [what frustrates them today]
- Success: [how they know things went well]

### [Tertiary Persona Name — often the buyer or admin]
- Role: [job title / function]
- Goal: [what they are trying to accomplish]
- Pain: [what frustrates them today]
- Success: [how they know things went well]

## Current Quarter (Q[X] [Year])

### OKRs
- **Objective 1**: [What we want to achieve]
  - KR 1.1: [Measurable result — include target number]
  - KR 1.2: [Measurable result — include target number]
- **Objective 2**: [What we want to achieve]
  - KR 2.1: [Measurable result]
  - KR 2.2: [Measurable result]

### Active Sprint Focus
[One sentence: what the team is working on right now]

### Biggest Open Question
[The one thing you are most uncertain about this quarter]
```

---

## Level 2: Active Context — 1 Hour {#level-2}

Level 2 adds three sections to Level 1: active decisions, competitive context, and writing voice. It takes an additional 30 minutes and makes a significant difference for strategic work.

### Before Level 2

```
You: "Compare our current approach to search with what Algolia offers"
Claude Code: [Provides a generic comparison based on public documentation]
```

### After Level 2

```
You: "Compare our current approach to search with what Algolia offers"
Claude Code: [Provides a comparison grounded in your specific implementation,
your user tier (SMB, not enterprise), and your pricing constraints,
framed in your team's direct, data-first communication style]
```

### Level 2 Template (add these sections to Level 1)

```markdown
## Active Decisions

These are decisions that have been made and should not be re-litigated
unless explicitly requested.

- **[Decision area]**: We are [choice]. Reason: [brief reason]. Date: [YYYY-MM-DD]
- **[Decision area]**: We are not doing [alternative] because [reason].
- **[Decision area]**: We are building [X] before [Y] because [sequencing reason].

Examples:
- **Mobile strategy**: We are building a progressive web app, not native iOS/Android.
  Reason: faster iteration, single codebase. Decided: 2025-10-15
- **Pricing model**: Staying with per-seat pricing. We evaluated usage-based and
  decided not to change this quarter. Revisit in Q3 2026.
- **Data storage**: We store in US region only until we have a data residency
  plan. Do not propose EU-first solutions.

## Competitive Context

### Primary competitors
- **[Competitor A]**: [One sentence on their position, their main strength vs. us,
  and where they are weak]
- **[Competitor B]**: [One sentence]
- **[Competitor C]**: [One sentence]

### Our differentiation
[2-3 sentences: what we do that competitors do not, and for whom this matters]

### What we do NOT compete on
[Features or price points we have explicitly decided not to chase]

## Writing Voice

When writing output for this product, Claude Code should:

**Do:**
- Write in second person ("you can", "your team") not third person
- Lead with the user benefit, not the feature
- Use specific numbers when available, not "significantly" or "many"
- Match a direct, low-jargon tone — our users are operations professionals,
  not software engineers
- Use active voice in all user-facing copy
- Keep sentences under 20 words when writing UI copy

**Do not:**
- Start with "Certainly!" or "Great question!" or similar filler
- Use passive voice in specifications ("it was determined" → "the team decided")
- Use product management jargon without definition (avoid "north star metric",
  "value proposition canvas", "jobs-to-be-done" unless the audience is PMs)
- Write bullet points longer than two lines
```

---

## Level 3: Full Orchestration — 2 Hours {#level-3}

Level 3 is where CLAUDE.md becomes a system, not a document. It adds:

- A context loading sequence that tells Claude Code what to read at the start of each session
- Agent role definitions with slash commands you define yourself
- Response format rules Claude Code follows every time
- Anti-patterns that prevent Claude Code from making mistakes you have seen before
- A session scope rule that keeps sessions focused

This is the version you commit to your team's repository.

### Before Level 3

```
You: "Let's do competitive research"
Claude Code: [Starts a broad research task with no clear output format,
saves nothing, and the next session has no memory of what was found]
```

### After Level 3

```
You: "/research competitor Algolia"
Claude Code: [Reads the competitive research template from context/,
scrapes Algolia's pricing and changelog pages via Bright Data MCP,
structures output in the standard competitive-analysis format,
saves to research/competitive/algolia-YYYY-MM-DD.md,
and appends a summary entry to decisions/competitive-log.md]
```

---

## The Level 3 Template {#level-3-template}

Copy and paste this entire file. Replace all `[bracketed]` fields.

```markdown
# [Product Name] — PM Context (Level 3)
# Last updated: [YYYY-MM-DD]

---

## Context Loading Sequence

At the start of every session, Claude Code loads context in this order.
If a file does not exist, note it and continue.

1. This file (CLAUDE.md) — current quarter, decisions, agent roles
2. context/product-context.md — product vision, personas, architecture overview
3. context/tone-and-voice.md — writing standards for all outputs
4. decisions/decision-log.md — all recorded decisions (last 10 entries)
5. [Optional] notes/current-sprint.md — active sprint context if it exists

Do not load files from research/ or specs/ at session start — they are
large and loaded on demand.

---

## Product Vision
[One sentence: what we are building and for whom]
[Three sentences: problem, solution, why us]

---

## Core Users

### [Persona 1 Name]
- Role: [job title]
- Goal: [primary goal in the product]
- Pain: [main frustration today]
- Success metric: [how they know it worked]

### [Persona 2 Name]
- Role: [job title]
- Goal: [primary goal]
- Pain: [main frustration]
- Success metric: [how they know it worked]

### [Persona 3 — Buyer / Admin]
- Role: [job title]
- Goal: [what they care about — usually cost, compliance, or control]
- Pain: [main frustration]
- Success metric: [how they know it worked]

---

## Current Quarter (Q[X] [Year])

### OKRs
- **Objective 1**: [Objective statement]
  - KR 1.1: [Target — include number]
  - KR 1.2: [Target — include number]
- **Objective 2**: [Objective statement]
  - KR 2.1: [Target]
  - KR 2.2: [Target]

### Active Sprint Focus
[One sentence on current sprint]

### Biggest Open Question
[The single most important uncertainty this quarter]

### Out of Scope This Quarter
- [Thing we are explicitly NOT doing, with brief reason]
- [Thing we are explicitly NOT doing]
- [Thing we are explicitly NOT doing]

---

## Active Decisions

Decisions that have been made. Do not propose reversals unless the context
section of this prompt explicitly requests reconsideration.

| Decision | Choice | Reason | Date |
|---|---|---|---|
| [Area] | [What was decided] | [Why] | [YYYY-MM-DD] |
| [Area] | [What was decided] | [Why] | [YYYY-MM-DD] |
| [Area] | [What was decided] | [Why] | [YYYY-MM-DD] |

---

## Competitive Context

### Primary Competitors
- **[Competitor A]**: [Position, main strength vs. us, main weakness]
- **[Competitor B]**: [Position, main strength vs. us, main weakness]

### Our Differentiation
[2-3 sentences on what we do that competitors do not]

### Do Not Compete On
- [Feature or price point we have explicitly decided not to chase]
- [Feature or price point we have explicitly decided not to chase]

---

## Agent Roles

These slash commands define focused work modes. Invoke them by typing
the command at the start of a message.

### /research [topic or competitor]
**Role:** Competitive intelligence and market research analyst
**When invoked:**
- Search for public information on the topic using available tools
  (Bright Data MCP if available, otherwise training data)
- Structure output in the standard competitive analysis format
- Save to research/competitive/[topic]-[YYYY-MM-DD].md
- Append a 2-sentence summary to research/competitive/index.md
- Do not editorialize — present facts and flag interpretation separately

**Output format:**
```
## [Topic] Research — [Date]
### Summary (3 sentences max)
### Key Facts
### Compared to Our Product
### Open Questions
### Sources
```

### /spec [feature name]
**Role:** Technical specification writer
**When invoked:**
- Load context/prd-template.md if it exists
- Generate a complete PRD using the loaded context from CLAUDE.md
- Ask one clarifying question before starting if the feature name is ambiguous
- Save to specs/[feature-slug]-[YYYY-MM-DD].md
- Do not include implementation details unless the engineering context
  files have been explicitly loaded

**Output must include:**
- Problem statement (with user quote or data point if available)
- User stories (3-5, in As/I Want/So That format)
- Acceptance criteria (Given/When/Then format)
- Success metrics (primary + 2 guardrails)
- Open questions (at least 3)
- Out of scope (at least 2 items)

### /data [question]
**Role:** Data analyst
**When invoked:**
- Query connected analytics tools (Amplitude, Mixpanel, or Metabase MCP
  if available)
- If no analytics MCP is connected, analyze data files in the notes/ folder
- Present findings with specific numbers, not qualitative descriptions
- Always state the time range, sample size, and any caveats
- Suggest one follow-up query after answering the question

### /devil [spec or proposal]
**Role:** Devil's advocate reviewer
**When invoked:**
- Read the spec or proposal (either pasted or from a file path)
- Identify the 3 strongest arguments AGAINST this proposal
- Identify the 3 biggest assumptions that must be true for this to succeed
- Identify 2 alternative approaches not considered in the spec
- Do not be diplomatic — the point is to surface what the team might
  miss in enthusiasm for the idea
- Do not suggest the proposal is good or bad overall — surface the
  questions and let the PM decide

### /review [spec file path]
**Role:** Peer PM reviewer
**When invoked:**
- Read the spec file
- Evaluate against five criteria:
  1. Problem clarity: Is the problem statement specific and evidence-backed?
  2. User centricity: Does every feature trace back to a user need?
  3. Measurability: Are success metrics specific and achievable?
  4. Scope discipline: Is there a clear out-of-scope section?
  5. Stakeholder readiness: Are open questions surfaced for team discussion?
- Rate each criterion 1-3 (1=needs work, 2=acceptable, 3=strong)
- Output a scorecard with one specific improvement suggestion per low score

### /decision [decision description]
**Role:** Decision recorder
**When invoked:**
- Format the decision using the standard decision log template
- Append to decisions/decision-log.md
- Also update the Active Decisions table in this CLAUDE.md
- Remind the user if this decision contradicts an existing decision
  in the log

---

## Response Format Rules

These rules apply to ALL outputs unless explicitly overridden.

**Language:**
- No filler phrases: do not start with "Certainly!", "Great question!",
  "Of course!", "Sure!", "Absolutely!", or any similar affirmation
- Start with the substance of the answer, not a preamble
- Use active voice: "The team decided" not "It was decided by the team"
- Use second person for user-facing copy: "You can export" not
  "Users can export"

**Structure:**
- Use markdown headers for anything over 200 words
- Tables for any comparison with 3+ options
- Code blocks for any file paths, commands, or code snippets
- Bold for key decisions and must-dos
- Never nest bullet points more than 2 levels deep

**Length:**
- Match length to request: a question → a paragraph; a spec → full document
- Do not pad short answers with additional context not requested
- For analysis: lead with the conclusion, then the evidence
  (not the other way around)

**File saving:**
- When asked to "write" or "create" a document, always save it to a file
- Confirm the file path after saving
- Use kebab-case for file names: feature-export-prd.md not FeatureExportPRD.md
- Include the date in file names for research and decisions: topic-YYYY-MM-DD.md

---

## Anti-Patterns

These are patterns Claude Code should avoid in all sessions.

**Do not:**
1. Propose reversing an Active Decision without being explicitly asked
2. Suggest enterprise-only features without first noting we are an SMB product
3. Write specs without success metrics — incomplete specs create ambiguity
4. Use competitor names in user-facing copy drafts
5. Propose a feature without identifying which persona it serves
6. Write OKRs that are activities ("we will build X") instead of outcomes
   ("X% of users will complete Y")
7. Suggest removing features that existing customers depend on without
   noting the migration requirement
8. Use "we should consider" or "it might be worth exploring" — be direct
9. Leave open questions unanswered in a spec if the answer is knowable
   from the codebase or existing documentation
10. Create files in the root directory — always use the correct subfolder

---

## Session Scope Rule

One session = one task. Before starting any session, the PM states the
session goal in one sentence. Claude Code confirms the goal and does not
deviate from it without explicit redirect.

If a session reveals a second important task, Claude Code notes it
("I noticed X, which seems important — want to address it now or
note it for a separate session?") and continues with the original task.

Good session scopes:
- "Write a PRD for the bulk export feature"
- "Research Notion's new AI features for competitive context"
- "Review the onboarding spec draft at specs/onboarding-v2.md"
- "Analyze why activation dropped in week 3 of Q1"

Bad session scopes:
- "Help me with roadmap stuff"
- "Let's talk about Q2"
- "What should we build next?"
```

---

## Folder Structure That Works With Level 3 {#folder-structure}

This folder structure is the companion to the Level 3 CLAUDE.md. It keeps stable context separated from working memory and outputs.

```
your-product-repo/
├── CLAUDE.md                    ← The main file. Commit to git.
│
├── context/                     ← Stable product information
│   ├── product-context.md       ← Product vision, history, architecture overview
│   ├── personas.md              ← Detailed persona profiles with research citations
│   ├── tone-and-voice.md        ← Full writing standards with examples
│   └── prd-template.md          ← Your team's PRD template (used by /spec)
│
├── notes/                       ← Working memory (do not commit to git)
│   ├── current-sprint.md        ← What the team is working on right now
│   └── open-questions.md        ← Parking lot for questions across sessions
│
├── research/                    ← Session research outputs
│   ├── competitive/
│   │   ├── index.md             ← Running log of all competitive research
│   │   ├── algolia-2026-01-15.md
│   │   └── notion-2026-02-01.md
│   └── user-pain/
│       ├── index.md
│       └── onboarding-interviews-2026-01.md
│
├── specs/                       ← Deliverable PRDs and feature specs
│   ├── bulk-export-2026-02-10.md
│   ├── smart-onboarding-2026-01-20.md
│   └── api-v2-2026-01-05.md
│
└── decisions/
    └── decision-log.md          ← Every recorded decision (template below)
```

### What to commit to git

```
# Commit these — shared team context
CLAUDE.md
context/
specs/
decisions/

# Do NOT commit these — personal working memory
notes/
research/          ← Optional: commit if your team shares competitive research
```

### Setting up the folder structure

```bash
mkdir -p context notes research/competitive research/user-pain specs decisions

# Create placeholder files so the folders exist in git
touch context/product-context.md
touch context/personas.md
touch context/tone-and-voice.md
touch context/prd-template.md
touch decisions/decision-log.md

# Add notes to .gitignore
echo "notes/" >> .gitignore
```

---

## Decision Log Template {#decision-log}

Copy this into `decisions/decision-log.md` and use the `/decision` agent role to append to it.

```markdown
# Decision Log — [Product Name]

> This log records significant product decisions: what was decided, why,
> who decided it, and what the alternatives were.
>
> Updated by: Claude Code's /decision agent role
> Format: newest first

---

## [YYYY-MM-DD] — [Decision Title]

**Status:** Decided / Under Review / Superseded

**Context:**
[1-3 sentences: what situation prompted this decision]

**Decision:**
[One sentence: exactly what was decided]

**Rationale:**
- [Reason 1]
- [Reason 2]
- [Reason 3]

**Alternatives Considered:**
| Alternative | Why Rejected |
|---|---|
| [Option A] | [Reason] |
| [Option B] | [Reason] |

**Implications:**
- [What this means for the product / roadmap / team]
- [Any follow-on work this requires]

**Who Decided:** [Name or "PM team"]

**Revisit Condition:** [Under what conditions should this decision be reconsidered]

---

## [YYYY-MM-DD] — [Decision Title]

[Repeat format above]

---

<!-- Template ends. Newest entries go at the top. -->
```

### How to use the /decision command

```
> /decision We are building a public API before a mobile app.
  Reason: enterprise customers have asked for API integration in
  6 of our last 10 renewal calls. Mobile was on the roadmap but
  has no direct customer request backing it.
  Alternatives: mobile-first (rejected: no evidence of demand),
  both simultaneously (rejected: team capacity).
```

Claude Code will format this using the template and append it to decisions/decision-log.md.

---

## Context Hygiene Rules {#context-hygiene}

Three rules that prevent the most common CLAUDE.md degradation problems.

### Rule 1: Load at Start, Not Mid-Session

Load your context files at the beginning of a session. Do not try to load a large context file mid-session to "fix" a session that has gone sideways.

**Why:** Claude Code's context window is finite. Loading large files mid-session pushes earlier conversation history out of context. If you need significantly different context, start a new session.

```
# Good — at session start
> Load CLAUDE.md and confirm context, then we will work on the onboarding spec

# Bad — mid-session context reload
> [After 20 minutes of work] Actually, load the full competitive research
  context now too
```

### Rule 2: Scope Sessions to Tasks, Not Topics

A session scoped to "help me with Q2 planning" will meander. A session scoped to "draft the Q2 roadmap slide for the board presentation, save to specs/q2-board-roadmap.md" will produce a finished artifact.

**Practical test:** Can you write the session goal on one line and circle back in 60 minutes with a specific output to show for it? If not, narrow the scope.

| Too broad (bad) | Scoped (good) |
|---|---|
| "Help with roadmap" | "Write the Q2 roadmap narrative for the company all-hands" |
| "Analyze user research" | "Extract the top 5 themes from the 8 interviews in notes/jan-interviews.md" |
| "Work on the spec" | "Review specs/export-feature.md and generate the acceptance criteria section" |
| "Competitive research" | "Research how Figma handles their free-to-paid upgrade flow, save to research/competitive/figma-upgrade-2026-02.md" |

### Rule 3: Persist Decisions, Not Reasoning

The decision log is for decisions. Not for the reasoning thread that led to them, not for options that were considered and rejected in conversation, not for tentative positions.

When you make a decision in a Claude Code session, use `/decision` to record it immediately. Do not rely on session history — it disappears when the session ends.

**Pattern to build the habit:**

```
# At the end of any session where a decision was reached:
> Before we close: we decided to [X]. Use /decision to log this.

# Claude Code will format and save it
```

---

## Team CLAUDE.md — Sharing With Engineering {#team-claude-md}

CLAUDE.md is not just for PMs. When your team commits a shared CLAUDE.md to the repository, every engineer who runs `claude` in that repo gets the same product context automatically.

### What to include in a team CLAUDE.md

**Keep in the team file (valuable for everyone):**
- Product vision and core users
- Current quarter OKRs
- Active technical decisions (especially architectural constraints)
- Anti-patterns relevant to the codebase
- Links to documentation files (context/product-context.md, etc.)

**Move to your personal `~/.claude/CLAUDE.md` (PM-specific, not for team):**
- Your personal writing voice preferences
- Your personal agent role definitions
- Any client-specific or confidential context
- Your session scope preferences

### Template: Minimal team CLAUDE.md

```markdown
# [Product Name] — Shared Team Context

## What We Are Building
[One sentence product vision]

## Core Users
- **[Persona 1]**: [One sentence description]
- **[Persona 2]**: [One sentence description]

## Current Quarter (Q[X] [Year])
- Focus: [One sentence on quarter theme]
- Key OKR: [The single most important outcome]

## Active Technical Decisions
- **[Decision]**: [What was decided and why — brief]
- **[Decision]**: [What was decided and why — brief]

## Where Things Live
- PRDs and specs: /specs
- Decision history: /decisions/decision-log.md
- Product context: /context/product-context.md
- PM contact: [name] — [slack handle]

## Do Not Change Without Discussion
- [Architecture constraint]
- [Data model constraint]
- [Integration constraint]
```

### Committing to git

```bash
# Check that sensitive data is not in CLAUDE.md before committing
# (API keys, passwords, customer data — these should never be in CLAUDE.md)
cat CLAUDE.md | grep -i "token\|key\|password\|secret"

# If nothing sensitive appears, commit it
git add CLAUDE.md context/
git commit -m "Add PM context for Claude Code sessions"
git push
```

### Onboarding new team members

When a new engineer or PM joins, the CLAUDE.md onboarding is simple:

```bash
# Clone the repo and run claude
git clone [repo]
cd [repo]
claude

# On first message:
> Load the CLAUDE.md and tell me what this product does, who uses it,
  and what we are focused on this quarter.
```

This replaces a portion of the traditional "read the wiki" onboarding.

---

## Common CLAUDE.md Mistakes {#common-mistakes}

### Mistake 1: Too Long

**Symptom:** Your CLAUDE.md is 800+ lines. Sessions start slow. Claude Code seems to "forget" context from earlier in a session.

**Why it happens:** It feels productive to put everything in CLAUDE.md. The more context, the better, right?

**What actually happens:** A very large CLAUDE.md consumes most of the context window before you even send your first message. Important details get pushed out as the session grows.

**Fix:** CLAUDE.md should be under 400 lines. Move detailed content to `context/` files and reference them in the Context Loading Sequence. Claude Code loads them on demand.

```markdown
# Good pattern — thin CLAUDE.md, fat context files
## Context Loading Sequence
1. This file
2. context/product-context.md   ← detailed vision, history, architecture
3. context/personas.md          ← full persona profiles

# Bad pattern — everything in CLAUDE.md
## Product Vision
[5 pages of product history and vision]
## Persona 1
[3 pages of persona research]
```

---

### Mistake 2: Too Generic

**Symptom:** Every output from Claude Code sounds like it was written by a generic PM assistant, not someone who knows your specific product.

**Why it happens:** The CLAUDE.md was filled in with template placeholder text that was never customized.

**Fix:** Replace every generic statement with something specific to your product. The test: could this sentence describe any SaaS product, or is it specific to yours?

```markdown
# Generic (bad)
## Product Vision
We build software that helps users be more productive.

# Specific (good)
## Product Vision
Acme replaces the 47-tab spreadsheet that enterprise operations managers
use to track cross-department project status. Our users are the one person
in a 200-person company who manually emails 12 team leads every Friday
to compile a weekly status report.
```

---

### Mistake 3: No Loading Sequence

**Symptom:** You have 4 context files in a `context/` folder but Claude Code never reads them unless you explicitly ask.

**Why it happens:** Claude Code only reads files you reference in CLAUDE.md or ask it to read explicitly.

**Fix:** Add a Context Loading Sequence section that tells Claude Code exactly which files to read at the start of every session.

```markdown
## Context Loading Sequence
At the start of every session, read these files in order:
1. context/product-context.md
2. context/personas.md
3. decisions/decision-log.md (last 10 entries only)
```

---

### Mistake 4: No Anti-Patterns

**Symptom:** Claude Code keeps making the same mistake — proposing features that conflict with a technical constraint, using a tone that does not match your brand, or generating specs without success metrics.

**Why it happens:** Without anti-patterns, Claude Code applies its defaults. Its defaults are reasonable for most products but not for yours specifically.

**Fix:** Every time Claude Code makes a mistake you have to correct, add it to the Anti-Patterns section.

```markdown
## Anti-Patterns (build this list over time)

First session:
- Do not start responses with "Certainly!" or "Great question!"

After you notice Claude Code proposing things that do not fit:
- Do not propose features for enterprise customers — we are SMB-only
- Do not suggest native mobile app development — we are web-only this year
- Do not write specs without a "Success Metrics" section

After a specific bad output:
- Do not suggest removing the CSV export feature — 40% of our users depend on it
- Do not propose freemium pricing models — we have explicitly decided against free tier
```

---

### Mistake 5: No Agent Roles

**Symptom:** Every session feels like starting from scratch. You use Claude Code for research, spec writing, review, and competitive analysis — but each time you have to explain the output format, the file naming convention, and what "good" looks like.

**Why it happens:** Without agent roles, Claude Code has no definition of what "do competitive research" means for your product.

**Fix:** Define 3-5 agent roles as slash commands. Each role defines: what to do, what to output, where to save it, and what format to use. Do this once and every session of that type becomes repeatable.

Start with the three you use most often. Common choices:
- `/spec [feature]` — because spec writing is the most common PM task
- `/research [topic]` — because competitive research should follow a consistent format
- `/devil [file]` — because every spec should have a challenger review before it goes to the team

See the Level 3 template above for full definitions of all 6 built-in agent roles.

---

## Summary: Which Level Should You Start With?

| If you... | Start with |
|---|---|
| Have never used Claude Code before | Level 1 — write it today |
| Use Claude Code a few times a week for ad-hoc questions | Level 1 |
| Use Claude Code daily for spec writing and research | Level 2 |
| Want Claude Code to work like a dedicated PM assistant | Level 3 |
| Are setting up a shared context for your whole team | Level 3 (team version) |

**The best CLAUDE.md is the one you actually maintain.** Start with Level 1 and upgrade it as your sessions reveal what is missing. A Level 1 CLAUDE.md you update weekly is more valuable than a Level 3 CLAUDE.md written once and forgotten.

---

*Back to: [01-getting-started.md](./01-getting-started.md) | [02-mcp-toolkit.md](./02-mcp-toolkit.md)*
