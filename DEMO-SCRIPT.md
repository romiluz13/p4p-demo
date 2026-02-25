# cc-p4p Live Demo Script
### A monday.com PM handles a competitive crisis in ~9 minutes

---

## Pre-Demo Checklist

Before you start, verify:

```bash
# 1. Plugin installed?
/plugin list
# → should show cc-p4p

# 2. Navigate to the demo folder
cd p4p-demo/live-demo

# 3. Start Claude Code
claude
```

If cc-p4p is not installed:
```
/plugin marketplace add romiluz13/cc-p4p
/plugin install cc-p4p@romiluz13
```

---

## The Setup (30 seconds, say this out loud)

> "It's Monday morning. The VP of Sales just slacked us: ClickUp made Gantt free for all plans.
> Enterprise prospects are asking what our answer is. We need talking points by noon.
>
> I'm going to run five PM workflows — research, spec, roadmap, comms, metrics —
> all in natural language. No slash commands. Watch the system route automatically,
> remember what it learned, and build on its own outputs."

---

## Workflow 1 — RESEARCH (~2 min)

**What to show:** Open `live-demo/prompts/01-research.md`. Copy the text. Paste into Claude Code.

**What cc-p4p does:**
- Router detects RESEARCH intent from "research this," "synthesis," "what's going on"
- Routes to research-synthesizer agent
- Evidence-grades every finding (Strong / Medium / Weak)
- Saves findings to `.claude/cc-p4p/insights.md`

**What to point out to audience:**
- "I typed plain English. No slash commands."
- "Look at the Router Contract at the bottom — it's a machine-readable handoff to the next workflow."
- "The findings are being written to memory. The spec agent will read these automatically."

---

## Workflow 2 — SPEC (~2 min)

**What to show:** Open `live-demo/prompts/02-spec.md`. Copy. Paste.

**What cc-p4p does:**
- Router detects SPEC intent from "spec," "PRD," "write requirements"
- Spec-writer reads `insights.md` BEFORE writing a single requirement
- Problem Statement quotes specific evidence from the research
- P0 criteria in Given/When/Then format, metrics with baselines

**What to point out:**
- "It read the research from the last workflow automatically."
- "The Problem Statement is quoting Marcus from Meridian Financial. I never typed his name in this prompt."
- "This is memory across workflows, not context window."

---

## Workflow 3 — ROADMAP (~2 min)

**What to show:** Open `live-demo/prompts/03-roadmap.md`. Copy. Paste.

**What cc-p4p does:**
- Router detects ROADMAP intent from "prioritize," "Q3," "RICE," "trade-offs"
- Reads the spec before scoring
- RICE scores all 5 items, shows what fits in Q3, what gets bumped
- Saves prioritized state to `.claude/cc-p4p/roadmap-state.md`

**What to point out:**
- "RICE scoring is built into the cc-p4p framework. I didn't define the formula."
- "It explicitly says what gets bumped and why. That's real prioritization."
- "It wrote roadmap-state.md. The communicate workflow will read that and won't promise dates that aren't confirmed."

---

## Workflow 4 — COMMUNICATE (~1.5 min)

**What to show:** Open `live-demo/prompts/04-communicate.md`. Copy. Paste.

**What cc-p4p does:**
- Router detects COMMUNICATE intent from "write a message," "talking points," "stakeholder"
- Reads `activeContext.md` and `roadmap-state.md` before drafting
- Writes two distinct outputs: sales talking points + customer message
- Tone-matches: sales = tactical objection handling, customer = direct/respectful

**What to point out:**
- "It's writing to Marcus with a specific ship date. That date came from the roadmap we just built."
- "Tone is completely different for the two audiences. Sales team vs. enterprise customer."
- "This took 90 seconds. Writing this manually takes an hour."

---

## Workflow 5 — METRICS (~1.5 min)

**What to show:** Open `live-demo/prompts/05-metrics.md`. Copy. Paste.

**What cc-p4p does:**
- Router detects METRICS intent from "success metrics," "leading indicators," "guardrails"
- Separates leading (2-4 weeks post-launch) from lagging (months) indicators
- Every metric has baseline, target, measurement method, timeline
- Pairs every success metric with a guardrail metric
- Connects explicitly to Q3 OKRs from context

**What to point out:**
- "Account-specific tracking for Marcus, Sarah, Rebecca — the three accounts that drove this whole effort."
- "Guardrail metrics. It's telling us what should NOT change when we ship."
- "Five linked documents in under 10 minutes. All cross-referencing each other."

---

## The Close (30 seconds)

Show them the output files:
```
docs/
├── research/     ← competitive synthesis
├── specs/        ← PRD ready for engineering
└── roadmap/      ← RICE-scored Q3 plan

.claude/cc-p4p/
├── insights.md       ← accumulated research memory
├── roadmap-state.md  ← current priorities
└── activeContext.md  ← session memory
```

> "That's cc-p4p. Natural language in. PM-grade work out.
> And because it writes to structured memory, every workflow builds on the last.
> The spec knew what the research found. The comms knew what the roadmap confirmed.
> That's the compound effect."

---

## If Something Goes Wrong

**Plugin not routing?** Check that `CLAUDE.md` is in the directory you ran `claude` from.

**Output looks generic?** Run: `What do you know about our product?` — verify context loaded from `context/product-context.md`.

**Router Contract missing?** The workflow ran but wasn't using cc-p4p. Confirm plugin is installed: `/plugin list`.
