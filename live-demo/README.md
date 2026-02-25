# live-demo — Monday.com Competitive Crisis

A monday.com PM wakes up to a VP Sales Slack: "ClickUp just made Gantt free for all plans. Enterprise prospects are asking what our answer is. I need talking points by noon."

Five cc-p4p workflows handle it: research → spec → roadmap → communicate → metrics.

---

## Run It

```bash
cd p4p-demo/live-demo
claude
```

Open each file in `prompts/`. Paste the text into Claude Code. Run in order. Watch the plugin route automatically — no slash commands.

---

## What's Here

```
live-demo/
├── CLAUDE.md                    ← monday.com context + cc-p4p plugin entry
│
├── context/
│   ├── product-context.md       ← Vision, OKRs, metrics, Q3 backlog
│   ├── personas.md              ← Three named enterprise personas
│   └── tone-and-voice.md        ← monday.com PM writing style
│
└── prompts/
    ├── 01-research.md           ← Paste → RESEARCH workflow
    ├── 02-spec.md               ← Paste → SPEC workflow
    ├── 03-roadmap.md            ← Paste → ROADMAP workflow
    ├── 04-communicate.md        ← Paste → COMMUNICATE workflow
    └── 05-metrics.md            ← Paste → METRICS workflow
```

---

**→ [Presenter guide with timing and talking points](../DEMO-SCRIPT.md)**
