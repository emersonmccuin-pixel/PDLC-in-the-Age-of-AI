# Phase 0: Strategy & Portfolio Prioritization

## Principle

**Portfolio decisions should be data-driven and continuous, not calendar-driven and periodic.**

Quarterly planning creates a 3-month lag between signal and response. By the time the roadmap is approved, the market has moved, customer needs have shifted, and the data is stale. In a world where building is nearly free, the cost of building the wrong thing dwarfs the cost of building itself. Portfolio prioritization must match the speed of execution.

---

## Pattern

### From Periodic Planning to Continuous Prioritization

**Traditional flow:**
```
Quarterly planning  -->  Roadmap locked  -->  Execute for 3 months  -->  Review
      (weeks)              (rigid)              (no course correction)     (too late)
```

**AI-native flow:**
```
Continuous signal    -->  Living priority   -->  Weekly decision    -->  Immediate
synthesis                 scores                 points                  execution
(AI background)          (always current)       (hours, not weeks)     (days, not months)
```

### Process Design

1. **AI Signal Synthesis (continuous, background)**
   - AI aggregates inputs: support tickets, usage analytics, churn data, NPS trends, competitor releases, market signals, sales pipeline data
   - AI maintains a living priority score for every potential initiative
   - Scores update in real-time as new data arrives
   - AI surfaces emerging patterns: "support tickets about X up 40% this month"

2. **AI-Drafted Business Cases (on demand)**
   - When an initiative crosses a priority threshold, AI drafts a business case
   - Includes: problem sizing, affected customer segments, estimated impact, effort level, risks
   - Humans validate and decide — they don't research and write

3. **Weekly Decision Points (human)**
   - Leadership reviews AI-surfaced priorities weekly (not quarterly)
   - Decisions: approve, defer, kill, investigate further
   - Decision is recorded with rationale — AI tracks decision history for pattern analysis

4. **Portfolio Rebalancing (continuous)**
   - AI flags when in-progress work conflicts with shifting priorities
   - AI identifies sunk cost situations: "this initiative's priority has dropped 60% since approval"
   - AI models portfolio balance: too many big bets? Too much maintenance? Not enough innovation?

### Democratization Governance Layer

When building is nearly free, portfolio governance becomes critical:
- AI flags proposed work that overlaps with existing tools or in-progress initiatives
- AI assesses orphan risk: "if the proposer leaves, who maintains this?"
- AI enforces the "should this exist?" question before build begins

---

## Implementation Example

**Scenario:** Mid-size SaaS company, 50-person engineering org, ~200 open feature requests.

**Tools:** Jira for backlog, Snowflake for analytics, HubSpot for customer data, Slack for internal comms.

**What it looks like:**

- An AI agent runs nightly, pulling data from Jira (ticket volume by category), Snowflake (feature usage, churn correlation), HubSpot (deal pipeline mentions, customer feedback), and support channels
- Each morning, product leadership gets a priority dashboard: top 10 initiatives ranked by composite score (customer impact x revenue impact x effort estimate x strategic alignment)
- When a score changes significantly, AI posts to a Slack channel: "Priority shift: SSO integration moved from #8 to #3 — enterprise churn mentions up 2x this quarter"
- Product lead clicks through, sees the AI-drafted business case, edits/approves it, and it enters the build pipeline within days
- Weekly 30-minute leadership sync reviews the top 5, makes go/no-go decisions, and moves on

**Contrast with traditional:** The same company used to spend 2 weeks in quarterly planning, debating a locked roadmap based on 3-month-old data. Features took weeks to even enter the pipeline after being prioritized.

---

## Current State

From Phase 1 baseline:
- Portfolio prioritization was not part of the original 9-phase mapping (it happens "before" the delivery pipeline)
- Most organizations run quarterly or annual planning cycles
- Decisions are based on executive intuition, stale data, and political dynamics
- AI has no role in this phase at most organizations today

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Cadence** | Quarterly/annual | Continuous with weekly decision points |
| **Data** | Stale reports, anecdotal evidence | Real-time signal synthesis from all data sources |
| **Business cases** | PM researches and writes (days/weeks) | AI drafts from data, human validates (hours) |
| **Decision speed** | Weeks to approve an initiative | Hours to days |
| **Portfolio visibility** | Spreadsheet updated monthly | Living dashboard, always current |
| **Governance** | "Is this on the roadmap?" | "Should this exist? Does it overlap? Who maintains it?" |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Product Leader** | Researches, writes business cases, presents to leadership | Reviews AI-drafted cases, makes judgment calls, sets strategic direction |
| **Executive** | Attends quarterly planning, debates priorities | Reviews weekly priority shifts, makes fast go/no-go decisions |
| **Data Analyst** | Builds priority reports on request | Configures AI signal sources, validates scoring model |

---

## Open Questions & Risks

1. **AI bias in signal synthesis.** If AI weights support ticket volume too heavily, squeaky-wheel customers dominate the roadmap. How do we balance signal sources?
2. **Executive buy-in.** Moving from quarterly planning to continuous prioritization requires executives to engage more frequently but in shorter bursts. Will they?
3. **Strategic vs. reactive.** Continuous prioritization risks becoming purely reactive to customer noise. How do we preserve space for strategic bets that don't show up in support tickets?
4. **Data quality.** AI signal synthesis is only as good as the data. If support tickets are poorly categorized or usage analytics are incomplete, the priority scores are misleading.
5. **Decision fatigue.** Weekly decision points could become another meeting that people dread. How do we keep it sharp?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Time from signal to decision** | How fast does a customer need become an approved initiative? | < 1 week (vs. 1-3 months today) |
| **Decision accuracy** | % of shipped features that hit adoption targets | Improve by 20%+ |
| **Portfolio freshness** | Average age of data used in priority decisions | < 1 week (vs. 1-3 months) |
| **Abandoned initiatives** | % of approved work that gets killed mid-build | Decrease (better upfront prioritization) |
| **Overlap incidents** | Duplicate/conflicting tools or features built | Zero tolerance |
