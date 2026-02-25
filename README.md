# cc-p4p — Claude Code for Product Managers

A PM orchestration plugin for Claude Code. You type a natural-language PM request. The plugin detects intent, routes to the right workflow, executes with PM-grade structure, and writes to persistent memory so the next workflow builds on what the last one found.

**Plugin source:** [github.com/romiluz13/cc-p4p](https://github.com/romiluz13/cc-p4p)

---

## The Demo

A Monday morning competitive crisis — handled in 9 minutes.

> *"ClickUp just announced free Gantt charts and timeline views for ALL plans. Enterprise prospects are asking what monday.com's answer is. I need talking points by noon."*

Five workflows run in sequence:

| # | Workflow | What happens |
|---|----------|-------------|
| 1 | RESEARCH | Synthesizes competitive gap with evidence grades. Writes findings to memory. |
| 2 | SPEC | Reads research automatically. Writes a full PRD grounded in that evidence. |
| 3 | ROADMAP | Reads the spec. RICE scores 5 competing items. Shows what fits in Q3. |
| 4 | COMMUNICATE | Reads the roadmap. Writes sales talking points + customer message with confirmed dates only. |
| 5 | METRICS | Defines leading/lagging indicators, guardrails, account-specific tracking. |

---

## Run It

```bash
# 1. Clone — this gives you the monday.com context, personas, prompts, everything
git clone https://github.com/romiluz13/p4p-demo
cd p4p-demo/live-demo

# 2. Install the cc-p4p plugin (run inside Claude Code)
/plugin marketplace add romiluz13/cc-p4p
/plugin install cc-p4p@romiluz13

# 3. Start the session
claude
```

Then open each file in `prompts/` and paste the text into Claude Code. Run them in order.

---

## What's in This Repo

```
p4p-demo/
└── live-demo/
    ├── CLAUDE.md           monday.com product context + cc-p4p plugin entry
    ├── context/
    │   ├── product-context.md   Vision, OKRs, metrics, Q3 backlog
    │   ├── personas.md          Three named enterprise personas
    │   └── tone-and-voice.md    monday.com PM writing style
    └── prompts/
        ├── 01-research.md       Paste → triggers RESEARCH workflow
        ├── 02-spec.md           Paste → triggers SPEC workflow
        ├── 03-roadmap.md        Paste → triggers ROADMAP workflow
        ├── 04-communicate.md    Paste → triggers COMMUNICATE workflow
        └── 05-metrics.md        Paste → triggers METRICS workflow
```

---

## Use It on Your Own Product

1. Replace `live-demo/CLAUDE.md` with your company's context (keep the `[CC-P4P]` block at the top)
2. Update `live-demo/context/` with your personas, metrics, and OKRs
3. Adapt the prompts in `live-demo/prompts/` to your scenario
4. Run `claude` from `live-demo/` — the plugin routes and remembers using your knowledge

The system is only as specific as what you put in. Generic personas → generic output. Named accounts with seat counts and renewal dates → output you could use tomorrow.
