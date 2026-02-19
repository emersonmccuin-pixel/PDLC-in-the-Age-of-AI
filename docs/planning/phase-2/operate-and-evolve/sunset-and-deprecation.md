# Phase 13: Sunset & Deprecation

## Principle

**Features should have a lifecycle, not a lifespan of "forever by default."**

In most organizations, features never die. They accumulate indefinitely — each one adding maintenance cost, cognitive complexity, test surface, and UX clutter. Nobody wants to own the removal because there's no process for it. In the AI-native PDLC, sunset evaluation is a continuous process: AI identifies candidates, quantifies impact, and drafts the removal plan. Humans make the decision. AI does the work.

---

## Pattern

### From Permanent Accumulation to Active Lifecycle Management

**Traditional flow:**
```
Feature ships  -->  Lives forever  -->  Maintenance cost grows  -->  Nobody removes it
                    (no review)         (invisible)                  (too risky, too hard)
```

**AI-native flow:**
```
AI monitors     -->  AI identifies     -->  AI drafts impact    -->  Human decides    -->  AI executes
feature health       sunset candidates      analysis + plan          (keep/sunset)         removal
(continuous)         (proactive)             (automatic)                                   (safe, verified)
```

### Process Design

1. **Continuous Feature Health Monitoring (AI, background)**
   AI maintains a health score for every feature, incorporating:
   - Usage trend: declining, stable, or growing
   - Maintenance cost: bug frequency, dependency on outdated libraries, test flakiness
   - Support burden: ticket volume about the feature
   - Supersession: does a newer feature or workflow make this one redundant?
   - User sentiment: are users satisfied or frustrated?
   - Revenue impact: does this feature drive revenue (premium tier, enterprise requirement)?

2. **Sunset Candidate Identification (AI, triggered by thresholds)**
   AI flags a feature for sunset evaluation when:
   - Usage has declined below a defined threshold for a sustained period
   - Maintenance cost exceeds the value delivered (AI estimates both)
   - A newer feature fully supersedes the old one
   - The feature's dependencies are end-of-life and migration is expensive
   - The feature has a high bug rate and no active ownership

   **AI doesn't recommend removal — it flags for evaluation.** The decision is human.

3. **AI Impact Analysis (automatic)**
   When a feature is flagged, AI generates a comprehensive impact analysis:
   - **Who's affected:** Active users (count, segments, revenue), integrations, downstream dependencies
   - **What breaks:** API consumers, linked features, data dependencies, external integrations
   - **Migration path:** What do affected users do instead? Is there a replacement workflow?
   - **Removal scope:** Code to remove, tests to remove, docs to update, configs to change
   - **Communication plan:** Who needs to be notified, how far in advance, through what channels
   - **Revenue risk:** Estimated impact on retention, revenue, contractual obligations

4. **Human Decision (judgment)**
   Stakeholders review the impact analysis and decide:
   - **Keep:** Feature stays. AI schedules next evaluation.
   - **Improve:** Feature is worth keeping but needs investment. Enters portfolio prioritization.
   - **Deprecate:** Feature enters deprecation period. Users notified. Migration support provided.
   - **Sunset:** Feature is removed. AI executes the removal plan.

5. **AI-Executed Removal (safe, verified)**
   If sunset is approved:
   - AI drafts deprecation communication: user notices, migration guides, timeline
   - AI generates the removal plan: code changes, test removals, doc updates, config changes
   - AI executes removal in phases:
     1. Deprecation notice (feature still works, users warned)
     2. Soft removal (feature hidden from new users, existing users can still access)
     3. Hard removal (code removed, data archived or migrated)
   - At each phase, AI verifies no downstream dependencies break
   - AI confirms removal is clean: no orphaned code, no broken references, no data loss

---

## Implementation Example

**Scenario:** The application has a "Classic Dashboard" view that was the default before the new customizable dashboard ([Phase 9](../go-to-market-and-adoption/go-to-market.md) launch). It's been 6 months since the new dashboard shipped.

**What happens:**

1. AI feature health monitoring flags Classic Dashboard:
   - Usage: Declined from 100% to 12% of DAU over 6 months (88% migrated to new dashboard)
   - Remaining users: 340 active users, mostly in 2 enterprise accounts
   - Maintenance cost: 3 bugs filed in last quarter, uses deprecated charting library (end-of-life in 9 months)
   - Support tickets: 2/month ("how do I switch back to classic"), declining
   - Supersession: New customizable dashboard fully replaces classic functionality

2. AI generates impact analysis:
   - **Affected users:** 340 active users across 2 enterprise accounts (combined ARR: $180K)
   - **What breaks:** Classic Dashboard API endpoint (no external consumers). No data dependencies.
   - **Migration path:** All classic widgets exist in new dashboard. Users can recreate their classic layout in 2 minutes.
   - **Removal scope:** 4,200 lines of code, 180 tests, 3 API endpoints, 12 help articles
   - **Communication plan:** 90-day deprecation notice → 30-day soft removal → hard removal. Email to affected accounts. In-app banner.
   - **Revenue risk:** Low — both enterprise accounts use other features extensively. Dashboard is not the retention driver.

3. PM and engineering lead review:
   - Decision: **Deprecate** with 90-day notice
   - Additional ask: "Offer the 2 enterprise accounts a white-glove migration — CSM walks them through setting up equivalent layouts"
   - AI adds CSM outreach to the communication plan

4. AI executes deprecation:
   - Day 1: Email to 340 users. In-app banner: "Classic Dashboard will be retired on [date]. Your widgets are available in the new dashboard."
   - Day 30: CSM completes white-glove migration for both enterprise accounts. 280 of 340 users already migrated naturally.
   - Day 60: Soft removal — Classic Dashboard hidden from navigation. Direct URL still works. 58 remaining users redirected with migration prompt.
   - Day 90: Hard removal — code, tests, API endpoints, docs removed. Data archived.
   - AI verifies: no broken imports, no orphaned references, no test failures. Clean removal.
   - **Result:** 4,200 lines removed, deprecated library dependency eliminated, 180 tests removed (test suite faster), 3 API endpoints decommissioned.

---

## Current State

From the brief:
- Sunset and deprecation is "often unplanned. Features accumulate forever. Dead code never dies."
- Most organizations have no formal sunset process
- Features accumulate indefinitely, each adding maintenance cost
- Nobody wants to own removal because there's no process, no data, and no safety net
- AI role today: None. Feature sunset is entirely manual and rare.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Feature lifecycle** | Ship and forget — lives forever | Continuous health monitoring with sunset evaluation |
| **Sunset trigger** | Crisis (security vuln, dependency EOL) or nothing | AI identifies candidates from health data proactively |
| **Impact analysis** | Manual investigation (if attempted) | AI-generated: affected users, breaks, migration path, revenue risk |
| **Decision data** | Gut feeling, political negotiation | Data-driven: usage trends, maintenance cost, revenue impact |
| **Execution** | Scary, error-prone, manual | AI-executed in phases with verification at each step |
| **Communication** | Ad hoc or absent | AI-drafted, phased, with migration support |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Product Manager** | Avoids sunset decisions (political risk) | Reviews AI impact analysis, makes data-driven keep/sunset decisions |
| **Engineering Lead** | Advocates for removal but can't prove the case | Uses AI-quantified maintenance cost to justify removal |
| **Customer Success** | Surprised by removals, scrambles to manage accounts | Proactively manages migration with AI-identified affected accounts |
| **Engineering** | Manual, risky code removal (if it happens at all) | Reviews AI-generated removal plan, approves phased execution |

---

## Open Questions & Risks

1. **Customer attachment.** Some features have low usage but extremely high attachment among the users who rely on them. Usage data alone can't capture this. How do we account for emotional/workflow dependency?
2. **Contractual obligations.** Enterprise contracts may guarantee feature availability. AI impact analysis needs to flag contractual commitments. Where does this data come from?
3. **Sunset velocity.** In a world where building is nearly free, features accumulate faster. Does the sunset rate need to match the build rate, or is some accumulation acceptable?
4. **Data retention.** When a feature is sunset, what happens to user data associated with it? Archival, deletion, migration? Different answers for different regulatory environments.
5. **Feature bundling.** Features don't exist in isolation. Sunsetting feature A may make feature B less useful. AI impact analysis needs to model these interdependencies.
6. **Organizational willingness.** Even with data, organizations resist removing features ("what if someone needs it?"). How do we build a culture where sunset is normal, not exceptional?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Feature lifecycle awareness** | % of features with current health scores | 100% |
| **Sunset evaluation rate** | Number of features evaluated for sunset per quarter | Track and trend (should match feature launch rate over time) |
| **Deprecation completion rate** | % of approved sunsets completed within timeline | > 90% |
| **Code removed** | Lines of code removed via sunset vs. added via new features | Track ratio (should approach 1:1 over time) |
| **Customer impact incidents** | Issues caused by feature removal | Near-zero (phased approach + verification) |
| **Migration success rate** | % of affected users who successfully migrate before removal | > 95% |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 12: Operations & Maintenance](operations-and-maintenance.md) | [Phase 2 Overview](../overview.md) | *(none — last phase)* |

### Related Documents
- [Full AI-Native PDLC Brief](../../../full-pdlc-ai-native-brief.md) — sunset as a new addition
