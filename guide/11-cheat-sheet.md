# PM Cheat Sheet — Claude Code Quick Reference
### February 2026 · Print this. Keep it open.

---

## CLI Essentials

```bash
claude                          # Start interactive session
claude --dangerouslySkipPermissions=false  # Plan Mode (read-only, safe)
claude "write a PRD for X"      # One-shot command
claude --model claude-opus-4-5  # Use specific model
/clear                          # Clear context, keep CLAUDE.md
/compact                        # Compress history, keep context
/status                         # See context usage
/exit                           # End session
```

---

## Official PM Slash Commands
*(after `/plugin install product-management`)*

```bash
/pm:prd-create          # Full PRD from a brief
/pm:feature-spec        # Detailed feature specification
/pm:user-story          # User stories with acceptance criteria
/pm:metrics-framework   # Define success metrics and measurement
/pm:competitive         # Structured competitive analysis
/pm:roadmap-review      # Review roadmap for gaps and priorities
```

---

## CLAUDE.md — Level 3 Structure

```markdown
## Context Loading Sequence
Before ANY task:
1. Read context/product-context.md
2. Read context/personas.md
3. Search notes/ for relevant background
4. Read context/tone-and-voice.md
5. Check context/examples/

## Agent Roles
When I invoke /research: [rigorous researcher, cite sources]
When I invoke /spec:     [senior PM, use template, grounded in JTBD]
When I invoke /data:     [analyst, lead with decision not finding]
When I invoke /devil:    [skeptical VP, exactly 3 objections]
When I invoke /review:   [peer reviewer, does spec solve actual pain?]
When I invoke /decision: [log to decisions/YYYY-MM-DD-topic.md]

## Response Format Rules
NEVER start with "Certainly!" or "Great!"
NEVER use passive voice in specs
NEVER write boilerplate user stories

## Anti-Patterns
[Things Claude should never do for YOUR product]

## Session Scope Rule
If we've covered 3+ topics: flag it and suggest new session.
```

---

## Folder Structure

```
/my-product/
├── CLAUDE.md                  ← The brain (loads every session)
├── context/
│   ├── product-context.md     ← Vision, OKRs, current focus
│   ├── personas.md            ← Users, JTBDs, pain points
│   ├── tone-and-voice.md      ← Writing style rules
│   └── prd-template.md        ← Your PRD structure
├── notes/                     ← Working memory (current sessions)
├── research/
│   ├── competitive/           ← Competitor research outputs
│   └── user-pain/             ← Pain research, JTBDs
├── specs/                     ← Delivered PRDs and specs
└── decisions/                 ← Decision log (one file per decision)
```

---

## Top MCPs for PMs

| MCP | Install | Best For |
|-----|---------|----------|
| Jira | `/mcp add jira` | Backlog queries, sprint analysis |
| Linear | `/mcp add linear` | Same (cleaner API) |
| Notion | `/plugin marketplace add danhilse/notion_mcp` | Docs queries (use CSV export for large DBs) |
| Figma (free) | `npx figma-developer-mcp` | Design → acceptance criteria |
| Figma (any plan) | `arinspunk/claude-talk-to-figma-mcp` | Free community option |
| Amplitude | Built-in official | Metrics queries |
| Google Analytics | `community/ga-mcp` | Traffic & behavior |
| Bright Data | `/mcp add brightdata` | Scrape competitors, forums, reviews |
| PowerPoint | `tfriedel/claude-office-skills` | Generate .pptx from CLI |
| NotebookLM | `jacob-bd/notebooklm-mcp-cli` | Connect research notebooks |

**Figma free/paid truth:**
- Remote server → FREE (all plans) · Desktop server → Paid (Dev seat ~$12/mo)

**PowerPoint truth:**
- Native PPTX = Claude Desktop only · CLI fix = `tfriedel/claude-office-skills`

---

## The 5 Compound Workflows

```
1. SPRINT HEALTH CHECK
   Jira (committed) + Amplitude (used) + specs (planned)
   → "Did we build what we said? Is it being used?"

2. COMPETITIVE PIPELINE
   Bright Data (scrape 3 competitors in parallel)
   → research/competitive/synthesis.md

3. USER RESEARCH SYNTHESIS
   Support tickets + interview transcripts + app reviews
   → JTBD extraction → PRD input

4. ROADMAP VALIDATION
   Jira (roadmap) + Amplitude (usage) + customer feedback
   → gaps: what we're building vs what customers want

5. RELEASE IMPACT
   Git (what shipped) + Amplitude (before/after)
   → did the release move the metrics?
```

---

## Agent Role Quick Reference

| Command | Persona | Output |
|---------|---------|--------|
| `/research` | Rigorous researcher | Sourced findings with uncertainty flags |
| `/spec` | Senior PM who shipped this | PRD using your template |
| `/data` | PM-trained analyst | Decision (not just finding) |
| `/devil` | Skeptical VP | Exactly 3 objections + mitigations |
| `/review` | Peer reviewer | Pass/revise + 3 specific improvements |
| `/decision` | Decision logger | Structured log → decisions/ folder |

---

## The PRD Pipeline (4 Steps)

```bash
# Step 1 — Research (Researcher agent)
/research
"Pull last 30 days of Jira tickets tagged [feature].
Synthesize top 3 pain patterns with evidence.
Save to research/user-pain/[feature]-research.md"

# Step 2 — JTBD (Analyst agent)
/data
"Read research/user-pain/[feature]-research.md
Extract the core Job to be Done statement.
Save to research/user-pain/[feature]-jtbd.md"

# Step 3 — Draft PRD (Spec Writer agent)
/spec
"Read research/user-pain/[feature]-jtbd.md + context/prd-template.md
Write PRD for [feature].
Save to specs/[feature]-prd.md"

# Step 4 — Review (Reviewer agent)
/review
"Read specs/[feature]-prd.md AND research/user-pain/[feature]-research.md
Does the spec actually solve the top 3 pains from research?
Verdict + 3 improvements if needed."
```

---

## Context Hygiene (3 Rules)

```
Rule 1: Load at start, not mid-session.
        If you find yourself re-explaining at message 30 → start new session.

Rule 2: One task per session, not one topic.
        Bad:  "Q3 planning" (a topic)
        Good: "Write discovery brief for Initiative A" (a task)

Rule 3: Persist decisions, not reasoning.
        After a session: extract decisions → decisions/ folder.
        Never save the whole conversation.
```

---

## PM Skills Libraries

```bash
# Official Anthropic (7,500★ — 6 slash commands)
/plugin install product-management

# Lenny's Podcast (86 skills)
/plugin marketplace add refoundai/lenny-skills

# Product Manager Skills (42 skills)
/plugin marketplace add deanpeters/Product-Manager-Skills

# Curated frameworks (Mom Test, Shape Up, etc.)
/plugin marketplace add assimovt/productskills
```

---

## Plan Mode (Safe Codebase Exploration)

```bash
# Start Plan Mode — reads but never writes
claude --dangerouslySkipPermissions=false

# What to ask in Plan Mode:
"How does the checkout flow work?"
"What would break if we removed feature X?"
"Where is the authentication system implemented?"
"What does this error state mean technically?"
"How many places in the codebase touch the payment provider?"
```

---

## Key URLs

| Resource | URL |
|----------|-----|
| Official PM Plugin | github.com/anthropics/knowledge-work-plugins |
| PM Course | ccforpms.com · github.com/carlvellotti/claude-code-pm-course |
| PM Skills World | prodmgmt.world/claude-code |
| Lenny Skills | github.com/refoundai/lenny-skills |
| pm-studio patterns | github.com/zoekdestep/pm-studio |
| product-org-os | github.com/yohayetsion/product-org-os |
| Figma MCP Guide | help.figma.com → search "MCP server" |
| PowerPoint skills | github.com/tfriedel/claude-office-skills |
| Anthropic Docs | code.claude.com/docs/en/best-practices |
| Free PM Course | anthropic.skilljar.com/claude-code-in-action |
| cc10x (dev orchestration) | github.com/romiluz13/cc10x |

---

## Common Mistakes (Don't Do These)

```
✗ CLAUDE.md too long (>400 lines)
  → Sweet spot: 100-150 lines of YOUR product's specific context

✗ Re-explaining context mid-session
  → Start a new session. CLAUDE.md reloads fresh.

✗ One long session for multiple topics
  → One task per session. One output file per session.

✗ MCPs connected but used in isolation
  → Combine MCPs in one prompt for compound intelligence

✗ Pasting old conversation as context
  → Save decisions to decisions/ folder. Load that next session.

✗ Never committing CLAUDE.md to Git
  → Team CLAUDE.md belongs in the repo. Everyone inherits it.

✗ Starting with the hardest thing
  → Week 1 = just CLAUDE.md. That's it.
```

---

## WINNING Feature Scoring (from community PM skill)

```
Score = Pain × Timing × Execution Capability

Pain:       How much does this hurt the user?     (1-10)
Timing:     Is there a window where this lands?   (1-10)
Execution:  Can our team actually build this well? (1-10)

Prompt:
"Score these features using WINNING framework: [list features].
For each: Pain score + rationale, Timing score + rationale,
Execution score + rationale. Sort by total score descending."
```

---

*Part of the Claude Code practical guide series.*
*Developer guide: `../practical/` · PM presentation: `../product-meetup/`*
