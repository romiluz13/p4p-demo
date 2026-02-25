# monday.com — PM Tone and Voice Guide

## Who We Write For

Enterprise buyers are smart, pressed for time, and skeptical of vendor communications. They've seen overpromising roadmaps. They've been burned by "coming Q3" that became "coming Q1 next year."

Internal stakeholders — executives, sales teams, engineering — are equally busy and equally skeptical of PM documents that bury the lead.

Our readers decide things. Give them the information they need to decide, then stop.

---

## The monday.com PM Voice

### In Specs and Research
- **Lead with the problem.** The solution is on page 3. The problem is on page 1.
- **Use numbers, not adjectives.** "34% of enterprise seats use Timeline weekly" not "Timeline has low adoption." "8-week renewal deadline" not "urgency."
- **Name the personas.** "Marcus's team manually tracks dependencies with a color legend" not "enterprise project managers have workarounds."
- **State what you don't know.** "We don't have data on whether ClickUp's dependency UI causes friction at scale — this is the highest assumption in the RICE score."
- **Evidence grade every claim.** Strong (3+ sources), Medium (1-2 sources), Weak (hypothesis).
- **Non-goals are not afterthoughts.** They are scope boundaries. Write them with as much care as requirements.

### In Customer-Facing Communication
- **Acknowledge before explaining.** They already know the problem. Tell them you know it too.
- **One timeline commitment max.** If you can't commit to a date, say "before your renewal" not "coming soon." Vague timelines land as broken promises.
- **Never use internal vocabulary externally.** Customers don't know what "P0 requirement" means. They don't know we use RICE. They don't need to.
- **One clear ask.** Every message ends with what you want them to do next. One thing, not three.
- **Concrete over warm.** "We're shipping dependency management in Advanced Timeline on August 15th — you'll be in the first beta cohort." Not "We really value your feedback and are working hard on this."

### In Roadmap and Prioritization Writing
- **Show the trade-off.** Never write a prioritization document that doesn't name what is NOT getting done. If everything is a priority, nothing is.
- **RICE is evidence, not verdict.** Write the score, then write the biggest assumption that could move it up or down. That's the real value.
- **Flag capacity constraints honestly.** "This requires 2 senior engineers for 6 weeks — that means AI Templates moves to Q4" is useful. "Capacity allows for this" is not.
- **State which OKR this serves.** Every item in the roadmap should connect to a named Q3 OKR. If it doesn't, it belongs in the parking lot.

---

## Specific Style Rules

### Do Write This
- "Marcus's team at Meridian Financial has a renewal in 8 weeks." (Specific, named, concrete)
- "ClickUp's free Gantt is a perceived parity gap — not a technical parity gap. Their implementation lacks cross-board dependency tracking." (Evidence-graded claim with qualifier)
- "If we don't ship dependency support before Marcus's renewal, the probability of renewal drops from high to medium." (Honest assessment)
- "P0: Dependency arrows on Timeline. This is the minimum viable competitive response." (Directional, not vague)
- "Timeline View WAU: 34% today. Target: 52% by end of Q3." (Baseline AND target)
- "We don't know if dependency UI adds cognitive load to casual Timeline users. This is the #1 assumption in the impact score." (Explicit uncertainty)

### Don't Write This
- "Users are frustrated with the current experience." (Vague, unsourced)
- "This will significantly improve our competitive position." (Adjective, no quantification)
- "We plan to explore dependency management as a potential future feature." (Non-committal hedge)
- "As mentioned above" / "in conclusion" / "to summarize" (Padding)
- "Exciting new capabilities" / "powerful features" / "best-in-class" (Marketing speak in PM docs)
- "Leverage synergies" / "seamless experience" / "user-friendly" (Meaningless filler)

---

## Format by Audience

### Executive Brief (Rebecca or VP level)
- Format: 3 bullets max, then one ask
- Lead with: business impact (ARR at risk, deal count, timeline)
- No implementation details
- No feature specifications
- Target: 100-150 words maximum

Example structure:
```
Situation: [One sentence on the crisis]
Response: [One sentence on what we're doing]
Impact: [One sentence on expected outcome]
Ask: [One clear action you need from them]
```

### Sales Team Update
- Format: Situation → Objection handling script → Timeline commitment → Resources
- They need to walk into a customer call tomorrow. Give them talking points, not context.
- Include: what ClickUp has, what we have, what we're building, when it ships, what beta access looks like
- Include explicit language they can use with customers
- Target: 250-350 words

### Customer Communication (Marcus, Sarah, Rebecca)
- Format: We heard you → Here's what we're building → Here's when → Here's your next step
- Tone: Direct, credible, no over-promising
- Never: internal metrics, internal roadmap language, vague timelines
- Target: 175-225 words

### Product Team / Eng Brief
- Format: Context → Decision → Requirements outline → Open questions
- Include: Why now (the forcing function), what we're NOT building, dependencies
- More technical depth is fine
- Target: 500-800 words

---

## Evidence Standards

**Strong — state as fact:**
- 3+ independent customer quotes or data points
- Internal product analytics with statistical significance
- NPS verbatims with 10+ instances of the same theme

**Medium — state as "data suggests":**
- 1-2 customer accounts describing the same pain
- Competitive feature comparison without user validation
- Industry benchmark comparisons

**Weak — state as "hypothesis" or "we believe":**
- Single account feedback
- Inference from adjacent data
- Product manager's assumption without customer validation

**Rule:** If you don't know the evidence grade, default to Weak and say so. Never present a hypothesis as fact.

---

## Format Rules (Non-Negotiable)

- Success metrics always include: current baseline, target, measurement method, and evaluation date
- Acceptance criteria in Given/When/Then format — not bullet lists of vague behaviors
- "The system shows" not "users will be able to see" (present tense, product as subject)
- P0/P1/P2 labeling: P0 = cannot ship without, P1 = significant improvement, P2 = future consideration
- Router Contract at the bottom of every workflow output

## Anti-Patterns (Flag These Immediately)

- Opening a spec with the solution before the problem
- User stories without acceptance criteria
- Success metrics without baselines — "increase engagement" is never acceptable
- Non-goals section missing or empty
- Open questions without tagged owners
- Customer messages with internal jargon
- Roadmap items without OKR alignment
- Confidence scores above 80 without strong evidence to back them
