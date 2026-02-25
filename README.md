# cc-p4p — Claude Code for Product Managers

A PM-grade orchestration system built on Claude Code. It detects workflow intent from natural language, passes structured memory between workflows, and validates handoffs with a machine-readable contract.

This repository contains a complete live demo and a getting-started guide.

---

## What It Does

You type a natural-language PM request. The system:

1. **Detects intent** — routes to the right workflow (Research, Spec, Roadmap, Communicate, Metrics) without slash commands or prompt engineering
2. **Executes** — runs a specialized agent with PM-grade output (evidence-graded research, P0/P1/P2 specs, RICE-scored roadmaps)
3. **Remembers** — writes structured findings to persistent memory files that the next workflow reads automatically
4. **Validates** — ends every workflow with a machine-readable Router Contract so the system knows what it can and can't do next

---

## The Demo

A Monday morning competitive crisis — handled in 9 minutes.

> *"ClickUp just announced free Gantt charts and timeline views for ALL plans. Enterprise prospects are asking what monday.com's answer is. I need talking points by noon."*

Five workflows run in sequence: competitive research → feature spec → Q3 roadmap prioritization → sales talking points + customer message → success metrics framework.

The outputs in `example-session/` were generated live in one session. Nothing was written by hand.

**→ [Run it yourself](live-demo/README.md)**

---

## Run the Demo in 3 Steps

```bash
# 1. Install Claude Code
npm install -g @anthropic-ai/claude-code

# 2. Clone this repo and navigate to the demo
git clone https://github.com/romiluz13/p4p-demo
cd p4p-demo/live-demo

# 3. Open Claude Code — context loads automatically
claude
```

Then paste the prompts from `live-demo/prompts/` one at a time. Watch the system route, execute, and remember across workflows.

---

## Use cc-p4p on Your Own Product

The demo runs on pre-loaded monday.com context. To run on your product:

```bash
# 1. Install Claude Code
npm install -g @anthropic-ai/claude-code

# 2. Install the cc-p4p plugin
# Inside Claude Code, run both:
/plugin marketplace add romiluz13/cc-p4p
/plugin install cc-p4p@romiluz13

# 3. Set up your context
# Copy live-demo/CLAUDE.md → replace monday.com content with your product
# Copy live-demo/context/ → replace with your personas, metrics, OKRs

# 4. Open Claude Code and start working
claude
```

Plugin source: [github.com/romiluz13/cc-p4p](https://github.com/romiluz13/cc-p4p)

---

## What's in This Repo

```
p4p-demo/
├── live-demo/          Run the Monday.com crisis demo yourself
│   ├── CLAUDE.md       The brain — pre-loaded company context
│   ├── context/        Product metrics, personas, OKRs
│   └── prompts/        5 natural-language prompts, one per workflow
│
├── example-session/    Real outputs from a complete session
│   ├── 01-competitive-synthesis.md
│   ├── 02-advanced-timeline-prd.md
│   ├── 03-q3-roadmap-rice.md
│   ├── 04-sales-talking-points.md
│   ├── 05-marcus-chen-message.md
│   └── 06-metrics-framework.md
│
└── guide/              Go deeper — installation, MCPs, CLAUDE.md templates, real stories
```

---

## Use It on Your Own Product

Everything runs on your context, not training data.

**→ [Step-by-step adaptation guide](live-demo/ADAPT-FOR-YOUR-PRODUCT.md)**

Short version:
1. Replace `live-demo/CLAUDE.md` with your company's product context
2. Update `live-demo/context/` with your personas, metrics, and OKRs
3. Adapt the prompts in `live-demo/prompts/` — swap monday.com for your scenario
4. Run `claude` — the system routes and remembers using your knowledge base

---

## Learn More

- `guide/01-getting-started.md` — install, first session, what to set up
- `guide/03-claude-md-for-pms.md` — 3-level CLAUDE.md template system
- `guide/04-skills-and-plugins.md` — the full PM skill ecosystem
- `guide/09-real-stories.md` — practitioners using this in production

---

*Built with [Claude Code](https://claude.ai/claude-code) + the cc-p4p plugin.*
