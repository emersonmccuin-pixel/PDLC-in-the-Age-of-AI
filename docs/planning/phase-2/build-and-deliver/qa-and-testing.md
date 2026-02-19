# Phase 6: QA & Testing

## Principle

**QA should validate that the system works as intended, not discover whether it was tested.**

In the traditional PDLC, QA is where testing begins. In the AI-native PDLC, QA is where testing is verified. Test specs were generated in Phase 2, UI test specs in Phase 3, and the full test suite was written and passing in Phase 4. QA's job is not to execute scripts — it's to challenge assumptions, explore edges, and apply human judgment about whether the product feels right.

---

## Pattern

### From Test Execution to Test Validation

**Traditional flow:**
```
Code merged  -->  QA writes test plan  -->  QA executes tests  -->  Bugs filed  -->  Fix/retest loop
                  (days)                    (1-2 weeks)             (days)           (1-2 weeks)
```

**AI-native flow:**
```
Code merged    -->  AI runs            -->  AI exploratory    -->  Human QA         -->  QA signoff
(tests already      regression +            testing                reviews AI            (hours-1 day)
passing)            visual + perf           (edge cases,           findings +
                    (hours)                 adversarial)           UX judgment
```

### Process Design

1. **Automated Test Verification (AI, instant on merge)**
   - Full test suite from Phase 4 runs automatically on merge
   - This is verification, not discovery — tests were written first and already pass in the feature branch
   - Any failures are regressions, not expected — flagged immediately

2. **AI Regression Testing (AI, hours)**
   - AI runs the full regression suite, not just the feature's tests
   - AI identifies areas of the codebase affected by the change that aren't covered by the feature tests
   - AI generates and executes additional regression tests for affected areas
   - Visual regression testing against design specs from Phase 3

3. **AI Exploratory Testing (AI, hours)**
   - AI generates edge cases from acceptance criteria: boundary values, empty states, concurrent operations, invalid inputs
   - AI generates adversarial inputs: XSS payloads, SQL injection attempts, oversized inputs, Unicode edge cases
   - AI runs performance and load testing against defined thresholds
   - AI triages its own findings: critical vs. cosmetic vs. false positive
   - **Output:** Categorized findings report for human review

4. **Human QA Review (judgment calls, hours)**
   - QA engineer reviews AI exploratory findings — validates critical, dismisses false positives
   - Human performs UX judgment testing: Does this feel right? Is the flow intuitive? Are there scenarios the AI missed?
   - Human tests real-world workflows (not individual features in isolation): "As a user who just onboarded, how does this feature fit into their existing workflow?"
   - **Focus:** Judgment, feel, integration with the broader product experience

5. **Bug Resolution (streamlined)**
   - Bugs go through the same AI-first pipeline: fix → tests → AI self-review → merge
   - AI auto-classifies: blocker (stop release), major (fix before release), minor (fix in next sprint), cosmetic (backlog)
   - Blocker and major bugs trigger immediate fix cycle; minor and cosmetic don't block release

---

## Implementation Example

**Scenario:** Dashboard customization — all 8 PRs merged, feature complete.

**What happens:**

1. Automated verification: All 126 feature tests pass. Full regression suite (2,400 tests) passes. No regressions detected.

2. AI regression testing:
   - Identifies that dashboard state is also loaded in the mobile app's dashboard view — runs mobile regression tests
   - Visual regression: compares dashboard screenshots against Phase 3 design specs — 100% match in edit mode, minor spacing diff in view mode (2px) flagged as cosmetic

3. AI exploratory testing:
   - Edge cases: 0 widgets (empty state handled), 50 widgets (performance OK up to 30, degrades at 50 — flagged)
   - Adversarial: widget titles with XSS payloads (properly escaped), extremely long widget names (truncated correctly)
   - Performance: dashboard load time 220ms (under 500ms threshold), edit mode toggle 45ms
   - Concurrent: two browser tabs with same user editing simultaneously (last-write-wins, no data corruption)
   - **Findings:** 1 major (performance at 50+ widgets), 1 cosmetic (2px spacing), 3 false positives dismissed

4. Human QA reviews:
   - Validates 50-widget performance finding: "Unlikely in production (max 12 widgets available), but should add a limit. File as minor."
   - Cosmetic spacing: "Actually this is intentional — edit mode has slightly different padding. Dismiss."
   - UX judgment: Tests the edit mode flow as a new user. "The 'Customize' button is hard to find — it's in a dropdown menu. Should be more prominent." Files as major UX feedback.
   - Tests the admin team defaults flow: "When admin sets defaults, existing users don't see any notification. Should we tell them?" Files as enhancement for next iteration.

5. Bug resolution:
   - Major (UX — button visibility): Fixed in 2 hours, AI self-review, merged. Re-tested. Passed.
   - Minor (50-widget limit): Ticket created for next sprint.
   - Total QA time: 6 hours.

**Traditional QA for same feature: 1-3 weeks.**

---

## Current State

From Phase 1 baseline:
- **Typical time:** 1-3 weeks
- **Bottlenecks:** QA team bandwidth, environment instability, unclear acceptance criteria, bug-fix-retest cycles
- **Key roles:** QA Engineers, Product Manager (UAT)
- **AI role today:** Some AI-powered testing tools exist (visual regression, fuzz testing), but they're supplements to manual QA, not replacements for it. Most QA teams still write and execute test plans manually.

From AI impact analysis:
- QA is in "Category 2: AI could help but isn't embedded yet"
- Key question: Can AI reduce QA cycle from 2 weeks to 2 days for standard features?

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Test plan** | Written by QA after code is merged | Test specs exist since Phase 2, test suite written in Phase 4 |
| **Test execution** | Manual + automated by QA team | AI-automated: regression, visual, performance, exploratory |
| **Edge case coverage** | Whatever QA thinks of + regression suite | AI-generated from acceptance criteria + adversarial inputs |
| **Human QA focus** | Execute test scripts, file bugs | Review AI findings, UX judgment, real-world workflow testing |
| **Bug classification** | QA files, PM triages | AI auto-classifies severity, human confirms |
| **Fix-retest cycle** | Days (fix, wait for QA, retest) | Hours (fix, AI re-verify, human confirms) |
| **QA cycle time** | 1-3 weeks | Hours to 1 day |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **QA Engineer** | Writes test plans, executes tests, files bugs | Reviews AI findings, performs UX/judgment testing, defines test strategy, expands AI testing coverage |
| **QA Lead** | Manages QA team workload, tracks test execution | Configures AI testing thresholds, reviews test strategy, focuses team on high-judgment areas |
| **Product Manager** | UAT (user acceptance testing) — manually clicks through feature | Reviews AI exploratory report, validates UX judgment findings, makes release decisions |

---

## Open Questions & Risks

1. **QA team existential threat.** This framework dramatically reduces the need for manual test execution. QA engineers need to evolve into test strategists and UX judges, or the role shrinks. How do we message and manage this transition?
2. **AI exploratory testing quality.** AI-generated edge cases may be comprehensive but lack real-world creativity. A human QA tester who has seen 100 production bugs thinks differently than AI generating from specs. How do we preserve this institutional knowledge?
3. **False positive fatigue.** If AI generates many findings that humans dismiss, the review becomes a chore and real issues get missed. How do we tune AI sensitivity?
4. **UX judgment is hard to define.** "Does this feel right?" is a real QA activity but hard to systematize. How do we ensure UX judgment testing happens consistently and isn't skipped because the AI report looked clean?
5. **Environment parity.** AI testing in CI is only as good as the test environment. Staging/production differences can cause false confidence. How do we maintain environment parity?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **QA cycle time** | Hours from merge to QA signoff | < 1 day (vs. 1-3 weeks) |
| **Defects found in production** | Bugs that escaped QA | Same or fewer than current |
| **AI finding accuracy** | % of AI findings confirmed as real issues by human QA | > 70% (low false positive rate) |
| **Human QA time** | Hours of human QA attention per feature | < 4 hours (focused on judgment, not execution) |
| **Test coverage** | % of acceptance criteria with automated tests at QA entry | 100% (shift-left complete) |
| **Regression detection rate** | % of regressions caught before production | > 99% |
