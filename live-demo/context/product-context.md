# monday.com — Product Context

## What monday.com Is

monday.com is a publicly traded Work OS platform (MNDY, NASDAQ). It helps teams plan, execute, and track work across every function — from software development to marketing, HR, construction, and operations. The core platform is highly flexible: customers build their own workflows using Boards, Automations, Dashboards, WorkForms, and WorkDocs.

**Key numbers:**
- 225,000+ paying customers globally
- ~$900M ARR, ~25% YoY growth
- $18B+ market cap at IPO; public company with quarterly earnings pressure
- Segments: SMB (under 50 seats), Mid-market (50-200 seats), Enterprise (200+ seats)
- Enterprise is the primary growth driver — higher ACV, lower churn, expansion via add-ons

**Primary value proposition:** One platform for all work. Replace the spreadsheet-and-email chaos that every team still uses with a structured, automatable, collaborative system.

---

## Core Product Surface Area

### Boards
The foundation of monday.com. Customizable, multi-view grids where teams track work items. Every board has columns (status, owner, date, etc.), multiple views (table, kanban, calendar, map), and can trigger automations.

### Timeline View
Gantt-like view built into Board. Shows items on a horizontal timeline. Currently available on Standard plan and above. Does NOT support dependency management (arrows between tasks) — this is the critical gap in the competitive crisis.

### Automations
No-code automation builder. 200M+ automated actions per month across the platform. Triggers, conditions, and actions across boards, integrations, and notifications.

### Dashboards
Cross-board reporting. Shows data from multiple boards in one view. Used heavily by enterprise PMs and VPs for status reporting.

### WorkDocs
Collaborative documents embedded inside monday.com. Can reference board data inline. Used for project briefs, meeting notes, status updates.

### WorkForms
Intake forms that automatically create board items. Used by ops, IT, and HR teams for request management.

### monday apps marketplace
600+ integrations. Key enterprise integrations: Salesforce, Jira, GitHub, Slack, Zoom, Microsoft Teams, SAP, ServiceNow.

---

## Current OKRs (Q3 2026)

### O1: Win and retain enterprise accounts
- **KR1:** Enterprise tier net revenue retention (NRR) from 118% to 125%
- **KR2:** Enterprise win rate against ClickUp from 54% to 65% in competitive deals
- **KR3:** Enterprise churn rate from 8.4% annual to under 7%

### O2: Increase Work OS stickiness through advanced project features
- **KR1:** Timeline View weekly active users in enterprise from 34% to 52% of seats
- **KR2:** Automations per enterprise account per month from 840 to 1,200
- **KR3:** Average features used per enterprise account from 3.2 to 4.5

### O3: Accelerate expansion revenue from existing accounts
- **KR1:** Expansion ARR contribution from 38% to 45% of new ARR
- **KR2:** Accounts with 3+ integrated apps from 41% to 55%

---

## Current Key Metrics

| Metric | Current | Target | Notes |
|--------|---------|--------|-------|
| Enterprise Timeline View WAU | 34% | 52% | Q3 target |
| Enterprise win rate (vs ClickUp) | 54% | 65% | Drops when Gantt comes up |
| Enterprise annual churn | 8.4% | <7% | |
| NPS (enterprise tier) | 42 | 52 | Industry median B2B SaaS: 45 |
| Time-to-first-value (new enterprise) | 6.2 days | <4 days | Onboarding friction high |
| Accounts using 3+ apps | 41% | 55% | Q3 target |
| Automations per enterprise acct/mo | 840 | 1,200 | |

---

## Competitive Landscape

### ClickUp (Primary Competitive Threat)
- Fastest-growing work management competitor
- Just announced: **Gantt charts and Timeline views now FREE for ALL plans** (including free tier)
- ClickUp's Gantt: supports dependencies, critical path, baseline tracking
- ClickUp's positioning: "We give you more, for less money"
- Weakness: ClickUp is overbuilt — users describe it as "too complex to set up." Enterprise migration from monday.com to ClickUp has high friction.
- Competitive vulnerability: Our Timeline view has NO dependency support. This is the #1 objection in enterprise deals when ClickUp is in the mix.

### Asana (Strong in Enterprise)
- Strong enterprise brand, especially for marketing and ops teams
- Recently upgraded Timeline with dependencies, milestones, and portfolio-level views
- Weakness: Less flexible than monday.com for cross-functional use; doesn't handle non-project work as well
- Their differentiator in competitive deals: "Project management done right"

### Notion (Emerging Threat in Mid-Market)
- Primarily a document and knowledge management tool but adding database+timeline features
- Not a direct enterprise PM tool yet, but stealing mid-market budget with all-in-one positioning
- Weakness: No dedicated project management features at enterprise scale

### Jira (Entrenched Enterprise)
- Dominant in software development teams; losing ground in non-engineering use cases
- Recently launched Jira Work Management to compete outside dev
- Weakness: Seen as "too technical" by business teams; monday.com beats Jira in ease-of-use consistently

---

## Q3 Feature Backlog (What's Competing for Resources)

The items competing for engineering capacity in Q3:

1. **Advanced Timeline View** (dependency management, critical path) — the feature at the center of the ClickUp crisis
2. **AI-generated Board Templates** — smart onboarding that generates a custom board from a prompt
3. **Enterprise Audit Logs** — required by multiple enterprise security teams; compliance blocker for 3 deals
4. **WorkDocs AI summaries** — AI-powered meeting note and status summary in WorkDocs
5. **Cross-board Dependency Manager** — connect items across different boards with dependency tracking
6. **Improved Time Tracking integration** (Harvest, Clockify) — requested by professional services customers
7. **Custom Branding for enterprise** — white-label dashboards; requested by 8 accounts

---

## Product Decisions Already Made

- **monday.com will NOT enter the "project management only" positioning.** The Work OS brand is the moat. We solve work across all functions, not just projects.
- **Enterprise tier is the growth engine.** Feature prioritization defaults to enterprise value unless SMB evidence is overwhelming.
- **AI features are additive, not core UX.** AI assists (suggests, generates, summarizes). It does not replace the board metaphor.
- **Mobile is not a priority in Q3.** Mobile DAU is 18% — not the leverage point for enterprise retention.

---

## Open Questions

- How much of the ClickUp Gantt announcement is real feature parity vs. marketing? (Need competitive deep-dive)
- Are enterprise accounts actually evaluating ClickUp seriously, or using it as leverage in renewal negotiations?
- Can we ship dependency support in Timeline View without rebuilding the underlying data model?
- If we ship Advanced Timeline in Q3, what gets cut — AI Templates or Audit Logs?
- Does the Enterprise Audit Logs blocker represent more churn risk than the Gantt gap?
