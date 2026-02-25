# Compound MCP Workflows — When Tools Talk to Each Other

> The single MCP is a lookup tool. Multiple MCPs in sequence is intelligence.

---

## The Key Insight

Most PMs use MCPs the wrong way.

They install Jira MCP and ask "what's in my backlog?" They install Amplitude MCP and ask "how's retention?" Each MCP is a window into one data source. That's useful, but it's not why orchestration matters.

The insight is this: **Claude Code is the bridge between data sources**. You don't connect Jira to Amplitude. You write one prompt that tells Claude to read from Jira, then read from Amplitude, then synthesize across both. Claude is the intelligence layer. The MCPs are data sources.

A single MCP call is a lookup. When multiple MCPs feed into one Claude synthesis, you get something your tools can't give you independently: a cross-source answer.

> "Every MCP is a data source. Orchestration = combining data sources into insights."

---

## MCPs as Workflow Nodes

The mental model shift:

**Before (isolated MCP):**
```
PM → "Look at my Jira backlog" → [Jira MCP] → list of tickets
```

**After (MCP as workflow node):**
```
Sprint end trigger
    → [Jira MCP] reads Sprint N tickets
    → [Amplitude MCP] reads feature usage for shipped items
    → [Notion MCP / local files] reads PRD success metrics
    → Claude synthesizes: planned vs. built vs. used
    → Output: sprint health analysis saved to file
```

The difference isn't the tools — it's the prompt structure. You write one compound prompt with multiple sequential steps. Claude executes each step, carries the context forward, and synthesizes at the end.

---

## The 5 Canonical Workflows

### Workflow 1: Sprint Health Check

**MCPs required:** Jira + Amplitude (or any analytics MCP)
**What it does:** Three-way comparison — what you committed to, what you shipped, and what users actually used

**When to run it:** End of every sprint, before retrospective

**The core question this answers:** "Is what we shipped actually being used?"

**Full prompt:**

```
Sprint health check for Sprint [N].

Step 1 — What we committed:
Read Jira: all tickets in Sprint [N].
For each ticket: ID, title, type (feature/bug/chore), story points, final status (done / carried over / cancelled).
Calculate: completion rate (story points done / total committed).

Step 2 — What we shipped (features only):
Filter to tickets where type = "feature" and status = "done".
List the feature tickets. These are what we'll check usage on.

Step 3 — What was actually used:
For each shipped feature, read Amplitude:
- Usage event name: [list your key events per feature, e.g., "checkout_started" for checkout feature]
- Metric: 7-day active users after release date vs. 7-day baseline before release
- Release dates: [list per feature or pull from Jira ticket]

If a feature has no Amplitude event: flag as "no measurement set up."

Step 4 — Planned vs. built vs. used comparison:
For each feature:
- Read the success metric from specs/[feature-name]-prd.md (if exists)
- Compare actual Amplitude data to planned metric
- Status: on-track / off-track / no measurement / no spec metric

Step 5 — Diagnose off-track features:
For any feature where actual usage is <40% of expected (or where users = 0):
Write a 2-sentence hypothesis for why.
Tag each hypothesis:
- discovery gap (we built the wrong thing)
- execution gap (we built it, but it's broken or hard to find)
- adoption gap (it works, but users don't know about it or haven't tried it)
- measurement gap (the feature is being used but we're tracking wrong)

Step 6 — Save outputs:
Full analysis → notes/sprint-[N]-health-[date].md
Executive summary (5 bullets) → notes/sprint-[N]-exec-summary-[date].md
```

**Expected output structure:**

```
Sprint N Health Check — [date]

Completion Rate: 8/10 features delivered (80% of story points)
Carryover: 2 features moved to Sprint N+1

Feature Usage Summary:
✓ Checkout V2: 1,240 DAU (day 7) vs. 800 baseline (+55%) — ON TRACK
~ Dashboard Redesign: 340 DAU vs. 600 target (57%) — OFF TRACK
✗ Bulk Export: 12 DAU — no usage baseline set (MEASUREMENT GAP)

Off-Track Hypotheses:
Dashboard Redesign: adoption gap — new nav location not discoverable,
no in-app announcement sent. Mitigation: onboarding tooltip.
```

**Performance note:** This workflow makes 2-3 MCP calls (Jira, Amplitude, optionally Notion for specs). Run at sprint end when context is clean. Don't run mid-sprint with other heavy context loaded.

---

### Workflow 2: Competitive Intelligence Pipeline

**MCPs required:** Bright Data (scrape) + Notion or GitHub (save)
**What it does:** Automated competitive tracker that pulls real data, not summaries

**When to run it:** Monthly, or when a competitor ships something significant

**The core question:** "What did competitors ship this month and how does it affect us?"

**Full prompt:**

```
Competitive intelligence update for [month/date].
Run research on the following 3 competitors in parallel using separate tasks.

--- TASK A: [Competitor 1] ---
Sources to check:
1. Changelog: [URL]
2. G2 reviews (last 30 days): search via Bright Data MCP
3. Job postings: [URL]
4. Twitter/X or LinkedIn announcements: search for "[Company] launch|new|release" last 30 days

Extract:
- Features shipped (product moves)
- User complaints in new reviews (pain they haven't solved)
- Hiring signals (what engineering/product roles suggest about roadmap)
- Marketing positioning changes (any new messaging emphasis)

Save to: research/competitive/[competitor-1-slug]-[date].md

--- TASK B: [Competitor 2] ---
[Same structure as Task A with Competitor 2's URLs]
Save to: research/competitive/[competitor-2-slug]-[date].md

--- TASK C: [Competitor 3] ---
[Same structure as Task A with Competitor 3's URLs]
Save to: research/competitive/[competitor-3-slug]-[date].md

--- SYNTHESIS (after all three complete) ---
Read all three files above.
Read: research/competitive/synthesis-previous.md (if exists — for trend comparison)

Synthesize:
1. Feature moves table (this month): what each competitor shipped, by category
2. Convergent bets: features 2+ competitors are investing in (industry signal)
3. Divergent bets: things only one competitor is doing (strategic differentiation)
4. Competitive gaps for us: features they have that we don't — ranked by user complaint frequency
5. Our differentiation: things we have that they don't
6. 3 strategic questions this data raises for our roadmap

Save to: research/competitive/synthesis-[date].md
```

**Manual fallback (no Bright Data MCP):**

```
I've exported the following competitive data manually.
[Paste changelogs, G2 exports, job posting lists here]

Synthesize using the same structure:
1. Feature moves table
2. Convergent bets
3. Divergent bets
4. Our gaps (ranked by frequency in user feedback)
5. Our advantages
6. 3 strategic questions

Save to: research/competitive/synthesis-[date].md
```

**Scheduling note:** To run this automatically on the first Monday of each month, add a Claude Code automation hook (Settings → Hooks → Schedule). The hook can trigger the prompt file at a set interval.

---

### Workflow 3: User Research Synthesis

**MCPs required:** Support tool (Intercom/Zendesk/any) + survey tool (optional) + Notion (optional for output)
**What it does:** Pain cluster extraction from support and qualitative data → direct input for PRDs

**When to run it:** At the start of any discovery sprint, before quarterly planning

**The core question:** "What are users actually struggling with, and how do we frame it as JTBD?"

**Full prompt:**

```
User research synthesis for [product area / quarter].

Input sources:
1. Support tickets: [use [support tool MCP] or export to research/support/[filename].csv]
2. Survey responses (if any): [file path or paste]
3. Interview transcripts (if any): read all files in research/interviews/
4. App store reviews (if any): [scrape via Bright Data or paste]

Step 1 — Volume analysis:
Count tickets/responses per category (use tags if available, or infer from content).
Top 10 categories by volume.
For each: volume, MoM trend (if date data available), severity signal (escalations, repeat contacts).

Step 2 — Pain cluster extraction:
Cluster the top issues into 5-7 pain themes (higher-level than individual categories).
For each theme:
- Description: what the user is experiencing
- Frequency: how often it appears
- Severity: how painful it is (emotional language, workarounds, escalations)
- Best representative quote: exact text, with source ID
- Segment(s) most affected: if identifiable from data

Step 3 — JTBD extraction:
For the top 3 pain themes, write a canonical JTBD statement:

"When [situation where this pain occurs],
[user segment] wants to [what they're trying to accomplish],
so they can [the outcome they care about]."

Also identify:
- Current workaround: what users do today instead of the solution they want
- Hiring condition: what would make a user adopt a solution (what triggers the hire)
- Frequency: how often this job comes up per user

Step 4 — Roadmap gap flags:
Read: roadmap/[current-roadmap-file].md (or context/product-context.md for OKRs)
For each of the top 5 pain themes: is it on the roadmap?
- Yes: note the initiative name
- Partial: note what's covered and what isn't
- No: flag as roadmap gap

Step 5 — Save outputs:
Full synthesis → research/user-pain/synthesis-[date].md
JTBD statements (clean format) → research/user-pain/jtbd-[date].md
Roadmap gaps summary → research/user-pain/gaps-[date].md
```

**Analyzing 200 support tickets in depth:**

```
Read: research/support/[export-file].csv
These are [N] support tickets from [date range].

Do not try to read each ticket individually. Instead:

1. Sample the 20 most recent tickets in each of the top 5 categories.
2. For each sample: identify the core user pain (not the surface issue).
3. Find: are these 5 category samples expressing 5 distinct pains, or do some overlap?

Output:
- 5-7 distinct user pain statements (what the user actually needs, not what they asked for)
- For each: ticket IDs that exemplify it
- Confidence level (high/medium/low) based on sample size
- What additional data would increase confidence
```

---

### Workflow 4: Roadmap Validation

**MCPs required:** Jira (committed items) + Amplitude (usage data) + customer feedback files
**What it does:** Three-way comparison to answer "are we building the right things?"

**When to run it:** Start of every planning cycle (quarterly), or before a major pivot decision

**The core question:** "Is our roadmap aligned with what customers actually want and use?"

**Full prompt:**

```
Quarterly roadmap validation for Q[N].

Step 1 — What we've committed to building:
Read all spec files in specs/ for Q[N] (or read Jira: epics tagged q[N]-roadmap).
Extract per initiative: name, JTBD it claims to solve, success metric, target segment.

Step 2 — What customers are asking for:
Read: research/user-pain/synthesis-[most-recent-date].md
List top 10 customer pain themes, ranked by frequency × severity.

Step 3 — What customers actually use:
Read Amplitude: 30-day active users for all features shipped in Q[N-1].
Sort: most used → least used.
Flag: any shipped feature with <20% of target adoption (possible over-investment signal).

Step 4 — Three-way analysis:

A. Alignment map:
For each committed Q[N] initiative: does it address one of the top 10 pain themes?
- Strong alignment: initiative directly solves top 5 pain
- Weak alignment: initiative addresses pain ranked 6-10
- No alignment: initiative isn't on the pain list
   → Note: this doesn't mean it's wrong. Strategic bets can be ahead of stated customer pain. Flag for explicit discussion.

B. Pain coverage:
Of the top 10 customer pain themes, how many are covered by Q[N] roadmap?
List uncovered pains — these are your potential roadmap gaps.

C. Usage-to-investment ratio:
For Q[N-1] shipped features: rank by (30-day adoption rate / estimated build effort).
Identify: low-adoption features that consumed significant engineering time.
These are "expensive misses" — important for retrospective and planning.

Step 5 — Recommendation:
Based on this analysis:
- Top 2 roadmap items to reconsider (low alignment or low usage signal)
- Top 2 customer pains to add to Q[N+1] consideration
- 1 strategic question this analysis raises (something not answered by data alone)

Save to: research/roadmap-validation-q[N]-[date].md
```

---

### Workflow 5: Release Impact Analysis

**MCPs required:** Git (what shipped) + Amplitude or analytics MCP (before/after metrics)
**What it does:** Measures whether the release moved the metrics it was supposed to move

**When to run it:** 7-14 days after any significant release

**The core question:** "Did the release move the metrics? What worked, what didn't, and why?"

**Full prompt:**

```
Post-release impact analysis: [Release Name or Version]
Release date: [date]

Step 1 — What shipped:
Read Git log for commits between [date-2] and [date] on main/release branch.
List: features and significant changes shipped.
Cross-reference with: specs/ files to find the stated success metric for each.

Step 2 — Baseline metrics (14 days pre-release):
Read Amplitude for [date-14] to [date-1]:
- Primary metric: [e.g., checkout completion rate, DAU, feature X usage rate]
- Secondary metrics: [e.g., session length, error rate, NPS if available]
- Segment breakdowns if relevant: [new users vs. returning, plan type, etc.]

Step 3 — Post-release metrics (7 days post-release):
Read Amplitude for [date] to [date+7]:
- Same metrics as Step 2
- Note: use [date+2] start if you want to exclude launch day anomalies

Step 4 — Delta analysis:
For each metric: [pre-release value] → [post-release value] = [delta absolute + %]
Flag:
- Metrics that moved as expected (positive signal)
- Metrics that didn't move (null result — was the hypothesis wrong, or is it too early?)
- Metrics that moved unexpectedly (positive surprises, and negative ones)
- Metrics we targeted in specs that we don't have Amplitude data for (measurement gap)

Step 5 — Feature attribution (best-effort):
For each shipped feature with an associated success metric:
- Did the metric move? Direction and magnitude.
- Confidence of attribution: high / medium / low / unable to attribute
  (low confidence: multiple things shipped at once, no holdout, confounding factors)

Step 6 — Learnings:
What worked: features where metrics moved as predicted
What didn't: features with flat or negative impact
Hypotheses: 1-2 sentences per underperformer — why did it miss?
Measurement gaps: what should we instrument before the next release?

Save to: research/release-impact-[release-name]-[date].md
```

---

## The Context-MCP Architecture

How the layers work together:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PM ORCHESTRATION STACK                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  PERSISTENT LAYER          CLAUDE.md                           │
│  (always loaded,           context/product-context.md          │
│   every session)           context/personas.md                 │
│                            context/prd-template.md             │
│                            ↓ loads on session start            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  DATA LAYER                MCPs (live queries)                 │
│  (session inputs,          ├── Jira → tickets, sprints         │
│   pulled per workflow)     ├── Amplitude → usage, metrics      │
│                            ├── Bright Data → web, reviews      │
│                            ├── Intercom/Zendesk → support      │
│                            └── GitHub → code, history          │
│                                                                 │
│                            Local Files (exports, archives)     │
│                            ├── research/competitive/*.md       │
│                            ├── research/user-pain/*.md         │
│                            └── research/support/*.csv          │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  PROCESSING LAYER          Agent roles (defined in CLAUDE.md)  │
│  (what Claude does         /research → rigorous researcher     │
│   with the data)           /spec → senior PM                   │
│                            /data → PM-trained analyst          │
│                            /devil → skeptical VP               │
│                                                                 │
│                            Execution patterns:                 │
│                            ├── Sequential (A → B → C → D)      │
│                            ├── Parallel (A ∥ B ∥ C → merge)   │
│                            └── Specialist + Synthesizer        │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  OUTPUT LAYER              specs/         ← PRDs, specs        │
│  (session outputs,         research/      ← competitive, pain  │
│   next session's inputs)   notes/         ← working memory     │
│                            decisions/     ← decision log       │
│                                                                 │
│                            Every output is a file.             │
│                            Files are the handoff between       │
│                            sessions and between agents.        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

This stack replaces: 5 open browser tabs + 3 manual copy-pastes + 2 CSV exports + a synthesis meeting.

---

## MCP Performance Notes

### Notion MCP

**The situation:** Notion MCP is real and works. It's also slow and token-heavy for large workspaces.

**What this means in practice:**
- Searching a large Notion workspace via MCP can consume 20-40% of your context window just for retrieval
- For a compound workflow with 3+ MCP calls, this leaves little room for synthesis

**Best practice:**
- Use Notion MCP for **live queries**: today's sprint items, current meeting notes, real-time updates
- Export large Notion databases and pages to Markdown and load them as local files
- Reference pattern in CLAUDE.md:
  ```
  For Notion reference material (roadmap history, archived specs, personas):
  Use local exports in context/ — not MCP.
  For live sprint data: use Notion MCP.
  ```

### How Many MCPs Before Hitting Context Limits

**Practical guideline:** 2-3 MCP calls per prompt is comfortable. 4+ starts competing for context budget.

**The token math:**
- Each MCP call adds retrieval payload to the context
- A Jira sprint read might add 2,000 tokens (10 tickets × 200 tokens each)
- An Amplitude query might add 1,000 tokens
- A Notion database search might add 5,000+ tokens (varies widely)
- Claude's response itself needs room: synthesis quality degrades if input fills >70% of context

**Mitigation strategies:**

```
Option A — Break into sequential steps:
Step 1 prompt: Read Jira. Save to notes/sprint-data.md. Done.
Step 2 prompt: Read Amplitude. Save to notes/amplitude-data.md. Done.
Step 3 prompt: Read both files above. Synthesize. No MCP calls needed.

Option B — Limit Jira/Notion queries:
Add to your query: "Return only the 10 most recent tickets" or "Return summary only, not full descriptions"
Smaller payloads = more room for synthesis.

Option C — Export heavy sources, use MCP for live data:
Export Notion reference content as .md files monthly.
Use MCP only for Jira (this week) and Amplitude (real-time).
```

**The general rule:** If a workflow feels slow or outputs seem cut off, break it into smaller steps where each step saves a file. The file becomes the input for the next step. No single step needs all the context.

### GitHub MCP vs. Local Files

For codebase-related research: Claude Code reads local files faster and more accurately than GitHub MCP. Use GitHub MCP for external repository research (competitor code, open-source libraries). For your own codebase, open the project folder directly.

---

## Automating Workflows with Hooks

Claude Code 2.0 (released early 2026) supports automation hooks — triggers that fire Claude Code actions on events.

### Setting Up the Sprint Health Check Automatically

**In Claude Code Settings → Hooks:**

```json
{
  "hooks": [
    {
      "trigger": "schedule",
      "schedule": "0 9 * * 1",
      "description": "Weekly Monday sprint health check",
      "action": "run_prompt_file",
      "file": "prompts/sprint-health-check.md"
    }
  ]
}
```

This runs the Sprint Health Check every Monday at 9am. The prompt file at `prompts/sprint-health-check.md` contains the full workflow above.

**Sprint health check as a saved prompt file (`prompts/sprint-health-check.md`):**

```
---
name: Sprint Health Check
schedule: weekly (Monday)
output: notes/sprint-health-[auto-date].md
---

Run a sprint health check for the most recently completed sprint.

[Full Workflow 1 prompt from above]

After completion, append one line to notes/index.md:
notes/sprint-health-[date].md | Sprint N health check, [completion rate]% delivered | [date]
```

### Other Useful Automation Hooks

**Post-session decision capture:**
```json
{
  "trigger": "session_end",
  "action": "run_prompt",
  "prompt": "Was any product decision made in this session? If yes, save it using the template in context/decision-template.md to decisions/[date]-[topic].md. If no: no action needed."
}
```

**Daily competitive monitor:**
```json
{
  "trigger": "schedule",
  "schedule": "0 8 * * 1",
  "description": "Weekly competitive intelligence pull",
  "action": "run_prompt_file",
  "file": "prompts/competitive-update.md"
}
```

**Note:** Hook syntax may vary by Claude Code version. Check the official docs for your version. The pattern above reflects Claude Code 2.0 behavior.

---

## Building Your First Compound Workflow

The fastest path to a working compound workflow:

**Step 1:** Pick one workflow from the 5 above that matches a recurring task you do.

**Step 2:** Copy the prompt, fill in your specific variables (MCP names, file paths, metric names).

**Step 3:** Run it. Let it fail. It will — you'll have wrong paths, missing files, or the wrong event names in Amplitude. That's normal.

**Step 4:** Fix the one thing that broke. Run again.

**Step 5:** When it works once, save the prompt as a file in `prompts/`. Give it a name. Now it's a reusable workflow.

The setup cost is one session. The payoff is every subsequent run.

---

## MCP Quick Reference for PM Workflows

| MCP | What It Gives You | Used In |
|-----|-------------------|---------|
| Jira | Tickets, sprints, epics, statuses | Workflows 1, 4 |
| Linear | Same as Jira (cleaner API) | Workflows 1, 4 |
| Amplitude | Feature usage, funnels, retention | Workflows 1, 4, 5 |
| Bright Data | Any public web page: changelogs, reviews, job posts | Workflow 2, Use Cases 2 + 4 |
| Intercom / Zendesk | Support tickets, conversation history | Workflow 3 |
| Notion | Live sprint data, meeting notes | Workflows 1, 3, 4 |
| GitHub | Code history, what shipped, PRs | Workflow 5 |
| Google Analytics / Mixpanel | Web usage, funnel data | Workflow 5 |

**Always-free alternative:** Export any of the above as CSV or Markdown and put in your project folder. Claude reads local files with the same quality as live MCPs — and faster.

---

## The Compound Workflow Mindset

Compound workflows feel complex to set up and obvious in retrospect.

Before you build one, you do the synthesis yourself: open 4 tabs, copy data from each, paste into a doc, write the analysis. It takes 2 hours.

After you build one, the prompt does the same work in 20 minutes. You review the output and apply judgment.

The setup is a one-time investment. The compound interest is every run after that.

Start with Workflow 1 (Sprint Health Check) or Workflow 2 (Competitive Intelligence). Both have clearly scoped inputs, clearly scoped outputs, and payoffs that are obvious to your team. Ship one working compound workflow before building the next.
