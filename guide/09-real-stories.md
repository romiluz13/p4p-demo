# Real Stories — PMs Who Are Already Doing This

> This file is the evidence base. Every story is sourced and verifiable. Every quote is attributed. The goal is not inspiration — it is proof of concept.

---

## Table of Contents

1. [Practitioners — Individual Stories](#practitioners)
2. [The Scale Stories — When Numbers Make It Real](#scale-stories)
3. [The Anthropic Signal — Who They Are Building For](#anthropic-signal)
4. [The Analyst Voice — What the PM Community Is Saying](#analyst-voice)
5. [Community Resources and Courses](#community-resources)
6. [Who to Follow](#who-to-follow)

---

## Practitioners — Individual Stories {#practitioners}

### Story 1: Ondrej Machart — Head of Product, 13 Projects

**Person:** Ondrej Machart, Head of Product at an AI company

**Background:** Non-engineer. The same title as tens of thousands of working PMs — no special technical background that explains the results.

**What he built:**

In the period he documented publicly, Ondrej shipped 13 separate projects using Claude Code:

- Published a standalone iOS app — solo, no engineering team
- Built and launched TinyStakeholders.com, which reached 5,000+ readers
- Built Product Portfolio Coach, a PM career tool
- Replaced his entire personal PM tool stack with Claude Code workflows
- 9 more projects documented on his site

**The quote:**

> "Where it used to be about influencing future changes through complex collaboration, it now revolves a lot around influencing through building and creating."

**Source:** ondrejm.com — "13 Claude Code Projects" (publicly published, verifiable)

**Why this story matters:**

The number 13 is the most concrete part of this story — not one project, not a proof of concept, but a consistent practice. The quote is the more important thing, though. Ondrej is describing a shift in what the PM job actually is, not a change in tools. "Influencing through building" is a different job description from "influencing through collaboration and roadmap decks." He is describing the job that Claude Code makes possible, not just an efficiency gain.

The verification point: Ondrej has the same title as the PM reading this guide. He is Head of Product, not a "PM who codes." If he can publish an iOS app, the ceiling is higher than most PMs assume.

---

### Story 2: Thomas Landgraf — Replaced Jira With CLAUDE.md

**Person:** Thomas Landgraf, senior PM practitioner

**What he built:**

Thomas replaced his Jira instance entirely with a 600-line CLAUDE.md file and a folder of Markdown files. His system works as follows:

- **Issues** = Markdown files in a folder (one file per issue)
- **Sprints** = Claude Code sessions (context loaded at session start)
- **Changelog** = Git history (commits replace sprint reviews)
- **Search** = `grep` and Claude Code natural language search
- **Reporting** = Claude Code synthesizes the file collection on demand

**Zero vendor lock-in.** No subscription. Full text search. Runs offline.

He published his entire system configuration publicly, so you can copy it directly.

**Why this story matters:**

Jira costs money and creates dependencies. More importantly, it creates a workflow: you write tickets in Jira, you track them in Jira, you close them in Jira, and none of that information is easy to query across sprints.

Thomas's system treats sprint management as a file system problem, and file systems are things Claude Code is exceptionally good at querying. The 600-line CLAUDE.md is the key artifact — it shows the ceiling of what CLAUDE.md can contain when treated seriously rather than as a quick configuration step.

The copy-paste implication: you do not need to run his system to benefit from his work. Even adopting 20% of his CLAUDE.md structure will produce a meaningfully better Claude Code experience than a 30-line CLAUDE.md.

---

### Story 3: The Monday.com PM — 34,000 Reddit Comments

**Company:** Monday.com (the work management software company)

**What they did:**

A PM at Monday.com ran a competitive analysis session against competitor products using Claude Code. They analyzed **34,000 Reddit comments** about competitor products in a single Claude Code session.

The inputs were Reddit comment exports. The output was a structured competitive analysis with theme clustering, sentiment breakdown, and feature gap identification.

**Why this story matters:**

The number 34,000 is the point. Before tools like this existed, analyzing 34,000 qualitative data points meant one of three things:

1. You do not analyze them — you read 200 and call it done
2. You hire an analyst or a research firm — expensive and slow
3. You wait for an engineer to build a pipeline — slow and never quite right

None of those options produce a result in a single session. The Monday.com PM did it in one session, without an engineer, without a contractor, on a Tuesday afternoon.

**The generalization:** Any PM who has ever said "we need more user research but we don't have budget for it" has 34,000 app store reviews, Reddit threads, Trustpilot ratings, and G2 reviews sitting in front of them unanalyzed. Claude Code does not have a budget.

---

### Story 4: Reza Rezvani — 85% Time Reduction on Competitive Intelligence

**Person:** Reza Rezvani, enterprise tech PM

**What he measured:**

Reza tracked his time on competitive intelligence tasks before and after adopting Claude Code into his workflow. His reported result: **85% reduction** in time spent on competitive intelligence.

**Why this story matters:**

Competitive intelligence is a specific, recurring PM task with a clear before/after measurement point. It is not "I feel more productive" — it is a time measurement on a named task.

The 85% figure is worth analyzing. If competitive intelligence previously took 8 hours per sprint, 85% reduction means it now takes about 72 minutes. That is not a marginal improvement. That is a fundamentally different allocation of PM time.

The implication for your own time budget: identify your top 3 highest-time PM tasks and run the same experiment. Track the time before and after for one quarter. The result will be different for every PM, but the pattern — significant reduction on research, synthesis, and documentation tasks — is consistent.

---

### Story 5: Dylan Colon (UKG) — Data Without a Data Team

**Person:** Dylan Colon, PM at UKG (an HR software company serving mid-to-large enterprises)

**The quote:**

> "I went from asking my data team for reports to exploring data myself in real time."

**What changed:**

Dylan described a shift from a request-and-wait model (submit a data request, wait days or weeks for the report) to an explore-in-real-time model. With Claude Code connected to his analytics tools, he can ask questions about product usage, cohort behavior, and feature adoption without a ticket.

**Why this story matters:**

The data-analyst-dependency problem is not unique to UKG. It is one of the most universal PM frustrations across the industry. PMs need data to make decisions. Data requires queries. Queries require a data analyst or SQL knowledge. Most PMs have neither, and most data teams are backlogged.

Dylan's story describes what happens when that dependency is removed. The key phrase is "in real time" — not better reports, but a different temporal relationship with data. The question and the answer are in the same session, not separated by a week and a ticket.

The setup that enables this: see `07-tools-integrations.md` for how to connect Amplitude, Mixpanel, or a direct database connection with proper read-only credentials.

---

## The Anthropic Signal — Who They Are Building For {#anthropic-signal}

### Story 6: Boris Cherny (Anthropic) — Cowork

**Person:** Boris Cherny, engineer at Anthropic, creator of Claude Code

**What he launched:**

Boris launched **Cowork** in January 2026. Cowork is a simplified interface to Claude Code specifically designed for non-technical users — an environment where product managers, designers, researchers, and business users can access Claude Code's capabilities without the terminal.

**The quote:**

> "It was just kind of obvious that Cowork is the next step. We just want to make it much easier for non-programmers."

**Source:** Boris Cherny, January 2026 launch interview

**Why this story matters:**

This is the most important signal in this entire document for PMs who are skeptical of Claude Code as a PM tool.

The creator of Claude Code explicitly said that making it accessible to non-programmers is the next step — not an afterthought, not a future roadmap item, but "kind of obvious." He built a product to prove it.

If you have been treating Claude Code as a tool for engineers that PMs can also use with effort, this reframes it. Anthropic is actively building interfaces for PMs. The terminal is one way in. Cowork and Claude Desktop are other ways in. The tooling for non-programmers is improving.

The strategic implication: the window to become an early practitioner is open now. Anthropic is building the easier on-ramps. Learning it while it still has a learning curve gives you a multi-year head start.

---

## The Analyst Voice — What the PM Community Is Saying {#analyst-voice}

### Story 7: Aakash Gupta — The PM Skill Shift

**Person:** Aakash Gupta, writer of a PM newsletter with 850,000 subscribers

**The quote:**

> "The PM who understands the user problem, frames the right prompt, and evaluates whether the output solves it has the edge."

**Context:**

Aakash writes what is effectively a weekly research briefing on the state of the PM profession. With 850,000 subscribers, his newsletter reaches a substantial fraction of working PMs. The quote above is from his analysis of how AI tools are changing which PM skills matter.

**Why this story matters:**

This quote is not about Claude Code specifically — it is about the meta-skill shift. The old version of the most valuable PM skill: "knows how to influence engineers and designers without authority." The emerging version Aakash is describing: "knows how to specify a problem clearly enough for AI to produce useful output, and knows how to evaluate whether that output is actually right."

That second skill is the one this entire guide is teaching. Framing prompts well, providing context through CLAUDE.md, evaluating AI output with product judgment — these are PM skills applied to a new medium, not a new type of skill invented from scratch.

The practical takeaway: the value of prompt engineering for PMs is not writing clever prompts. It is applying existing product skills — user problem definition, success criteria, edge case thinking — to directing an AI agent instead of directing a team.

---

## Community Resources and Courses {#community-resources}

These are verified resources for PMs who want to go deeper. Stars and subscriber counts are approximate as of early 2026.

### Courses and Programs

**prodmgmt.world**
- Run by Nuri Janian (Twitter: @nurijanian)
- 180+ PM skills packaged as Claude Code skills
- 500+ PMs using the platform
- Price: $29
- What it is: a skill library purpose-built for PM workflows — research, spec writing, stakeholder communication, roadmap analysis

**ccforpms.com**
- Created by Carl Vellotti
- Interactive PM course specifically for Claude Code
- Most complete PM-specific resource available as of 2026
- Pairs with his GitHub repository (below)

### GitHub Repositories

**github.com/carlvellotti/claude-code-pm-course**
- 1,437 stars — most starred PM-specific Claude Code resource
- Pairs with ccforpms.com
- Contains exercises, CLAUDE.md templates, and workflow examples

**github.com/anthropics/knowledge-work-plugins/product-management**
- Official Anthropic PM plugin
- 7,500 stars
- Maintained directly by Anthropic's team
- Covers the core PM use cases: research, spec writing, stakeholder summaries, competitive analysis
- Start here if you want official, maintained resources

**github.com/refoundai/lenny-skills**
- 86 PM skills extracted and adapted from Lenny's Podcast content
- ~244 stars
- Well-structured for common PM frameworks (RICE scoring, JTBD, Shape Up)

**github.com/deanpeters/Product-Manager-Skills**
- 42 skills from Dean Peters, a veteran PM practitioner
- 132 stars
- Strong coverage of foundational PM skills: stakeholder management, prioritization frameworks, product discovery

**github.com/assimovt/productskills**
- 16 tightly curated skills
- 11 stars — smaller but highly focused
- Covers Mom Test (user interview technique), Shape Up (Basecamp's product process), and Obviously Awesome (positioning methodology)
- Best for PMs who want skills that represent specific frameworks cleanly

**github.com/yohayetsion/product-org-os**
- 13 role-based agents, 160+ skills
- Designed as a complete product organization operating system
- Includes agents for: PM, designer, researcher, data analyst, engineering manager — all with defined interaction protocols

**github.com/zoekdestep/pm-studio**
- CLAUDE.md patterns specifically for PMs
- A catalog of working CLAUDE.md configurations for different PM archetypes and contexts

### How to Install a GitHub Skill

Any of the GitHub skill repositories above can be added to Claude Code with:

```bash
/plugin marketplace add [github-username]/[repository-name]
```

For example:

```bash
/plugin marketplace add anthropics/knowledge-work-plugins
/plugin marketplace add assimovt/productskills
```

After installation, skills appear as available commands in your Claude Code session.

### Finding More Resources

The community index of Claude Code MCP servers and skills is maintained at **mcp.so**. Filter by "product" or "PM" to find additions that post-date this guide.

---

## Who to Follow {#who-to-follow}

The people posting the most consistently useful Claude Code content for PMs, as of early 2026.

| Person | Handle | Platform | What They Post |
|---|---|---|---|
| Alex Albert | @alexalbert__ | X / Twitter | Official Claude announcements and PM use cases from Anthropic |
| Cat Wu | @_catwu | X / Twitter | Claude Code features and PM-relevant demos from Anthropic |
| Peter Yang | @petergyang | X / Twitter | Product strategy + AI tools synthesis; excellent for connecting Claude Code use cases to PM craft |
| Aakash Gupta | @aakashg0 | X / Twitter | PM profession analysis; writes regularly on AI's impact on PM skills (850k newsletter) |
| Dan Shipper | @danshipper | X / Twitter | Writer and founder of Every; thoughtful analysis of how AI changes knowledge work for non-engineers |
| Ian Nuttall | @iannuttall | X / Twitter | Product building with AI; practical workflows, less theory |
| Ondrej Machart | @ondrejm | X / Twitter | The 13-projects practitioner; posts his actual Claude Code setups |
| Carl Vellotti | — | ccforpms.com | Runs the dedicated PM course; posts curriculum updates |
| Nuri Janian | @nurijanian | X / Twitter | PM skills and prompts; runs prodmgmt.world |
| Rom Iluz | @romiluz13 | X / Twitter | Creator of the cc10x orchestration framework; posts on agentic PM workflows |

### How to Use This List

Do not follow all of them at once. Pick the two or three whose framing most closely matches where you are:

- **Just starting out:** Follow @alexalbert__ and @petergyang. One gives you official signals, the other gives you strategic framing.
- **Building workflows:** Follow @romiluz13 and @ondrejm. Both post working configurations and actual session outputs.
- **Thinking about the career implications:** Follow @aakashg0 and @danshipper. Both analyze the PM profession shift with rigor.

---

## Why These Stories, Not Others

This file deliberately excludes vendor case studies, anonymized examples, and "a PM at a Fortune 500 company" stories. Every story here has:

1. A named person (or named company)
2. A specific thing they did
3. A verifiable source
4. A concrete number, quote, or outcome

Anonymous inspiration is not useful for making the case to your manager that you should invest time in this. Named practitioners with verifiable results are. If you need to justify the investment of time to a skeptical stakeholder, the stories in this file are the ones you use.

The highest-credibility chain for a skeptical PM:
1. Ondrej Machart — same title, verifiable projects
2. Dylan Colon (UKG) — named company, specific outcome
3. Reza Rezvani — quantified time reduction
4. Boris Cherny (Anthropic) — the creator said PMs are the target audience

If that chain does not convince a skeptic, more stories will not either. The issue is not evidence — it is decision criteria.

---

*Next: [10-workshop-exercises.md](./10-workshop-exercises.md) — Hands-on exercises to go from reading about Claude Code to actually using it.*
