# Phase 4: Implementation (Build)

## Principle

**Code should be written to pass tests, not the other way around. Implementation is execution against a contract, not exploration.**

Build is already the fastest phase — AI collapsed it from weeks to days. The remaining gains aren't about making coding faster. They're about making the build produce higher-quality output from the start: tests written first, self-review before human review, documentation generated alongside code, and continuous small PRs instead of big-bang merges.

---

## Pattern

### From Code-Then-Test to Test-Then-Code

**Traditional flow (even with AI):**
```
Write code  -->  Write tests  -->  Fix failures  -->  Submit PR  -->  Wait for review
(hours-days)     (after, maybe)    (rework)           (big batch)     (days-weeks)
```

**AI-native flow:**
```
AI writes tests    -->  AI writes code   -->  AI self-reviews   -->  Small PR
from test specs         to pass tests         (security, style,     submitted
(hours)                 (hours)               performance)          continuously
```

### Process Design

1. **Test Suite Generation (AI, first)**
   - AI generates the full test suite from test specifications produced in [Phase 2](02-requirements-and-specification.md) and [Phase 3](03-design.md)
   - Unit tests, integration tests, component tests — all written before implementation code
   - Tests initially fail (red). Implementation makes them pass (green).
   - AI identifies missing test specs: "acceptance criterion 4 has no corresponding test spec — should I generate one or flag for PM?"
   - **This is test-driven development enforced by process, not developer discipline**

2. **Implementation (AI-driven, human-directed)**
   - AI writes code to pass the test suite
   - AI runs tests continuously — every change is validated immediately
   - AI follows project conventions by default: coding style, architectural patterns, data access patterns
   - AI breaks work into logical, independently-shippable units — small PRs, not monoliths
   - Developer directs, reviews AI output, and handles judgment calls (architecture decisions, performance tradeoffs, ambiguous requirements)

3. **AI Self-Review (automatic, before any human sees the code)**
   - Security scan: injection risks, auth bypasses, data exposure
   - Style compliance: linting, formatting, convention adherence
   - Performance analysis: N+1 queries, unnecessary re-renders, memory leaks
   - Dependency audit: new dependencies justified? version conflicts?
   - Test coverage check: all acceptance criteria covered?
   - **Output:** Self-review report attached to each PR

4. **Continuous Small PRs**
   - AI breaks features into independently-reviewable, independently-deployable units
   - Each PR is small enough to review in 15-30 minutes
   - PR includes: code changes, passing tests, AI self-review report, summary of what and why
   - PRs submitted continuously as units are complete — not batched at the end

5. **Documentation Generation (parallel, automatic)**
   - API docs generated from code as it's written
   - User-facing help text drafted from acceptance criteria
   - Changelog entries from PR descriptions
   - Migration guides if the change affects existing users
   - **GTM brief update:** Technical decisions, architecture choices, integration notes added automatically

---

## Implementation Example

**Scenario:** Dashboard customization — implementing drag-and-drop widget reordering (Story 2 of 8).

**What happens:**

1. AI generates test suite from Phase 2-3 test specs:
   - Unit tests for widget order state management (12 tests)
   - Integration tests for drag-and-drop behavior (8 tests)
   - Component tests for edit mode toggle (6 tests)
   - Accessibility tests for keyboard reordering (4 tests)
   - All 30 tests fail (no implementation yet)

2. AI implements:
   - Adds `react-dnd` for drag-and-drop (AI checked: already in project dependencies)
   - Creates `useWidgetOrder` hook for state management
   - Implements `EditMode` component wrapper
   - Tests go green progressively — AI runs suite after each logical change

3. AI self-review runs:
   - Security: No issues (no user input handled, no API calls)
   - Style: 2 lint warnings fixed automatically
   - Performance: "Widget order state updates trigger full dashboard re-render — recommend memoizing widget components" — AI fixes this
   - Dependencies: No new dependencies added (react-dnd already present)
   - Coverage: 30/30 tests passing, all acceptance criteria covered

4. PR submitted:
   - 147 lines changed across 4 files
   - All tests passing
   - Self-review report attached: "Low-risk change. No security concerns. Performance optimized. Recommend 1 human reviewer."
   - Summary: "Implements drag-and-drop widget reordering in edit mode. Keyboard accessible. State persists to localStorage (API persistence in Story 4)."

**Total time for this story:** 3 hours. One of 8 stories — all submitted as independent PRs over 1-2 days.

---

## Current State

From Phase 1 baseline:
- **Typical time:** 1-5 days with AI assistance (already collapsed from 2-4 weeks)
- **Bottlenecks:** Ambiguous tickets, blocked dependencies, environment issues
- **Key roles:** Software Engineers, Tech Lead
- **AI role today:** AI is heavily used for coding (Copilot, Claude Code, Cursor). But test-first workflows, AI self-review, and continuous small PRs are not standard practice. Most teams still write tests after code and submit large PRs.

---

## What Changes

| Dimension | Traditional (even with AI) | AI-Native |
|-----------|---------------------------|-----------|
| **Test timing** | After code, if at all | Before code — tests are the contract |
| **PR size** | Large, batched at end of feature | Small, continuous, independently reviewable |
| **Self-review** | Developer reads own code | AI scans for security, style, performance, coverage before any human |
| **Documentation** | Retrofitted after build, if ever | Generated alongside code automatically |
| **Convention adherence** | Developer knowledge + linting | AI writes to conventions by default — the builder doesn't need to know all conventions |
| **Reviewer preparation** | Reviewer reads raw diff | Reviewer gets summary, risk assessment, self-review report |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Software Engineer** | Writes code, writes tests (maybe), submits PR, waits | Directs AI, reviews AI output, handles architecture/judgment calls, submits continuous small PRs |
| **Tech Lead** | Reviews PRs for architecture compliance | Reviews AI self-review reports, intervenes on high-risk changes, focuses on system design |
| **Junior Engineer** | Writes simple code, learns by doing | Directs AI on simple tasks, learns by reviewing AI output and understanding why it made specific decisions |

---

## Open Questions & Risks

1. **Test-first discipline.** TDD has been advocated for decades and rarely adopted consistently. Can process enforcement (AI generates tests first, refuses to implement without specs) succeed where developer discipline failed?
2. **AI self-review reliability.** If AI self-review misses a security issue, the false confidence is worse than no self-review. How do we calibrate trust in AI self-review output?
3. **Small PR overhead.** More PRs means more review cycles. Does the reduction in per-PR review time outweigh the increase in PR count? (Hypothesis: yes, because each review is 15 minutes instead of 2 hours.)
4. **Developer skill atrophy.** If AI writes the code and the tests, what happens to developer skills over time? Is "directing AI" a sufficient skill for career growth?
5. **Convention enforcement for non-engineers.** The democratization angle: when non-engineers use AI to build, AI-enforced conventions ensure output quality. But how do we handle edge cases where conventions don't exist yet?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Test-first compliance** | % of features where tests are written before implementation | > 90% |
| **PR size** | Average lines changed per PR | < 200 (vs. 500-1000 today) |
| **Self-review catch rate** | % of issues caught by AI self-review that would have been found in human review | > 60% |
| **Build-to-review time** | Time from PR submission to first human review | < 4 hours (small PRs get reviewed faster) |
| **Documentation coverage** | % of new code with auto-generated docs | > 90% |
| **Defect escape rate** | % of bugs found post-merge that should have been caught during build | Decrease by 40%+ |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 3: Design](03-design.md) | [Phase 2 Overview](overview.md) | [Phase 5: Code Review](05-code-review.md) |

### Related Documents
