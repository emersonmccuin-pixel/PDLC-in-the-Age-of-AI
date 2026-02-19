# Phase 5: Code Review

## Principle

**Human review should focus on judgment and architecture, not on things a machine can verify.**

Code review is the phase most damaged by AI-accelerated development. More code, faster, with less author context, same reviewer pool. The fix isn't more reviewers — it's restructuring what humans review. AI handles the verifiable (style, security, conventions, test coverage). Humans handle the judgment (architecture fitness, business logic correctness, long-term maintainability).

---

## Pattern

### From Sequential Human Review to Triaged AI+Human Review

**Traditional flow:**
```
PR submitted  -->  Wait for reviewer  -->  Review  -->  Feedback  -->  Revise  -->  Re-review
                   (days-weeks)          (hours)     (async)       (hours)     (more days)
```

**AI-native flow:**
```
PR submitted  -->  AI pre-review   -->  Risk triage    -->  Human review
                   (instant)            (automatic)         (focused, fast)
                                        Low-risk: 1          High-risk: deep
                                        reviewer             review, same day
```

### Process Design

1. **AI Pre-Review (instant, mandatory)**
   Before any human reviewer is notified, AI has already:
   - Verified all tests pass (from the test-first suite in Phase 4)
   - Checked for security vulnerabilities: injection, auth bypasses, data exposure
   - Validated style and convention compliance
   - Identified logic issues: dead code, unreachable branches, off-by-one patterns
   - Checked for edge cases not covered by tests
   - Summarized the PR: what it does, why, what the risk areas are
   - **Output:** Pre-review report with pass/fail on verifiable criteria + narrative summary for human reviewer

2. **Risk Triage (automatic)**
   AI classifies each PR by risk level:

   | Risk Level | Criteria | Human Review Required |
   |-----------|----------|----------------------|
   | **Low** | Refactors, style changes, boilerplate, docs, config | 1 reviewer, async |
   | **Medium** | New features with tests, standard CRUD, UI changes | 1 reviewer, same-day |
   | **High** | Auth/payments/data logic, architecture changes, API contracts, security-sensitive | 2 reviewers, prioritized |
   | **Critical** | Infrastructure, deployment config, database migrations, shared library changes | 2 reviewers + tech lead, blocking |

3. **Human Review (focused)**
   - Reviewer sees: AI summary, risk assessment, highlighted concern areas — not a raw diff
   - AI pre-review already handled: style, security scan, test coverage, convention compliance
   - Human focuses on: Does this architectural approach make sense? Is the business logic correct? Will this be maintainable? Are there design implications the AI missed?
   - Review feedback is specific and actionable — AI can suggest fixes for identified issues

4. **Revision & Re-Review (streamlined)**
   - If changes requested, AI assists with revisions
   - AI re-runs pre-review on revised code
   - If revisions are mechanical (fix style, add test), AI can auto-approve the revision
   - Human re-review only needed for substantive changes

---

## Implementation Example

**Scenario:** Dashboard customization — PR for drag-and-drop widget reordering (147 lines, 4 files).

**What happens:**

1. AI pre-review runs instantly on PR submission:
   - Tests: 30/30 passing
   - Security: No issues
   - Conventions: Passes all lint rules, follows existing component patterns
   - Logic: No dead code, no unreachable branches
   - Edge cases: "What happens if a user has 0 widgets? Test suite doesn't cover empty state." — AI adds this to review notes
   - Summary: "Adds drag-and-drop widget reordering in edit mode. Uses existing react-dnd library. State managed via useWidgetOrder hook with localStorage persistence. Low complexity, well-tested."

2. Risk triage: **Medium** (new feature with tests, UI change, no auth/data concerns)
   - 1 reviewer required, same-day turnaround

3. Human reviewer sees:
   - AI summary (reads in 30 seconds)
   - Risk assessment: Medium, 1 reviewer sufficient
   - Flagged concern: empty state not tested
   - Highlighted code sections: only the `useWidgetOrder` hook logic (the part that requires judgment)
   - Reviewer spends 15 minutes reviewing the hook logic, agrees it's sound
   - Reviewer adds: "Good point on empty state — please add a test and a graceful fallback"

4. Developer adds empty state test and fallback (10 minutes). AI re-runs pre-review, confirms fix. PR merged.

**Total review time:** 30 minutes elapsed, 15 minutes of human attention. Traditional: 3-14 days.

---

## Current State

From Phase 1 baseline:
- **Typical time:** 3-14 days (often weeks)
- **Bottlenecks:** Reviewer bandwidth, context switching, sequential approval gates (2-3 required approvals), review-revise-re-review loops
- **Key roles:** Peer Engineers (2-3 reviewers), Tech Lead
- **AI role today:** Some teams use AI review tools (GitHub Copilot review, Greptile, CodeRabbit), but they're supplements to human review, not restructuring of the review process. Required reviewer counts haven't changed.
- **The math that broke:** 1 dev now produces 3-5x the PRs. Reviewer capacity is unchanged. Queue explodes.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Pre-review** | None — reviewer reads raw diff | AI pre-review handles all verifiable checks before human is notified |
| **Reviewer context** | Read the diff, figure out what it does | AI summary, risk assessment, highlighted areas — reviewer is prepared |
| **Required reviewers** | 2-3 for all changes | Risk-triaged: 1 for low/medium, 2+ for high/critical |
| **Review focus** | Everything: style, security, logic, architecture | Humans focus on judgment; AI already handled the mechanical |
| **Time to first review** | Days to weeks (queue + context switching) | Hours (AI pre-review is instant, human review is smaller and focused) |
| **Revision cycle** | Revise, wait for re-review (days) | Mechanical fixes auto-approved by AI, only substantive changes need human re-review |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Peer Reviewer** | Reviews everything: style, security, logic, architecture. 2-3 required per PR. | Reviews architecture and judgment calls. 1 required for standard changes. |
| **Tech Lead** | Reviews high-risk PRs when flagged by team | Automatically notified for Critical-risk PRs. Reviews architecture decisions. |
| **Junior Engineer** | Gets PRs reviewed heavily, waits a long time | Gets AI pre-review feedback instantly, human review focused on mentoring and judgment |
| **AI** | Optional supplement | Mandatory first reviewer with structured output |

---

## Open Questions & Risks

1. **Rubber-stamp risk.** If AI pre-review says "everything looks good," will human reviewers just approve without reading? The point is focused review, not no review.
2. **AI blind spots.** AI is good at pattern matching (security, style, conventions) but weak at understanding business intent. A PR that's technically correct but architecturally wrong will pass AI review. How do we ensure humans catch these?
3. **Reviewer skill atrophy.** If humans only review architecture decisions, do they lose the ability to catch low-level bugs? Does this matter if AI catches them reliably?
4. **Trust calibration.** Risk triage must be tuned per organization. A payment company's "low risk" is different from a social media company's. How do we calibrate?
5. **Review as mentoring.** Code review is a major learning mechanism for junior engineers. If AI handles most review, how do juniors learn? (Possible answer: AI explains why it flagged issues, becoming a teaching tool.)
6. **Organizational politics.** "Only 1 reviewer?! That's not safe!" Reducing required reviewers will face pushback from risk-averse engineering leadership. What's the evidence-based argument?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Time to first human review** | Hours from PR submission to first human feedback | < 4 hours (vs. 3-14 days) |
| **Human review time per PR** | Minutes of focused human attention per PR | < 30 minutes (vs. 1-2 hours) |
| **Review queue depth** | Number of PRs waiting for human review | < 3 per reviewer (vs. 10+) |
| **Defect escape rate** | Bugs found in QA/production that should have been caught in review | Same or better than current (AI + human >= human alone) |
| **False positive rate** | % of AI-flagged issues that reviewers dismiss as non-issues | < 20% (AI review must be useful, not noisy) |
| **PR cycle time** | Hours from PR submission to merge | < 8 hours for low/medium risk (vs. days-weeks) |
