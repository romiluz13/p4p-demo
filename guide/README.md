# Claude Code for Product Managers

> **You already use Claude. This is the version that actually knows your product.**

Most PMs use Claude like a smarter search engine — paste context, get output, repeat. Claude Code is different. It reads your files, remembers your product, connects to your tools, and runs multi-step workflows.

**And it has a skill ecosystem specifically built for PM work.** RICE prioritization, continuous discovery, PRD generation, stakeholder communication, competitive analysis — all available as installable skills that activate automatically when you need them.

---

## Two-Minute Quick Start

```bash
# 1. Install Claude Code
brew install claude

# 2. Install the official PM plugin (6 slash commands + all the connectors)
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install product-management@knowledge-work-plugins

# 3. Create your product brain (copy-paste to start)
mkdir my-product && cd my-product
cat > CLAUDE.md << 'EOF'
# My Product

## Product Vision
[What you're building, for whom, and why now]

## Core Users
[Who they are and what they're trying to do]

## Current Quarter
[Your OKRs or primary initiative]

## My Writing Voice
[Tone, format preferences, what to avoid]
EOF

# 4. Start
claude
```

You now have a PM plugin with 6 slash commands, a product brain that loads every session, and a foundation for everything else in this guide.

---

## What Makes This Different

This is not a generic "AI for PMs" guide. It covers:

- **The official Anthropic PM plugin** — 6 pre-built slash commands, connectors for Jira/Figma/Amplitude/Intercom/Pendo
- **PM skills libraries** — Lenny's 86 skills, RICE/Kano frameworks, continuous discovery, Mom Test, Shape Up
- **PM agent systems** — BMAD's John (Product Manager persona), slgoodrich/agents (multi-specialist routing), NioPD
- **Cowork** — the non-terminal option (Windows + macOS, visual interface, ships with PM plugin pre-installed)
- **Verified tool facts** — Figma free/paid truth, PowerPoint CLI situation, Notion performance caveat
- **Real prompts** — not described, shown. Copy-paste prompts for every use case.

---

## What PMs Are Actually Doing With This

| The problem | The old way | With Claude Code |
|-------------|------------|-----------------|
| Write a PRD grounded in user pain | 2 hrs of synthesis + drafting | 20 min pipeline: research → JTBD → draft → /devil review |
| Competitive analysis | 2 days, 5 tabs, one contractor | 34,000 Reddit comments in one session¹ |
| Answer a data question | Open a Jira ticket, wait 3 days | Ask your database in plain English |
| Prioritize a backlog | Gut feel + RICE spreadsheet | RICE skill activates automatically, scores with reasoning |
| Understand a feature technically | Schedule an eng meeting | Plan Mode — reads the code, no changes |
| Synthesize 200 interviews | 2-week analysis project | User research synthesis skill in one session |
| Write a spec that sounds like you | Generic AI + heavy editing | CLAUDE.md knows your voice, template, product |

¹ Monday.com PM, one session. [Full story →](09-real-stories.md)

---

## Start Here — In Order

### This week:

**1 → [Getting Started](01-getting-started.md)**
Install in 5 minutes. First session. Plan Mode. Claude Cowork (the non-terminal option).

**2 → [MCP Toolkit](02-mcp-toolkit.md)**
Connect Jira, Linear, Notion, Figma, Amplitude. Verified Figma free/paid answer.

**3 → [CLAUDE.md for PMs](03-claude-md-for-pms.md)**
Your product brain. Level 1 → Level 3 templates. The file that changes everything.

**4 → [Skills, Plugins & PM Agents](04-skills-and-plugins.md)** ← The centerpiece
The official PM plugin. BMAD's PM agent. Lenny's 86 skills. The PRD Builder. How to write your own skill in 15 minutes. The full skill marketplaces.

---

### Once you're running:

| File | What's in it |
|------|-------------|
| [05 — PM Use Cases](05-pm-use-cases.md) | 9 use cases, copy-paste prompts, real people |
| [06 — Compound MCP Workflows](06-compound-mcp-workflows.md) | Sprint health check, competitive pipeline, release impact — full prompts |
| [07 — Orchestration](07-orchestration-for-pms.md) | Agent roles, pipelines, session patterns |
| [08 — Tools & Integrations](08-tools-integrations.md) | Figma, PowerPoint, Google Slides, data tools |
| [09 — Real Stories](09-real-stories.md) | 7 verified practitioners, names and numbers |
| [10 — Workshop Exercises](10-workshop-exercises.md) | 10 team exercises, facilitator guide |
| [11 — Cheat Sheet](11-cheat-sheet.md) | Print this. One page. Everything. |

---

## The PM Skill Stack

```bash
# Week 1: Official plugin (slash commands + all connectors)
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install product-management@knowledge-work-plugins

# Week 2: PM frameworks
claude plugin marketplace add refoundai/lenny-skills       # 86 skills from Lenny's Podcast
claude plugin marketplace add assimovt/productskills        # Mom Test, Shape Up, JTBD

# Week 3: Full PM agent workflows
npx bmad-method install                                     # BMAD PM Agent (PRD + epics)
claude plugin marketplace add deanpeters/Product-Manager-Skills  # 42 skills
```

After Week 1: `/product-management:write-spec`, `/product-management:competitive`, RICE scoring, stakeholder updates — all available.

---

## The Tools You'll Connect

```
PROJECT MANAGEMENT                ANALYTICS
──────────────────                ─────────
Jira MCP               80★        Amplitude   (official)
Linear MCP            129★        Google Analytics     177★
Notion MCP            206★        Mixpanel              19★
GitHub Issues                     Power BI             200★

DESIGN                            RESEARCH
──────────                        ────────
Figma (remote server: FREE)       Bright Data  (scrape anything)
Figma (desktop: paid Dev seat)    skills.sh    (PM skill library)
```

---

## Real People. Real Results.

> *"Where it used to be about influencing through collaboration, it now revolves around **influencing through building and creating**."*
> — **Ondrej Machart**, Head of Product. Non-engineer. Published an iOS app solo. 13 projects.

| Metric | Number |
|--------|--------|
| Time saved on competitive intel | **85%** (Reza Rezvani) |
| Reddit comments in one session | **34,000** (Monday.com PM) |
| Official Anthropic PM plugin | **7,700★** on GitHub |
| Skills in Lenny's library | **86** from top PM interviews |
| Skills in antigravity collection | **868+** across 18 AI agents |

---

## The Honest Answers

**"Do I need to know how to code?"**
No. Skills fire automatically. Plan Mode is read-only. Prompts are plain English. Your skill is evaluating output — which you already do.

**"Does the Figma MCP cost extra?"**
Remote server is free on all Figma plans. Desktop server requires a paid Dev seat. [Details →](08-tools-integrations.md)

**"I don't want to use the terminal."**
Use Claude Cowork. Same model, same skills, visual interface. Ships with PM plugin pre-installed. Available on all platforms as of Feb 2026. [Details →](04-skills-and-plugins.md)

**"Where do I start if I only have 30 minutes?"**
Install the PM plugin + write 3 sections of CLAUDE.md. That's it. 30 minutes, pays back every day.

---

## What's in This Repo

```
CC-PM/
├── README.md                    ← You're here
├── 01-getting-started.md        ← Install, Plan Mode, Cowork intro
├── 02-mcp-toolkit.md            ← Every PM MCP, verified
├── 03-claude-md-for-pms.md      ← Level 1 → Level 3 templates
├── 04-skills-and-plugins.md     ← The PM skill ecosystem ← START HERE
├── 05-pm-use-cases.md           ← 9 use cases, real prompts
├── 06-compound-mcp-workflows.md ← 5 multi-tool workflows, full prompts
├── 07-orchestration-for-pms.md  ← Pipelines, agents, session patterns
├── 08-tools-integrations.md     ← Figma, PowerPoint, data tools
├── 09-real-stories.md           ← 7 verified PM stories + community
├── 10-workshop-exercises.md     ← 10 team exercises, facilitator guide
├── 11-cheat-sheet.md            ← One page. Print it.
└── presentation.html            ← Slide deck: open in Chrome
```

---

## Further Reading

| Resource | What it is | Stars |
|----------|-----------|-------|
| [prodmgmt.world/claude-code](https://prodmgmt.world/claude-code) | 180+ PM skills | — |
| [ccforpms.com](https://ccforpms.com) | Interactive PM course | — |
| [refoundai/lenny-skills](https://github.com/refoundai/lenny-skills) | 86 PM skills from Lenny's Podcast | 244★ |
| [deanpeters/Product-Manager-Skills](https://github.com/deanpeters/Product-Manager-Skills) | 42 PM skills | 132★ |
| [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins) | Official Anthropic plugins (11 roles) | 7,700★ |
| [bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | PM + dev full lifecycle agent system | active |
| [skills.sh](https://skills.sh) | Open PM skill ecosystem | — |
| [Anthropic free course](https://anthropic.skilljar.com/claude-code-in-action) | Claude Code in Action | — |

---

*Built by [Rom Iluz](https://github.com/romiluz13), creator of [cc10x](https://github.com/romiluz13/cc10x). All data verified February 2026.*
