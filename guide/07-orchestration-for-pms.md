# Orchestration for PMs — Pipelines, Agents, and Context Management

> This is the advanced track. You use Claude Code. You've hit a ceiling. This is the fix.

---

## What "Orchestration" Actually Means

Orchestration is not a feature. It's a practice.

Ad hoc Claude Code: open a session, explain context, ask a question, get an answer, start over the next day.

Orchestrated Claude Code: context loads automatically, roles are defined, outputs go to the right files, sessions hand off to each other, and the quality of work compounds over time instead of resetting.

Same tool. Different practice.

This file covers four problems PMs consistently hit, and the architecture to solve all four.

---

## The 4 Ceilings PMs Hit

### Ceiling 1: Context Rot

**Symptom:** Sessions start strong and degrade by message 30. Claude starts contradicting what you said earlier. Output gets generic. It feels like the model "forgot."

**What's actually happening:** The context window is finite. When it fills, the model deprioritizes content loaded earlier — including your CLAUDE.md instructions. By message 60-80, the instructions you set up at session start are getting partial attention at best.

**Fix:** Session scoping. One task, one session. End when the task is done. Don't use one session for three different topics. CLAUDE.md loads at session start with full attention. Every new session gets that full load again.

Additional fix: split your CLAUDE.md into a lean orchestration file (100-150 lines of things Claude gets wrong without being told) and separate `context/` files that Claude loads on demand. This keeps the core instructions light and the context relevant.

### Ceiling 2: Repetitive Prompts

**Symptom:** You write the same setup text at the start of every prompt. "Act as a PM who..." or "Remember our product is..." — you're doing context loading manually, every time.

**What's actually happening:** You haven't externalized your prompt patterns. Each session you're rebuilding context from scratch because it lives in your head, not in files.

**Fix:** Agent roles defined in CLAUDE.md. When you type `/spec`, Claude already knows the persona, the rules, the template, and the output format. You don't re-explain it. It's in the file. The one-time investment of writing agent role definitions pays back on every subsequent use.

### Ceiling 3: MCPs Not Compound

**Symptom:** You have 3 MCPs connected. You use each one individually, never together. The competitive intelligence still takes manual synthesis. The sprint analysis is still split across browser tabs.

**What's actually happening:** You're treating MCPs as lookup tools instead of workflow nodes. Each MCP is one step in a compound prompt — but you're treating each as a standalone query.

**Fix:** Write compound prompts that use multiple MCPs in sequence. Claude reads from Jira, reads from Amplitude, reads from your local specs — and synthesizes across all three. See `05-compound-mcp-workflows.md` for all five canonical workflows.

### Ceiling 4: Sequential Bottleneck

**Symptom:** Competitive research across 3 companies takes 3x as long as it should. You're researching each one by hand, then synthesizing. Or you're asking Claude to do them one at a time.

**What's actually happening:** You're thinking single-threaded. Claude Code's Task tool can spawn parallel agents — multiple instances of Claude running simultaneously, each working on one competitor, saving to its own file, merging at the end.

**Fix:** The parallel research pattern. Three competitors in one prompt with explicit parallel instruction. Claude spawns three tasks, they complete simultaneously, synthesis runs after all three finish.

---

## Agent Roles for PMs

### What "Agent" Means in PM Context

An agent is not a plugin. It's not a separate tool. In PM practice, an agent is **a specialized Claude session with a defined role, tools, and context**.

You define agents in CLAUDE.md. You invoke them with a slash command (e.g., `/spec`, `/research`). Claude Code reads the definition and shifts into that role for the session.

No installation required. No external tools. Just CLAUDE.md definitions.

### The 6 PM Agent Definitions

Copy these into the `## Agent Roles` section of your CLAUDE.md:

```markdown
## Agent Roles

### /research
You are a rigorous PM researcher.
Rules:
- Every claim requires evidence: source file path, URL, or ticket ID
- Distinguish fact from inference: label inferences explicitly
- Flag sample size issues: "(n=3, low confidence)"
- Flag when you're uncertain: "I'm not certain about X — here's what I found"
- End every research output with: "Key uncertainty that would change this analysis: [X]"

### /spec
You are a senior PM who has shipped this feature type before.
Rules:
- Reference the specific persona from context/personas.md — name them
- Every user story has exactly 3 acceptance criteria: Given/When/Then format
- Success metrics must be measurable within 30 days of launch
- v1.0 scope: the smallest thing that validates the core hypothesis
- End with: "Top open question that could kill this spec: [X]"
- Use our PRD template: context/prd-template.md

### /data
You are a PM-trained data analyst.
Rules:
- Lead with the decision implication, not the methodology
- Format: "[Metric] is [X], which means we should [action]"
- Flag data quality issues before interpreting
- Never bury the key finding in paragraph 3
- End with: "The one number the decision-maker should focus on: [X]"

### /devil
You are the skeptical VP of Product who must approve this work.
Rules:
- Assume positive intent. Zero tolerance for weak reasoning.
- Output exactly 3 objections — no more, no fewer
- For each objection: the specific problem, what evidence would change your mind, the mitigation
- End with: "Would approve if [condition]" OR "Would not approve because [reason]"
- Do not soften objections to be polite

### /review
You are a senior PM peer reviewer.
Rules:
- Read the spec AND the original research/pain file before reviewing
- Core check: does the spec actually solve the pain in the research? (not just the stated requirement)
- Verdict: ship as-is OR revise + exactly 3 specific improvements
- Include exactly 1 thing that's genuinely good (balanced feedback is required)
- If the spec and research are misaligned: say so directly

### /decision
You are logging a product decision.
Rules:
- Use the template in context/decision-template.md
- Include: what was decided, why, what was rejected, who owns it, review trigger
- Maximum 1 paragraph of rationale — dense, not verbose
- Save to: decisions/[YYYY-MM-DD]-[topic-slug].md
- If any information for the template is missing: ask before logging
```

### Using Agent Roles

```
# To invoke an agent role:
/spec

Write a PRD for the bulk export feature.
User research: research/user-pain/bulk-export-research.md

# Claude shifts into /spec mode — uses the senior PM persona,
# reads the template, references the persona by name, etc.
```

---

## The 3 Orchestration Patterns

### Pattern 1: Sequential Pipeline

**What it is:** A chain of steps where each step's output is the next step's input.

**When to use it:** When you need each step's output to inform the next — research → analysis → spec → review.

**The pattern:**

```
Step 1 (Researcher):
/research
Pull user pain data for Feature X.
Sources: Jira tickets tagged [label], Intercom support tickets, research/interviews/
Save findings to: research/user-pain/feature-x-pain.md

---

Step 2 (Analyst):
/data
Read: research/user-pain/feature-x-pain.md
Extract canonical JTBD statements.
Save to: research/user-pain/feature-x-jtbd.md

---

Step 3 (Spec Writer):
/spec
Read: research/user-pain/feature-x-jtbd.md
Read: context/prd-template.md
Write the PRD.
Save to: specs/feature-x-prd.md

---

Step 4 (Reviewer):
/review
Read: specs/feature-x-prd.md
Read: research/user-pain/feature-x-pain.md
Does the spec solve the original pain?
```

Each step can be a separate session (recommended for long pipelines). The file is the handoff protocol. No context needs to be re-explained — just reference the file.

### Pattern 2: Parallel Research

**What it is:** Multiple research tasks running simultaneously, results merged by a synthesis step.

**When to use it:** Competitive research, multi-stakeholder interview synthesis, anything where you're doing the same analysis on N different subjects.

**The pattern:**

```
Research the following 3 competitors in parallel using separate tasks.
For each: save results to its own file before synthesizing.

Competitor A ([Company]):
- Changelog: [URL]
- G2 reviews: [URL or Bright Data scrape]
- Job postings: [URL]
Save to: research/competitive/competitor-a-[date].md

Competitor B ([Company]):
[Same sources]
Save to: research/competitive/competitor-b-[date].md

Competitor C ([Company]):
[Same sources]
Save to: research/competitive/competitor-c-[date].md

After all three complete:
Read all three files.
Synthesize:
- Feature matrix (who has what)
- Our gaps
- Our advantages
- Convergent bets (things 2+ competitors are doing)
Save to: research/competitive/synthesis-[date].md
```

Claude Code spawns three parallel tasks using the Task tool. All three complete before synthesis begins. What used to take 3 separate sessions now takes one.

### Pattern 3: Specialist + Synthesizer

**What it is:** Different agents with different expertise analyzing the same problem from different angles, then one synthesizer combines their output.

**When to use it:** Complex decisions where you want genuine multi-perspective analysis (not a single model trying to "be balanced").

**The pattern:**

```
This is a specialist analysis of our decision to [decision topic].

Task A — Market Analyst:
Act as a market analyst evaluating market timing.
Focus: competitive landscape, market size, TAM timing.
Read: research/competitive/synthesis-[date].md
Save 1-page analysis to: research/decision/market-analysis.md

Task B — User Research Analyst:
Act as a user researcher evaluating user demand.
Focus: pain severity, JTBD fit, user willingness to pay.
Read: research/user-pain/synthesis-[date].md
Save 1-page analysis to: research/decision/user-analysis.md

Task C — Technical Analyst:
Read codebase context from [relevant files].
Act as a technical evaluator assessing execution risk.
Focus: implementation complexity, dependencies, timeline risk.
Save 1-page analysis to: research/decision/technical-analysis.md

[After all three complete]

Synthesis task:
Read: research/decision/market-analysis.md
Read: research/decision/user-analysis.md
Read: research/decision/technical-analysis.md

Synthesize into a decision brief:
- Where all three analyses agree (strong signal)
- Where they conflict (tension to surface)
- Net recommendation and rationale
- Top 3 risks
Save to: specs/decision-[topic]-[date].md
```

Three agents, three roles, one compound output. The synthesis step does what's actually hard: finding where expert perspectives converge and where they conflict.

---

## The Full PRD Pipeline

This is the canonical 4-step pipeline. Each step is a separate prompt (and ideally a separate session). Each step saves a file. The next step reads the file.

**Before starting:** Create the folder structure if it doesn't exist.

```bash
mkdir -p research/user-pain research/competitive specs decisions notes context
```

### Step 1: Research — Pull User Pain

```
/research

Task: research user pain for [Feature Name].

Sources to pull from (use available MCPs or local files):
1. Jira tickets tagged [label] from the last 90 days
   → Use Jira MCP, or read from research/support/jira-export.csv
2. Support tickets mentioning [feature area]
   → Use [support tool MCP] or read research/support/[export].csv
3. Interview transcripts in research/interviews/ (if any)
4. User feedback in research/feedback/ (if any)

Find:
- Top 3 pain patterns with evidence (quote + source ID for each)
- Users' current workarounds for each pain
- Any pain mentioned by 70%+ of sources — flag as critical
- Which user segment (from context/personas.md) is most affected

End with: "Key uncertainty that would change this analysis: [X]"

Save to: research/user-pain/[feature-name]-research.md
```

### Step 2: Analysis — Extract JTBD

```
/data

Read: research/user-pain/[feature-name]-research.md

Synthesize the core JTBD:

For the primary pain:
- Situation: when does this need arise?
- Motivation: what is the user trying to accomplish?
- Outcome: what does "done" look like from the user's perspective?
- Workaround: what do they do today without this?
- Hiring condition: what would make them actively seek a solution?
- Frequency: how often does this job arise?

Format as canonical JTBD statement:
"When [situation], [segment] wants to [motivation], so they can [outcome]."

Also flag:
- Is this a functional job, social job, or emotional job?
- What's the minimum viable solution that "hires" it?

End with: "The one metric that would tell us if this JTBD is being solved: [X]"

Save to: research/user-pain/[feature-name]-jtbd.md
```

### Step 3: Spec — Write the PRD

```
/spec

Read: research/user-pain/[feature-name]-jtbd.md
Read: context/prd-template.md
Read: context/personas.md
Read decisions/ — any relevant prior decisions

Write a PRD for [Feature Name].

Requirements:
- Ground the Problem Statement in the JTBD from the research file
- Reference the persona by name (from context/personas.md)
- v1.0 scope: the smallest version that tests the JTBD hypothesis
- Success metrics: directly tied to the JTBD outcome, measurable in 30 days
- Every user story: exactly 3 acceptance criteria in Given/When/Then format

End with: "Top open question that could kill this spec: [X]"

Save to: specs/[feature-name]-prd.md
```

### Step 4: Review — Does the Spec Solve the Pain?

```
/review

Read: specs/[feature-name]-prd.md
Read: research/user-pain/[feature-name]-research.md

This is the critical check: does the PRD actually solve the pain we found in research?

Check:
1. Does the proposed solution address all 3 top pain patterns from the research?
2. Is there anything in the research that the spec ignores or underweights?
3. Are success metrics tied to what users said they need — or to what we want to show?
4. Is v1.0 scope defensible given pain severity?
5. Any acceptance criteria that aren't actually testable?

Output:
- Verdict: ship as-is | revise before review
- If revise: exactly 3 specific improvements (concrete, not vague)
- Gaps between research and spec (where the spec doesn't solve the pain)
- One thing that's genuinely good
```

**Why Step 4 matters:** This is the most common PRD failure mode. A spec that's technically complete and internally consistent but doesn't solve the actual pain the research found. The /review step with explicit access to both the spec AND the research catches this before it goes to the engineering team.

---

## Handing Context Between Sessions

### The Core Problem

Sessions don't share memory. Everything Claude knows comes from: (1) what's in CLAUDE.md, and (2) what you reference in the current session. If you don't reference yesterday's research file, it doesn't exist for today's session.

This is a feature, not a bug. It forces you to be explicit about what context matters. Explicit context is better than implicit context.

### The File-as-Memory Pattern

Every session produces a file. Every subsequent session reads the relevant file.

```
Session A: research session
→ Output: research/user-pain/feature-x-research.md

Session B: analysis session
→ Reads: research/user-pain/feature-x-research.md
→ Output: research/user-pain/feature-x-jtbd.md

Session C: spec writing session
→ Reads: research/user-pain/feature-x-jtbd.md
→ Output: specs/feature-x-prd.md

Session D: review session
→ Reads: specs/feature-x-prd.md + research/user-pain/feature-x-research.md
→ No new output (or saves feedback to specs/feature-x-review-[date].md)
```

The files are your handoff protocol. You don't carry context in your head between sessions — the files carry it. This works across days, weeks, and multiple PMs on a team.

### Recommended Folder Structure

```
/[product-name]/
├── CLAUDE.md                         ← orchestration brain (100-150 lines max)
├── context/
│   ├── product-context.md            ← vision, OKRs, current focus
│   ├── personas.md                   ← user archetypes, JTBDs
│   ├── tone-and-voice.md             ← writing style rules
│   ├── prd-template.md               ← your standard PRD format
│   ├── decision-template.md          ← standard decision log format
│   └── examples/
│       ├── good-prd.md               ← gold standard for output quality
│       └── good-user-story.md
├── notes/
│   ├── index.md                      ← auto-maintained list of all files
│   └── [date]-[topic].md             ← session working notes
├── research/
│   ├── competitive/
│   │   ├── competitor-a-[date].md
│   │   └── synthesis-[date].md
│   ├── user-pain/
│   │   ├── feature-x-research.md
│   │   └── feature-x-jtbd.md
│   └── support/
│       └── [exports].csv
├── specs/
│   └── feature-x-prd.md
├── decisions/
│   └── 2026-q1-[topic].md
└── prompts/                          ← saved reusable workflows
    ├── sprint-health-check.md
    └── competitive-update.md
```

### Why Files Beat Conversation History

| | Files | Conversation History |
|---|---|---|
| Survives session end | Yes | No |
| Shareable with team | Yes | No |
| Searchable | Yes | No |
| Reference-able in next session | Yes | No |
| Editable | Yes | No |
| Version-controlled (Git) | Yes | No |
| Context-efficient | Yes (load only what's needed) | No (loads everything) |

The conversation history is a scratchpad. Files are your knowledge base.

---

## The Review Loop

### The Most Underused PM Technique

After writing any major output, ask Claude to review it from a specific perspective. This is the mini-ReviewArena — one extra prompt that approximates what engineering harnesses do with 5 parallel reviewers.

**Basic review prompt (any output):**

```
You just wrote [spec / analysis / strategy document].

Now read it as the skeptical VP who has to approve it.

Ask:
1. What are the 3 weakest points?
2. Where would a senior engineer push back?
3. What assumption is most likely to be wrong?
4. What critical information is missing?

Be direct. Don't soften the feedback.
```

**Persona-specific review prompts:**

```
# VP Product review
Read this PRD as the VP of Product approving the Q3 roadmap.
You care about: strategic fit with OKRs, resource allocation defensibility, customer impact.
What are your 3 concerns?

# Engineering lead review
Read this PRD as the engineering lead who has to staff and scope it.
You care about: scope clarity, acceptance criteria testability, dependencies called out.
What's missing or unclear?

# Customer review
Read this PRD as the power user who will be most affected.
You care about: does this solve the actual problem, are there edge cases you haven't thought of.
What would frustrate you about this?
```

**The ReviewArena approximation (one extra prompt):**

```
/devil

Read: specs/[feature-name]-prd.md

You are reviewing this as all three of these people simultaneously:
1. The VP of Product who will approve it (strategic + resource view)
2. The engineering lead who will build it (scope + technical view)
3. The power user who will use it (usability + job-fit view)

Output: the 3 most important objections across all three perspectives.
For each: which perspective it comes from, the specific problem, what would address it.
```

This approximates a multi-agent review in one prompt. Not as thorough as 5 parallel agents — but it takes one extra minute and catches most of the critical gaps.

---

## Session Hygiene Rules

### Rule 1: One Task, One Session

A task is a deliverable, not a topic. "Q3 planning" is a topic. "Write the discovery brief for Initiative A" is a task.

One task per session means:
- Context stays relevant and high-quality throughout
- Your output is a single file (or a small set of related files)
- The session ends when you have what you set out to get

**When to start a new session:**
- You've completed the task you set out to do
- You realize you're on topic #2 or #3 in the same session
- The session has been going for 90+ minutes on complex reasoning
- Output quality is noticeably degrading

### Rule 2: Scope to Deliverable at Session Start

Start every session with an explicit scope statement. This loads the right framing and prevents drift.

```
This session: write the user research synthesis for the bulk export feature.
Input: research/interviews/ and research/support/bulk-export-tickets.csv
Output: research/user-pain/bulk-export-synthesis.md
We're done when that file is saved and I've confirmed it.
```

That's 4 lines. It takes 30 seconds. It prevents 60-minute sessions that drift and end without a clear output.

### Rule 3: Rolling Summaries for Long Strategy Sessions

Some work genuinely requires extended sessions — multi-day strategic planning, complex architecture decisions, long-form research synthesis.

For these, use the rolling summary pattern:

```
[At any natural breakpoint in a long session]

Summarize the decisions and key findings from this session so far.
Format: 5 bullets, each one a decision or confirmed finding.
I'll use this to resume cleanly in a new session if needed.
```

Then, when starting the next session:

```
Continuing the Q3 strategic planning work.
Summary of what we've decided so far: [paste 5-bullet summary]
Today's task: [specific deliverable]
```

The summary is your checkpoint. It costs 30 seconds to generate and saves you from losing an hour of context.

---

## The PM Harness Concept

### What Engineering Harnesses Do

Engineers solved the context management and quality gate problems first. A harness (like cc10x) does 4 things:

1. **Router:** reads your intent and routes to the right specialist
2. **Specialists:** separate agents with defined roles, memory, and toolsets
3. **Quality gates:** each agent's output is checked before the next agent runs
4. **Memory:** learnings persist across sessions in structured files

The result: you don't manage the workflow manually. The harness manages it. You just work.

### The PM Equivalent (No Installation Required)

You can implement the same 4 principles in CLAUDE.md without installing anything:

**Router → CLAUDE.md context loading sequence + agent roles**

Your CLAUDE.md is your router. When you open a session, it loads product context, then reads what you're asking, and routes to the right agent role. Not as automated as cc10x's router — but functionally equivalent.

```markdown
## Context Loading Sequence (ALWAYS run first)
Before ANY task:
1. Read context/product-context.md
2. Read context/personas.md
3. Search notes/ for relevant context for this task
4. Check decisions/ for any decisions relevant to this work

## Intent Routing
When I type /spec: → activate Spec Writer role (see Agent Roles)
When I type /research: → activate Researcher role
When I type /data: → activate Data Analyst role
When I type /devil: → activate Skeptical VP role
When I type /review: → activate Peer Reviewer role
```

**Specialists → Agent role definitions in CLAUDE.md**

You defined these in the Agent Roles section above. Each one is a specialist. Each has a defined persona, rules, output format, and output location.

**Quality gates → The /review step in every pipeline**

After any major output: run the /review agent or a /devil check. This is your manual quality gate. The output either passes or you get specific improvements. It's not automated — but it's the same function: don't ship bad output.

Add this to CLAUDE.md as a rule:

```markdown
## Quality Gate Rule
After any /spec output: before marking done, run /review
After any /research output: before using in a spec, run /devil
After any strategic recommendation: run /devil with VP perspective
```

**Memory → File structure + rolling summaries**

The `context/`, `notes/`, `research/`, `specs/`, `decisions/` folder structure is your memory system. CLAUDE.md tells Claude to load the right files at the start of every session. This is the same pattern engineering harnesses use — structured files that persist across sessions.

Add to CLAUDE.md:

```markdown
## Memory System
After any session that produces a new file:
1. Append one line to notes/index.md:
   [filename] | [one sentence description] | [date]
2. If a product decision was made: log it using /decision
3. If this session produces context that will matter next week:
   save a summary to notes/[date]-[topic]-summary.md
```

### The Full PM Harness (CLAUDE.md Architecture)

Putting it all together, a Level 3 CLAUDE.md implements all four harness components:

```markdown
# [Product Name] — PM Orchestration System

## Context Loading Sequence           ← ROUTER (context layer)
Before ANY task:
1. Read context/product-context.md
2. Read context/personas.md
3. Search notes/ for relevant context
4. Read context/tone-and-voice.md
5. Check decisions/ for relevant prior decisions

## Agent Roles                        ← SPECIALISTS
[/research, /spec, /data, /devil, /review, /decision definitions]
[See full definitions above]

## Quality Gate Rules                 ← QUALITY GATES
After /spec: run /review before marking done
After major analysis: run /devil
After any session decision: log with /decision

## Memory System                      ← MEMORY
[Rolling summary rules]
[notes/index.md maintenance]
[Decision logging rules]

## Anti-Patterns                      ← QUALITY CONSTRAINTS
NEVER start output with "Certainly!" or "Great!" or "Sure!"
NEVER use passive voice in spec writing
NEVER write generic user stories without product-specific context
NEVER pad output with "in conclusion" paragraphs
NEVER add caveats that dilute a direct recommendation

## Session Scope Rule
If we've covered 3+ different topics: flag it and suggest a new session.
```

This CLAUDE.md — roughly 100-150 lines — gives you a PM orchestration system that approximates what engineered harnesses do. No plugins. No installation. Just a well-structured text file.

---

## Putting It All Together: Your First Orchestrated Week

**Day 1 (2 hours): Upgrade CLAUDE.md**

- Add the context loading sequence
- Add the 6 agent role definitions
- Add quality gate rules
- Add memory system instructions
- Add anti-patterns
- Create the folder structure

**Day 2 (1 hour): Run your first pipeline**

- Pick a feature in active discovery
- Run the 4-step PRD pipeline (one step per session)
- Experience the handoff: each step reads the previous file

**Day 3 (1 hour): Wire one compound MCP workflow**

- Pick Workflow 1 (Sprint Health Check) or Workflow 2 (Competitive Intelligence)
- Run it end-to-end
- Save as a prompt file in `prompts/`

**Day 4 (30 min): Run /devil on something you're about to present**

- Take a spec or analysis you're presenting this week
- Run the /devil check
- Address the objections before the meeting

**Day 5: Review what changed**

- What output quality was noticeably different?
- What still felt manual or repetitive?
- What would you add to CLAUDE.md based on this week?

Orchestration is compounding. Week 2 is faster than week 1 because the files from week 1 are the context for week 2.

---

## Quick Reference

### The 6 Agent Roles

| Role | When to Use | Core Behavior |
|------|-------------|---------------|
| `/research` | Before any spec work | Every claim needs evidence. Flag uncertainty. |
| `/spec` | Writing PRDs | Senior PM, grounded in research, uses template |
| `/data` | Analyzing metrics | Lead with decision, not finding |
| `/devil` | Before any review meeting | Exactly 3 objections, VP perspective |
| `/review` | After any major output | Checks spec vs. original pain |
| `/decision` | After any product decision | Logs with rationale, alternatives, review trigger |

### The 4 Orchestration Principles

| Principle | Engineering Harness | PM Implementation |
|-----------|--------------------|--------------------|
| Router | Automatic intent detection | CLAUDE.md context loading sequence |
| Specialists | Separate agents with roles | Agent role definitions in CLAUDE.md |
| Quality gates | RouterContract per step | /review and /devil after major outputs |
| Memory | Structured memory files | File structure + index.md maintenance |

### The Session Hygiene Checklist

Before starting a session:
- [ ] Do I know the single deliverable this session produces?
- [ ] Do I know which files to reference at session start?
- [ ] Is this a new session (not continuing a stale 3-hour one)?

After completing a session:
- [ ] Did I save the output to the right folder?
- [ ] Did I append a line to notes/index.md?
- [ ] Were any decisions made? If yes: /decision to log them.
- [ ] What does the next session need to read?
