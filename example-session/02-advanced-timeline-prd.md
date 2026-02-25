# Advanced Timeline View — Product Requirements Document

## Status
Draft — Ready for Engineering Review (2026-02-23)

---

## Problem Statement

monday.com's Timeline View has zero dependency support. Enterprise customers managing multi-team, multi-workstream projects cannot sequence work, visualize blockers, or determine which path is time-critical — they either abandon Timeline entirely or maintain manual dependency workarounds in parallel tools. 34 NPS verbatims from Q4 2025 name this gap explicitly. The absence of dependency management is directly costing deals: win rate drops 10 percentage points when ClickUp Gantt enters the conversation in enterprise evaluations, and we have at least three named accounts with active renewal risk tied to this capability gap.

---

## Goals

1. **Restore competitive parity with Asana and close the ClickUp gap in enterprise deals** — equip AEs with a dependency-capable Timeline offering that neutralizes "ClickUp has a free Gantt" in competitive scenarios, targeting recovery of the observed 10pp win rate drop.
2. **Eliminate manual dependency workarounds for enterprise users** — measurable by a reduction in support tickets and community posts citing dependency management, and by direct customer confirmation from named accounts (Meridian, Constellation, Northgate).
3. **Increase Timeline View feature retention** — accounts on Advanced Timeline churn at 4.1% annually vs. 8.4% average; expanding dependency capability should deepen Timeline adoption and maintain or improve this retention anchor.
4. **Deliver a concrete offering for Meridian Financial's 8-week renewal window** — provide a credible beta or committed roadmap milestone that satisfies Marcus Chen's infrastructure workstream sequencing requirement before the renewal decision date.
5. **Establish monday.com as the enterprise-grade dependency management platform** — differentiate from ClickUp (F2S only, no cross-board, no bulk editing) by shipping a more complete dependency model that signals depth over marketing breadth.

---

## Non-Goals

1. **Portfolio-level Gantt across boards** — cross-board dependency management is an open question for this release (see Open Decisions below). Even if in-scope for beta, a full portfolio Gantt view is out of scope for this spec cycle. Reason: architectural scope and dedicated roadmap slot required.
2. **Resource leveling and capacity planning** — dependency visualization is not the same as resource conflict detection. Automatic schedule adjustment based on capacity is out of scope for v1. Reason: adds significant complexity and is a distinct product surface.
3. **Native Jira/GitHub dependency sync** — importing or syncing dependencies from third-party tools is out of scope. Reason: integration platform work tracked separately; not blocking core value delivery.
4. **Dependency management on mobile** — Advanced Timeline View is a desktop-first surface. Mobile dependency editing is out of scope for this release. Reason: mobile Timeline is a separate initiative; blocking on mobile parity would delay enterprise delivery.
5. **Public API for dependency data** — programmatic access to dependency relationships is not required for GA. Reason: API roadmap is tracked separately; internal users and UI-driven workflows are the target for v1.

---

## Open Decisions (BLOCKING — Do Not Assume)

These three questions are unresolved as of 2026-02-23 and MUST be answered before engineering finalizes scope for the 8-week beta. They are flagged here to prevent the spec from being used to make commitments before resolution.

### OD-1: Dependency Type Coverage
**Question:** Does the Advanced Timeline beta include all four dependency types (Finish-to-Start, Start-to-Start, Finish-to-Finish, Start-to-Finish) or only Finish-to-Start (F2S)?

**Why it matters:** ClickUp only supports F2S; shipping all four types is the key differentiator from ClickUp and parity signal vs. Asana. Meridian Financial's infrastructure workstream requirements may require S2S or F2F. If beta ships F2S only, customer-facing communications, competitive battlecards, and Meridian commitments must be scoped accordingly.

**Owner:** Engineering Lead

**Blocking for:** Meridian talking points, competitive battlecard, beta scope definition, P0 requirement AC below.

**Resolution needed by:** Before beta kick-off.

---

### OD-2: Cross-Board Dependency Scope
**Question:** Is cross-board dependency management in scope for the 8-week beta?

**Why it matters:** Meridian Financial's stated need is cross-board dependency management for infrastructure workstreams spanning multiple boards. If cross-board dep is not in beta scope, Meridian cannot be offered beta access as a solution to their renewal concern. This also affects the Non-Goal boundary above — if cross-board is in scope, Non-Goal #1 must be revised.

**Owner:** Engineering Lead

**Blocking for:** Meridian beta offer decision, VP Sales alignment, scope of P0 requirements.

**Resolution needed by:** This week — VP Sales + Product alignment required (per activeContext.md).

---

### OD-3: Meridian Beta Access Offer
**Question:** Can Meridian Financial be formally offered beta access to Advanced Timeline View?

**Why it matters:** Meridian (Marcus Chen, IT Director, 340 seats) has an 8-week renewal window. A formal beta access commitment is the win condition for this account. Resolution requires alignment between VP Sales and Product on (a) beta readiness timeline, (b) whether cross-board dep is in beta (OD-2), and (c) contractual or SLA implications of beta access.

**Owner:** VP Sales + Product (joint)

**Blocking for:** Account retention action, renewal talking points.

**Resolution needed by:** This week.

---

## User Stories

### Primary: Enterprise Project Manager (Dependency Sequencing)
- As a **Senior PM managing a multi-team delivery** (e.g., Sarah Kim, Constellation Brands), I want to create finish-to-start dependency links between items on the Timeline so that downstream tasks automatically reflect their true start constraint and I stop maintaining a separate dependency spreadsheet.
- As a **Senior PM**, I want the critical path highlighted visually on the Timeline so that I can immediately identify which tasks, if delayed, will push the project end date — without having to calculate this manually.
- As a **Senior PM with 50+ interconnected tasks**, I want to select multiple items and bulk-assign or bulk-remove dependencies so that restructuring a project phase does not take an hour of one-by-one editing.

### Secondary: IT Director / Buyer (Cross-Team Infrastructure)
- As an **IT Director managing infrastructure workstreams across multiple boards** (e.g., Marcus Chen, Meridian Financial), I want to define dependencies between items on different boards so that infrastructure sequencing is visible in a single view without stitching together exports.
  - NOTE: This story is conditional on OD-2 resolution. If cross-board dep is out of scope for beta, this story moves to P2 for the initial release.

### Secondary: VP / Executive Sponsor (Visibility and Credibility)
- As a **VP of Strategic Programs** (e.g., Rebecca Okafor, Northgate Healthcare), I want to see a clear critical path visualization on the Timeline so that I can present a credible project schedule to executive leadership without building a separate slide deck.
- As a **VP / Executive Sponsor**, I want project health signals (e.g., items where predecessor is overdue) surfaced directly on the Timeline so that I get early warning of at-risk delivery without relying on manual status updates from PMs.

### Error and Edge Cases
- As any Timeline user, when I attempt to create a circular dependency (A depends on B, B depends on A), I want to receive an immediate, clear error message that prevents the circular link from being saved.
- As any Timeline user, when I delete an item that has active dependencies, I want to be prompted to confirm the deletion and informed which downstream items will be affected.
- As any Timeline user, if a predecessor item's end date is moved past a successor's start date, I want a visual conflict indicator on the successor item so I can resolve the conflict explicitly.

---

## Requirements

### P0 — Must Have (Beta does not ship without these)

**P0-1: Finish-to-Start Dependency Creation**
Users can draw a dependency link from the end of one item to the start of another item on the Timeline. Links are persisted and visible on reload.
- Acceptance:
  - Given two items on the Timeline, when I drag from the right edge of Item A to the left edge of Item B, then a directional arrow is drawn from A to B and saved.
  - Given a saved F2S dependency, when I reload the board, then the dependency arrow is still visible.
  - Given a dependency would create a circular reference, when I attempt to draw it, then the system rejects the link and shows an explanatory error message.

**P0-2: Dependency Visualization (Arrows on Timeline)**
Saved dependency relationships are rendered as directional arrows on the Timeline canvas, clearly distinguishing predecessor from successor.
- Acceptance:
  - Given saved dependencies exist, when I open the Timeline View, then each dependency is rendered as a directed arrow between the two items.
  - Given 10+ dependencies on a single item, when I view the Timeline, then arrows do not obscure item labels (collision handling / render priority required).

**P0-3: Critical Path Visualization**
The longest dependency chain from project start to project end is calculated and highlighted visually on the Timeline (e.g., colored differently from non-critical items and links).
- Acceptance:
  - Given a project with multiple dependency chains, when I enable "Show Critical Path" (toggle or default-on), then items and links on the critical path are highlighted in a distinct color or style.
  - Given a critical-path item is rescheduled to end later, then the critical path recalculates and the highlight updates automatically.
  - NOTE: Calculation method (CPM algorithm) is an engineering decision. PM requirement is correct identification of the longest path.

**P0-4: Dependency Conflict Indicator**
When a predecessor item's end date is after a successor item's start date (i.e., the dependency is violated), a visual conflict indicator is shown on the successor item.
- Acceptance:
  - Given Item A ends on Day 10 and Item B (successor) starts on Day 8, then Item B shows a conflict badge or color indicator.
  - Given the conflict is resolved (A moved to end before Day 8, or B moved to start after Day 10), then the conflict indicator clears.

**P0-5: Dependency Deletion**
Users can remove a dependency link via the Timeline UI.
- Acceptance:
  - Given a dependency arrow, when I click it and select "Delete dependency" (or equivalent gesture), then the link is removed and the change is persisted.
  - Given deleting an item with outbound or inbound dependencies, when I confirm deletion, then associated dependency links are also removed and no orphan references remain.

**P0-6: Enterprise-Tier Gate**
Advanced Timeline dependency features (P0-1 through P0-5) are available only to Enterprise plan accounts.
- Acceptance:
  - Given a non-Enterprise account, when a user opens Timeline View, then dependency creation controls are not visible (or are visible with an upgrade prompt — design decision).
  - Given an Enterprise account, when a user opens Timeline View, then all P0 dependency controls are accessible without additional configuration.

---

### P1 — Nice to Have (Significantly improves experience; ship in GA if capacity allows)

**P1-1: Additional Dependency Types (S2S, F2F, S2F)**
Support for Start-to-Start, Finish-to-Finish, and Start-to-Finish dependency relationships in addition to F2S.
- NOTE: This requirement is CONTINGENT on OD-1 resolution. If engineering confirms all 4 types are in beta scope, this becomes P0. If F2S only for beta, this is P1 targeted for GA.
- Value: Differentiates from ClickUp (F2S only) and achieves parity with Asana.

**P1-2: Bulk Dependency Editing**
Users can select multiple items and batch-create or batch-remove dependency relationships.
- Acceptance:
  - Given multiple items selected, when I open a context menu or toolbar action, then I can set "all selected items depend on [item X]" or "remove all dependencies between selected items."
- Value: Directly closes ClickUp structural gap (ClickUp has no bulk editing). Addresses Constellation Brands workaround-fatigue use case.

**P1-3: Cross-Board Dependency Management**
Dependency links can span items on different boards, visible in a unified Timeline or via a cross-board dependency indicator.
- NOTE: This requirement is CONTINGENT on OD-2 resolution. If cross-board is confirmed in beta scope, this may be elevated to P0 for the beta milestone (specifically to unblock Meridian Financial).
- Value: Directly addresses Meridian Financial's infrastructure workstream requirement (OD-3).

**P1-4: Dependency Panel / Sidebar**
A panel listing all dependencies for a selected item (predecessor list + successor list) with direct navigation to linked items.
- Value: Reduces context-switching for PMs managing complex dependency graphs. Supports the "50+ interconnected tasks" user story above.

**P1-5: Predecessor Date Violation Notification**
When a predecessor item's date is moved such that it violates a successor's start date, notify the item owner via monday.com's notification system (not just a visual indicator).
- Value: Proactive conflict detection without requiring users to stay on the Timeline View.

---

### P2 — Future Considerations (Out of scope for v1; architecture must not block)

**P2-1: Auto-Schedule / Date Propagation**
When a predecessor item is delayed, automatically shift the successor item's dates forward by the same amount.
- Why noting now: Architecture for dependency storage must support date-aware recalculation to enable this later without a rewrite.

**P2-2: Portfolio Gantt (Cross-Board Timeline)**
A dedicated cross-board, cross-project Timeline view showing all dependencies in a portfolio-level Gantt.
- Why noting now: This is a natural evolution of cross-board dependency (P1-3) and a clear enterprise ask. Requires its own spec and capacity allocation.

**P2-3: Public API for Dependency Data**
Programmatic read/write access to dependency relationships via monday.com's API.
- Why noting now: Enterprise customers with custom integrations will ask for this. API schema design should account for dependency objects from day one.

**P2-4: Dependency Templates**
Reusable dependency patterns that can be applied to new projects (e.g., "standard software release sequence").
- Why noting now: Reduces setup time for recurring project types. Design should avoid hardcoding dependency logic in ways that prevent templating.

**P2-5: Mobile Dependency Viewing (Read-Only)**
View (but not edit) dependency arrows on mobile Timeline.
- Why noting now: Edit is excluded from v1, but read-only mobile access is a reasonable fast-follow once mobile Timeline rendering is validated.

---

## Success Metrics

| Metric | Type | Baseline | Target | Measurement | Evaluate At |
|--------|------|----------|--------|-------------|-------------|
| Timeline View adoption (Enterprise accounts) | Leading | Current Enterprise Timeline adoption rate (establish at beta launch) | +15pp within 90 days of GA | % of Enterprise accounts with at least 1 active Timeline board | 30, 60, 90 days post-GA |
| Dependency feature activation rate | Leading | 0% (feature does not exist today) | 40% of Enterprise Timeline users create at least 1 dependency within 30 days of GA | % of Enterprise Timeline users who have created >= 1 dependency link | 30 days post-GA |
| Critical path feature usage | Leading | 0% (feature does not exist today) | 25% of dependency-active boards have critical path enabled | % of boards with >= 1 dependency where critical path is toggled on | 60 days post-GA |
| Enterprise win rate (Timeline-competitive deals) | Lagging | 10pp lower when ClickUp Gantt enters deal | Recover to parity (close the 10pp gap) within 2 quarters of GA | CRM win/loss data filtered by competitive tag "ClickUp Gantt" | 2 quarters post-GA |
| Annual churn rate — Advanced Timeline accounts | Lagging | 4.1% (existing Timeline users) | Maintain <= 4.1% for Advanced Timeline accounts; target 3.5% | Annual churn cohort analysis, Advanced Timeline user segment | 12 months post-GA |
| NPS verbatims citing dependency gap | Lagging | 34 verbatims in Q4 2025 | Zero verbatims citing "no dependency tracking" within 2 quarters of GA | Quarterly NPS open-text analysis | Q4 2026 NPS cycle |
| Named account renewals (Meridian, Constellation, Northgate) | Lagging | All 3 at renewal risk | All 3 renewed and citing Advanced Timeline as contributing factor | Account health / renewal notes in CRM | Per renewal date |

---

## Timeline Considerations

### Hard Deadline
- **Meridian Financial renewal: 8 weeks from 2026-02-23** — approximately 2026-04-20. This is not a feature deadline but an account retention forcing function. A credible offering (beta access or formal commitment) must be in place before the renewal conversation. Resolution of OD-2 and OD-3 this week is prerequisite.

### Development Schedule (Current State, per roadmap-state.md)
- Both squads currently assigned to Advanced Timeline View.
- Beta target: Week 8 (approximately 2026-04-20).
- GA target: Week 14 (approximately 2026-06-02).

### Beta Scope Principles
- P0 requirements (P0-1 through P0-6) constitute the minimum viable beta.
- P1-1 (additional dep types) and P1-3 (cross-board) move to P0 or remain P1 pending OD-1 and OD-2 resolution.
- Beta is Enterprise-only (P0-6). Named accounts (Meridian, Constellation Brands, Northgate) are priority beta candidates pending OD-3 resolution.

### Phasing Summary
| Phase | Scope | Target |
|-------|-------|--------|
| Beta (Week 8) | P0-1 through P0-6; P1-1 and P1-3 if OD-1/OD-2 resolved in scope | ~2026-04-20 |
| GA (Week 14) | Beta scope + P1-2 (bulk editing), P1-4 (dependency panel), P1-5 (notifications) | ~2026-06-02 |
| Fast Follow | Remaining P1s not in GA | Post-GA sprint cycle |
| Future | P2 items | Separate spec + roadmap slot |

---

## Open Questions (Full List)

- [ ] **OD-1: Dep type coverage (F2S only vs. all 4?)** — Owner: Engineering Lead — **BLOCKING** — Needed before beta scope lock and Meridian talking points
- [ ] **OD-2: Cross-board dependency in 8-week beta scope?** — Owner: Engineering Lead — **BLOCKING** — Needed before Meridian beta offer and P1-3 priority decision
- [ ] **OD-3: Can Meridian be formally offered beta access?** — Owner: VP Sales + Product — **BLOCKING** — Needed this week; renewal window is live
- [ ] **What is the exact count/methodology for "34 NPS verbatims"?** — Owner: Data/Research — Non-blocking — Needed for success metric baseline validation
- [ ] **What is current Enterprise Timeline adoption rate (baseline for success metric row 1)?** — Owner: Data/Analytics — Non-blocking — Needed before GA launch to set a valid baseline
- [ ] **Does the Enterprise tier gate (P0-6) apply to viewing-only or also to creation?** — Owner: Design + Product — Non-blocking — Affects upgrade prompt UX design decision
- [ ] **Circular dependency detection: handled at UI layer, API layer, or both?** — Owner: Engineering — Non-blocking — Affects acceptance criteria completeness for P0-1
- [ ] **Critical path algorithm: which CPM variant? Does it account for calendars/non-working days?** — Owner: Engineering — Non-blocking — Affects correctness of P0-3 implementation

---

## Competitive Context (Grounding)

This spec is grounded in the following evidence. Claims are not assumed.

| Claim | Source | Strength |
|-------|--------|----------|
| monday.com Timeline has zero dependency support | SoftwareAdvice third-party review; 34 Q4 2025 NPS verbatims | Strong |
| ClickUp Gantt is now free tier (Feb 2026) | ClickUp pricing page confirmed Feb 2026 | Strong |
| ClickUp supports F2S only, no cross-board, no bulk editing | Direct product analysis, ProjectManager.com competitive review | Strong |
| Asana supports all 4 dep types + portfolio Gantt | Direct product analysis | Strong |
| 10pp win rate drop when ClickUp Gantt appears in deals | Enterprise sales data (CRM win/loss analysis Q4 2025) | Strong |
| Timeline users churn at 4.1% vs. 8.4% average | Product analytics (retention cohort) | Strong |
| 34 enterprise NPS verbatims citing dependency tracking gap | Q4 2025 NPS survey open-text analysis | Strong |
| Meridian Financial: 340 seats, 8-week renewal, cross-board dep requirement | Account data — Marcus Chen, IT Director | Strong |
| Constellation Brands: 600 seats, manual workaround fatigue | Account data — Sarah Kim, Senior PM | Strong |
| Northgate Healthcare: 180 seats, exec credibility concern | Account data — Rebecca Okafor, VP Strategic Programs | Strong |

---

## Appendix: Dependency Type Definitions

| Type | Description | Example |
|------|-------------|---------|
| Finish-to-Start (F2S) | Task B cannot start until Task A finishes | Testing cannot start until Dev is done |
| Start-to-Start (S2S) | Task B cannot start until Task A starts | Documentation starts when Dev starts |
| Finish-to-Finish (F2F) | Task B cannot finish until Task A finishes | QA cannot finish until Bug Fixes finish |
| Start-to-Finish (S2F) | Task B cannot finish until Task A starts | Rare; legacy schedule compatibility |

ClickUp supports F2S only. Asana supports all four. This spec targets all four (P0 if OD-1 confirms, P1 if F2S only for beta per OD-1).
