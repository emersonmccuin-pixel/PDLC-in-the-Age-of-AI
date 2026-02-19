# Phase 2: Solution Scaffolding — Overview

## Purpose

This document maps the full 14-phase AI-native PDLC, organized by domain. It identifies handoff points between domains, cross-cutting themes that span multiple phases, and open questions that the working group needs to resolve.

---

## Phase Index

### Strategy & Planning
- [Phase 0: Portfolio Prioritization](strategy-and-planning/portfolio-prioritization.md)
- [Phase 1: Ideation & Discovery](strategy-and-planning/ideation-and-discovery.md)
- [Phase 2: Requirements & Specification](strategy-and-planning/requirements-and-specification.md)

### Build & Deliver
- [Phase 3: Design](build-and-deliver/design.md)
- [Phase 4: Implementation](build-and-deliver/implementation.md)
- [Phase 5: Code Review](build-and-deliver/code-review.md)
- [Phase 6: QA & Testing](build-and-deliver/qa-and-testing.md)
- [Phase 7: Staging](build-and-deliver/staging.md)
- [Phase 8: Release](build-and-deliver/release.md)

### Go-to-Market & Adoption
- [Phase 9: Go-to-Market](go-to-market-and-adoption/go-to-market.md)
- [Phase 11: Customer Success & Adoption](go-to-market-and-adoption/customer-success.md)

### Operate & Evolve
- [Phase 10: Monitoring & Feedback](operate-and-evolve/monitoring-and-feedback.md)
- [Phase 12: Operations & Maintenance](operate-and-evolve/operations-and-maintenance.md)
- [Phase 13: Sunset & Deprecation](operate-and-evolve/sunset-and-deprecation.md)

---

## Domain Map

```
+----------------------------------------------------------------------+
|                                                                      |
|  STRATEGY & PLANNING          OWNER: Product / Leadership            |
|  +----------------------------------------------------------+       |
|  | Phase 0: Portfolio    Phase 1: Ideation   Phase 2: Reqs  |       |
|  | Prioritization        & Discovery         & Specification |       |
|  +-----------------------------+---+-------------------------+       |
|                                |   |                                 |
|                   Handoff A    v   v    Handoff B                    |
|                   (scope +     |   |    (PRD + test specs            |
|                    priority)   |   |     + GTM brief seed)           |
|                                |   |                                 |
|  BUILD & DELIVER              OWNER: Engineering                     |
|  +----------------------------------------------------------+       |
|  | Phase 3   Phase 4   Phase 5   Phase 6   Phase 7  Phase 8 |       |
|  | Design    Build     Review    QA/Test   Staging  Release  |       |
|  +-----+---------+---------+---------+--------+------+------+       |
|        |         |         |         |        |      |               |
|        |    Handoff C      |    Handoff D     | Handoff E            |
|        |    (GTM brief     |    (release       | (live feature       |
|        |     grows)        |     candidate)    |  + GTM materials)   |
|        v                   v                   v                     |
|  GO-TO-MARKET & ADOPTION      OWNER: Marketing / Sales / CS         |
|  +----------------------------------------------------------+       |
|  | Phase 9: Go-to-Market       Phase 11: Customer Success    |       |
|  +---+------------------------------------------------------+       |
|      |                                                               |
|      |  Handoff F (adoption data, feature health, user feedback)     |
|      v                                                               |
|  OPERATE & EVOLVE             OWNER: DevOps / SRE / Engineering     |
|  +----------------------------------------------------------+       |
|  | Phase 10: Monitoring  Phase 12: Ops &    Phase 13: Sunset |       |
|  | & Feedback            Maintenance        & Deprecation    |       |
|  +---------------------------+-------------------------------+       |
|                              |                                       |
|                              | Feedback loop back to Phase 0         |
|                              v                                       |
|                    STRATEGY & PLANNING (next cycle)                   |
|                                                                      |
+----------------------------------------------------------------------+
```

### Handoff Points

| Handoff | From | To | What Transfers |
|---------|------|----|----------------|
| A | Strategy & Planning | Build & Deliver | Prioritized scope, problem definition, success criteria |
| B | Strategy & Planning | Build & Deliver | PRD, acceptance criteria, test specs, GTM brief seed |
| C | Build & Deliver | Go-to-Market | Running GTM brief (value props, design decisions, screenshots, API docs) |
| D | Build & Deliver | Operate & Evolve | Release candidate, deployment config, monitoring thresholds |
| E | Build & Deliver | Go-to-Market | Live feature, finalized GTM materials, support documentation |
| F | Operate & Evolve | Strategy & Planning | Adoption data, feature health, user feedback, sunset candidates |

**Key shift:** In the traditional PDLC, handoffs are documents thrown over a wall. In the AI-native PDLC, handoffs are living artifacts that AI maintains continuously. The GTM brief doesn't get "handed off" — it accumulates content from Phase 2 through Phase 8 and is ready for human review at launch.

---

## Cross-Cutting Themes

### 1. Shift-Left Everything (Especially Testing)

**Applies to:** Phases 1-6

Testing is not a phase. It's a continuous activity that starts at ideation.

| Phase | What Gets Shifted Left |
|-------|----------------------|
| Phase 1 (Ideation) | Initial acceptance criteria and testable hypotheses |
| Phase 2 (Requirements) | Test specifications from acceptance criteria — if you can't write a test spec, the requirement isn't clear |
| Phase 3 (Design) | UI test specs, visual regression baselines, accessibility criteria |
| Phase 4 (Build) | Full test suite written FIRST, code passes tests |
| Phase 5 (Review) | AI pre-review handles style/security/convention checks before humans |
| Phase 6 (QA) | 80% of testing already done — human QA focuses on judgment and feel |

**Implication:** The "QA phase" shrinks dramatically. QA engineers shift from test execution to test strategy and exploratory testing.

### 2. Parallel Documentation & Marketing Stream

**Applies to:** Phases 2-4, 9, 11

Documentation and marketing materials are side effects of the development process, not post-hoc activities.

| Phase | What Gets Captured |
|-------|-------------------|
| Phase 2 (Requirements) | Value propositions, problem/solution framing, key decisions |
| Phase 3 (Design) | UX rationale, screenshots, design decisions |
| Phase 4 (Build) | Technical decisions, API docs, architecture notes, changelog entries |
| Phase 9 (GTM) | AI drafts all materials from accumulated brief — humans refine, not write |
| Phase 11 (Customer Success) | Onboarding content generated from GTM brief |

**Implication:** Marketing doesn't wait for engineering to finish. The GTM brief is a living document that's ready for human review before the code ships.

### 3. Criteria Gates Replace Calendar Gates

**Applies to:** Phases 5-8

The traditional PDLC uses time as a proxy for quality: "let it bake for a week," "wait for 3 reviewer approvals," "schedule a stakeholder demo." These are calendar gates — they measure time passed, not quality achieved.

| Traditional Gate | AI-Native Replacement |
|-----------------|----------------------|
| "3 reviewer approvals" | AI pre-review + 1 human for standard changes, deeper review for high-risk |
| "QA signs off after 2 weeks" | Defined test coverage thresholds + AI exploratory testing pass |
| "Bake in staging for a week" | 15-30 health metrics green for defined threshold |
| "Stakeholder demo meeting" | AI-generated walkthrough for async review; sync meeting for complex/controversial only |
| "Change advisory board" | AI risk assessment + auto-approve for low-risk; human approval for high-risk |

**Implication:** Features that pass criteria can ship in hours. Features that fail criteria get flagged immediately, not after a week of waiting.

### 4. AI-Enforced Quality Gates (Democratization Governance)

**Applies to:** Phases 0, 2, 4-8, 12

When building is nearly free, everyone builds. Without governance, you get shadow IT on steroids. The solution: AI enforces standards regardless of who wrote the code.

| Gate | What AI Enforces |
|------|-----------------|
| Build time | Code follows project conventions, patterns, data access rules |
| Review time | Convention violations, architectural drift, duplicate functionality, missing tests |
| Merge time | Intent-aware CI/CD — "this duplicates module X" or "this doesn't follow our data access pattern" |
| Portfolio time | "Should this exist?" — AI flags overlap with existing tools, orphan risk |

**Implication:** You don't restrict who can build. You restrict what can ship. The AI-enforced pipeline is the new governance mechanism.

---

## Open Questions

These come from the brief and from Phase 1 analysis. The working group needs to discuss and take positions.

### Trust & Autonomy

1. **At each phase, what level of AI autonomy is appropriate?** Where must a human approve before proceeding vs. where can AI auto-advance?
2. **Who is liable when AI-reviewed code causes a production incident?** If AI pre-review "approved" a change and a human rubber-stamped it, who owns the failure?
3. **How do we prevent AI review from becoming a rubber stamp?** If humans trust AI review too much, we lose the judgment layer.

### Tooling Reality

4. **Which AI capabilities exist today vs. require custom tooling vs. are aspirational?** We need a reality check on each phase's proposals.
5. **What's the integration cost?** Most organizations have 5-15 tools in their PDLC. Adding AI to each one is a real engineering effort.
6. **Build vs. buy?** For each AI capability, is this a commercial product, an open-source tool, or custom development?

### Organizational Resistance

7. **Which phases will face the most pushback?** Code review (senior devs), QA (QA team existential threat), Portfolio (executive prerogative).
8. **What's the adoption strategy?** Big bang vs. phase-by-phase? Top-down mandate vs. bottom-up adoption?
9. **How do we handle the "AI took my job" fear?** Role evolution messaging is critical — especially for QA, technical writers, and junior developers.

### Metrics

10. **How do we measure whether the AI-native PDLC is actually better?** Lead time? Defect rate? Developer satisfaction? Customer time-to-value?
11. **What are the leading indicators?** We can't wait 6 months to know if this is working.
12. **How do we avoid Goodhart's Law?** If we measure cycle time, people will ship fast and break things.

### Risk

13. **What new failure modes does AI autonomy create?** AI-approved bad code, AI-generated wrong test specs, AI-summarized misleading PRDs.
14. **Where does speed create risk?** Faster shipping means faster production incidents if quality isn't maintained.
15. **What's the rollback strategy?** If the AI-native process is worse for a specific phase, can we revert without disrupting everything?

### Scale

16. **Does this framework scale differently for 5-person startups vs. 500-person orgs?** Small teams may skip formal phases. Large orgs may need more gates, not fewer.
17. **How does this interact with compliance requirements?** SOC 2, HIPAA, PCI — some gates exist for regulatory reasons, not process reasons.

---

## Interdependencies Between Domains

| Dependency | Description | Risk if Broken |
|-----------|-------------|----------------|
| Strategy --> Build | Build depends on clear, prioritized scope from Strategy | Engineers build the wrong thing or build without context |
| Build --> GTM | GTM depends on the running brief that accumulates during Build | Marketing starts from scratch after launch, delaying adoption |
| Build --> Operate | Operate depends on monitoring config, health thresholds, runbooks from Build | Incidents take longer to resolve, no AI-assisted triage |
| Operate --> Strategy | Strategy depends on adoption data, feature health, and feedback from Operate | Portfolio decisions made on stale data, dead features never sunset |
| QA <--> Build | Shift-left testing means QA and Build are tightly coupled, not sequential | If decoupled, you get test-after-build again — the whole shift-left collapses |
| GTM <--> Requirements | GTM brief starts at Requirements, not at launch | Late GTM start means marketing is always playing catch-up |

---

## How to Read the Phase Docs

Each phase document in this directory follows a consistent structure:

1. **Principle** — The universal truth driving the redesign. Applies regardless of organization size, industry, or tooling.
2. **Pattern** — Platform-agnostic process design. How this phase works in an AI-native PDLC, independent of specific tools.
3. **Implementation Example** — A concrete, org-specific example showing what this looks like in practice.
4. **Current State** — What this phase looks like today (from Phase 1 baseline).
5. **What Changes** — Process, tooling, and role shifts.
6. **Open Questions & Risks** — Phase-specific uncertainties.
7. **How to Measure Success** — Metrics for this phase.

The goal is not prescriptive solutions. It's structured scaffolding — enough to discuss, debate, and prioritize with the group.

---

## Navigation

- [Back to Plan](../../plan.md)
- [Phase 1 Summary](../phase-1/summary.md)
- [Full AI-Native PDLC Brief](../../full-pdlc-ai-native-brief.md) — the input document that shaped this phase
- [Context Pipeline Architecture](../../context-pipeline-architecture.md) — the implementation challenge that emerged from these scaffolds
