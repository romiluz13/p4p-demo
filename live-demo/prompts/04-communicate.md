# Prompt 04 — Communicate

## What This Is
The fourth prompt. Triggers after ROADMAP has completed and updated roadmap-state.md.
Natural language. The router detects COMMUNICATE intent.
COMMUNICATE reads activeContext.md, roadmap-state.md, and tone-and-voice.md.

## The Prompt (copy everything between the lines)
---
I need two things written:

**1. Sales team talking points for the VP of Sales**
She needs these by noon. Something the sales team can use in calls with prospects today when ClickUp's free Gantt comes up. Not a product essay — specific objection handling, what to say, what's coming and when.

**2. A message to Marcus at Meridian Financial**
He asked directly: "What's monday.com's answer to ClickUp's free Gantt?" He has a renewal in 8 weeks and 340 seats. I need a real response — not corporate-speak. Acknowledge the gap exists. Tell him what we're building and when. Give him a reason to stay. He's an IT director, not a marketer — he'll see through anything that isn't straight with him.

Use the timeline from the roadmap we just built. Don't promise anything that isn't in there.
---

## What the Router Should Do
Detect COMMUNICATE intent from: "write talking points," "need a message," "respond to," "draft something."
Route to: COMMUNICATE workflow (comms-drafter agent).
COMMUNICATE reads: roadmap-state.md (for timeline), insights.md (for specific customer pain),
tone-and-voice.md (for voice guidelines).

## What to Watch For
- Sales talking points: tactical, objection-specific, concrete timeline
- Marcus message: direct, acknowledges the gap honestly, references his specific renewal context
- Timeline references match what was in the roadmap output (from roadmap-state.md)
- No internal jargon in customer-facing message
- No overpromising — only what's confirmed in the roadmap
- Tone matches tone-and-voice.md (Marcus is an IT director, not a marketer)
- Router Contract at end

## Beat 10 Moment (Memory Threading)
After the messages appear, find where the Marcus message references:
- The specific timeline ("available in Q3" or a specific month)
- His renewal context ("your renewal in 8 weeks")

Point at the timeline reference and say:
"That date came from the roadmap that was just built."
"This message is grounded in four workflows of work — research, spec, roadmap, communicate."
"All of it in one session. Zero copy-paste between workflows."

## Fallback
If messages are too generic, add:
"For Marcus specifically: he told us he couldn't answer his board's question about
dependencies when ClickUp came up in the conversation. Reference that specific situation.
And reference the 8-week renewal window so the urgency is clear."
