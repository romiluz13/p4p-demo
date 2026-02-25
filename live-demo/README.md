# live-demo — Monday.com Competitive Crisis Scenario

A monday.com PM wakes up to a VP Sales Slack: "ClickUp just made Gantt free for all plans. Enterprise prospects are asking what our answer is. I need talking points by noon."

Five workflows handle it: research → spec → roadmap → communicate → metrics.

---

## What's in This Folder

```
live-demo/
├── CLAUDE.md                    ← The brain — monday.com context + agent rules
├── ADAPT-FOR-YOUR-PRODUCT.md   ← How to run this on your own product
│
├── context/
│   ├── product-context.md      ← Vision, OKRs, metrics, Q3 backlog
│   ├── personas.md             ← Three named enterprise personas
│   └── tone-and-voice.md       ← monday.com PM writing style
│
└── prompts/
    ├── 01-research.md          ← Triggers RESEARCH workflow
    ├── 02-spec.md              ← Triggers SPEC workflow
    ├── 03-roadmap.md           ← Triggers ROADMAP workflow
    ├── 04-communicate.md       ← Triggers COMMUNICATE workflow
    └── 05-metrics.md           ← Triggers METRICS workflow
```

---

## Run the Demo

```bash
cd p4p-demo/live-demo
claude
```

Open each prompt file in `prompts/`. Copy the text between the `---` lines. Paste into Claude Code. Run them in order.

Watch the system route to the right workflow automatically — no slash commands.

---

## Run It on Your Own Product

→ **[ADAPT-FOR-YOUR-PRODUCT.md](ADAPT-FOR-YOUR-PRODUCT.md)**
