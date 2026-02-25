# Prompt 02 — Spec

## What This Is
The second prompt. Triggers after RESEARCH has completed and written to insights.md.
Natural language. The router detects SPEC intent.
SPEC will read insights.md before writing — this is the memory threading moment.

## The Prompt (copy everything between the lines)
---
Based on what we just found, I want to define our response feature.

The core gap is clear: our Timeline View doesn't support dependency management. Marcus at Meridian Financial has a renewal in 8 weeks. Sarah's team at Constellation is manually working around this. ClickUp's free Gantt has dependency arrows. We don't.

I want to spec "Advanced Timeline View" — a significant upgrade to our existing Timeline that adds:
- Dependency management (task A must complete before task B can start)
- Critical path visualization
- Bulk dependency editing
- Probably enterprise-only given the complexity

Pull in the research findings. I want the spec grounded in actual evidence — the customer pain, the competitive gap, the enterprise-specific requirements. Not assumptions.

Can you write the full PRD? P0/P1/P2 cut. Success metrics with baselines. I want it to be the thing I could actually take to engineering tomorrow.
---

## What the Router Should Do
Detect SPEC intent from: "write the full PRD," "want to spec," "spec this," "define our response feature."
Route to: SPEC workflow (spec-writer agent).
The spec-writer MUST read insights.md before writing the Problem Statement.

## What to Watch For
- SPEC reads insights.md at the start — look for memory load in output
- Problem Statement references the ClickUp competitive announcement
- User stories use persona names: Marcus, Sarah, Rebecca
- Success metrics include baselines from product-context.md
  (Timeline WAU: 34%, enterprise win rate vs ClickUp: 54%)
- Router Contract YAML at end with STATUS: SPEC_COMPLETE and file path
- File saved to docs/specs/

## The Key Demo Moment (Beat 6)
After the spec appears, scroll to the Problem Statement or User Stories section.
Find where it references:
- ClickUp's competitive announcement
- Marcus's Meridian Financial renewal situation
- The "34% Timeline WAU" or "54% win rate" baseline metrics

Point at those references and say:
"That came from the RESEARCH workflow. Three minutes ago. SPEC read insights.md.
I didn't paste anything."

## Fallback
If spec doesn't reference research findings, add:
"Make sure to ground the Problem Statement in the competitive research we just ran —
specifically the ClickUp Gantt feature gap and the enterprise customer pain from
Marcus, Sarah, and Rebecca."
