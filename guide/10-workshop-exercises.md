# Workshop Exercises — Hands-On PM Practice

> Ten exercises, ordered by complexity. Each one produces a real artifact you keep. Do them solo or use them to run a team workshop. Expected outputs are stated upfront — you will know you succeeded when you have the thing.

---

## Table of Contents

1. [Exercise 1: Your First CLAUDE.md](#exercise-1)
2. [Exercise 2: Plan Mode Exploration](#exercise-2)
3. [Exercise 3: Your First MCP Connection](#exercise-3)
4. [Exercise 4: Competitive Research Sprint](#exercise-4)
5. [Exercise 5: PRD Pipeline](#exercise-5)
6. [Exercise 6: Upgrade Your CLAUDE.md to Level 3](#exercise-6)
7. [Exercise 7: Compound MCP Workflow](#exercise-7)
8. [Exercise 8: The /devil Review](#exercise-8)
9. [Exercise 9: Prototype a Feature](#exercise-9)
10. [Exercise 10: Full PM Day Simulation](#exercise-10)
11. [Tips for Facilitators](#facilitator-tips)

---

## Exercise 1: Your First CLAUDE.md {#exercise-1}

**Time:** 15 minutes solo / 25 minutes in a group

**Goal:** Write a real CLAUDE.md for your actual product — not a tutorial example, not lorem ipsum. A file that will make your next session measurably better than if you had not written it.

**What you will produce:** A committed CLAUDE.md file in your project folder with three working sections.

### Setup

- Terminal open
- A folder for your project (create one if needed: `mkdir ~/pm-workspace && cd ~/pm-workspace`)
- The product you actually work on in your head

### Steps

**Step 1: Create the folder and file**

```bash
mkdir ~/pm-workspace
cd ~/pm-workspace
touch CLAUDE.md
```

**Step 2: Open CLAUDE.md in any text editor**

On macOS:
```bash
open -e CLAUDE.md
```

On Windows:
```powershell
notepad CLAUDE.md
```

**Step 3: Write the three sections**

Do not write what you wish were true. Write what is actually true right now.

```markdown
# [Your Product Name] — PM Context

## Product Vision
[One sentence: what we are building and for whom]
[Two to three sentences: what problem we solve and why we are positioned to solve it]

## Core Users

### [Persona Name]
- Who they are: [job title, company size, context]
- Primary job to be done: [the thing they are trying to accomplish]
- Biggest frustration with current solutions: [be specific]

### [Persona Name]
[repeat format]

## Current Quarter — [Q and Year]
- OKR 1: [Objective: outcome. Key Result: metric.]
- OKR 2: [Objective: outcome. Key Result: metric.]
- Active sprint theme: [one line description]
- The decision we are most stuck on right now: [one honest sentence]
```

**Step 4: Start Claude Code and test it**

```bash
claude
```

```
> Load CLAUDE.md and tell me what you know about our product.
```

Read the response carefully. Claude Code will tell you what it understood. If it misunderstood something, fix the CLAUDE.md. If it got it right, move on.

**Step 5: Test with a real question**

```
> Given our current quarter focus, what are the three most likely reasons
  we would miss our OKRs? Answer based only on what you know from CLAUDE.md.
```

### Expected Output

A `CLAUDE.md` file in `~/pm-workspace/` that:
- Loads correctly when Claude Code starts
- Produces a meaningful answer to "what do you know about our product?"
- Contains information accurate enough to make the OKR risk question useful

### Debrief Questions

- What was hardest to write? (This usually reveals what is not yet clear in your own product strategy)
- What did writing it clarify that you had not articulated before?
- Is the "decision we are most stuck on" actually the right one, or did you write a safer version?
- If this were a consultant onboarding document, what would it be missing?

**Group format:** Pair up. Read each other's CLAUDE.md without explanation. What did you understand about the product? Where were you confused? The confusion points are the gaps in the document.

---

## Exercise 2: Plan Mode Exploration {#exercise-2}

**Time:** 10 minutes solo

**Goal:** Ask and get answered three specific questions about your product's codebase that you previously would have had to ask an engineer — without changing a single file.

**What you will produce:** Three written answers in a Markdown file you keep.

### Setup

- Access to your product's Git repository (on your machine or cloned locally)
- Claude Code installed

### Steps

**Step 1: Navigate to your repository**

```bash
cd ~/path/to/your-product-repo
```

**Step 2: Start Claude Code in Plan Mode**

```bash
claude --plan
```

You will see `[PLAN]` in the prompt. Nothing you do in this session will change any file.

**Step 3: Ask your three questions**

Pick from this list, or write your own. The rule: ask something you genuinely do not know and would normally have to ask an engineer.

**Option A — Architecture question**
```
> Walk me through how the [feature you own] actually works technically.
  What files are involved, what external services does it call,
  and what are the dependencies I should know about before writing a spec?
```

**Option B — Recent changes question**
```
> Look at the git history for the past 3 weeks.
  What was shipped? Were there any hotfixes?
  Give me a non-technical summary I could use in a sprint review.
```

**Option C — Technical debt question**
```
> Where are the areas of this codebase with the most TODO comments
  or debt markers? What is the estimated user-facing impact
  if we do not address them before Q3?
```

**Option D — Test coverage question**
```
> What is the test coverage for the [feature] module?
  Are there any user-facing flows with no automated tests?
  What would break silently if we changed this feature?
```

**Step 4: Save your answers**

```
> Summarize the three things I just learned about the codebase
  and save them to docs/codebase-notes.md
```

### Expected Output

A `docs/codebase-notes.md` file with three factual answers about your codebase.

### Debrief Questions

- What did you learn that you did not know before?
- Which answer would have required a meeting to get without Claude Code?
- What question did you want to ask but did not know how to phrase?
- What did Claude Code get wrong or leave out? (Always verify the answers you plan to act on)

---

## Exercise 3: Your First MCP Connection {#exercise-3}

**Time:** 15 minutes solo

**Goal:** Connect one PM tool to Claude Code and ask it a real question about your actual backlog or project data.

**What you will produce:** A working MCP connection and one useful answer from your real tool data.

### Setup

- An API token for your tool (Jira, Linear, or Notion — pick one you use daily)
- Claude Code installed and running

### Getting Your API Token

**Jira:** Settings → Security → API tokens → Create API token

**Linear:** Settings → API → Personal API keys → Create key

**Notion:** notions.so/my-integrations → New integration → Copy Internal Integration Token

### Steps

**Step 1: Install your MCP**

```bash
# Jira
/plugin marketplace add atlassian/jira-mcp

# Linear
/plugin marketplace add linear/mcp-server

# Notion
/plugin marketplace add makenotion/notion-mcp-server
```

**Step 2: Configure credentials**

Edit `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "jira": {
      "command": "npx",
      "args": ["-y", "@atlassian/jira-mcp"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_EMAIL": "your@email.com",
        "JIRA_API_TOKEN": "your-api-token"
      }
    }
  }
}
```

**Step 3: Restart Claude Code and verify the connection**

```bash
claude
```

```
> What projects do I have access to in Jira?
```

If you get a list, the connection works. If you get an error, check that your credentials are correct.

**Step 4: Ask a real question about your backlog**

```
> Look at the open issues in the [project name] backlog.
  Which three tickets have been open the longest?
  What labels or themes do they share?
  Write a one-paragraph summary of the pattern.
```

**Step 5: Ask a question you would normally take to a meeting**

```
> How many tickets were closed in the last sprint in [project]?
  How does that compare to what was committed?
  Save this as a one-paragraph sprint velocity note to docs/sprint-notes.md
```

### Expected Output

- A working MCP connection to your tool
- At least one useful answer pulled from your real project data
- `docs/sprint-notes.md` with an actual sprint velocity note

### Debrief Questions

- What question did you ask? What did it cost you before to get that answer (time, a meeting, a ticket)?
- How long did setup take? Was it faster or slower than you expected?
- What question do you want to ask next that you did not have time for in this exercise?

---

## Exercise 4: Competitive Research Sprint {#exercise-4}

**Time:** 20 minutes solo

**Goal:** Complete a structured competitive analysis of three competitors in one session.

**What you will produce:** A `research/competitive-synthesis.md` file you can share with your team.

### Setup

**Option A (Recommended):** Bright Data MCP installed

```bash
/plugin marketplace add brightdata/mcp
```

**Option B (No MCP needed):** Open the changelog or "what's new" page of three competitors in your browser and have the URLs ready.

### Steps

**Step 1: Set up your research folder**

```bash
mkdir -p research
```

**Step 2: Start Claude Code**

```bash
claude
```

**Step 3: Name your three competitors and run the analysis**

If you have Bright Data MCP:
```
> Research the three main competitors to [our product]: [Competitor A], [Competitor B], [Competitor C].

For each competitor, find:
1. Their most recent feature releases (last 90 days if available)
2. What they are emphasizing in their marketing copy
3. What their users complain about in public forums

Then synthesize: what theme or capability are all three moving toward?
What is the gap none of them are addressing?

Save to research/competitive-synthesis.md
```

If you are using URL mode:
```
> I am going to give you the URLs for three competitor changelogs.
  For each one, analyze:
  1. What they shipped in the last 90 days
  2. What patterns you see in their release priorities

  Competitor A changelog: [URL]
  Competitor B changelog: [URL]
  Competitor C changelog: [URL]

  After reading all three, synthesize the findings.
  What is the common direction? What are they avoiding?
  Save to research/competitive-synthesis.md
```

**Step 4: Push it further**

```
> Based on this competitive analysis, write three specific implications
  for our Q3 roadmap. Each implication should be actionable:
  something we should start, stop, or accelerate based on what competitors are doing.
  Add this section to research/competitive-synthesis.md
```

### Expected Output

`research/competitive-synthesis.md` containing:
- Analysis of 3 competitors
- Pattern synthesis across all three
- 3 actionable roadmap implications

### Debrief Questions

- How long did this take? How long did comparable research take before?
- What surprised you in the output?
- What would you have missed if you had only looked at one competitor?
- What would a human analyst have added that Claude Code did not?

---

## Exercise 5: PRD Pipeline {#exercise-5}

**Time:** 30 minutes solo / 45 minutes in a group

**Goal:** Generate a complete, grounded PRD for a real feature using a four-step research-first pipeline.

**What you will produce:** A full PRD in `specs/[feature-name]-prd.md`

### Setup

- A feature you are currently working on or planning
- Related Jira tickets, support tickets, or user feedback (even rough notes will work)
- Your CLAUDE.md from Exercise 1

### The Four-Step Pipeline

**Step 1: Research (5 minutes)**

Gather and load all available context before writing a single word of spec.

```
> I am writing a PRD for [feature name].

Before I give you instructions, I want you to gather context.
Please read:
1. CLAUDE.md (our product context)
2. Any files in research/ related to [feature name]
3. Any mentions of [feature name] in specs/ or docs/

Tell me:
- What do we already know about this problem?
- What is missing from the context that I should provide?
- What assumptions would you have to make if we started writing now?
```

Fix the gaps the research step identifies before moving on.

**Step 2: JTBD Extraction (5 minutes)**

```
> Based on the context, define the Job to Be Done for this feature.

Format:
- Functional job: [what the user is trying to accomplish]
- Emotional job: [how the user wants to feel while doing it]
- Social job: [how the user wants to be perceived by others]
- Current workaround: [what they do today instead of using this feature]
- Trigger: [the specific moment that makes them need this]
```

Read this output carefully. If it does not match what you believe about your users, correct it before Step 3. The JTBD section is the filter for the entire PRD.

**Step 3: Write the PRD**

```
> Using the research context and JTBD definition above, write a complete PRD
  for [feature name].

Structure:
1. Problem Statement (2 paragraphs)
2. Proposed Solution (what we are building and what we are explicitly not building)
3. User Stories (5-8 stories, format: As a [persona], when [trigger], I want to [action] so that [outcome])
4. Acceptance Criteria (numbered, testable, pass/fail)
5. Success Metrics (leading indicators and lagging indicators)
6. Open Questions (things we do not know yet that must be answered before engineering starts)
7. Out of Scope (what this PRD explicitly does not cover)
8. Assumptions I Made (flag everything I should validate)

Save to specs/[feature-name]-prd.md
```

**Step 4: Review Loop**

```
> You just wrote this PRD. Now review it as a skeptical VP of Product.
  Identify:
  1. The three weakest sections — where is the reasoning thinnest?
  2. Any acceptance criteria that are not actually testable
  3. Any success metrics that cannot be measured with our current tooling
  4. The one open question that would kill the project if the answer is wrong

  Do not rewrite anything yet. Just list the problems.
```

Address the problems, then ask Claude Code to update the file.

### Expected Output

`specs/[feature-name]-prd.md` — a complete PRD that went through research, JTBD extraction, and a skeptical review pass.

### Debrief Questions

- Is this PRD better or worse than your typical process produces?
- Which step added the most value: research, JTBD, or the review loop?
- What did the "Assumptions I Made" section reveal?
- What would you change about the pipeline for next time?

**Group format:** Swap PRDs and critique each other's "Open Questions" section. Is every question genuinely open? Are there questions that should be open but were not listed?

---

## Exercise 6: Upgrade Your CLAUDE.md to Level 3 {#exercise-6}

**Time:** 30 minutes solo

**Goal:** Take your CLAUDE.md from Exercise 1 and upgrade it to the full Level 3 structure with context loading, agent roles, and anti-patterns.

**What you will produce:** A CLAUDE.md that loads context automatically, has named agent roles, and tells Claude Code what to avoid.

### Setup

- Your CLAUDE.md from Exercise 1
- Your CLAUDE.md open in a text editor

### The Five Sections to Add

Open your CLAUDE.md and add these five sections after your existing content.

**Section 4: Context Loading Sequence**

This tells Claude Code what to read at the start of every session.

```markdown
## Context Loading Sequence

When starting a session, read these files in order:
1. CLAUDE.md (this file — already loaded)
2. research/latest-research.md (if it exists)
3. specs/CURRENT-SPRINT.md (if it exists)
4. decisions/DECISION-LOG.md (if it exists)

After loading, confirm: "Loaded [N] context files. Ready."
```

**Section 5: Agent Roles**

Name the modes you want to be able to invoke.

```markdown
## Agent Roles

### /research
Goal: Gather information before writing anything.
Behavior: Ask clarifying questions, check existing research files before going external,
flag assumptions that need validation before a spec can be written.
Output format: Structured notes with clear source attribution.

### /spec
Goal: Write a grounded product specification.
Behavior: Always read CLAUDE.md and any related research before writing.
Follow the PRD pipeline: Research → JTBD → Draft → Review.
Output format: Save to specs/[feature-name]-prd.md

### /devil
Goal: Identify weaknesses in a plan, spec, or strategy.
Behavior: Respond as a skeptical, experienced VP of Product.
Find the three most likely failure modes. Do not soften feedback.
Do not propose solutions — only identify problems.
Output format: Numbered list of objections, each with a specific reason.

### /standup
Goal: Prepare a concise daily standup update.
Behavior: Read recent git commits, open tickets, and any session notes.
Format: Yesterday / Today / Blockers — maximum 5 bullets total.
Output format: Plain text, paste-ready for Slack.
```

**Section 6: Anti-Patterns (Do Not Do These)**

```markdown
## Anti-Patterns

Do not do these without explicit instruction:
- Do not create files outside of specs/, research/, docs/, or deliverables/
- Do not update CLAUDE.md itself during a session
- Do not commit to git without asking me first
- Do not write more than one document per session unless asked
- Do not invent user research — flag when you are extrapolating vs. citing
```

**Section 7: Output Defaults**

```markdown
## Output Defaults

- Language: Plain English, no jargon, no bullet points inside bullet points
- File format: Markdown (.md) unless another format is requested
- Save location: ask me if unclear; do not assume
- Length: write as long as needed, not longer
- When uncertain: flag the uncertainty explicitly rather than proceeding with an assumption
```

**Section 8: Team Glossary**

```markdown
## Team Glossary

[Add your product-specific terms here. Examples:]
- "The platform" = [what this means in your product context]
- "Core users" = [your primary persona name] and [secondary persona name]
- "The legacy API" = [what this refers to specifically]
```

### Testing Each Agent Role

After adding the sections, test each role:

```
> /research — I am considering adding a CSV export feature. What do we know from our existing research?
```

```
> /devil — Here is our proposed solution for the CSV export feature: [paste a brief description]
```

```
> /spec — Write a one-page spec for CSV export using the standard pipeline.
```

### Expected Output

An upgraded CLAUDE.md with all five new sections. Each `/agent` command produces a meaningfully different output style.

### Debrief Questions

- Which agent role gave the most useful output?
- Which anti-pattern were you violating most often before you wrote it down?
- What term belongs in the glossary that you forgot to add?

---

## Exercise 7: Compound MCP Workflow {#exercise-7}

**Time:** 25 minutes solo

**Goal:** Run a workflow that combines two MCP tools to produce an insight that neither tool alone could give.

**What you will produce:** A saved insight document that combines data from two tools.

### Setup

Two MCPs connected — pick one combination:
- Jira + Amplitude
- Jira + Notion
- Linear + Slack (via Zapier)
- GitHub Issues + any analytics tool

### The Two Compound Workflows

**Option A: Sprint Health Check**

```
> I want a Sprint Health Check report for the current sprint.

Pull from Jira:
- Total tickets committed
- Tickets completed so far
- Tickets blocked or flagged
- Any tickets added after sprint start (scope creep)

Pull from Amplitude:
- DAU trend for the past 14 days
- Any error rate spikes in the past 7 days
- Top 3 features by usage this week

Synthesize:
- Is the sprint on track?
- Are we shipping things that people are actually using?
- What is the one signal that should change our priorities before sprint end?

Save to docs/sprint-health-[date].md
```

**Option B: Roadmap Validation**

```
> I want to validate our Q3 roadmap against actual user behavior.

Pull from Notion (or wherever the roadmap lives):
- Our 5 planned Q3 initiatives

Pull from Amplitude:
- Current usage of the features that Q3 initiatives build on
- Drop-off points in the flows Q3 initiatives are meant to improve

Synthesize:
- For each initiative: does the usage data support the priority?
- Which initiative has the weakest data justification?
- Which initiative is most directly validated by what users are doing?

Save to docs/roadmap-validation-q3.md
```

### What Makes This Compound

The point of this exercise is that the insight only exists at the intersection of the two tools. Your Jira view does not show you whether completed tickets produce usage. Your Amplitude view does not show you whether sprint execution is healthy. The compound question lives between the tools.

### Expected Output

`docs/sprint-health-[date].md` or `docs/roadmap-validation-q3.md` — a synthesized insight that required both tools to produce.

### Debrief Questions

- What was the combined insight that neither tool alone could give?
- Where did the two data sources contradict each other? (This is always the most interesting part)
- What workflow would you run every Monday morning if this were set up and automatic?

---

## Exercise 8: The /devil Review {#exercise-8}

**Time:** 10 minutes solo

**Goal:** Put your most recent spec or PRD through a genuinely skeptical review — the kind you want to get from a VP before you are in front of a VP.

**What you will produce:** Three specific objections to your spec and documented responses to each.

### Setup

- Any existing PRD or spec file (the one from Exercise 5 works well)
- Claude Code open

### Steps

**Step 1: Load the spec**

```
> Read specs/[your-feature]-prd.md
```

**Step 2: Run the /devil review**

```
> /devil

You are a skeptical VP of Product. You have seen a hundred features fail because
the PM had not thought through the following categories of risk:
1. User adoption risk — will people actually use it the way we expect?
2. Scope risk — is this bigger than we think?
3. Success metric risk — will we be able to tell if it worked?
4. Dependency risk — what has to be true for this to succeed that we do not control?
5. Prioritization risk — why this over something else?

Read the spec I just loaded.
Give me your three hardest objections. Be specific. Use evidence from the spec itself.
Do not propose solutions. Only raise objections.
```

**Step 3: Address each objection**

For each objection Claude Code raises, write your response. Do this in the session:

```
> Objection 1 is [X]. My response is [Y].
  Objection 2 is [X]. My response is [Y].
  Objection 3 is [X]. My response is [Y].

  For each objection, tell me: is my response adequate, or does it still leave a hole?
```

**Step 4: Update the spec**

```
> Based on the objections that I could not fully answer, update specs/[feature]-prd.md
  to address the gaps. Add the unresolved objections to the "Open Questions" section.
```

### Expected Output

- Three specific, evidence-based objections to your spec
- Your documented response to each
- An updated spec with unresolved objections in the Open Questions section

### Debrief Questions

- Which objection was hardest to answer?
- Would you have caught any of these in a real review? Be honest.
- What does the /devil review tell you about what you were avoiding thinking about?
- How does this compare to peer review from a colleague?

---

## Exercise 9: Prototype a Feature {#exercise-9}

**Time:** 45–60 minutes solo

**Goal:** Turn a PRD into a working prototype in a browser. Not a mockup. Not a diagram. Something you can click on.

**What you will produce:** A working HTML prototype accessible at `localhost:[port]`

### Setup

- PRD from Exercise 5
- Claude Code with write permissions (not in Plan Mode)
- A web browser

### Steps

**Step 1: Scoping — ask Claude Code what is feasible**

```
> Read specs/[feature]-prd.md.
  I want to build a prototype that demonstrates the core user flow.
  What is the minimum set of screens or interactions I need to show
  the main user story?
  Do not start building yet. Just tell me the scope.
```

Agree on a scope before any code is written. A common mistake is asking for the whole spec — start with the one flow that matters most.

**Step 2: Architecture recommendation**

```
> Given that scope, recommend the simplest possible technical approach
  for a prototype I can run in a browser.
  Options to consider: plain HTML/CSS/JavaScript, React, or a static site generator.
  I do not need a database. I need something I can click through.
```

**Step 3: Build the first screen**

```
> Build the first screen: [screen name].
  Requirements from the PRD: [paste the relevant acceptance criteria]
  Save to prototype/[feature-name]/index.html
  It should work when I open it directly in a browser — no build step required.
```

**Step 4: Open it and test it**

```bash
open prototype/[feature-name]/index.html
```

Test the screen against the acceptance criteria from your PRD. Note what is missing or wrong.

**Step 5: Iterate**

```
> I opened the prototype. Here is what I see vs. what I expected:
  - [specific observation]
  - [specific observation]

  Make these changes and re-save the file.
```

**Step 6: Add the next screen**

```
> Add screen 2: [screen name].
  When the user [action], they should see this screen.
  Requirements: [paste acceptance criteria]
```

### Expected Output

A working HTML prototype at `prototype/[feature-name]/index.html` that demonstrates the core user flow from your PRD.

### Debrief Questions

- What surprised you about implementing the feature? What looked simple in the PRD but got complicated in practice?
- What would have taken a full sprint to build as production code?
- What did the prototype reveal that you would not have caught in a spec review?
- How would you use this prototype in your next planning meeting?

---

## Exercise 10: Full PM Day Simulation {#exercise-10}

**Time:** 60–90 minutes in a group (or solo as a sprint simulation)

**Goal:** Run a compressed simulation of a full PM workday through Claude Code. Experience the complete workflow — morning competitive brief, sprint review, spec writing, and end-of-day documentation — in a single session.

**What you will produce:** Four documents representing a full day of PM output.

### Setup

- Full project folder with CLAUDE.md, at least two MCPs connected (ideally Jira + one analytics tool)
- Some existing research files (can be from Exercise 4)
- The PRD from Exercise 5

### The Day

**Morning (15 minutes): Competitive Brief**

```
> It is Monday morning. I have 15 minutes before standup.
  Run a competitive brief: what happened with our top 3 competitors last week?
  Check their public changelogs and recent press.
  Format: 3 bullet points per competitor, total reading time under 3 minutes.
  Save to docs/competitive-brief-[date].md
```

**Midday (20 minutes): Sprint Review Analysis**

```
> Sprint [N] just ended. I need to run the sprint review.

Pull from Jira:
- What was committed vs. what was delivered?
- Any carry-over tickets and their reasons?
- Velocity trend: this sprint vs. last 3 sprints

Pull from [analytics tool]:
- Did any of the shipped features show immediate usage signals?
- Any regressions in existing metrics this sprint?

Write a 5-minute sprint review summary for the team.
Save to docs/sprint-review-sprint-[N].md
```

**Afternoon (20 minutes): Write a Spec**

```
> I have 20 minutes. I need a one-page spec for the next highest priority item.
  Use the standard PRD pipeline (research → JTBD → draft).
  Scope: one page, enough to start an engineering conversation, not a full spec.
  Save to specs/[feature]-one-pager.md
```

**End of Day (10 minutes): Update Documentation**

```
> End of day cleanup.

1. Update the decision log: we decided today to [decision].
   Rationale: [brief reason]. Alternatives we rejected: [list].
   Save to docs/decision-log.md (append, do not overwrite)

2. Update the context index: what changed today that future-me should know?
   Append a dated note to docs/context-index.md

3. What is the single most important thing to do first thing tomorrow?
   Add it to the top of docs/tomorrow.md
```

### Expected Output

Four documents:
1. `docs/competitive-brief-[date].md`
2. `docs/sprint-review-sprint-[N].md`
3. `specs/[feature]-one-pager.md`
4. Updated `docs/decision-log.md` and `docs/context-index.md`

### Debrief Questions

- Time estimate comparison: how long would a real version of this day have taken?
- What is still missing that a human PM must provide and a tool cannot?
- Which of the four blocks produced the most value for the time invested?
- What would you do differently in your actual workflow after doing this simulation?

**Group format:** Run in teams of two. One person drives the session, the other plays "VP" — challenging assumptions, demanding clearer answers, simulating interruptions. Debrief together on where Claude Code's output needed the most human judgment.

---

## Tips for Facilitators {#facilitator-tips}

### If You Only Have 15 Minutes

Run Exercise 1 only. The CLAUDE.md exercise produces a real artifact in 15 minutes and starts a habit. Every other exercise assumes a CLAUDE.md exists. Start there.

### Common Mistakes in Workshop Settings

**Mistake 1: Participants use generic example products instead of their real product.**

The CLAUDE.md exercise is significantly less useful when people write about a fictional "task management app" instead of their actual product. Enforce this: participants must use their real product in Exercise 1.

**Mistake 2: Too much time on tool setup, not enough on output quality.**

If Jira MCP credentials are causing issues in Exercise 3, skip it and substitute a folder of exported Markdown files from any tool. The learning objective is "ask Claude Code a question about your backlog," not "configure OAuth correctly under time pressure."

**Mistake 3: Participants skip the debrief questions.**

The debrief is where the learning happens. The exercises produce artifacts. The debrief produces behavior change. Build at least 5 minutes of debrief time into every exercise.

**Mistake 4: Rushing through the review step in Exercise 5.**

The PRD pipeline exercise is often run as "write the PRD and move on." The review loop in Step 4 is the highest-value part. Participants who skip it miss the experience of Claude Code finding weaknesses they wrote.

### Timing Adjustments

| Session Length | Recommended Exercises |
|---|---|
| 15 minutes | Exercise 1 only |
| 30 minutes | Exercises 1 + 2 |
| 60 minutes | Exercises 1, 2, 3, and 8 |
| 90 minutes | Exercises 1–5 |
| Half day | All 10 exercises |

### Handling "My Company Doesn't Allow This"

This comes up in roughly 20% of corporate workshop settings. The practical response:

1. **Security concern about codebase access:** Use Plan Mode (Exercise 2) — no data leaves the machine. Claude Code reads locally.
2. **Concern about data sent to Anthropic:** Use Claude Code with an Enterprise subscription, which includes data processing agreements. Or use the exercises that work with documents only (Exercises 1, 5, 6, 8).
3. **Concern about MCP tool connections:** Skip Exercise 3 and 7. Run Exercise 4 with URLs from public competitor sites instead. The research workflow still works.
4. **Blanket prohibition:** Acknowledge it. Run the exercises in a personal context using personal projects instead of company data. The skills transfer back to the workplace.

The framing that often works: "We are not asking you to change your tools at work today. We are asking you to develop a new skill in a safe environment so you understand the capability."

### The One Exercise to Do If You Only Have Time for One

Exercise 5 (PRD Pipeline) is the single highest-value exercise if a participant only has time for one. It:
- Requires CLAUDE.md context (reinforces Exercise 1)
- Produces an artifact they will actually use
- Demonstrates the research-first workflow that makes every other output better
- Includes the review loop, which shows the difference between "Claude Code as autocomplete" and "Claude Code as a thinking partner"

If pressed further to pick one moment within Exercise 5, it is Step 4: the review loop where participants ask Claude Code to criticize what it just wrote.

---

*Previous: [09-real-stories.md](./09-real-stories.md) — Real practitioners, verified sources, and the community resources that support this work.*

*Back to index: [README.md](./README.md)*
