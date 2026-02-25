# Prompt 01 — Research (The Crisis)

## What This Is
The first prompt you paste into Claude Code during the demo.
It reads like a real PM message — natural language, no slash commands.
The router will detect RESEARCH intent automatically.

## The Prompt (copy everything between the lines)
---
Just got this Slack from the VP of Sales:

"Head's up — ClickUp announced this morning that Gantt charts and Timeline views are now free for ALL plans, including their free tier. We already have two enterprise prospects mentioning it in email threads. I had a call with Meridian Financial (340 seats, renewal in 8 weeks) and they asked me directly: 'What's monday.com's answer to this?' I need talking points by noon. Also saw that Northgate Healthcare and Constellation Brands are asking similar questions in their CSM threads."

I need to understand what's actually going on here before I can respond. Specifically:
- What exactly did ClickUp ship? Is this real feature parity or marketing?
- What do our enterprise customers actually need from Timeline/Gantt that we don't have today?
- What's the real competitive gap — and is it solvable?
- How urgently do we need to respond?

Can you research this and give me a synthesis of what we know?
---

## What the Router Should Do
Detect RESEARCH intent from: "research this," "give me a synthesis," "what do customers actually need," "what exactly did ClickUp ship."
Route to: RESEARCH workflow (research-synthesizer agent).

## What to Watch For
- The router routing automatically — point this out when it happens: "I didn't tell it to do research."
- The workflow creating tasks you can track with `tasks` command
- The synthesis output grading evidence: Strong/Medium/Weak
- Competitive feature comparison table (monday.com vs. ClickUp)
- "What we still don't know" section at the end
- Router Contract YAML at the end with STATUS: SYNTHESIS_COMPLETE

## Fallback
If cc-p4p routes incorrectly, rephrase:
"I need competitive research on ClickUp's new Gantt feature and what enterprise customers need from timeline management tools."
