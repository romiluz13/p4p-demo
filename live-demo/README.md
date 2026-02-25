# cc-p4p Live Demo — Monday.com Scenario

## The Scenario in One Sentence

It's Monday morning. You're the PM for monday.com's Work OS platform. You wake up to a Slack from the VP of Sales: "ClickUp just announced free Gantt charts and timeline views for ALL plans. Enterprise prospects are asking what monday.com's answer is. I need talking points by noon."

---

## Why This Crisis Works for the Demo

This is a real thing that happens to real PMs at real B2B SaaS companies. It demands action across every workflow:

- **RESEARCH** — What exactly did ClickUp ship? What are enterprise customers saying about Gantt features? What does monday.com have vs. what's missing?
- **SPEC** — A concrete response: Advanced Timeline View with dependency management for enterprise plans
- **ROADMAP** — Where does this land against the Q3 backlog? What RICE score does it get? What gets bumped?
- **COMMUNICATE** — A message to the Sales team today, and an executive brief
- **METRICS** — How do we know if our response actually worked?

Every workflow has an authentic reason to exist. Nothing is manufactured.

---

## Demo Timeline

| Beat | Name | Duration | Workflow | Key Moment |
|------|------|----------|----------|------------|
| 1 | The Crisis | 45s | Setup | Read the Slack message. Set stakes. $900M ARR company. |
| 2 | First Prompt | 30s | None yet | Presenter types in plain English. No slash commands. |
| 3 | Router Detects | 60s | RESEARCH routes | **MOMENT A** — "I didn't tell it what to do." |
| 4 | Research Runs | 90s | RESEARCH executes | Competitive synthesis + user signals appear. |
| 5 | Memory Reveal | 60s | Memory inspection | **MOMENT B** — Open insights.md live. Spec quotes it. |
| 6 | Spec Prompt | 45s | SPEC triggered | Types natural-language follow-up. PRD writes itself. |
| 7 | Full PRD | 60s | SPEC output | Scroll through PRD. User stories by persona name. |
| 8 | Router Contract | 45s | Contract reveal | **MOMENT C** — Read YAML block aloud. |
| 9 | Roadmap Prompt | 45s | ROADMAP triggered | RICE scores, trade-offs, dependencies. |
| 10 | Task Visibility | 30s | Tasks panel | **MOMENT D** — `tasks` mid-session. |
| 11 | Communicate | 60s | COMMUNICATE | Sales team message + exec brief. Two outputs. |
| 12 | Session Close | 45s | Continuity | **MOMENT E** — Close terminal, reopen, "where were we?" |

**Total: ~9 minutes.** Budget 12 minutes for Q&A buffer in a 15-minute slot.

---

## The Five Showcase Moments

### Moment A: Router Detects Intent (Beat 3)
Presenter types the Slack crisis in plain natural language. The audience watches the router classify the intent as RESEARCH and route there. No menus. No slash commands. No prompt engineering.

**Presenter says:** "I didn't tell it what to do. It read the intent."

### Moment B: Memory Reveal (Beat 5)
After RESEARCH completes, presenter opens `.claude/cc-p4p/insights.md` in the terminal. Structured findings are there — competitive intel, user signals, evidence grades. Then SPEC is triggered. The PRD quotes those exact findings.

**Presenter says:** "It read the memory. I didn't paste anything."

### Moment C: Router Contract (Beat 8)
Presenter scrolls to bottom of ROADMAP output. Finds the YAML block. Reads `STATUS: ROADMAP_UPDATED` and `CONFIDENCE: 87` aloud.

**Presenter says:** "This isn't for you. This is the system talking to itself. The next workflow reads this to know if it can proceed."

### Moment D: Task Visibility (Beat 10)
During a running workflow, presenter types `tasks` in a side panel. Shows the in-progress task with status.

**Presenter says:** "You can always see what the system is doing."

### Moment E: Session Continuity (Beat 12)
Presenter closes terminal. Reopens. Types: "What's the status of the Timeline feature?" Memory reloads. Session continues from where it left off.

**Presenter says:** "The conversation compacted. The memory didn't."

---

## What to Say While Workflows Run

Each workflow takes 1-3 minutes. Don't fill the silence with filler — narrate what's happening architecturally.

### While RESEARCH runs (Beat 3-4, ~90 seconds)
> "Right now the router handed this to a research-synthesizer agent. It's reading the CLAUDE.md context — our personas, the competitive situation, Q3 OKRs. Then it's cross-referencing that against live web searches on ClickUp's pricing page. When it's done, it won't give me a summary. It will write structured findings to a memory file that the next workflow can read."

### While SPEC runs (Beat 6-7, ~60 seconds)
> "Watch what it does first. Before writing a single requirement, it reads insights.md — the memory the research workflow just wrote. The PRD you're about to see will quote specific customer verbatims and competitive findings from that file. Not because I pasted them in. Because that's how the system is wired."

### While ROADMAP runs (Beat 9, ~45 seconds)
> "It's scoring five competing items right now using RICE — Reach, Impact, Confidence, Effort. Every score will have explicit assumption flags. If the number is invented, it'll say so. That's what makes this PM-grade and not just AI-generated."

### While COMMUNICATE runs (Beat 11, ~60 seconds)
> "Two audiences, two outputs. The sales talking points need to be scannable — reps are reading them between calls. The customer message needs to be peer-to-peer, not corporate. It read the roadmap state before writing either one so it won't promise a date that isn't confirmed."

---

## Setup Instructions

### Before the Demo (Morning Of)

1. **Navigate to the live-demo directory:**
   ```
   cd /path/to/p4p-demo/live-demo
   ```

2. **Copy CLAUDE.md one level up** so Claude Code auto-loads it on launch:
   ```
   cp CLAUDE.md ../CLAUDE.md
   ```
   *(Or run `claude` from inside `live-demo/` — CLAUDE.md auto-loads from the working directory.)*

3. **Initialize clean memory state:**
   ```
   mkdir -p .claude/cc-p4p
   ```
   Memory files are created fresh by the workflows. Start clean every demo.

4. **Open Claude Code:**
   ```
   claude
   ```

5. **Verify context loads** by typing:
   ```
   What do you know about our product?
   ```
   Should describe monday.com, OKRs, the crisis — without pasting anything. If it does, you're ready.

6. **Set up two terminal panels:**
   - Panel A: Claude Code (full screen, 22pt minimum)
   - Panel B: Pre-queued with `cat .claude/cc-p4p/insights.md` ready to run at Beat 5

7. **Pre-load prompts** from `prompts/` in a separate editor tab for copy-paste.

### Font and Display
- Terminal font size: 22pt minimum for projection
- Dark theme: Dracula, One Dark, or Tokyo Night
- Maximize terminal — no desktop visible

### Fallback Plan

| What Breaks | Recovery |
|-------------|----------|
| Router picks wrong workflow | Rephrase naturally — "I need competitive research on..." |
| Workflow hangs | Show `tasks` panel — "this shows what's running" |
| Memory file empty | Navigate to `../example-session/` — "here's what a full session produces" |
| Full Claude Code failure | Open `example-session/` files — walk through them as live artifacts |

**Rule:** Never apologize. Never say "this usually works." Pivot immediately to: "Let me show you what the output looks like when it runs."

---

## File Map

```
live-demo/
├── README.md                    ← This file
├── CLAUDE.md                    ← The brain — monday.com context + agent rules
│
├── context/
│   ├── product-context.md      ← Vision, OKRs, metrics, Q3 backlog
│   ├── personas.md             ← Three named enterprise personas
│   └── tone-and-voice.md       ← monday.com PM writing style
│
└── prompts/
    ├── 01-research.md          ← Triggers RESEARCH workflow
    ├── 02-spec.md              ← Triggers SPEC workflow
    ├── 03-roadmap.md           ← Triggers ROADMAP workflow
    ├── 04-communicate.md       ← Triggers COMMUNICATE workflow
    └── 05-metrics.md           ← Optional METRICS workflow
```

---

## What to Emphasize

**Not:** "AI writes specs for PMs."

**Yes:** "cc-p4p is a PM-grade orchestration system. It detects workflow intent from natural language. It passes structured memory between workflows. It uses a machine-readable contract to validate handoffs. The PM gets leverage on the hard parts — research synthesis, prioritization trade-offs, stakeholder communication — not just text generation."

---

## The One-Liner

"A Monday morning competitive crisis, handled in 9 minutes. Not because AI is fast — because the system knows which workflow to run and what the previous one already figured out."
