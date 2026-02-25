# Example Session — Generated Live, Not Written by Hand

These 6 files are the output of a single cc-p4p session running the Monday.com competitive crisis scenario.

One session. One VP of Sales Slack. Everything below is what the system produced.

---

## What Was Generated

| File | Workflow | Description |
|------|----------|-------------|
| `01-competitive-synthesis.md` | RESEARCH | ClickUp competitive analysis — 5 themes, evidence grades, 3 account urgency profiles |
| `02-advanced-timeline-prd.md` | SPEC | Full PRD — P0/P1/P2 cut, acceptance criteria, 7 success metrics with baselines |
| `03-q3-roadmap-rice.md` | ROADMAP | RICE scoring for 5 competing items, capacity math, explicit assumption flags |
| `04-sales-talking-points.md` | COMMUNICATE | Objection handling for sales reps — specific language for calls |
| `05-marcus-chen-message.md` | COMMUNICATE | Peer message to a named enterprise account at renewal risk |
| `06-metrics-framework.md` | METRICS | 6 leading indicators, 5 lagging, 6 guardrails, 3-account tracking plan, churn predictor |

---

## What to Notice

**The spec quotes the research.** Open `02-advanced-timeline-prd.md` and find the Problem Statement. It cites "34 enterprise NPS verbatims Q4 2025" and the "10pp win rate drag." Those numbers came from `01-competitive-synthesis.md` — written by the RESEARCH workflow, read automatically by the SPEC workflow. Not pasted. Not repeated in the prompt.

**The roadmap uses the spec.** `03-q3-roadmap-rice.md` scores Advanced Timeline View (RICE 240) using effort estimates from the spec's P0/P1/P2 breakdown. The system read the spec before scoring.

**The customer message doesn't promise what isn't confirmed.** `05-marcus-chen-message.md` is deliberately hedged on beta scope because the roadmap had 3 unresolved open decisions. The system knew not to commit.

**The metrics framework references the personas by name.** Marcus, Sarah, and Rebecca each have specific tracking gates and monitoring flags — because METRICS read the personas from memory, not from the prompt.

---

## How to Reproduce This

```bash
cd p4p-demo/live-demo
claude
```

Paste the prompts from `prompts/` one at a time. The session will produce outputs like these — grounded in the context you loaded, not in what the model already knows.
