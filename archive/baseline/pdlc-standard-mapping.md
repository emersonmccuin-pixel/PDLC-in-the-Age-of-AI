# Standard PDLC (Product Development Life Cycle) Mapping

## Purpose

Reference document mapping the traditional PDLC as it exists in most mid-to-large software organizations today. This is the baseline — the "before" picture — against which we measure AI's actual impact on delivery speed.

## The Core Problem

AI has collapsed **build time** from weeks to hours/days. But build time was never the whole cycle. The PDLC is a pipeline, and the throughput is limited by the slowest stage. Right now, those slow stages are *human-dependent, sequential, and largely unchanged by AI*.

---

## PDLC Phases

### Phase 1: Ideation & Discovery

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | Problem identification, user research, market analysis, stakeholder alignment |
| **Inputs**       | Customer feedback, analytics, business strategy, support tickets |
| **Outputs**      | Problem statements, opportunity briefs, initial scope        |
| **Key Roles**    | Product Manager, UX Researcher, Business Stakeholders        |
| **Typical Time** | 1-4 weeks                                                   |
| **Dependencies** | Stakeholder availability, data access                        |
| **Bottlenecks**  | Scheduling alignment meetings, consensus-building, unclear ownership |

### Phase 2: Requirements & Specification

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | PRD writing, acceptance criteria, technical feasibility, ticket creation |
| **Inputs**       | Opportunity brief, technical constraints, design system      |
| **Outputs**      | PRD, Jira epics/stories, wireframes, technical design docs   |
| **Key Roles**    | Product Manager, Tech Lead, Designer, Engineering Manager    |
| **Typical Time** | 1-3 weeks                                                   |
| **Dependencies** | Completed discovery, design availability, architecture review |
| **Bottlenecks**  | PRD review cycles, requirement ambiguity, ticket review/commenting (can take DAYS just for PM review) |

### Phase 3: Design

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | UI/UX design, prototyping, design review, accessibility audit |
| **Inputs**       | PRD, wireframes, design system, brand guidelines             |
| **Outputs**      | High-fidelity mockups, interactive prototypes, design specs  |
| **Key Roles**    | UX Designer, UI Designer, Product Manager, Frontend Engineer |
| **Typical Time** | 1-3 weeks                                                   |
| **Dependencies** | Finalized requirements, design system availability           |
| **Bottlenecks**  | Design review loops, stakeholder feedback cycles, pixel-level debates |

### Phase 4: Implementation (Build)

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | Coding, unit testing, local integration, documentation       |
| **Inputs**       | Design specs, technical design docs, Jira tickets            |
| **Outputs**      | Feature branches, passing unit tests, draft PRs              |
| **Key Roles**    | Software Engineers, Tech Lead                                |
| **Typical Time** | **1-5 days** (with AI-assisted development)                  |
| **Dependencies** | Clear requirements, environment access, API contracts        |
| **Bottlenecks**  | Ambiguous tickets, blocked dependencies, environment issues  |

### Phase 5: Code Review

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | PR review, feedback cycles, revision, re-review, approval   |
| **Inputs**       | Pull requests, coding standards, architecture guidelines     |
| **Outputs**      | Approved PRs, merged code                                   |
| **Key Roles**    | Peer Engineers (2-3 reviewers), Tech Lead                    |
| **Typical Time** | 3-14 days (often **WEEKS**)                                  |
| **Dependencies** | Reviewer availability, reviewer context/familiarity          |
| **Bottlenecks**  | Reviewer bandwidth, context switching costs, sequential review gates (must get N approvals), review-revise-re-review loops, reviewers juggling their own sprint work |

### Phase 6: QA & Testing

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | Functional testing, regression testing, edge case testing, bug filing, fix verification |
| **Inputs**       | Merged code, test plans, acceptance criteria                 |
| **Outputs**      | QA signoff, bug reports, test results                        |
| **Key Roles**    | QA Engineers, Product Manager (UAT)                          |
| **Typical Time** | 1-3 weeks                                                   |
| **Dependencies** | Stable test environment, complete acceptance criteria        |
| **Bottlenecks**  | QA team bandwidth, environment instability, unclear acceptance criteria, bug-fix-retest cycles |

### Phase 7: Staging & Pre-Production

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | Deploy to staging/dev, integration testing, performance testing, stakeholder demo |
| **Inputs**       | QA-approved code, deployment scripts, environment config     |
| **Outputs**      | Staging signoff, release candidate                           |
| **Key Roles**    | DevOps, Engineering Manager, Product Manager, Stakeholders   |
| **Typical Time** | 1-3 weeks                                                   |
| **Dependencies** | Environment availability, stakeholder scheduling             |
| **Bottlenecks**  | Environment contention, deployment queue, "let it bake" culture with no defined criteria, stakeholder availability for signoff |

### Phase 8: Production Release

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | Production deploy, monitoring, rollback readiness, release notes |
| **Inputs**       | Release candidate, deployment plan, rollback plan            |
| **Outputs**      | Live feature, monitoring dashboards, release documentation   |
| **Key Roles**    | DevOps, On-call Engineer, Engineering Manager                |
| **Typical Time** | 1-3 days                                                    |
| **Dependencies** | Release window, change management approval                   |
| **Bottlenecks**  | Release train scheduling, change advisory board, deploy freezes |

### Phase 9: Post-Release & Feedback

| Attribute        | Detail                                                       |
| ---------------- | ------------------------------------------------------------ |
| **Activities**   | Monitoring, user feedback collection, metrics analysis, iteration planning |
| **Inputs**       | Production metrics, user feedback, error logs                |
| **Outputs**      | Iteration backlog, incident reports, retrospective notes     |
| **Key Roles**    | Product Manager, Engineering, Support                        |
| **Typical Time** | Ongoing                                                      |
| **Dependencies** | Instrumentation, feedback channels                           |
| **Bottlenecks**  | Feedback loop speed, prioritization against new work         |

---

## Time Distribution (Typical Feature, Pre-AI vs. Post-AI-Assisted Dev)

### Traditional (Pre-AI)

| Phase                | Time       | % of Total |
| -------------------- | ---------- | ---------- |
| Ideation & Discovery | 2 weeks    | 13%        |
| Requirements & Spec  | 2 weeks    | 13%        |
| Design               | 2 weeks    | 13%        |
| **Implementation**   | **3 weeks**| **20%**    |
| Code Review          | 1.5 weeks  | 10%        |
| QA & Testing         | 2 weeks    | 13%        |
| Staging              | 1.5 weeks  | 10%        |
| Release              | 0.5 weeks  | 3%         |
| **TOTAL**            | **~15 weeks** | **100%** |

### Current Reality (AI-Assisted Build Only)

| Phase                | Time          | % of Total | Change     |
| -------------------- | ------------- | ---------- | ---------- |
| Ideation & Discovery | 2 weeks       | 17%        | No change  |
| Requirements & Spec  | 2 weeks       | 17%        | No change  |
| Design               | 1.5 weeks     | 13%        | Slight     |
| **Implementation**   | **3-5 days**  | **5%**     | **-80%**   |
| Code Review          | 1.5-3 weeks   | 20%        | **Worse*** |
| QA & Testing         | 2 weeks       | 17%        | No change  |
| Staging              | 1.5 weeks     | 13%        | No change  |
| Release              | 0.5 weeks     | 4%         | No change  |
| **TOTAL**            | **~12 weeks** | **100%**   | **-20%**   |

> *Code review often gets WORSE because: (1) AI-generated code produces larger PRs faster, (2) reviewers now have even less context on code they didn't write, (3) the same human reviewers are bottlenecked on their own AI-accelerated output too.

**The punchline: AI cut build time by 80% but total delivery time only improved ~20%.** Implementation went from being a major phase to a trivial one, but the pipeline didn't adapt.

---

## Key Observations

1. **Build is no longer the bottleneck.** It's now ~5% of total cycle time. Optimizing it further has diminishing returns.

2. **The new bottlenecks are human-gated, sequential processes:** code review, QA, stakeholder signoff, deployment queues.

3. **AI made one bottleneck worse:** Code review. More code, faster, with less human authorship context means reviews take longer and are harder.

4. **Most phases are "waiting for humans" problems**, not "doing work" problems. Calendar availability, reviewer bandwidth, queue position.

5. **The PDLC was designed for a world where build was the slow part.** All the gates, checkpoints, and review processes exist because changing code was expensive. That assumption is now wrong.

6. **Sequential gates compound delay.** Each phase waits for the previous one. A 3-day delay in code review + 5-day delay in QA + 3-day delay in staging = 11 days of pure wait time, even if the actual work at each stage is hours.

---

## Questions for the Group

- Which phases could run in parallel instead of sequentially?
- Where can AI participate as a reviewer/tester/gatekeeper instead of just a builder?
- What review/QA work is genuinely about quality vs. organizational CYA?
- If build is near-instant, should the feedback loop tighten to "ship smaller, ship constantly"?
- What new risks does AI-speed development introduce that the current PDLC doesn't account for?
- Can we separate "quality gates" from "calendar gates"?

---

## Navigation

- [Back to Plan](../plan.md)
- [Phase 1 Summary](../planning/phase-1/summary.md)

### Related Baseline Documents
- [Mermaid Charts](pdlc-mermaid-charts.md) — visual representations of this mapping
- [AI Impact Analysis](ai-impact-analysis.md) — where AI helps and hurts across these phases

### Where This Feeds Forward
- [Phase 2 Overview](../planning/phase-2/overview.md) — solution scaffolds built on this baseline
- [Full AI-Native PDLC Brief](../full-pdlc-ai-native-brief.md) — expanded from 9 to 14 phases based on gaps found here
