# Advanced Timeline View — Full Metrics Framework
**Date:** 2026-02-23
**Feature:** Advanced Timeline View (dependency management, critical path, bulk editing)
**Beta:** Week of April 20 | **GA:** Early June

---

## The Problem with the Current Three Metrics

All three lagging metrics (Timeline WAU, win rate, churn) measure outcomes that won't appear for 6-12 months post-GA. This framework adds leading indicators (2-4 weeks), guardrails (what should not change), account-specific gates (Marcus, Sarah, Rebecca), and the leading signal that predicts churn reduction.

---

## 1. Leading Indicators — Weeks 1–4 Post-Launch

| # | Metric | Target | Measure At | Method |
|---|---|---|---|---|
| L1 | Dep activation rate — % of Enterprise Timeline accounts creating ≥1 dep within 14 days of GA | 30% of active Enterprise Timeline accounts | Day 7, Day 14 | `dependency_created` event, enterprise plan filter |
| L2 | Time to first dependency — median days from first Timeline open post-GA to first dep created | ≤5 days | Day 14 | Event timestamp delta |
| L3 | Multi-user dep adoption — % of dep-activated accounts where ≥2 distinct users have created or modified a dep | 50% of dep-activated accounts | Day 30 | Distinct `user_id` on dep events per account |
| L4 | Critical path enable rate — % of dep-active boards where critical path is on | 25% of dep-active boards | Day 30 | `critical_path_enabled` per board |
| L5 | Dep graph density — avg dep links per dep-active board at 30 days | ≥5 links per board | Day 30 | `dependency_count` per board |
| L6 | Conflict indicator trigger rate — % of dep-active accounts with ≥1 conflict indicator fired | >40% (higher = feature is doing real work) | Day 14, Day 30 | `conflict_indicator_shown` event |

### Diagnostic interpretation

| Signal | Reading | Diagnosis |
|--------|---------|-----------|
| L1 <15% by Day 14 | Activation failure | Dep creation UX not discoverable — check gesture visibility on existing boards |
| L2 >7 days | Friction | Drop-off in creation flow — watch for UX confusion at dep draw gesture |
| L3 <30% by Day 30 | Champion-only tool | Feature not team-embedded — will not drive retention |
| L4 <10% | Sequencing without visibility | Teams using deps for task ordering but not for exec visibility — renewal case weak |
| L5 <3 links | Exploration, not embedding | Monitor through Day 60 before acting |
| L6 <20% | Feature not in real use | Either teams not using deps for real projects, or conflict detection broken |

---

## 2. Lagging Indicators — Current Three + Two Additions

| # | Metric | Baseline | Target | Evaluate At |
|---|---|---|---|---|
| Lag1 | Timeline View WAU — Enterprise accounts | 34% | 52% | 90 days post-GA |
| Lag2 | Enterprise win rate vs. ClickUp | 54% | 65% | 2 quarters post-GA |
| Lag3 | Enterprise annual churn | 8.4% | <7% | 12-month cohort post-GA |
| **Lag4** | **Advanced Timeline account churn (dep-active only)** | **4.1% (existing Timeline users)** | **≤3.5%** | **12-month cohort post-GA** |
| **Lag5** | **NPS verbatims citing dependency tracking gap** | **34 (Q4 2025)** | **0** | **Q4 2026 NPS cycle** |

**Why Lag4 matters:** Timeline users already churn at 4.1% vs. 8.4% average. Advanced Timeline users should churn lower still — dep-embedded accounts have higher switching costs. If Lag4 is not meaningfully below 4.1%, the feature isn't creating additional stickiness beyond what Timeline alone already provided.

---

## 3. Guardrail Metrics — What Should NOT Change

| # | Guardrail | Threshold | Rationale |
|---|---|---|---|
| G1 | Timeline load time — non-dep boards | P75 must not increase >500ms for boards with 0 dependencies | Dep graph infrastructure must not tax boards that didn't opt in |
| G2 | Non-Enterprise Timeline WAU | Must not drop >5% in 4 weeks post-GA | Enterprise gate UX (upgrade prompt) must not drive non-Enterprise users off Timeline entirely |
| G3 | Support tickets — Timeline surface | Must not exceed 2× baseline in weeks 1–4 post-GA | Spike = UX confusion: dep creation unclear, conflict indicator alarming, or gate generating frustrated upgrade attempts |
| G4 | Dep-active account churn rate | Must not exceed non-dep Timeline account churn rate | If dep-active accounts churn higher, the feature exists but fails to deliver value |
| G5 | Mobile Timeline session count | Must not drop >10% post-GA | Mobile dep editing excluded; dep graph infrastructure changes must not degrade mobile rendering |
| G6 | Enterprise board creation rate | Must not drop in first 30 days post-GA | UX addition must not hurt discoverability or navigation for the broader Timeline surface |

---

## 4. Account-Specific Tracking

### Marcus Chen — Meridian Financial
340 seats | Renewal: ~April 20, 2026 | Win condition: beta access + dep roadmap commitment

| Gate | Signal | Target Date | Owner |
|------|--------|-------------|-------|
| OD-3 resolved | Beta access formally offered | This week | VP Sales + Product |
| Beta onboarded | Meridian account active in beta | April 6 (2 weeks pre-renewal) | CSM |
| Beta activated | First dep created in Meridian account | Within 7 days of beta access | Product analytics |
| Cross-team spread | ≥3 distinct dep users in Meridian | Within 21 days of beta access | Product analytics |
| Dep depth | ≥3 dep-active boards | Day 30 of beta access | Product analytics |
| Renewal | Meridian renews; CRM notes cite dep management | ~April 20 | CRM / Sales |

**Monitoring flag:** If Meridian activates beta but has zero dep-active boards within 14 days — trigger CSM outreach. Feature is not solving the stated use case. Either cross-board scope (OD-2) wasn't resolved in their favor, or onboarding failed.

---

### Sarah Kim — Constellation Brands
600 seats | Win condition: champion enablement + early preview

| Gate | Signal | Target Date | Owner |
|------|--------|-------------|-------|
| Early preview | Sarah's account enabled on Advanced Timeline | GA week | CSM |
| Dep activation | Sarah's team creates deps within 7 days | GA + 7 days | Product analytics |
| Workaround signal | Dep board density ≥5 links (manual workarounds declining) | GA + 30 days | Product analytics |
| Bulk editing | Account uses bulk dep editing | GA + 30 days | Product analytics |
| Champion spread | ≥5 distinct dep users in Constellation | GA + 60 days | Product analytics |
| Account health | No churn signal | GA + 180 days | CSM |

**Important:** Bulk editing (P1-2) is GA scope, not beta. Sarah's specific pain — managing deps one-at-a-time — is not resolved until GA. Don't offer beta as a full solution if bulk editing is her primary need. Set expectation explicitly.

---

### Rebecca Okafor — Northgate Healthcare
180 seats | Win condition: executive credibility signal (roadmap letter)

| Gate | Signal | Target Date | Owner |
|------|--------|-------------|-------|
| Exec letter sent | Product leadership roadmap letter delivered | This week | CPO / Product |
| Critical path adoption | ≥2 Northgate boards with critical path enabled | GA + 30 days | Product analytics |
| Exec visibility | VP-level users opening Timeline boards in Northgate | GA + 45 days | Product analytics |
| Exec reporting | Timeline board views increasing during weekly/monthly exec review cycles | GA + 60 days | Session timing analysis |
| Account health | No churn signal | GA + 90 days | CSM |

**Note:** Rebecca's win condition is exec credibility, not operational embedding. The roadmap letter is the near-term action. Product metrics confirm whether the feature delivered on the credibility promise.

---

## 5. Leading Signal That Predicts Churn Reduction

### The causal chain

```
Activation → Team spread → Dep graph depth → Exec embedding → Switching cost → Renewal
```

Each stage raises the organizational cost of leaving:
- One PM creates deps → one person can decide to stop
- Team creates deps → requires organizational consensus to switch tools
- 3+ boards with dep graphs → rebuilding project plans mid-quarter in a new tool is a disruption
- Execs consuming critical path views → switching tools breaks exec reporting workflows; PM no longer has unilateral authority to switch

### The proxy predictor — "3-Board Depth Score"

An account qualifies if, within **60 days of GA**, it has ALL THREE:
1. ≥3 dep-active boards
2. ≥2 distinct dep users
3. ≥1 board with critical path enabled

**Hypothesis:** Accounts that hit this composite signal by Day 60 will show meaningfully lower 12-month churn than dep-activated-but-shallow accounts.

**How to operationalize:** Build as a single account health flag in CRM or data warehouse. Once an account hits all three conditions, mark as "dep-embedded." Track that cohort's renewal rate separately from (a) dep-activated but shallow accounts and (b) non-dep Timeline users.

### The signal timeline

| Week | Signal | What it predicts |
|------|--------|-----------------|
| 1–2 | Dep activation rate (L1) | Teams are exploring the feature |
| 3–4 | Multi-user dep adoption (L3) | Feature has moved from champion to team — tipping point |
| 6–8 | 3-Board Depth Score forming | Project planning operationally embedded — switching cost rising |
| 8–12 | Critical path embedding (L4) | Exec workflows dependent — switching cost now organizational |
| Month 6+ | Dep-active churn rate (Lag4) | The lagging signal confirms or denies the prediction |

**Highest-risk failure mode:** If multi-user spread (L3) stalls — meaning one PM per account activates but teammates don't engage — the churn reduction won't materialize. Single-user tools don't create organizational switching costs. If L3 is below 30% at Day 30, that is the highest-priority signal to investigate before any other metric.

---

## Summary — The Monitoring Calendar

| Milestone | Actions |
|-----------|---------|
| **Beta launch (April 20)** | Start tracking L1, L2 immediately; open Marcus monitoring gates |
| **Beta + 7 days** | First L1 read; check Marcus activation; send Sarah early preview access |
| **Beta + 14 days** | L1 final read; L6 first read; Marcus cross-team spread check |
| **GA launch (early June)** | Start full L1-L6 tracking; open Sarah and Rebecca monitoring gates |
| **GA + 30 days** | Full leading indicator review (L3, L4, L5); all guardrails checked; account gates reviewed |
| **GA + 60 days** | 3-Board Depth Score first read; dep graph density cohort analysis |
| **GA + 90 days** | Timeline WAU (Lag1) 90-day read; Marcus/Rebecca renewal outcomes confirmed |
| **GA + 2 quarters** | Win rate vs ClickUp (Lag2) read; NPS verbatim analysis |
| **GA + 12 months** | Churn cohort analysis (Lag3, Lag4); 3-Board Depth Score predictive validation |
