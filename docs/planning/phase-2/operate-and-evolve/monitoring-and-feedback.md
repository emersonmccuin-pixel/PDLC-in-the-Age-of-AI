# Phase 10: Post-Release Monitoring & Feedback

## Principle

**Monitoring should detect, diagnose, and route — not just alert.**

Traditional monitoring tells you something is wrong. AI-native monitoring tells you what's wrong, why, which release caused it, who's affected, and what to do about it. The feedback loop from production back to the planning pipeline should be continuous and automatic, not dependent on a PM checking a dashboard.

---

## Pattern

### From Passive Dashboards to Active Intelligence

**Traditional flow:**
```
Alert fires  -->  Human investigates  -->  Human diagnoses  -->  Human files ticket  -->  Fix prioritized
(noisy)           (minutes-hours)          (hours)               (manual)                 (days-weeks)
```

**AI-native flow:**
```
AI detects     -->  AI diagnoses    -->  AI routes          -->  Fix enters pipeline
anomaly              + correlates        (auto-file,              (hours)
(continuous)         with release         notify team,
                     (seconds)            suggest fix)
```

### Process Design

1. **Continuous AI Monitoring (background process)**
   - AI monitors production metrics continuously (not just during deploy windows):
     - Error rates, latency, resource utilization
     - Business metrics: conversion funnels, checkout completion, feature engagement
     - User behavior patterns: rage-clicks, abandoned flows, unusual navigation paths
     - Log analysis: error patterns, warning trends, new exception types
   - Monitoring is context-aware: AI knows what shipped recently and correlates anomalies with releases

2. **AI Diagnosis & Correlation (automatic, seconds)**
   - When an anomaly is detected, AI immediately:
     - Correlates with recent releases: "This error started 2 hours after deploy X"
     - Identifies affected scope: which users, which endpoints, which features
     - Analyzes root cause from logs and traces: "New database query in widget_order handler missing index — timeout at scale"
     - Checks if the issue was seen in staging (and if so, why it wasn't caught)
   - **Output:** Diagnosis report with release correlation, root cause hypothesis, affected scope, and suggested fix

3. **Intelligent Routing (automatic)**
   Based on severity and type, AI routes the issue:

   | Severity | AI Action |
   |----------|-----------|
   | **Critical** (system down, data loss) | Page on-call, trigger auto-rollback if release-correlated, file P0 ticket |
   | **Major** (degraded, feature broken) | Notify team channel, file P1 ticket with diagnosis, suggest fix |
   | **Minor** (edge case, cosmetic) | File P2 ticket with reproduction steps, no alert |
   | **Trend** (gradual degradation) | Weekly report to engineering lead, suggest investigation |

4. **User Sentiment & Feedback Analysis (continuous)**
   - AI monitors user feedback channels: support tickets, in-app feedback, NPS surveys, social mentions
   - AI correlates sentiment with features: "Negative sentiment about dashboard loading speed increased 15% since widget customization launch"
   - AI generates weekly "feature health" reports per feature: adoption, errors, sentiment, support volume
   - Feedback automatically tagged and routed to the relevant product/engineering owner

5. **Portfolio Feedback Loop (automatic)**
   - Monitoring data feeds directly back to [Phase 0](../strategy-and-planning/portfolio-prioritization.md) (portfolio prioritization):
     - Features causing excessive support tickets get flagged for improvement or sunset
     - Features with declining usage get flagged for investigation
     - Pain points surfacing in support data become initiative candidates
   - AI maintains a continuous signal that informs the next planning cycle — no manual report needed

---

## Implementation Example

**Scenario:** Dashboard customization has been live for 3 weeks. Monitoring detects issues.

**What happens:**

1. Week 3, Tuesday 10:15 AM: AI detects anomaly
   - Dashboard page load P95 spiked from 240ms to 680ms in the last hour
   - Error rate still 0% — no failures, just slowness
   - AI correlates: no new deploys in 24 hours. Not release-correlated.
   - AI diagnoses: "widget_order table grew past 100K rows — query plan changed from index scan to sequential scan. Cause: widget order saved on every page load (should only save on edit)."
   - Severity: Major (performance degraded, no data loss)

2. AI routes:
   - Files P1 ticket: "Dashboard load performance regression — unnecessary widget_order writes on page load"
   - Includes: diagnosis, affected query, suggested fix ("move save to edit-mode exit only"), reproduction steps
   - Notifies #engineering channel: "P1: Dashboard load P95 at 680ms (baseline 240ms). Ticket PROJ-3042 filed with diagnosis."
   - Does NOT page on-call (not critical, no data loss)

3. Week 3, Friday: AI generates weekly feature health report
   - "Dashboard Customization — Week 3 Health Report"
   - Adoption: 41% DAU (up from 34% at week 2)
   - Performance: P95 degraded to 680ms (P1 filed, fix in progress)
   - Support tickets: 3 this week ("how to reset to default" — FAQ already covers this, but users aren't finding it)
   - Sentiment: Positive — 8 NPS comments mention customization favorably
   - Recommendation: "Improve discoverability of reset-to-default option. Consider adding to edit mode UI."

4. Portfolio feedback:
   - AI updates initiative score: "Dashboard customization delivering value (41% adoption) but has a performance issue in progress. Recommend holding on Phase 2 (custom widgets) until P1 resolved."

---

## Current State

From [Phase 1 baseline](../../../baseline/pdlc-standard-mapping.md):
- **Typical approach:** Ongoing but passive. Wait for bug reports. Check dashboards manually.
- **Bottlenecks:** Feedback loop speed, prioritization against new work
- **Key roles:** Product Manager, Engineering, Support
- **AI role today:** APM tools (Datadog, New Relic, Sentry) provide alerting and some diagnostics. But release correlation, automated diagnosis with fix suggestions, sentiment analysis, and portfolio feedback loops are mostly manual.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Anomaly detection** | Threshold-based alerts (noisy) | AI pattern detection with release correlation |
| **Diagnosis** | Human investigates logs, traces, dashboards | AI diagnoses and provides root cause hypothesis in seconds |
| **Routing** | Human decides severity, creates ticket | AI auto-classifies, auto-files, auto-routes based on severity |
| **User feedback** | Support reads tickets, PM reads NPS (when they remember) | AI continuously analyzes all feedback channels, correlates with features |
| **Feature health** | PM manually checks dashboards (if they check) | AI generates weekly per-feature health reports |
| **Portfolio feedback** | Annual review, anecdotal evidence | Continuous data feed to portfolio prioritization |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **SRE/On-call** | Investigate every alert, diagnose manually, file tickets | Review AI diagnoses, handle escalations, focus on systemic issues |
| **Product Manager** | Manually check dashboards, read NPS comments | Review AI feature health reports, make decisions based on data |
| **Support Lead** | Analyze ticket trends manually | Review AI-surfaced patterns, improve FAQ coverage proactively |
| **Engineering Lead** | Prioritize bugs from manual triage | Review AI-generated P1/P2 tickets with diagnoses, approve fixes |

---

## Open Questions & Risks

1. **Alert fatigue.** AI monitoring can detect more anomalies than human monitoring. More detection doesn't help if teams are overwhelmed. How do we calibrate sensitivity?
2. **Diagnosis accuracy.** AI root cause analysis is probabilistic. A wrong diagnosis wastes investigation time and builds distrust. How do we validate AI diagnoses and track accuracy?
3. **Feedback attribution.** Correlating user sentiment with specific features is hard. Users rarely say "Feature X is slow" — they say "your product is slow." How confident can AI be in feature-level attribution?
4. **Portfolio feedback gaming.** If monitoring data feeds portfolio decisions, teams may optimize their features' metrics at the expense of overall system health. How do we prevent this?
5. **Monitoring cost.** Continuous AI monitoring on all metrics, logs, and user behavior is computationally expensive. What's the ROI vs. traditional threshold alerts for smaller organizations?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Mean time to detect (MTTD)** | Minutes from anomaly start to detection | < 5 minutes (vs. hours with manual monitoring) |
| **Mean time to diagnose** | Minutes from detection to root cause hypothesis | < 15 minutes (vs. hours of human investigation) |
| **Diagnosis accuracy** | % of AI diagnoses confirmed correct by engineering | > 70% |
| **Auto-filing accuracy** | % of auto-filed tickets that are valid (not false positives) | > 85% |
| **Feedback loop time** | Days from production issue to portfolio signal | < 1 day (vs. weeks/months) |
| **Feature health coverage** | % of features with weekly AI health reports | 100% of features launched in last 6 months |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 9: Go-to-Market](../go-to-market-and-adoption/go-to-market.md) | [Phase 2 Overview](../overview.md) | [Phase 11: Customer Success](../go-to-market-and-adoption/customer-success.md) |

### Related Documents
- [Context Pipeline Architecture](../../../context-pipeline-architecture.md) — monitoring feeds back into the context pipeline
