# Prompt 03 — Roadmap

## What This Is
The third prompt. Triggers after SPEC has completed and saved to docs/specs/.
Natural language. The router detects ROADMAP intent.
ROADMAP reads the spec AND current roadmap-state.md.

## The Prompt (copy everything between the lines)
---
We have the spec. Now I need to figure out where Advanced Timeline lands in the Q3 roadmap.

Here's what's competing for engineering capacity:

1. **Advanced Timeline View** (the spec we just wrote) — estimated 8 weeks of eng work across 2 squads

2. **Enterprise Audit Logs** — security compliance requirement. Three enterprise accounts have explicitly said they can't sign a new contract without SOC 2-aligned audit logs. Combined deal value: ~$240k new ARR. Estimated: 4 weeks.

3. **AI-generated Board Templates** — smart onboarding that generates a custom board from a prompt. High marketing value. No customer is blocking on this. Estimated: 6 weeks.

4. **WorkDocs AI summaries** — AI-powered meeting note and status update generation inside WorkDocs. High NPS value, moderate retention signal. Estimated: 5 weeks.

5. **Custom Branding for enterprise** — white-label dashboards. 8 enterprise accounts have explicitly requested it. Moderate churn risk. Estimated: 3 weeks.

Q3 total capacity: ~14 weeks of senior eng time (two squads of 3 engineers each, 7-week sprint cycle).

Where does Advanced Timeline land? RICE score each item. Show me what fits in Q3, what doesn't, and what the trade-offs are. Be honest about the assumptions driving the scores.
---

## What the Router Should Do
Detect ROADMAP intent from: "where does this land," "Q3 roadmap," "RICE score," "trade-offs," "what fits in Q3," "competing for engineering capacity."
Route to: ROADMAP workflow (roadmap-planner agent).
The roadmap-planner reads the most recent spec AND roadmap-state.md.

## What to Watch For
- RICE scores with assumptions flagged for each item
- Explicit capacity math (14 weeks total — what fits?)
- Advanced Timeline vs. Enterprise Audit Logs tension (both address ARR)
- Explicit statement of what gets deferred and why
- Dependencies called out (e.g., does Advanced Timeline require a data model change?)
- Router Contract YAML at bottom — this is the Beat 9 reveal moment

## Beat 9 Moment (Router Contract)
After roadmap output appears, scroll to the very bottom.
The Router Contract YAML should look like:

```yaml
STATUS: ROADMAP_UPDATED
CONFIDENCE: 87
BLOCKING: false
REQUIRES_REMEDIATION: false
REMEDIATION_REASON: null
MEMORY_NOTES:
  recent_work: ["2026-02-23 Roadmap updated — Advanced Timeline Q3 prioritization - .claude/cc-p4p/roadmap-state.md"]
  decisions: ["Advanced Timeline: Q3 P0 given ClickUp competitive threat and Marcus renewal; Audit Logs: parallel sprint 1 given deal value; AI Templates: deferred to Q4"]
  references: ["Roadmap: .claude/cc-p4p/roadmap-state.md"]
```

Read aloud: "STATUS: ROADMAP_UPDATED. CONFIDENCE: 87. BLOCKING: false."
Then say: "This is not for you. This is for the next workflow — COMMUNICATE —
to validate before it starts. That's the protocol."

## Fallback
If roadmap output doesn't include trade-off reasoning, add:
"Which two items get deferred out of Q3 given 14 weeks of capacity?
Name them explicitly and explain why."
