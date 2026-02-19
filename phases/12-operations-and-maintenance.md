# Phase 12: Operations & Maintenance

## Principle

**Maintenance should be predictive and continuous, not reactive and periodic.**

Traditional maintenance is reactive: things break, we fix them. Tech debt accumulates silently until it becomes a crisis. Dependency updates pile up until a security vulnerability forces action. In the AI-native PDLC, AI monitors system health trends, predicts failures, and proposes maintenance work continuously — surfacing problems before they become incidents and drafting fixes for human approval.

---

## Pattern

### From Reactive Firefighting to Predictive Maintenance

**Traditional flow:**
```
Incident    -->  Investigate  -->  Fix   -->  Repeat
happens          (scramble)       (rush)      (same problems recur)
(surprise)

Tech debt:  Accumulates silently  -->  "Tech debt sprint"  -->  Accumulates again
                                       (every 3-6 months)
```

**AI-native flow:**
```
AI predicts    -->  AI drafts fix  -->  Human approves  -->  AI monitors outcome
(continuous)        (proactive)         (judgment)           (continuous)

Tech debt:  AI tracks continuously  -->  AI proposes fixes  -->  Prioritized in backlog
            (never invisible)            (always available)      (not batched)
```

### Process Design

1. **Predictive Health Monitoring (AI, continuous)**
   - AI monitors trends, not just thresholds:
     - Growing response times (not yet at threshold, but trending up)
     - Increasing test flakiness (2% last month, 5% this month)
     - Database query plan changes (new slow queries appearing)
     - Memory/CPU usage trends (growing toward capacity)
     - Dependency vulnerability disclosures
   - AI predicts when trends will cross thresholds: "At current growth rate, database connection pool will saturate in 3 weeks"
   - **Key shift:** Maintenance is triggered by prediction, not by failure

2. **AI-Drafted Maintenance PRs (proactive)**
   AI proposes and drafts maintenance work for human approval:

   | Maintenance Type | AI Action |
   |-----------------|-----------|
   | **Dependency updates** | Drafts PR with updated dependencies, runs test suite, flags breaking changes |
   | **Deprecation fixes** | Identifies deprecated API usage, drafts migration code |
   | **Performance optimization** | Identifies slow queries/endpoints, proposes indexes/caching |
   | **Test maintenance** | Identifies flaky tests, proposes fixes or marks for investigation |
   | **Dead code removal** | Identifies unused code paths, proposes cleanup |
   | **Security patches** | Monitors CVEs, drafts patches for affected dependencies |

   Each draft PR includes: what it changes, why (data-driven rationale), risk assessment, and rollback plan.

3. **Incident Response (AI-assisted)**
   When incidents occur despite predictive monitoring:
   - AI performs initial triage: severity classification, affected systems, blast radius
   - AI executes runbook steps for known incident types (restart services, scale up, enable circuit breakers)
   - AI drafts stakeholder communication: "Service degradation affecting dashboard load times. Investigating. ETA: 30 minutes."
   - Humans handle escalation, judgment calls, and novel incidents
   - AI generates post-incident report: timeline, root cause, contributing factors, prevention recommendations

4. **Tech Debt Tracking (AI, continuous)**
   - AI maintains a living tech debt inventory:
     - Code complexity scores per module (and trends)
     - Test coverage gaps and trends
     - Dependency staleness (days since last update)
     - Architecture drift (code that doesn't follow established patterns)
   - Each tech debt item has an estimated cost (risk of incident, performance impact, maintenance burden) and a proposed fix
   - Tech debt is visible in portfolio prioritization ([Phase 0](00-portfolio-prioritization.md)) — not hidden until it explodes

5. **Capacity Planning (AI, continuous)**
   - AI models resource usage against growth projections
   - AI recommends scaling actions before capacity becomes an issue
   - AI identifies cost optimization opportunities: over-provisioned resources, unused infrastructure

---

## Implementation Example

**Scenario:** The application has been running for 6 months with regular feature releases. AI maintenance monitoring is active.

**What happens in a typical week:**

1. Monday: AI dependency scan
   - 3 dependency updates available (2 minor, 1 patch)
   - AI drafts PR for each, runs test suite, all pass
   - 1 minor update has a breaking API change — AI flags: "lodash 4.18 → 4.19 changes _.merge deep copy behavior. 3 call sites affected. Draft includes migration."
   - Engineers review and approve 2 (auto-merged), investigate the third

2. Wednesday: AI trend alert
   - "API response time for /dashboard endpoint trending up. P95 increased from 240ms to 320ms over 2 weeks. Not yet at threshold (500ms). Cause: widget_order table growing linearly — current query plan degrades above 500K rows. Projected threshold breach: 4 weeks."
   - AI drafts optimization PR: adds composite index, rewrites query to use pagination
   - Engineer reviews, approves. PR merged. P95 drops to 200ms.

3. Thursday: AI dead code detection
   - "Module `legacy_dashboard_v1` has 0 imports across the codebase. Last modified 8 months ago. 1,200 lines. Recommend removal."
   - AI drafts cleanup PR with all references removed and tests updated
   - Engineer confirms the module is truly unused, approves removal

4. Friday: AI tech debt report (weekly)
   - Test flakiness: 3.2% (up from 2.8% last week). 2 flaky tests identified.
   - Dependency staleness: 4 dependencies > 90 days old, 1 has known vulnerability (CVSS 4.2, low severity)
   - Code complexity: `src/components/Dashboard/` complexity increased 15% this quarter — "consider refactoring into smaller modules"
   - Overall system health score: 82/100 (down from 85 last week due to flaky tests and trending response time)

**No incidents this week.** The predictive approach caught the performance issue 4 weeks before it would have become an incident.

---

## Current State

From the brief:
- Operations and maintenance is traditionally reactive: incident response when things break, periodic tech debt sprints
- AI role today: APM tools provide alerting, some organizations use automated dependency update tools (Dependabot, Renovate). Predictive maintenance, AI-drafted PRs, and continuous tech debt tracking are not standard.
- Tech debt is invisible until it causes incidents — then it gets a "tech debt sprint" that's never enough

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Maintenance trigger** | Incidents, periodic sprints | Predictions, continuous proposals |
| **Dependency updates** | Manual, batched, often deferred | AI-drafted PRs with test results, continuous |
| **Tech debt visibility** | Hidden until crisis | Living inventory with cost estimates |
| **Incident response** | Human investigates from scratch | AI triages, executes runbooks, drafts comms, humans handle escalation |
| **Capacity planning** | Annual review, reactive scaling | Continuous modeling and proactive recommendations |
| **Post-incident analysis** | Manual report (often skipped) | AI-generated report with timeline and prevention recommendations |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **SRE/DevOps** | Firefighting, manual monitoring, periodic maintenance | Configures AI monitoring thresholds, reviews AI maintenance PRs, handles novel incidents |
| **Engineering Lead** | Allocates "tech debt time" periodically | Reviews AI tech debt reports, prioritizes high-impact items continuously |
| **On-call Engineer** | Investigates all incidents from scratch | Reviews AI triage and diagnosis, handles escalations and judgment calls |
| **Engineering Manager** | Negotiates tech debt time with product | Uses AI-quantified tech debt data to make data-driven trade-off decisions |

---

## Open Questions & Risks

1. **Auto-merge safety.** AI-drafted maintenance PRs (dependency updates, dead code removal) are tempting to auto-merge. Where's the line between safe auto-merge and required human review?
2. **Prediction accuracy.** Predictive monitoring works for linear trends but struggles with discontinuities (traffic spikes, viral content, DDoS). How reliable are predictions in practice?
3. **Tech debt quantification.** AI estimating "cost" of tech debt is imprecise. A flaky test might be low-cost (annoying) or high-cost (masking a real bug). How do we validate cost estimates?
4. **Maintenance fatigue.** Continuous maintenance proposals are better than batched tech debt sprints, but engineers may resist reviewing AI-drafted PRs if the volume is high. How do we batch appropriately?
5. **Incident runbook coverage.** AI can execute runbooks for known incident types. But novel incidents require human investigation. How much of incident response can realistically be automated?
6. **Organizational incentives.** Maintenance is often deprioritized because it doesn't produce visible features. AI-quantified tech debt helps, but cultural change is needed to value maintenance work.

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Incidents prevented** | Issues caught by prediction before becoming incidents | Track and trend upward |
| **Mean time to resolve (MTTR)** | Minutes from incident start to resolution | < 30 minutes (vs. hours) |
| **Tech debt inventory freshness** | % of codebase with current health assessment | 100% |
| **Dependency staleness** | Average days since last dependency update | < 30 days |
| **AI maintenance PR approval rate** | % of AI-drafted PRs approved without significant changes | > 80% |
| **System health score trend** | Weekly health score trending stable or improving | Stable at > 80/100 |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 11: Customer Success](11-customer-success.md) | [Phase 2 Overview](overview.md) | [Phase 13: Sunset & Deprecation](13-sunset-and-deprecation.md) |

### Related Documents
- [Context Pipeline Architecture](../context-pipeline.md) — ops as a context consumer
