# Phase 7: Staging & Pre-Production

## Principle

**Staging is done when criteria are met, not when a calendar says so.**

"Let it bake for a week" is not a quality strategy — it's a comfort ritual. Time-based staging adds days or weeks of latency without a clear quality signal. Metrics-based staging replaces time with evidence: define what "healthy" looks like, monitor it, and advance when the criteria are met. If criteria are met in 2 hours, ship in 2 hours. If criteria aren't met in a week, baking longer won't fix it.

---

## Pattern

### From Time-Based Baking to Metrics-Based Advancement

**Traditional flow:**
```
Deploy to staging  -->  "Let it bake"  -->  Schedule demo  -->  Wait for signoff  -->  Release
                        (1-2 weeks)         (calendar)         (days)
```

**AI-native flow:**
```
Deploy to staging  -->  AI monitors     -->  Async stakeholder  -->  Auto-advance
(automatic)             health metrics       review                  when criteria met
                        (hours)              (concurrent)            (hours)
```

### Process Design

1. **Automated Staging Deployment (instant)**
   - Deployment to staging is automatic on QA signoff
   - No manual deploy process, no deploy queue, no scheduling
   - Deployment includes: feature code, database migrations, configuration changes, feature flags

2. **AI Health Monitoring (continuous, hours)**
   AI monitors 15-30 defined health metrics from the moment of staging deploy:

   | Category | Example Metrics |
   |----------|----------------|
   | **Error rates** | Exception rate, 5xx responses, client-side errors |
   | **Performance** | P50/P95/P99 latency, database query time, page load time |
   | **Functional** | Key user flows completing successfully, API contract compliance |
   | **Resource** | CPU, memory, database connections, queue depth |
   | **Business** | Conversion rates, feature engagement, checkout completion |

   - AI compares staging metrics against production baselines
   - AI flags anomalies, not just errors: "latency increased 40% compared to production" even if still under threshold
   - Criteria are defined per feature, not globally: a dashboard feature has different health metrics than a payment feature

3. **Async Stakeholder Review (concurrent with monitoring)**
   - AI generates a feature walkthrough: interactive demo, annotated screenshots, or video
   - Stakeholders review on their own time — no scheduling required
   - Synchronous demo meetings reserved for complex or controversial features only
   - Stakeholders provide async approval or flag concerns
   - **This runs in parallel with health monitoring, not after it**

4. **AI-Generated Release Artifacts (automatic)**
   - Release notes from PR descriptions and commit messages
   - Migration guides if the change affects existing users or APIs
   - Rollback procedures specific to this release
   - Support documentation (FAQ, known limitations) from QA findings

5. **Auto-Advance on Criteria Met**
   - When all health metrics are green for the defined threshold AND stakeholder approval is received: auto-advance to production release
   - If metrics fail: AI diagnoses and flags the specific issue (not just "staging failed")
   - If stakeholder concerns raised: routed to PM for decision (release with caveat, fix first, or descope)

---

## Implementation Example

**Scenario:** Dashboard customization — QA signed off, deploying to staging.

**What happens:**

1. Automatic staging deployment at 2:00 PM:
   - Feature code deployed with database migration (widget_order column)
   - Feature flag: enabled for 100% of staging users

2. AI health monitoring begins:
   - Minute 0-15: All metrics baseline comparison running
   - Minute 15: "Dashboard page load P95 increased from 180ms to 240ms" — flagged as anomaly (still under 500ms threshold)
   - AI diagnosis: "Increase due to additional localStorage read for widget order. Acceptable for P95. P50 unchanged at 120ms."
   - Minute 30: All error rates at 0. API response times nominal. Database query time unchanged.
   - Hour 1: Feature engagement metrics available. Edit mode accessed by 12% of staging users (internal testers).
   - Hour 2: All 22 monitored metrics green. Performance anomaly accepted (within threshold, explained).

3. Async stakeholder review (started at 2:00 PM, concurrent):
   - AI generates walkthrough: 4 annotated screenshots showing default view → edit mode → drag reorder → saved state
   - PM reviews at 3:30 PM: "Looks good. The 'Customize' button placement works after the QA-driven UX fix."
   - VP Product reviews at 4:15 PM: "Ship it. Add a tooltip for first-time users — file as follow-up, don't block."

4. Release artifacts generated:
   - Release notes: "Dashboard widgets can now be reordered via drag-and-drop. Click 'Customize' to enter edit mode."
   - Migration guide: "No user action required. Existing dashboard layouts are preserved as the default order."
   - Rollback procedure: "Revert migration, remove feature flag. Widget order column is additive — no data loss on rollback."

5. Auto-advance at 4:30 PM: All metrics green for 2+ hours, both stakeholders approved. Release candidate flagged as ready.

**Total staging time:** 2.5 hours. Traditional: 1-3 weeks.

---

## Current State

From Phase 1 baseline:
- **Typical time:** 1-3 weeks
- **Bottlenecks:** Environment contention, deployment queue, "let it bake" culture with no defined criteria, stakeholder availability for signoff
- **Key roles:** DevOps, Engineering Manager, Product Manager, Stakeholders
- **AI role today:** Some organizations use automated health checks, but metrics-based staging advancement, AI-generated walkthroughs, and async stakeholder review are rare. Most staging is still time-based with manual demos.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Deploy to staging** | Manual, scheduled, queued | Automatic on QA signoff |
| **"Bake" criteria** | Time-based ("1 week") | Metrics-based (15-30 defined health metrics) |
| **Monitoring** | Manual dashboard checks | AI continuous monitoring with anomaly detection and diagnosis |
| **Stakeholder demo** | Scheduled meeting (days to align calendars) | AI-generated walkthrough for async review |
| **Signoff** | Sync meeting or email chain | Async approval with auto-advance on criteria met |
| **Release artifacts** | Written manually before release | Auto-generated from PRs, commits, and QA findings |
| **Total time** | 1-3 weeks | Hours |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **DevOps** | Manages staging deployments, monitors manually, maintains deploy queue | Configures health metrics, maintains deployment automation, handles anomaly escalations |
| **Engineering Manager** | Coordinates staging schedule, attends demo meetings | Reviews AI health reports, makes go/no-go for anomaly escalations |
| **Product Manager** | Schedules and runs stakeholder demos, collects feedback | Reviews AI-generated walkthrough, approves async, resolves stakeholder concerns |
| **Stakeholders** | Attend demo meeting, provide sync feedback | Review walkthrough on their own time, approve async |

---

## Open Questions & Risks

1. **Metrics selection.** Which 15-30 metrics define "healthy" for a given feature? Too few and you miss issues. Too many and you get noise. Who defines them and when?
2. **Baseline accuracy.** Metrics-based staging compares against production baselines. What about net-new features with no baseline? How do you define "healthy" for something that didn't exist before?
3. **Stakeholder async adoption.** Will stakeholders actually review AI-generated walkthroughs on their own time? Or will they demand live demos? Some stakeholders need to "click through it themselves."
4. **Environment parity.** Staging that doesn't match production gives false confidence. How do we ensure staging is close enough to production for metrics to be meaningful?
5. **Regulatory requirements.** Some industries (healthcare, finance) require specific staging durations or sign-off processes. How does metrics-based staging interact with compliance requirements?
6. **Risk of speed.** Shipping 2.5 hours after QA signoff feels fast. Is there value in a minimum staging duration as a circuit breaker, even if metrics are green?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Staging cycle time** | Hours from staging deploy to release-ready | < 4 hours for standard features (vs. 1-3 weeks) |
| **Metrics-based advancement rate** | % of releases that auto-advance on criteria met | > 80% (rest require human investigation) |
| **Production incidents from staging misses** | Bugs in production that were present in staging but not caught | Same or fewer than current |
| **Stakeholder review time** | Hours from walkthrough delivery to approval | < 4 hours (vs. days to schedule meeting) |
| **Environment downtime** | Hours staging is unavailable due to contention | Near-zero (no queue, automated deploys) |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 6: QA & Testing](06-qa-and-testing.md) | [Phase 2 Overview](overview.md) | [Phase 8: Release](08-release.md) |

### Related Documents
