# monday.com — Customer Personas

---

## Primary Persona: Marcus Chen (Enterprise IT Director / Buyer)

**Role:** Director of IT & Operations
**Company:** Meridian Financial Services — 2,200 employees, publicly traded mid-cap financial services firm
**monday.com plan:** Enterprise, 340 seats, $84,000/year contract up for renewal in 8 weeks
**Team:** Manages IT operations, internal tooling, and the monday.com rollout across 12 departments

**His relationship with monday.com:**
Marcus championed the monday.com rollout 18 months ago. He fought internal resistance from teams using Excel and Asana. It took him 6 months to fully migrate. The platform is now embedded across operations, HR, marketing, and finance.

The problem: his CFO is asking why they're paying $84k/year for a "fancy to-do list." Marcus needs the platform to demonstrate ROI at the enterprise level. He needs to show that monday.com is better than ClickUp — which his VP of Operations just forwarded him an article about, noting "it's free now."

**His real pain:**
The Timeline View is the feature his operations team uses most for project tracking. But when projects have dependencies — Task B can't start until Task A finishes — the team workarounds with manual date-shifting. Two project managers have told Marcus that ClickUp's Gantt chart "actually works" for their dependency-heavy infrastructure projects. He hasn't made a decision yet. But he will at renewal.

**The quote:**
> "I put my credibility on the line to get monday.com into this company. Now I need
> it to hold up against a free competitor. I need to go to my CFO with a reason
> to renew at this price point — not a promise of 'features coming soon.'"

**What would keep him:**
A concrete, credible commitment that Advanced Timeline with dependency management ships before his renewal date. Not a vague roadmap. A date and a beta access invitation.

**Technical comfort:** High — manages enterprise SaaS stack, familiar with integrations, comfortable with admin configuration.

**Decision authority:** Owns the contract renewal decision. CFO approves annual spend over $50k but defers to Marcus on tool selection.

---

## Secondary Persona: Sarah Kim (Senior Project Manager / Power User)

**Role:** Senior Project Manager, Global Marketing Operations
**Company:** Constellation Brands (large CPG company), 7,000 employees
**monday.com plan:** Enterprise, part of a 600-seat org-wide rollout
**Team:** Manages 3 project coordinators and coordinates campaigns across 8 regional marketing teams

**Her relationship with monday.com:**
Sarah is the monday.com power user on her team. She's built 14 custom boards, created a complex automation system for campaign intake, and trained her entire team. She knows the platform better than the IT team that manages it.

She uses the Timeline view daily. She has built detailed campaign project timelines. The problem: her campaigns involve dependencies across teams — the creative brief can't start until the strategy brief is approved, the media buy can't finalize until the creative is approved. She currently manages these dependencies manually with a color-coded legend in her board description.

**Her real pain:**
She had a competitor evaluation conversation with ClickUp last week — not because she wants to leave, but because her SVP asked "why can't monday.com show us when deliverables are blocking each other?" She spent 45 minutes trying to explain how she manually tracks it. The meeting ended awkwardly.

**The quote:**
> "I tell everyone monday.com is the best platform I've used. But when someone asks
> about dependencies, I go quiet. I have to explain my workaround. It's embarrassing
> when a free product does something we don't."

**What she needs:**
The ability to draw an arrow between two timeline items and have monday.com automatically shift downstream dates when the upstream task is delayed. She doesn't need critical path analysis. She needs the basic dependency arrow.

**Technical comfort:** Very high. Daily power user. Reads product release notes. Participates in beta programs.

**Decision authority:** None — she's not the buyer. But she is the internal champion. If Sarah stops advocating for monday.com, the enterprise account is at risk at the next renewal.

---

## Tertiary Persona: Rebecca Okafor (Department VP / Exec Sponsor)

**Role:** VP of Strategic Programs
**Company:** Northgate Healthcare (regional hospital network), 4,500 employees
**monday.com plan:** Enterprise, 180 seats across Strategic Programs, IT, and Operations
**Team:** Oversees the digital transformation program office; monday.com is the system of record for all program tracking

**Her relationship with monday.com:**
Rebecca sponsored the monday.com procurement two years ago. She uses Dashboards to monitor program health. She does not use the Timeline view herself — she sees rolled-up summaries from her team.

She recently sat in a board meeting where a healthcare system peer mentioned ClickUp as a "more affordable enterprise project management tool." She flagged it to her IT director (similar to Marcus) and asked for an honest competitive assessment.

**Her real pain:**
Rebecca doesn't care about specific features. She cares about three things:
1. Does monday.com justify its cost at renewal? (Her IT Director needs an answer to this.)
2. Is the vendor a credible partner? (Does monday.com communicate proactively about improvements?)
3. Are her teams actually using it and finding it valuable? (She hears mixed signals from project managers.)

**The quote:**
> "I don't evaluate software features. My team does that. What I need to know is:
> will this vendor be ahead of the market 18 months from now? Or will I be explaining
> a mistake to my CFO?"

**What she needs:**
A proactive communication from monday.com about the competitive response. Not marketing language — a genuine acknowledgment that she's asking the right question, a clear statement of what's coming, and enough confidence to take back to her procurement review.

**Technical comfort:** Low — uses Dashboards, not Boards. Does not configure anything.

**Decision authority:** Owns the renewal decision for the entire 180-seat contract. Final word.

---

## Persona Quick Reference

| Persona | Role | Seat Count | Key Pain | Wants to Hear |
|---------|------|-----------|----------|---------------|
| Marcus Chen | IT Director (buyer) | 340 seats | "Free ClickUp has dependencies, my team wants it" | "Here's what we're shipping and when" |
| Sarah Kim | Sr. PM (power user) | Part of 600-seat org | "I have to explain my dependency workaround every time" | "Dependency arrows are coming — here's what they look like" |
| Rebecca Okafor | VP Strategic Programs (exec sponsor) | 180 seats | "Is this vendor going to be ahead of the market?" | "We heard you. Here's our roadmap commitment." |

---

## Anti-Persona: SMB Customer

monday.com has 190,000+ SMB customers. The ClickUp Gantt feature announcement does concern SMB customers — but less acutely. SMB customers using free or Basic plans care about price. Enterprise customers care about features, security, and partner credibility. The feature we're speccing (Advanced Timeline with dependency management) is an Enterprise response, not an SMB play.

Designing this feature for SMB users would sacrifice the enterprise-grade requirements (cross-project dependencies, bulk dependency editing, SLA-bound support) that make it worth building.
