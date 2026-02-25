# Skills, Plugins & PM Agents — The Part Everyone Misses

> CLAUDE.md is not the only way. Skills, plugins, and agents are how you
> turn Claude Code into a specialist that actually knows product management.
> This is what the talk is mostly about.

---

## The Mental Model First

| Mechanism | What it does | When it activates | Where it lives |
|-----------|-------------|-------------------|----------------|
| **CLAUDE.md** | Always-on project context (your product, your voice) | Every session, automatically | `CLAUDE.md` in project root |
| **Skill** | Teaches Claude a specific PM framework or workflow | Ambient (auto) or commanded (explicit) | `.claude/skills/*.md` |
| **Plugin** | Bundle: skills + commands + tool connectors | On install, persists | Plugin marketplace |
| **Agent** | Specialist with a persona, role, and memory | You invoke it | `.claude/agents/*.md` |
| **MCP** | Live connection to Jira, Figma, Amplitude, etc. | When active | Config JSON |

**Why skills matter more than CLAUDE.md for PM craft:**
CLAUDE.md tells Claude about *your product*. Skills teach Claude *how to do PM work* — the frameworks, methodologies, and workflows from the best product leaders. You need both, but for the PM discipline itself, skills carry more weight.

**How skills load:** Claude reads only ~100 tokens of each skill's description to decide relevance, then loads full instructions only when needed. Context window stays lean. You can have 50 skills installed and they don't slow things down.

---

## The Official Anthropic PM Plugin — What It Actually Does

**Repo:** github.com/anthropics/knowledge-work-plugins
**Stars:** 7,700+
**Install:**
```bash
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install product-management@knowledge-work-plugins
```

This plugin has two layers: **commands** (you invoke explicitly) and **skills** (fire automatically).

### The 6 Slash Commands — Full Workflows

**`/product-management:write-spec <feature or problem>`**

Full workflow:
1. Accepts vague inputs (feature name, problem statement, user request, vague idea)
2. Asks 5 questions: user problem, target users, success metrics, constraints, prior art
3. Pulls context from connected Jira/Linear + Notion + Figma (if wired)
4. Generates complete PRD: Problem Statement, Goals, Non-Goals, User Stories, Requirements (P0/P1/P2), Success Metrics, Open Questions, Timeline Considerations
5. Offers follow-up artifacts: design brief, ticket breakdown, stakeholder pitch

---

**`/product-management:competitive-brief <competitor or feature area>`**

Full workflow:
1. Scopes the analysis (which competitors, what focus, what decision it informs)
2. Researches via web search: product pages, pricing, G2/Capterra reviews, job postings, press coverage
3. Checks connected knowledge base for existing research
4. Generates: Competitor Overview, Feature Comparison, Positioning Analysis, Strengths/Weaknesses, Opportunities, Threats, Strategic Implications
5. Offers follow-up: one-page exec summary, sales battle cards, "how to win against" guide, monitoring plan

---

**`/product-management:roadmap-update <update description>`**

Full workflow:
1. Pulls current roadmap from connected project tracker (Jira/Linear/Asana) or asks to paste it
2. Determines operation: Add Item, Update Status, Reprioritize, Move Timeline, or Create New
3. Applies RICE/MoSCoW/ICE frameworks from the roadmap-management skill
4. Generates: Status Overview, Roadmap Items table (Done/On Track/At Risk/Blocked/Not Started), Risks and Dependencies, Changes This Update
5. Offers audience-specific formatting: executive summary, engineering detail, customer-facing version

---

**`/product-management:synthesize-research <research topic or question>`**

Full workflow:
1. Accepts pasted text, uploaded files, or pulls from connected Intercom/knowledge base/analytics
2. Extracts observations, quotes, behaviors, pain points per source
3. Applies thematic analysis via affinity mapping (see skill below)
4. Creates priority matrix: High/Low Frequency × High/Low Impact
5. Generates: Research Overview, Key Findings (5-8 with evidence, frequency, impact, confidence level), User Segments, Opportunity Areas, Recommendations, Open Questions

---

**`/product-management:metrics-review <time period or metric focus>`**

Full workflow:
1. Pulls metrics from connected Amplitude/Pendo/analytics tool (or you paste)
2. Structures review using North Star → L1 health indicators → L2 diagnostic hierarchy
3. Analyzes trends: current value, trend direction, vs target, rate of change, anomalies
4. Generates: Summary (2-3 sentences), Metric Scorecard table (Metric | Current | Previous | Change | Target | Status), Trend Analysis, Bright Spots, Areas of Concern, Recommended Actions

---

**`/product-management:stakeholder-update <update type and audience>`**

Full workflow:
1. Determines update type (Weekly/Monthly/Launch/Ad-hoc) + audience (Executives/Engineering/Cross-functional/Customers/Board)
2. Pulls context from project tracker, chat history, meeting transcription (Fireflies), knowledge base
3. Generates audience-specific format:
   - **Executives:** TL;DR + G/Y/R status + under 300 words
   - **Engineering:** Shipped items + owners + blockers + decisions needed
   - **Customers:** Benefits-framed language, no internal jargon

---

### The 6 Passive Skills (auto-activate when relevant)

These run in the background when you do PM work, injecting methodology expertise:

| Skill | What it injects |
|-------|----------------|
| `feature-spec` | PRD structure (8 sections), INVEST criteria for user stories, MoSCoW with "be ruthless about P0s", success metrics (leading vs lagging), acceptance criteria in Given/When/Then, 6 signs of scope creep |
| `user-research-synthesis` | Thematic analysis (6-step process), affinity mapping protocol, triangulation methodology, opportunity sizing matrix: (Users affected × Frequency × Severity) = impact score |
| `competitive-analysis` | Competitive set mapping (Direct/Indirect/Adjacent/Substitute), feature comparison with Strong/Adequate/Weak/Absent scale, positioning statement analysis, win/loss interview questions |
| `roadmap-management` | RICE, MoSCoW, ICE frameworks, value-vs-effort matrix |
| `metrics-tracking` | North Star metric hierarchy, L1/L2 metrics structure |
| `stakeholder-comms` | G/Y/R status definitions, ROAM risk communication framework |

**Key point:** The passive skills make every PM interaction better even when you don't use the slash commands. When you write a spec, the feature-spec skill is active. When you discuss metrics, the metrics-tracking skill is active.

### PM Plugin Connectors

Slack · Linear · Asana · Monday · ClickUp · Jira · Notion · Figma · **Amplitude** · **Pendo** · **Intercom** · **Fireflies**

---

## Claude Cowork — The Non-Terminal Option

> Same model as Claude Code. Same skill format, same connector types. Visual interface.

**Available:** Windows + macOS (Intel + Apple Silicon). Full feature parity as of Feb 21, 2026.
**Requires:** Claude Pro, Max, Team, or Enterprise plan.

Cowork ships with all 11 official Anthropic plugins pre-installed. You get the PM plugin with zero setup.

**What Cowork does that Chat doesn't:**
- Spawns parallel sub-agents for complex tasks
- Creates real files (.docx, .pptx, .xlsx, .pdf) delivered to your folder
- Runs multi-step workflows with visible progress
- Visual Plugins panel for browsing, installing, creating plugins
- Scheduled tasks

**Cross-compatibility:** Skills installed in Cowork work in Claude Code CLI and vice versa. Same format.

**Cross-session memory (2-minute setup):**
In Settings → Cowork → Global Instructions, paste:
```
## Memory Management
When you discover something valuable — product decisions, user research insights,
gotchas — immediately append to ~/product-memory.md
Format: date, what, why. Keep entries short.
Read this file at the start of every session.
```

---

## The PM Skills Libraries — With Real Content

### 1. menkesu/awesome-pm-skills — 29 Skills in 7 Modes
**Stars:** 180 · **Forks:** 63 · The most popular dedicated PM skills collection

```bash
/plugin marketplace add menkesu/awesome-pm-skills
```

**7 Modes, 29 Skills:**

**Builder Mode (11 skills):**
| Skill | Based On | Views |
|-------|----------|-------|
| `zero-to-launch` | Kevin Weil (OpenAI), Dylan Field (Figma), Brian Chesky | 783K |
| `strategic-build` | Shreyas Doshi, Marty Cagan | 486K |
| `continuous-discovery` | Teresa Torres — Continuous Discovery Habits | — |
| `design-first-dev` | Brian Chesky, Dylan Field, Katie Dill | 580K |
| `ai-product-patterns` | Kevin Weil (OpenAI CPO) — evals as specs | 202K |
| `jtbd-building` | Bob Moesta (JTBD co-creator) | 69K |
| `growth-embedded` | Gustaf Alstromer (YC), Elena Verna | 129K |
| `exp-driven-dev` | Ronny Kohavi, Netflix culture | — |
| `quality-speed` | Dylan Field, Brian Chesky | 580K |
| `ship-decisions` | Shreyas Doshi, Marty Cagan, Tobi Lutke | 486K |

**Strategist Mode (4 skills):**
`decision-frameworks` (Annie Duke) · `strategy-frameworks` (Playing to Win) · `okr-frameworks` (Christina Wodtke) · `prioritization-craft` (Shreyas Doshi + RICE)

**Communicator Mode (4 skills):**
`strategic-storytelling` (Andy Raskin) · `positioning-craft` (April Dunford) · `exec-comms` (Amazon 6-pager) · `confident-speaking` (Stanford)

**Measurement Mode:** `metrics-frameworks` (North Star/AARRR) · `user-feedback-system` (Superhuman PMF)

**Navigator Mode:** `influence-craft` · `stakeholder-craft` (Radical Candor) · `workplace-navigation`

**Leader Mode:** `culture-craft` (Stripe, HubSpot) · `career-growth` · `strategic-pm` (Anneka Gupta)

**The `continuous-discovery` skill** (Teresa Torres — full implementation):
- Full Weekly Discovery Plan template (Monday-Wednesday: research; Thursday: synthesis; Friday: planning)
- Interview Snapshot template (Date, Participant, Focus, Key Insights, Opportunities, Memorable Quotes, Updated Assumptions, Next Steps)
- Opportunity Solution Tree template (Outcome → Opportunities → Solutions → Assumptions → Tests)
- Assumption Test Plan (risk assessment, results tracking)
- Quick Reference: discovery vs delivery balance checklist
- Real examples: Spotify Discover Weekly (algorithmic assumption test before UX), Netflix Continue Watching (5% A/B before rollout)

**The `strategic-pm` skill** (shifts from tactical to strategic):
```
Strategic: WHY / What problem / How it ladders to company goals / 2-year vision
Tactical:  WHAT features / How to build / When to ship

Time horizons:
NOW   (0-3 months): execution focus
NEXT  (3-12 months): strategic choices
LATER (12+ months): vision

"Strategic PMs start with why. Tactical PMs start with what." — Anneka Gupta
```

---

### 2. Lenny's Podcast PM Skills — 86 Skills
**Repo:** github.com/refoundai/lenny-skills · **Stars:** ~244

```bash
/plugin marketplace add refoundai/lenny-skills
```

**Based on 300+ podcast episodes.** Each skill includes principles from named guests, questions to ask users, and common mistakes to flag.

**The most important skills for daily PM work:**

**`writing-prds`** — 11 product leaders. Key insights:
- Lead with problem and context (Maggie Crowley: "The most important section is background and context")
- PR/FAQ forces clarity (Amazon's Bill Carr): write the press release first
- For AI features: "Prompt sets are the new PRDs" (Aparna Chennapragada/Google)
- "Evals as living PRDs" (Hamel Husain): translate requirements into executable evaluations
- Keep lightweight for action (Eric Simons): "minimal context that ensures alignment"

**`prioritizing-roadmap`** — 75 product leaders:
- Separate truth from hypothesis (Alex Hardimen)
- Use "seasons" in fast-changing markets (Asha Sharma/Pinterest)
- Build a common currency using growth models (Dan Hockenmaier)
- Balance: 80% cannonballs (big bets), 20% lead bullets (Adriel Frederick)
- Kill low-usage features: if <2% adoption, remove it (Gibson Biddle)

**`conducting-user-interviews`** — 43 product leaders:
- Collect stories, not opinions (Teresa Torres): "Tell me about the last time you..."
- Only interview people who've taken action (Bob Moesta)
- Watch, don't just ask (Gustaf Alstromer): screen share, look for normalized pain
- Falsify, don't validate (Judd Antin): "We don't validate, we falsify"
- Never ask what they want built
- Right-size sample: 7-14 interviews sweet spot (Shaun Clowes)

**Full skill list highlights:** ai-evals · ai-product-strategy · behavioral-product-design · competitive-analysis · defining-product-vision · designing-growth-loops · measuring-product-market-fit · positioning-messaging · pricing-strategy · problem-definition · retention-engagement · scoping-cutting · setting-okrs-goals · shipping-products · stakeholder-alignment · working-backwards · writing-north-star-metrics · writing-specs-designs

---

### 3. deanpeters/Product-Manager-Skills — 42 Skills
**Stars:** 132 · github.com/deanpeters/Product-Manager-Skills

```bash
/plugin marketplace add deanpeters/Product-Manager-Skills
```

**Three skill types:**
- `type: component` — Building blocks (jobs-to-be-done, proto-persona, user-story)
- `type: workflow` — End-to-end multi-phase processes (prd-development, discovery-process)
- `type: interactive` — Guided dialogues (opportunity-solution-tree, prioritization-advisor)

**The `prd-development` workflow skill (8 phases, 2-4 days):**
1. Executive Summary (30 min) — "Write this first, refine it last"
2. Problem Statement (60 min) — Uses `problem-statement` component
3. Target Users & Personas (30 min) — Uses `proto-persona` component
4. Strategic Context (45 min) — OKRs, TAM/SAM/SOM, "why now"
5. Solution Overview (60 min) — Uses `user-story-mapping-workshop` component
6. Success Metrics (30 min) — Primary, secondary, guardrail metrics
7. User Stories & Requirements (90-120 min) — Uses `epic-hypothesis`, `epic-breakdown-advisor`
8. Out of Scope & Dependencies (30 min) — Explicit risks, mitigations

Full worked example with a "guided onboarding checklist" feature throughout. Each phase includes common pitfalls documented.

**The `jobs-to-be-done` component skill:**
- 3 job types: Functional (tasks to perform) · Social (how to be perceived) · Emotional (states to seek/avoid)
- 4 pain categories: Challenges, Costliness, Common Mistakes, Unresolved Problems
- 4 gain categories: Expectations, Savings, Adoption Factors, Life Improvement
- 5 pitfalls: "Confusing jobs with solutions ('I need Slack' → ask WHY? 5 times)"
- References: Christensen *Competing Against Luck*, Ulwick *Outcome-Driven Innovation*, Osterwalder *Value Proposition Canvas*

**Full skill list:** opportunity-solution-tree · competitive-landscape · customer-journey-map · discovery-interview-prep · epic-breakdown-advisor · feature-investment-advisor · finance-based-pricing-advisor · lean-ux-canvas · pestel-analysis · positioning-statement · press-release · prd-development · problem-framing-canvas · roadmap-planning · saas-economics-efficiency-metrics · stakeholder-brief · tam-sam-som-calculator · user-story-mapping-workshop · workshop-facilitation (and 23 more)

---

### 4. assimovt/productskills — 16 Curated Framework Skills
**Stars:** 11 · github.com/assimovt/productskills

```bash
/plugin marketplace add assimovt/productskills
```

16 skills built on specific, named frameworks. No generic advice.

**Discovery & Research (5):**
`user-interview` (Mom Test) · `problem-validation` (frequency × intensity × WTP) · `jtbd-analysis` (Forces of Progress) · `research-synthesis` (atomic insights) · `opportunity-mapping` (Opportunity Solution Trees)

**Strategy & Positioning (3):**
`competitor-analysis` (feature matrix + positioning map) · `product-positioning` (April Dunford's *Obviously Awesome*) · `strategy-doc` (Rumelt's Kernel + Playing to Win)

**Prioritization & Scoping (3):**
`feature-prioritization` (RICE + enablers vs blockers) · `scope-cutting` (Shape Up appetite) · `bet-sizing` (Type 1/2 decision framework)

**The PRD (1):**
`prd-writing` — Linear's philosophy. The critical rule:
> **800-1200 words maximum.** "Longer PRDs mean the thinking isn't done."
> "If you have no evidence, say so explicitly — that's an opinion-driven PRD."
> Section 5 (Success Metrics): ALWAYS pair with a counter-metric to prevent gaming.
> Section 6 (Out of Scope): mandatory, prevents scope creep.

**The `strategy-doc` skill** — Rumelt + Playing to Win:
> "A strategy without tradeoffs is not a strategy."
>
> **Bad:** "We will build the best product by focusing on quality, speed, satisfaction."
> **Good:** "We will win technical PMs at Series A-B startups by being the fastest path from customer insight to shipped feature — sacrificing enterprise compliance features to stay opinionated and fast."

**Launch & Measure (4):**
`launch-plan` (tiers: silent/soft/big-bang) · `metrics-framework` (North Star + input/output tree) · `experiment-design` (hypothesis-driven A/B with sample size) · `roadmap-planning` (Now/Next/Later — outcomes, not features)

---

### 5. refoundai/lenny-skills Already Covered Above

---

## The BMAD PM Agent — John

**Repo:** github.com/bmad-code-org/BMAD-METHOD
**Install:** `npx bmad-method install`

John is a dedicated PM agent inside the BMAD Method. Here is his actual definition:

```yaml
name: John
title: Product Manager
capabilities: PRD creation, requirements discovery, stakeholder alignment, user interviews

persona:
  identity: Product management veteran with 8+ years launching B2B and consumer products.
             Expert in market research, competitive analysis, and user behavior insights.
  communication_style: "Asks 'WHY?' relentlessly like a detective on a case.
                        Direct and data-sharp, cuts through fluff to what actually matters."
  principles:
    - PRDs emerge from user interviews, not template filling
    - Ship the smallest thing that validates the assumption
    - Technical feasibility is a constraint, not the driver — user value first
```

**John's 6 menu commands:**

| Trigger | What it runs |
|---------|-------------|
| `CP` — Create PRD | Expert-led facilitated PRD from scratch via structured workflow |
| `VP` — Validate PRD | Checks comprehensiveness, leanness, cohesion |
| `EP` — Edit PRD | Updates an existing PRD |
| `CE` — Create Epics & Stories | Specs that drive development |
| `IR` — Implementation Readiness | Ensures PRD + UX + Architecture are aligned |
| `CC` — Course Correction | Mid-implementation pivot workflow |

**How John's PRD workflow runs** (step-file architecture):
- Each step is a self-contained file loaded one at a time (never loads future steps)
- Sequential: Discovery → Vision → Executive Summary → Domain → Innovation → Project Type → Scoping → Complete
- State tracked in frontmatter: `stepsCompleted: [step-01, step-02...]`
- "NEVER create mental todo-lists from future steps" — real instruction in the YAML

---

## Product Org OS — The Full Thing

**Repo:** github.com/yohayetsion/product-org-os

13 agents + 61 skills + organizational memory. This simulates a full product org.

**The 13 Role-Based Agents:**

| Agent | Role |
|-------|------|
| `@cpo` | Chief Product Officer — executive strategy, portfolio |
| `@vpp` | VP of Product — vision, roadmap accountability, pricing |
| `@pm-dir` | Director PM — roadmap governance, team coordination |
| `@pmm-dir` | Director PMM — GTM strategy, positioning, launches |
| `@pm` | Product Manager — feature specs, user stories, delivery |
| `@pmm` | Product Marketing Manager — campaigns, sales enablement |
| `@plt` | Product Leadership Team — portfolio tradeoffs (Meeting Mode) |
| `@bizops` | Business Ops — business cases, financial analysis |
| `@ci` | Competitive Intelligence — competitor analysis, market research |
| `@prod-ops` | Product Ops — process optimization, launch coordination |
| `@value-realization` | Success metrics, customer outcomes |
| `@bizdev` | Partnerships, market expansion |
| `@ux-lead` | User research, design specs |

**Two gateways:**
- `@product` — routes your request to the right agent automatically
- `@plt` — Meeting Mode: all PLT agents respond in their own voice, showing agreement AND tension

**61 Skills including:**
`/prd` · `/roadmap` · `/competitive-landscape` · `/market-analysis` · `/gtm-strategy` · `/pricing-strategy` · `/business-case` · `/launch-plan` · `/qbr-deck` · `/feature-spec` · `/user-story` · `/decision-record` · `/strategic-bet` · `/positioning-statement` + 47 more

**Organizational Memory:** Persistent across sessions. Decisions (DR-YYYY-NNN), Strategic Bets (SB-YYYY-NNN), feedback (FB-YYYY-NNN), portfolio, learnings.

**Meeting Mode example:**
```
@plt Should we prioritize mobile app or API first?
```
→ @cpo speaks from portfolio perspective, @vpp from roadmap, @pm-dir from execution, each in their own voice, points of tension visible.

---

## pm-studio — The CLAUDE.md Enhancement Layer

**Repo:** github.com/zoekdestep/pm-studio

Not a plugin — a CLAUDE.md workspace pattern that wraps the anthropics PM plugin with personal context and quality rules.

**9 Rules that make it exceptional:**

**Rule 1: Always Load Context First**
Before any writing command, reads in sequence:
1. `context/product-context.md` — product strategy, metrics, competitive landscape
2. `context/about-me.md` — role, stakeholders, your PM philosophy
3. `/notes/` — research, ideas, prior work
4. `context/tone-and-voice.md`
5. `context/examples/` — gold standard outputs (source of truth for voice + structure)
6. `context/references.md` — curated "always read these" sources

**Rule 5: Anti-AI Writing Rules (copy these into your CLAUDE.md)**
```
NEVER use:
- Em dashes (—)
- "leverage", "utilize", "delve", "crucial", "robust", "comprehensive"
- Generic openings ("In today's fast-paced...")
- Passive voice dominance
- Vague qualifiers instead of specific numbers
```

**Rule 7: Agent Analysis After Every Document**
After any output, Claude offers:
- `reviewer` — structural completeness + reasoning quality
- `editor` — voice matching to your writing
- `edge-case-finder` — scenarios, risks, failure modes
- `metrics-designer` — KPIs and guardrails
- `question-generator` — open questions worth exploring

**Commands pm-studio adds:**
- `/strategy [topic]` → research-backed strategy doc
- `/shipping [feature]` → shipping decision with learnings + recommendation
- `/note [content]` → capture to knowledge base (raw, no tone formatting)
- `/word [file]` → export markdown to Word

---

## More PM Agent Systems

### slgoodrich/agents — PM Workflow Routing
Multi-agent system where `product-manager` orchestrator routes to specialists:
- `market-analyst` — market research, competitive intelligence
- `user-researcher` — interview synthesis, persona development
- `roadmap-strategist` — roadmap planning, prioritization
- `spec-writer` — feature specification, acceptance criteria
- `metrics-analyst` — KPI definition, measurement planning

### abhitsian/compound-pm-marketplace
9 agents · 15 commands · 6 skills for personalized PM workflows. Specifically designed as a PM marketplace.

### omriariav/omri-cc-stuff
"Claude Code Skills/Agents/Commands built for my PM day-to-day work." Updated Feb 22, 2026 — most recently maintained personal PM setup on GitHub.

### CrashBytes/claude-role-skills
7 role-based skills including dedicated `Product Owner` and `Product Manager` skill files.

### valllabh/claude-agents
8 enhanced agents including `product-manager` agent with:
- `create-doc` workflow
- `prd-template` structured generation
- Business analyst companion agent

---

## Write Your Own PM Skill in 15 Minutes

A skill is a markdown file with frontmatter. That's all.

```markdown
---
name: user-research-synthesis
description: Synthesizes user research into JTBD statements and pain clusters.
             Activates when you share interviews, support tickets, or survey data.
---

# User Research Synthesis

## My Process

1. Read ALL input before synthesizing — no premature conclusions
2. Identify pain clusters by theme, not just keywords
3. Distinguish symptom from root cause — users describe symptoms
4. Extract JTBD: "When [situation], [user] wants [motivation] so they can [outcome]"
5. Rank by frequency × severity — low-volume critical pain is not noise

## Output Format

For each cluster:
- Theme: [1-line]
- Frequency: [how often]
- Severity signals: [workarounds, escalations, churn mentions]
- Best quote: "[verbatim, with source ID]"

Top 3 clusters get canonical JTBD statements.

## What I Will Not Do
- Summarize what you already told me
- Use "user journey" without specifics
- Bury the most important finding at the bottom
- Report raw counts without interpretation
```

**Install:**
```bash
cp user-research-synthesis.md ~/.claude/skills/       # global
cp user-research-synthesis.md .claude/skills/         # project-level
```

**The skill format spec:** github.com/agentskills.io

---

## Skill Marketplaces for PMs

| Marketplace | PM-relevant content | Install |
|-------------|--------------------|---------||
| skills.sh | PRD generator, discovery interview guides, pricing strategy, launch playbooks, analytics setup | `npx skills add [name]` |
| skillhub.club | 7,000+ AI-evaluated with quality scores | One-click desktop install |
| skillsmp.com | 200,000+ indexed from GitHub | Browse + copy |
| claudemarketplaces.com | Discover new marketplace repos | Browse |
| antigravity-awesome-skills | 868+ skills with PM bundles (Startup Founder, Marketing & Growth) | `cp skills/* ~/.claude/skills/` |

---

## The Full Install Stack — Week by Week

### Week 1: Official Foundation
```bash
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install product-management@knowledge-work-plugins
# You now have: /write-spec /competitive-brief /roadmap-update
#               /synthesize-research /metrics-review /stakeholder-update
#               + 6 passive skills firing automatically
```

### Week 2: PM Frameworks
```bash
# 29 skills from 300+ podcast episodes (most popular PM skills repo)
claude plugin marketplace add menkesu/awesome-pm-skills

# 86 skills from Lenny's Podcast guests
claude plugin marketplace add refoundai/lenny-skills

# 16 curated frameworks (Mom Test, Shape Up, Obviously Awesome, Teresa Torres)
claude plugin marketplace add assimovt/productskills
```

### Week 3: Full PM Agent Workflows
```bash
# BMAD John (PRD creation, epics, implementation readiness)
npx bmad-method install

# 42 typed skills (interactive/component/workflow)
claude plugin marketplace add deanpeters/Product-Manager-Skills
```

### Install Specific Skills by Name
```bash
npx skillfish add cch96/claude-plugins prd-builder    # 4-stage PRD workflow
npx skills add product-strategy                        # via skills.sh
```

---

## Quick Reference: All Install Commands

```bash
# Official Anthropic PM Plugin (6 commands + 6 passive skills)
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install product-management@knowledge-work-plugins

# menkesu/awesome-pm-skills (29 skills, 7 modes, 180★)
claude plugin marketplace add menkesu/awesome-pm-skills

# Lenny's Podcast skills (86 skills, 244★)
claude plugin marketplace add refoundai/lenny-skills

# deanpeters PM Skills (42 skills, 136★)
claude plugin marketplace add deanpeters/Product-Manager-Skills

# Curated frameworks: Mom Test, Shape Up, JTBD (assimovt)
claude plugin marketplace add assimovt/productskills

# BMAD PM Agent (John — PRD + epics + implementation readiness)
npx bmad-method install

# Product Org OS (13 agents, 61 skills)
# github.com/yohayetsion/product-org-os (manual install)

# PRD Builder (4-stage guided workflow)
npx skillfish add cch96/claude-plugins prd-builder

# Direct skill file copy
cp my-skill.md ~/.claude/skills/           # global
cp my-skill.md .claude/skills/             # project-level

# Browse installed
/plugin list
```

---

## Skills vs CLAUDE.md — When to Use What

| Use this content | In this place |
|-----------------|---------------|
| Your product vision, users, OKRs | `CLAUDE.md` |
| Your writing voice, anti-patterns | `CLAUDE.md` |
| Current decisions and team conventions | `CLAUDE.md` |
| PM frameworks (RICE, JTBD, OST) | Skills |
| PRD writing methodology | Skills |
| User interview frameworks | Skills |
| Competitive analysis process | Skills |
| Stakeholder communication format | Skills |
| Jira / Amplitude / Figma live data | MCP |
| Full PM agent with persona and routing | Plugin (anthropics/knowledge-work-plugins or BMAD) |
| Full product org simulation | product-org-os |

---

*Research sources: actual skill file content from github.com/anthropics/knowledge-work-plugins,
menkesu/awesome-pm-skills, deanpeters/Product-Manager-Skills, assimovt/productskills,
refoundai/lenny-skills, yohayetsion/product-org-os, zoekdestep/pm-studio,
bmad-code-org/BMAD-METHOD — February 22, 2026*
