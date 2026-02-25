# The 9 PM Use Cases — With Real Prompts

> Copy-paste ready prompts for every high-leverage PM workflow.
> Each prompt is grounded in verified real-world examples and community patterns.

---

## How to Read This File

Each use case follows the same structure: what it is, why it matters, who's doing it in the real world, and the exact prompts you can use today. No theory. No setup required beyond a working Claude Code install.

For MCP-dependent prompts, the required MCP is noted. If you don't have it, the manual fallback is included.

---

## Use Case 1: PRD & Spec Writing

**What it is:** Context-aware spec generation grounded in your actual product — not a generic template.

### The Problem It Solves

Every PM knows the gap between writing a spec in a generic AI assistant versus writing one in a tool that knows your product. Claude.ai gives you a PRD that could apply to any company. Claude Code with CLAUDE.md gives you a PRD that references your actual personas, matches your voice, uses your template, and is grounded in decisions you've already made.

The difference isn't the quality of the language model. It's the context it's working from.

### Real Example

Dennis Yang (PM at Chime) describes his workflow: "Every PRD now starts with 30 seconds of Claude Code loading context, then writes itself with the right tone and structure." The context loading isn't incidental — it's the entire differentiator. Carl Vellotti built an entire interactive course (ccforpms.com, 1,437 GitHub stars) specifically teaching this workflow to PMs.

### Copy-Paste Prompts

**Generate a PRD from a user story:**

```
Write a PRD for [feature name].

Before writing, read:
- context/product-context.md (product vision, current OKRs)
- context/personas.md (reference [persona name] specifically)
- decisions/ (any relevant prior decisions)
- context/prd-template.md (use our format exactly)

The user story: [paste user story here]

Constraints:
- v1.0 scope only: smallest version that validates the hypothesis
- Success metrics must be measurable within 30 days of launch
- Every user story needs 3 acceptance criteria (Given/When/Then format)
- End with: the one open question that could kill this spec

Save to: specs/[feature-name]-prd.md
```

**Review a PRD against user research:**

```
Review the PRD at specs/[feature-name]-prd.md.

Also read: research/[feature-name]-pain.md (the source user research)

Check:
1. Does the proposed solution actually address the top 3 pain patterns in the research?
2. Is there anything from the research the PRD ignores?
3. Are success metrics tied to what users said they need, or to what we want to show?
4. Is v1.0 scope defensible given how severe the pain is?

Output:
- Verdict: ship as-is / revise before review
- If revise: exactly 3 specific improvements
- Gaps between research and spec
```

**Write acceptance criteria for an existing user story:**

```
Here is a user story: [paste story]

Write acceptance criteria in Given/When/Then format.
Rules:
- Each criterion must be testable by QA without judgment calls
- Cover: happy path, error states, edge cases, rollback behavior
- Maximum 5 criteria — if you need more, the story is too big
- Flag any ambiguities in the story that would make testing impossible
```

**Run the /devil check before a review meeting:**

```
/devil

Read: specs/[feature-name]-prd.md

You are the skeptical VP who needs to approve this. You have 10 minutes.
Output exactly 3 objections.

For each objection:
- What's the specific problem
- What evidence would change your mind
- What mitigation the PM should offer

End with: "Would approve if [condition]" or "Would not approve because [reason]"
```

### Expected Output

A PRD that references your actual persona by name, uses your template structure, has measurable success metrics, and doesn't contain generic filler. The /devil output gives you VP-level objections to prepare for — so you walk into the review meeting with answers, not surprises.

### Pro Tips

- Put your best existing PRD in `context/examples/good-prd-example.md`. Add to CLAUDE.md: "Match the style and depth of context/examples/good-prd-example.md." Output quality jumps immediately.
- The review step (Step 4) catches the most common PRD failure: a spec that's technically complete but doesn't solve the actual pain. Run it every time.
- Keep your PRD template in `context/prd-template.md` and reference it explicitly. Claude Code will follow a template more reliably than a style instruction.

---

## Use Case 2: Competitive Research at Scale

**What it is:** Automated collection and synthesis of competitive intelligence at a scale that was previously impossible without a team.

### The Problem It Solves

Manual competitive research is a time tax that compounds. You read 10 sources, synthesize by hand, write a brief, share it in a meeting where it's already out of date. The feedback loop is too slow to be useful for product decisions.

Claude Code with Bright Data MCP turns this from a 2-day project into a 2-hour session — and makes it repeatable on demand.

### Real Examples

A PM at Monday.com analyzed **34,000 Reddit comments** about competitor products in a single Claude Code session. What used to require a contractor and a week now costs a prompt.

Reza Rezvani (enterprise tech PM) reports an **85% reduction in time spent on competitive intelligence**. The shift: from reading and synthesizing manually, to reviewing AI-synthesized output and applying judgment.

### Copy-Paste Prompts

**Scrape competitor changelogs via Bright Data MCP:**

```
(Requires: Bright Data MCP)

Research [Competitor Name] product activity for the last 90 days.

Step 1 — Changelog and releases:
Fetch and read their public changelog at [URL].
List every feature shipped in the last 90 days: feature name, date, category (UX / API / analytics / security / pricing).

Step 2 — User sentiment on G2:
Search G2 for [Competitor Name] reviews posted in the last 90 days.
Find: top 3 pain points, top 3 praise points, any mentions of switching (to or from).

Step 3 — Roadmap signals:
Check their job postings at [URL or "search [Company] jobs"].
Infer: what they're investing in based on engineering/product role patterns.

Step 4 — Synthesize:
- Recent product bets: what they're doubling down on
- User pain they haven't solved: what their users still complain about
- Positioning: how their marketing describes them vs. 6 months ago
- Where we're differentiated vs. where we're behind

Save to: research/competitive/[competitor-name]-[date].md
```

**Manual fallback (no MCP required):**

```
I've pasted the [Competitor Name] changelog below. Also pasted: last 20 G2 reviews.

Synthesize:
1. Feature velocity: how many things shipped in the last quarter, in which categories
2. User complaints: patterns in the negative reviews
3. What users love: patterns in the positive reviews
4. Anything that suggests a new strategic direction

[Paste changelog text here]
[Paste G2 reviews here]
```

**Synthesize G2 reviews across competitors:**

```
I have review exports for 3 competitors in research/competitive/:
- competitor-a-reviews.txt
- competitor-b-reviews.txt
- competitor-c-reviews.txt

For each competitor, find:
- Top 3 complaints (with frequency signal: "mentioned in X of Y reviews")
- Top 3 strengths
- Any reviews that mention switching to or from them — what were the reasons?

Then: build a feature gap matrix.
Columns: Feature category
Rows: Competitor A, Competitor B, Competitor C, Us (based on CLAUDE.md)
Cells: Has it / Doesn't have it / Partially

Save matrix to: research/competitive/feature-matrix-[date].md
```

**Run 3 competitors in parallel:**

```
Research the following 3 competitors in parallel using separate tasks.
For each, save results to its own file before synthesizing.

Competitor A: [Company]
- Source: [changelog URL], [G2 URL], job postings at [URL]
- Save to: research/competitive/competitor-a.md

Competitor B: [Company]
- Source: [changelog URL], [G2 URL], job postings at [URL]
- Save to: research/competitive/competitor-b.md

Competitor C: [Company]
- Source: [changelog URL], [G2 URL], job postings at [URL]
- Save to: research/competitive/competitor-c.md

After all three complete:
Synthesize into research/competitive/synthesis-[date].md:
- Our gaps: things they have that we don't
- Our advantages: things we have that they don't
- Their current bets: what they're all investing in (convergent trends)
- Where the market is heading based on combined signals
```

### Expected Output

A structured competitive brief with sourced insights, a feature matrix, and strategic implications. Repeatable on demand — run it monthly for a competitive tracker that stays current.

### Pro Tips

- Bright Data is the key MCP for web-level research. For a one-time analysis, manual export + Claude synthesis gets you 80% of the value.
- Save competitive research to `research/competitive/` with dates. Reference it explicitly in roadmap and strategy prompts — it becomes your competitive context layer.
- The parallel pattern (3 competitors in separate tasks) is significantly faster than running them sequentially. Claude Code's Task tool handles this natively.

---

## Use Case 3: Data Analysis Without SQL

**What it is:** Direct access to your product data for business questions — without waiting for a data team.

### The Problem It Solves

Most PMs have a mental queue of questions they want answered. Most of those questions sit in a data team backlog, waiting for someone to write the SQL, run it, format it, and send it back. The turnaround is days. The feedback loop for product decisions is broken.

### Real Example

Dylan Colon (PM at UKG, an HR software company): **"I went from asking my data team for reports to exploring data myself in real time."**

This isn't about replacing data analysts. It's about eliminating the queue for the questions only you know to ask.

### Copy-Paste Prompts

**Natural language database queries (requires database MCP):**

```
Connect to our analytics database.

Answer these questions:
1. What is our 30/60/90-day retention rate by signup cohort for the last 6 months?
2. Which acquisition source (organic / paid / referral / direct) has the best 90-day retention?
3. Is there a feature usage pattern that predicts 12-month retention?

Format: tables I can paste into a slide.
For question 3: show me both the correlation and a plain-English interpretation.
```

**Retention analysis from CSV exports (no MCP required):**

```
Read these 3 CSV files in my project folder:
- signups.csv (monthly, by source and plan type)
- churn.csv (monthly, with reason codes)
- feature_usage.csv (weekly by feature and user segment)

Answer:
1. What's our overall monthly churn rate trend for the last 6 months?
2. Which features are used by churned vs. retained users? (usage pattern differences)
3. Are there any churn reason codes that are increasing month-over-month?
4. Which user segment is churning fastest? Which is most stable?

Don't just give me the numbers. For each finding, tell me what action it implies.
```

**Feature correlation analysis:**

```
Read: feature_usage.csv
Read: retention.csv (or query directly)

I want to understand which features correlate with long-term retention.

Analysis:
1. For each feature, calculate: % of 12-month retained users who used it in their first 30 days
2. Compare to % of churned users who used it in their first 30 days
3. Rank features by retention correlation (highest delta = strongest signal)
4. Flag anything surprising — features with high usage but low retention correlation

Format: ranked table, then a narrative paragraph with the key takeaway for product decisions.
```

**QBR preparation:**

```
I'm preparing for our quarterly business review. Pull together:

1. MoM revenue growth for Q[N] (connect to [database/analytics tool])
2. Feature adoption rates for the 3 features we shipped this quarter:
   - [Feature A]: [usage event name in Amplitude/analytics]
   - [Feature B]: [usage event name]
   - [Feature C]: [usage event name]
3. NPS delta: score at start of quarter vs. end
4. Support ticket volume trend vs. last quarter

Format as executive summary: one metric per bullet, number first, trend second, implication third.
Example format: "30-day retention: 68% (+4pp QoQ) — onboarding flow changes are holding"
```

### Expected Output

Business answers formatted for decisions, not raw data dumps. The key instruction is always: "Don't just give me the numbers — tell me what action it implies."

### Pro Tips

- For CSV analysis without an MCP, drop your exported data files into the project folder and reference them by name. Claude Code reads them natively.
- The WINNING framework (from the community PM skill) applies here: `Score = Pain × Timing × Execution Capability`. Use it when the data gives you multiple options and you need a prioritization recommendation built into the analysis output.
- Always ask for the finding AND the business implication in the same output. The implication is what you actually need.

---

## Use Case 4: User Research at Scale

**What it is:** Processing, synthesizing, and extracting patterns from user research at a scale that was previously impractical for a single PM.

### The Problem It Solves

Before: synthesize 10 user interviews over 2 weeks, and that's considered a solid research sprint.

After: synthesize 200 interview transcripts, 500 support tickets, and 10,000 app reviews in an afternoon, with sourced quotes for every theme.

### Real Examples

The Monday.com PM analyzed 34,000 Reddit comments in one session. Ondrej Machart (Head of Product, non-engineer) built TinyStakeholders.com — a user validation tool with 5,000+ readers — directly in Claude Code. Both demonstrate the same principle: the bottleneck in user research is synthesis, and Claude Code eliminates it.

### Copy-Paste Prompts

**Synthesize interview transcripts:**

```
Read all files in research/interviews/.
Each file is a transcript from a user interview (participant ID in filename).

Synthesize:
1. Top 3 pain points across all participants
   - For each: frequency ("mentioned by X of Y participants"), severity signal (emotional language, workarounds)
   - Best direct quote per pain point (exact text + participant ID)
2. Any pain point mentioned by 70%+ of participants — flag this as a critical signal
3. Things participants said they want that we don't currently offer
4. Any patterns by segment (if segment info exists in filenames or first line of transcripts)

Format as a JTBD analysis. For each major theme, write:
"When [situation], [segment] wants to [motivation], so they can [outcome]."

Save to: research/user-pain/interviews-synthesis-[date].md
```

**Cluster pain themes from support tickets:**

```
Read: [path to support ticket export or use [support tool MCP]]
These are [N] support tickets from Q[N].

Find:
1. Top 10 issue categories by volume (count per category)
2. Categories that are increasing MoM vs. decreasing
3. Tickets that stayed open >7 days — what makes these hard to resolve?
4. Any tickets that mention a competitor by name — what are users comparing?
5. Top 5 tickets with the highest customer sentiment signals (frustration, escalation language)

End with: prioritization recommendation.
Format: top 3 issues to address with rationale for each ranking.

Save to: research/user-pain/support-synthesis-[date].md
```

**Extract JTBD statements:**

```
Read: research/user-pain/interviews-synthesis-[date].md
Read: research/user-pain/support-synthesis-[date].md

Extract 5-7 canonical JTBD statements from the combined research.

Format each as:
- Situation: [when this need arises]
- Motivation: [what they're trying to do]
- Outcome: [what "done" looks like for them]
- Current workaround: [what they do today without this]
- Hiring condition: [what triggers the "hire" — when they'd switch or adopt]
- Frequency: [how often this job occurs]
- Segment: [which users this applies to most]

Rank by: frequency × severity.
Flag: any JTBD that is not addressed by our current roadmap.
```

**App review analysis:**

```
(Requires: Bright Data MCP, or paste reviews as text)

Read/scrape [N] most recent [App Store / Google Play] reviews for [product name].

Analyze:
1. What do users love? (recurring praise themes with frequency)
2. What frustrates users? (recurring complaint themes with frequency)
3. Reviews that mention switching — from what, and why
4. Any patterns in 1-2 star reviews that could be addressed with product changes vs. support

Also flag: any review that contradicts our internal user research assumptions.
(Our current assumptions are in context/personas.md)
```

### Expected Output

A structured synthesis with sourced quotes, frequency signals, JTBD statements, and prioritization signals. The output feeds directly into Step 1 of the PRD pipeline (Use Case 1).

### Pro Tips

- File organization matters: put all interview transcripts in `research/interviews/`, all support exports in `research/support/`. Claude Code reads whole directories at once.
- Always ask for exact quotes with participant IDs. When you present research in a review meeting, citing specific users by ID is more credible than paraphrasing.
- The JTBD extraction step is worth running separately even if you've done synthesis. JTBD statements are the direct input for PRD Problem Statements — having them pre-extracted saves a full round of prompting.

---

## Use Case 5: Prototyping

**What it is:** Building functional software from a product description — without writing code yourself.

### The Problem It Solves

The gap between "idea" and "working prototype" used to be weeks of engineering time and stakeholder calendars. Claude Code closes that gap. You describe what you want; Claude builds it. You review and iterate. The feedback loop that used to take weeks now takes hours.

### Real Examples

Ondrej Machart (Head of Product, not an engineer) published an iOS app on the App Store, solo. He also built TinyStakeholders.com, which serves 5,000+ readers.

Boris Cherny (Anthropic), who shipped Cowork in January 2026 — Anthropic's tool specifically for non-technical Claude Code users: **"It was just kind of obvious that Cowork is the next step. We just want to make it much easier for non-programmers."**

This is Anthropic's official signal: product managers and non-technical users are a primary audience.

### Copy-Paste Prompts

**PRD to architecture to prototype:**

```
Read: specs/[feature-name]-prd.md

I want to build a working prototype of this feature.

Constraints:
- I am not an engineer. Explain technical choices simply.
- Build for browser (HTML/CSS/JS or React is fine)
- Use static data for now — no database needed
- Simpler is always better. When in doubt, build less.
- The goal: something a real user can click through in 10 minutes to give feedback

Step 1: Propose the minimal architecture. What do we actually need to build?
Step 2: Build the first screen and the single most important interaction.
Step 3: Tell me how to open it in my browser.

If anything breaks or is confusing: tell me exactly what to do, don't assume I know.
```

**Iterate on a prototype:**

```
I ran the prototype and here's what I'm seeing: [describe what's working, what's not]

The specific thing I want to change: [describe the change]

Before changing anything: confirm you understand what I'm asking.
After: tell me which files you changed and what I should look at to verify it worked.
```

**Build a data processing tool:**

```
I need a simple tool to [describe what the tool should do].

Input: [describe the data — CSV, text, form input]
Output: [describe what comes out]

Build it as a web tool I can open in a browser.
No login, no database, just: upload/paste input, get output.
Simple, functional, handles the main case well.
```

### Expected Output

A working prototype you can open in a browser and walk a real user through. Not production-ready — that's not the goal. The goal is a concrete artifact to react to, faster than any other method.

### Realistic PM Prototype Scope

What consistently works: single-screen web tools, internal dashboards, data processing scripts, simple CRUD apps, mobile apps (with more iteration), browser extensions.

What requires more iteration or engineering support: complex multi-user real-time apps, high-performance at scale, apps with complex security requirements.

### Pro Tips

- Start with the PRD as input to the prototype. You've already specified what it should do — let Claude read it.
- Always ask Claude to explain what you should see and how to open it. Don't assume you know how to run it.
- Build the simplest version first. Evaluate with users before adding features. The mistake is over-specifying the prototype before getting feedback.

---

## Use Case 6: Technical Exploration (Plan Mode)

**What it is:** Safe, read-only codebase understanding — answering technical questions without any risk of changing code.

### The Problem It Solves

PMs need technical context to write good specs, scope features accurately, and have credible conversations with engineers. The traditional path: schedule a meeting, ask your tech lead to walk you through it, hope they have time. The result: you're always one meeting behind on context.

Plan Mode gives you direct access to the same source of truth engineers use — the code itself — with zero risk of breaking anything.

### How Plan Mode Works

When you open Claude Code in a project with read-only permissions (or use the `--plan` flag), it can read every file in the codebase but cannot modify anything. It's a safe exploration mode for PMs.

### Copy-Paste Prompts

**Explain a feature end-to-end:**

```
[Plan Mode — read only]

Explain how [feature name] works, step by step.

Start from the user action: "[what the user does]"
Trace through to: "[the outcome the user sees]"

For each step:
- What code/service handles it
- What data it reads or writes
- Any external services involved
- Anything that could go wrong at this step

Format: numbered list, one step per item, plain English not code.
```

**Impact analysis of removing a feature:**

```
[Plan Mode — read only]

We're considering removing [feature / component / API endpoint].

Before we do: what depends on it?

Find:
1. All the places in the codebase that call or import [feature]
2. Any external systems (integrations, webhooks, third parties) that might depend on it
3. Any database tables or fields that exist only to support this feature
4. User-facing impact: what would users lose if this were removed?

I want to understand the full blast radius before we make this decision.
I am not asking you to make any changes.
```

**Understand the payment flow:**

```
[Plan Mode — read only]

Explain how payments work in our product.

Specifically:
1. What happens when a user initiates a payment?
2. Which payment provider(s) do we use?
3. How do we handle payment failures?
4. What does the retry logic look like?
5. How are refunds processed?
6. Where does payment data live and what is it?

I'm a PM scoping a feature that touches the payment flow.
I need enough context to write a spec that doesn't create problems.
```

**Scope estimation for a feature:**

```
[Plan Mode — read only]

I'm scoping a feature: [describe the feature]

Help me understand the technical surface area.

1. What existing code would this feature touch or extend?
2. What new code would need to be written?
3. Are there any existing abstractions I could reuse?
4. Are there any parts that look particularly complex or risky?
5. Any existing technical debt in the relevant areas that might slow this down?

I'm not asking for estimates. I'm asking for context to have a better scoping conversation with the engineering team.
```

### Expected Output

A plain-English explanation of how the codebase works in the relevant area, with specific file references. More accurate than documentation (which may be outdated) and more detailed than what an engineer summarizes in a meeting.

### Pro Tips

- Always specify "Plan Mode — read only" at the top of technical exploration prompts. It signals intent and Claude will be careful not to suggest changes.
- Ask Claude to cite the specific file and line number for key claims. "Tell me where in the codebase this is implemented" is a verification step before you act on any technical explanation.
- Use this before writing any spec that touches existing infrastructure. 30 minutes of technical exploration can save hours of back-and-forth with engineering.

---

## Use Case 7: Roadmap & Strategy

**What it is:** Using Claude Code to accelerate strategic analysis, roadmap alignment, and prioritization — removing the scaffolding work so you can focus on judgment.

### The Problem It Solves

Strategic analysis takes time not because the thinking is hard, but because the assembly is hard. You need to read 3 research files, your roadmap doc, last quarter's feedback synthesis, and a competitor brief — then synthesize across all of them. Claude Code reads all of it and synthesizes it. You apply the judgment.

### Real Example

Thomas Landgraf builds Gantt charts from markdown files, generates strategic analysis in a few hours that previously took a week. His system uses CLAUDE.md as the orchestration document, markdown files as the data layer, and Git as the changelog.

### Copy-Paste Prompts

**Roadmap vs. customer feedback gap analysis:**

```
Read: roadmap/[current-roadmap-file].md
Read: research/user-pain/q[N]-synthesis.md
Read: research/competitive/synthesis-[date].md (if exists)

Find:
1. Roadmap items that directly address top customer pain points (strong alignment)
2. Roadmap items we're building that customers aren't explicitly asking for
   (possible over-investment, or things we're doing for strategic vs. customer reasons)
3. Customer pain points in the top 5 that are NOT on the roadmap
   (potential gaps — flag severity and frequency from research)

Present as:
- Alignment score: X of [N] roadmap items directly map to stated customer pain
- Top 3 gaps: customer pain we're not addressing
- Top 3 questions: roadmap items whose customer rationale isn't clear from research

This is for internal review, not external use.
Save to: research/roadmap-gap-analysis-[date].md
```

**Prioritization recommendation using WINNING:**

```
I have [N] potential features to prioritize for next quarter.
Here is the list with brief descriptions:
[list features and 1-sentence descriptions]

Score each using the WINNING framework:
  Score = Pain × Timing × Execution Capability

Pain (1-10): How much does this problem hurt the user right now?
  Evidence: [reference research/user-pain/ files or specify "use CLAUDE.md context"]
Timing (1-10): Is there a window where shipping this would land better? Market conditions, competitive pressure, customer requests?
Execution (1-10): Can our current team build this well, given current technical state and team capacity?

For each feature:
- Three scores with rationale
- Final WINNING score (P × T × E)
- Top concern (what would make this score drop)

Rank by score. Flag any feature where scores are highly uncertain — we may need more data.
```

**Dependency mapping:**

```
Read: roadmap/[current-roadmap-file].md

For each initiative on the roadmap:
1. What does it technically depend on? (must be built/refactored first)
2. What does it strategically enable? (unlocks another initiative)
3. What external dependencies exist? (third parties, other teams, infrastructure)

Build a dependency tree:
- Critical path: what's the longest chain? What's the bottleneck?
- Parallel tracks: what can run simultaneously?
- Risk concentration: where does one delay cascade into many?

Flag any initiative that is a hidden prerequisite for 3+ other initiatives.
That's a sequencing risk worth escalating.

Save to: roadmap/dependency-map-[date].md
```

### Expected Output

A structured gap analysis or prioritization output that makes your reasoning explicit and auditable. The goal isn't to replace your judgment — it's to do the assembly work so you can spend time on the judgment.

### Pro Tips

- The WINNING framework from the community PM skill (`ooiyeefei/ccc`) is more practical than RICE for teams where reach data is hard to get. Pain, Timing, and Execution Capability are all things a PM can estimate without a data pull.
- Always save roadmap analysis outputs with dates. You'll want to compare them quarter over quarter.
- The dependency mapping prompt is worth running at the start of every planning cycle. It surfaces sequencing risks before the sprint starts, not during it.

---

## The WINNING Framework (Reusable Template)

From the community PM skill (`ooiyeefei/ccc`) — more practical than RICE for solo PMs and small teams.

```
Score each feature/initiative using WINNING:

  WINNING Score = Pain × Timing × Execution Capability

PAIN (1-10)
How severely does this problem hurt the user?
- 10: Users report it as their biggest blocker. High churn signal. Workarounds are painful.
- 5:  Annoying but users have workable workarounds.
- 1:  Minor inconvenience. Users rarely mention it.
Evidence: [source — support tickets, interviews, NPS comments]

TIMING (1-10)
Is there a window where this lands better than average?
- 10: Competitor just launched this. Customer contract renewal pressure. Regulatory deadline.
- 5:  No special pressure, but no reason to wait either.
- 1:  Better to do later — team is distracted, market timing is off.
Evidence: [source — competitive research, customer feedback, market signals]

EXECUTION CAPABILITY (1-10)
Can this team build this well right now?
- 10: We've built similar things. Clear architecture. Team has bandwidth.
- 5:  Doable but will require learning or some refactoring.
- 1:  New domain for the team. High technical debt in the area. Key person unavailable.
Evidence: [source — engineering input, current sprint state, technical context]

FINAL SCORE = Pain × Timing × Execution Capability
Maximum: 1000. Interpret relative to other items being scored.

TOP CONCERN:
[One sentence: what would make this score wrong]
```

---

## Use Case 8: Replacing or Augmenting PM Tools

**What it is:** Using Claude Code as the intelligence layer on top of your PM tools — or, in the extreme case, replacing those tools entirely.

### The Problem It Solves

PM tools are expensive, rigid, and optimized for project tracking — not for the kind of synthesis and analysis PMs actually need. Claude Code can query your existing tools via MCP and give you the analysis layer those tools don't have. For some PMs, that's augmentation. For a few, it's complete replacement.

### Real Example

Thomas Landgraf replaced Jira with a **600-line CLAUDE.md**. Issues are markdown files in a directory structure. Status is tracked via file naming conventions and frontmatter. Sprint planning is a Claude Code session. Gantt charts are generated on demand from markdown. Git history is the changelog. Zero vendor lock-in, full-text search over everything, no subscription.

His quote: "Zero vendor lock-in. Complete control. Full-text search over everything. Git history instead of activity logs. No subscription."

This is the extreme end of the spectrum. You don't have to go there. But knowing it's possible changes how you think about what's optional in your current stack.

### The Spectrum

**Light augmentation (most PMs):** Keep Jira/Linear/Notion. Connect them via MCP. Ask Claude to synthesize, analyze, prioritize.

**Medium:** Build custom reports and summaries on top of existing tools. Claude Code generates the analysis; the tool stores the data.

**Full replacement (rare, radical):** Thomas Landgraf pattern. Markdown files + CLAUDE.md + Git. Requires investment but eliminates tool costs and lock-in.

### Copy-Paste Prompts

**Jira sprint analysis (requires Jira MCP):**

```
(Requires: Jira MCP)

Look at Sprint [N] in our Jira board.

Pull:
1. All tickets in the sprint: ID, title, type, story points, assignee, final status
2. Tickets carried from last sprint (calculate % of sprint that was carryover)
3. Tickets added mid-sprint after sprint start (scope creep indicator)
4. Average cycle time (created → done) for tickets that completed

Analyze:
- Sprint completion rate: X% of committed story points delivered
- Carryover pattern: are we consistently carrying tickets?
- Scope change rate: how much was added after sprint start?
- Any tickets that took 3× longer than estimated?

End with: the 2 things this sprint tells us about our planning accuracy.
Save to: notes/sprint-[N]-retrospective-[date].md
```

**Backlog prioritization via RICE scoring:**

```
(Requires: Jira/Linear MCP or paste backlog)

Read our current backlog from Jira: issues labeled [backlog-label], status = open.

For each issue, score using RICE:
- Reach: how many users affected? (1-1000 scale, estimate from description + labels)
- Impact: how much does it improve experience? (0.25 / 0.5 / 1 / 2 / 3)
- Confidence: how sure are we about R and I? (50% / 80% / 100%)
- Effort: person-weeks to complete (estimate from story points if available)

RICE = (Reach × Impact × Confidence) / Effort

Output: ranked table (highest RICE first), with scores and rationale.
Flag: top 3 items where RICE score conflicts with current priority order — explain why.
```

**Backlog prioritization via WINNING scoring:**

```
Read our current backlog: [Jira/Linear MCP query or paste list]

Score each item using WINNING:
  Score = Pain × Timing × Execution Capability (each 1-10)

Pain: from user research context (context/personas.md and research/user-pain/ files)
Timing: from competitive context (research/competitive/ files) and current OKRs (context/product-context.md)
Execution: from what I know about current team state (use conservative estimates)

Rank by WINNING score.
Flag: items in the top 5 by WINNING that are currently ranked lower than #10 in backlog.
Those are your undervalued opportunities.
```

### Expected Output

Structured analysis with explicit scoring rationale. The key value is making implicit prioritization explicit — so you can defend it in planning meetings or challenge it with data.

### Pro Tips

- The Jira MCP has 80 stars and is stable. Linear MCP has 129 stars and a cleaner API. Both work for the patterns above.
- Notion MCP is real but slow and token-heavy for large workspaces. Export your Notion pages to Markdown, put them in the project folder, and reference them as files. Use MCP only for live queries (today's sprint, current meeting notes).
- If you want to try the Thomas Landgraf pattern (markdown-based PM system): start with issues only. Keep Jira for stakeholder visibility if needed; use markdown + Claude for your personal workflow.

---

## Use Case 9: Documentation That Stays Current

**What it is:** Generating and maintaining product documentation from actual source material — the codebase, meeting notes, and research — rather than maintaining it manually.

### The Problem It Solves

Documentation drifts. You write it at launch; engineers change the implementation; nobody updates the doc. Six months later, new PMs are making decisions based on outdated documentation, and nobody knows which parts are accurate.

Claude Code reads the actual source of truth (the code itself, the actual meeting notes, the real research) and generates documentation from that. It also flags when documentation and implementation have drifted apart.

As Karl Wirth (enterprise tech PM) puts it: "The moment you let Claude read the actual codebase instead of the documentation, you stop working from a copy of a copy."

### Copy-Paste Prompts

**Generate docs from code:**

```
[Plan Mode — read only]

Read the code in [directory path].

Write user-facing documentation for [feature name].

Audience: [who reads this — end users / internal team / API developers]
Tone: [per context/tone-and-voice.md, or describe: technical / friendly / concise]

Include:
- What the feature does (1 paragraph, no jargon)
- How to use it (numbered steps)
- What to do when it doesn't work as expected (top 3 error states)
- Any limits or constraints users should know about

Do not document what isn't implemented. If you see discrepancies between comments and code, note them.
```

**PRD vs. implementation gap detection:**

```
[Plan Mode — read only]

Read: specs/[feature-name]-prd.md (what we planned to build)
Read: [directory path] (what is actually implemented)

Compare:
1. Acceptance criteria in the PRD — which are fully implemented, partially, or not at all?
2. Any functionality in the code that isn't in the PRD (scope that was added)
3. Any PRD requirements where the implementation seems different from the spec

Format:
- Fully implemented: [list]
- Partially implemented: [list + what's missing]
- Not implemented: [list]
- Added scope (not in PRD): [list]
- Spec vs. implementation discrepancies: [describe each]

This is for internal audit. Tone: direct, no diplomatic softening.
```

**Decision log from meeting notes:**

```
Read: notes/meeting-[date].md (or paste meeting notes here)

Extract all product decisions made in this meeting.

For each decision, use this format:

---
Decision: [topic]
Date: [date from meeting]
Decided: [what was chosen, one sentence]
Why: [rationale, 2-3 sentences]
Alternatives rejected: [what was considered but not chosen, and why]
Owner: [who is responsible for this going forward]
Review trigger: [what would cause us to revisit this decision]
Open questions: [anything still unresolved]
---

If a decision was discussed but not finalized: flag it as "pending" and include what would close it.

Save each decision as a separate file:
decisions/[YYYY-MM-DD]-[topic-slug].md
```

**Rolling documentation maintenance:**

```
Instruction for CLAUDE.md (add this to your file):

---
## Documentation Maintenance Rule
At the end of any session that produces a new file (spec, research, decision):
1. Append a one-line entry to notes/index.md:
   [filename] | [one sentence description] | [date]
2. If any session decision contradicts something in context/product-context.md, flag it.
   Do not automatically update — ask me to confirm before changing stable context.
---
```

### Expected Output

Documentation that reflects what the code actually does, PRD/implementation gap reports for audits, and a decision log that captures institutional memory. The rolling documentation pattern (the CLAUDE.md instruction above) creates a self-maintaining index of everything in your project.

### Pro Tips

- The PRD vs. implementation gap detection prompt is worth running before any feature retrospective. It answers the question "did we build what we said we'd build?" with evidence.
- Keep `notes/index.md` maintained. After a few weeks, it becomes an auto-generated knowledge map. Ask Claude: "Read notes/index.md and tell me what context exists for Initiative X" — it surfaces relevant files before you start.
- Decision logs in `decisions/` with dates are more durable than decisions buried in Notion. They're searchable, version-controlled, and readable by Claude Code in the next session without any context-setting.

---

## Quick Reference: Which Prompt for Which Problem

| Problem | Use Case | Key MCP |
|---------|----------|---------|
| Writing a PRD and it sounds generic | 1: PRD Writing | None — CLAUDE.md is the fix |
| Competitive brief takes 2 days | 2: Competitive Research | Bright Data |
| Waiting on data team for a report | 3: Data Analysis | Database MCP or CSV |
| Too many user interviews to synthesize | 4: User Research | None |
| Need to validate an idea before spec | 5: Prototyping | None |
| Don't understand how a feature works | 6: Technical Exploration | None (Plan Mode) |
| Roadmap gaps vs. customer needs | 7: Roadmap & Strategy | Jira + Analytics MCPs |
| Jira sprint review is manual | 8: Tool Augmentation | Jira MCP |
| Documentation is always out of date | 9: Documentation | None (Plan Mode) |

---

## The Files Claude Code Needs to Be Useful

The fastest path to better prompts is better context loading. These files, referenced in CLAUDE.md, are what turn generic Claude into your PM assistant:

```
context/
├── product-context.md    ← vision, OKRs, current focus, key metrics
├── personas.md           ← user archetypes, jobs-to-be-done, segments
├── tone-and-voice.md     ← how you write (what to do and not do)
├── prd-template.md       ← your standard PRD structure
├── decision-template.md  ← standard decision log format
└── examples/
    ├── good-prd.md       ← your best existing PRD (Claude will match it)
    └── good-user-story.md ← example of your acceptance criteria format
```

Invest 2 hours building these files once. Every prompt you write for the next year benefits.
