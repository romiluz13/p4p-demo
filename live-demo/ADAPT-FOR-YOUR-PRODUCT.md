# Run cc-p4p On Your Own Product

You watched the demo. Now do it for your company. Here's how.

---

## What You're Adapting

The system runs on context — not on monday.com. Swap three things and the entire pipeline works for your product:

```
live-demo/
├── CLAUDE.md              ← The brain — swap company info here
├── context/
│   ├── product-context.md ← Your product, OKRs, metrics
│   ├── personas.md        ← Your actual customers
│   └── tone-and-voice.md  ← Your writing style
└── prompts/               ← The 5 prompts — adapt the scenario
```

---

## Step 1 — Replace the Company Context

**`live-demo/CLAUDE.md`**

Keep the structure. Replace the content:

| Replace this | With this |
|-------------|-----------|
| `monday.com` | Your company name |
| `$900M ARR`, `225,000+ customers` | Your actual metrics |
| The Crisis Context section | Your current competitive or strategic situation |
| The workflow trigger words | Keep as-is — they're universal |

**`live-demo/context/product-context.md`**

Replace:
- Product vision → your actual vision
- OKRs → your current quarter's OKRs
- Key metrics (Timeline WAU 34%, win rate 54%) → your baselines
- Q3 backlog items → your actual backlog

**`live-demo/context/personas.md`**

Replace Marcus, Sarah, and Rebecca with:
- 2-3 of your real customer profiles (name, company, seat count, renewal date, specific pain)
- The more specific, the better. "Enterprise customer" is not a persona.

**`live-demo/context/tone-and-voice.md`**

Replace with your writing style rules. Or keep it — the rules are generic enough.

---

## Step 2 — Open Claude Code

```bash
cd p4p-demo/live-demo
claude
```

Verify context loaded:
```
What do you know about our product?
```
Should describe YOUR company from what you put in CLAUDE.md. If it does, you're ready.

---

## Step 3 — Run the Prompts in Order

Each prompt file in `prompts/` has the actual prompt between the `---` lines.
Copy everything between those lines. Adapt the bolded parts. Paste into Claude Code.

---

### Prompt 01 — Research (`prompts/01-research.md`)

Swap:
- **ClickUp** → your competitor
- **Gantt charts and Timeline views are now free** → what they actually announced
- **Meridian Financial (340 seats, renewal in 8 weeks)** → one of your real accounts
- **Northgate Healthcare and Constellation Brands** → two more of your accounts
- **Timeline/Gantt** → the feature area being threatened

Paste. Wait. The router routes to RESEARCH automatically — you didn't use a slash command.

---

### Prompt 02 — Spec (`prompts/02-spec.md`)

Swap:
- **Advanced Timeline View** → your feature response name
- **dependency management, critical path, bulk editing** → your specific capabilities
- **Marcus at Meridian Financial** → your customer from prompt 01
- **enterprise-only** → your target tier

The spec-writer reads what RESEARCH just wrote to `insights.md`. The Problem Statement will quote your competitive situation and customer pain — not because you pasted it, because the system reads its own memory.

---

### Prompt 03 — Roadmap (`prompts/03-roadmap.md`)

Swap:
- The **5 competing items** → your actual backlog items with honest effort estimates
- **Q3 total capacity: ~14 weeks** → your actual eng capacity this quarter
- Keep the framing: "RICE score each item. Show me trade-offs. Be honest about assumptions."

---

### Prompt 04 — Communicate (`prompts/04-communicate.md`)

Swap:
- **VP of Sales** → your actual stakeholder
- **Marcus at Meridian Financial** → your customer
- **ClickUp's free Gantt** → your competitive threat
- The system reads `roadmap-state.md` before writing — it won't promise a date that isn't in the roadmap.

---

### Prompt 05 — Metrics (`prompts/05-metrics.md`)

Swap:
- The three lagging metrics → your actual success metrics with real baselines
- **Marcus, Sarah, and Rebecca** → your named accounts
- Keep the questions — they're universal for any feature launch.

---

## What to Expect

After each prompt, you'll see:
1. The router classifying intent — no slash command needed
2. The workflow running — 1-3 minutes
3. Output saved to `docs/` — grounded in your context
4. A Router Contract YAML at the bottom — the system's handoff to the next workflow

After all 5: you have a competitive analysis, a PRD, a prioritized roadmap, stakeholder communications, and a metrics framework — all cross-referencing each other, all in your voice, all about your actual product.

---

## The One Thing That Breaks It

Vague context. If your `personas.md` has "enterprise customers who want productivity" — the outputs will be generic. If it has "James Park, VP Engineering at Acme Corp, 280 seats, renewal March 15, specific pain: no audit trail for compliance" — the outputs will be something you could actually use tomorrow.

The system is only as specific as what you put in.
