# Prompt 05 — Metrics (Optional Follow-up)

## What This Is
Optional fifth prompt. Use this if you have time after Beat 10 or for extended Q&A.
Shows the METRICS workflow. The router detects METRICS intent.
Especially useful when audience asks "but how do you know if it worked?"

## The Prompt (copy everything between the lines)
---
Before we ship this, I want to make sure the success metrics in the spec are actually measurable.

The metrics I have right now:
- Timeline View WAU in enterprise from 34% to 52%
- Enterprise win rate vs ClickUp from 54% to 65%
- Enterprise annual churn from 8.4% to under 7%

A few things I'm not sure about:
- These are all lagging indicators. What tells us within 2-4 weeks of launch whether this is working?
- What guardrail metrics should I set — what should NOT change after we ship?
- We're building this specifically because of Marcus, Sarah, and Rebecca. How do we track whether we actually retained those three accounts?
- Is there a leading signal from Timeline feature adoption that predicts churn reduction?

Give me the full metrics framework — leading, lagging, account-specific, and guardrails.
---

## What the Router Should Do
Detect METRICS intent from: "success metrics," "guardrail metrics," "how do we know if it worked," "leading indicators."
Route to: METRICS workflow (in-context — may be handled inline).

## What to Watch For
- Clear distinction between leading indicators (2-4 weeks) and lagging indicators (60-90 days)
- Baselines from product-context.md matched to each metric
- Guardrail metric for each success metric (what should NOT move)
- Account-specific tracking for Marcus, Sarah, Rebecca
- Connection back to Q3 OKRs
- Router Contract at end with STATUS: METRICS_DEFINED

## Expected Output Shape

The response should include something like:

**Leading indicators (visible 2-4 weeks post-launch):**
- Dependency feature adoption: % of enterprise accounts creating their first dependency within 14 days
  - Baseline: 0% (feature doesn't exist)
  - Target: 45% of enterprise accounts within 14 days
  - Guardrail: if <20% after 14 days, trigger onboarding intervention

**Lagging indicators (visible 60-90 days post-launch):**
- Timeline View WAU: from 34% to 52% (enterprise tier)
- Enterprise win rate vs. ClickUp: from 54% to 65% in competitive deals
- Enterprise annual churn: from 8.4% to <7%

**Account-specific (Marcus, Sarah, Rebecca):**
- 30-day: all three accounts have created at least one dependency in their boards
- 60-day: all three have not initiated churn or downgrade conversation
- 90-day: all three accounts have renewed or are in active renewal discussion

**Guardrail metrics (should not change):**
- Non-enterprise Timeline WAU: should not decrease (feature complexity shouldn't hurt SMB users)
- Board creation rate: should not decrease (dependency UI shouldn't add friction to core creation flow)

## Timing
Use this if:
- You finish Beat 10 with 90+ seconds to spare
- Audience asks "how do you measure whether this worked?"
- You want to show a 5th workflow in an extended demo slot
