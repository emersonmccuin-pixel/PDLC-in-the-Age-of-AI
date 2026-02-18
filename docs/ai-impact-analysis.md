# AI Impact on the PDLC: Where It Helps, Where It Doesn't, Where It Makes Things Worse

## Purpose

Honest assessment of where AI actually changes the PDLC today, where it could but doesn't yet, and where it's actively creating new problems. For discussion with the Pittsburgh working group.

---

## Category 1: AI Has Already Collapsed This Phase

### Implementation / Build
- **Before:** 2-4 weeks for a mid-complexity feature
- **After:** 1-5 days
- **How:** AI coding assistants (Copilot, Claude Code, Cursor) generate code, tests, boilerplate at 5-10x human speed
- **But:** The speed gain is largely absorbed by downstream bottlenecks

### Unit Test Writing
- **Before:** Often skipped or minimal due to time pressure
- **After:** Generated alongside code, near-instant
- **But:** Quality of AI-generated tests varies; can create false confidence

### Boilerplate & Scaffolding
- **Before:** Hours of setup, config, ceremony
- **After:** Minutes
- **Status:** good, but needs refinement 

---

## Category 2: AI Could Help But Isn't Embedded Yet

### Code Review
- **Current state:** 100% human, sequential, calendar-dependent
- **AI opportunity:**
  - AI pre-review catches style, bugs, security issues before human eyes touch it
  - AI summarizes PR intent and risk areas so reviewers focus on what matters
  - AI flags "this is a low-risk refactor" vs. "this touches payment logic" to triage reviewer effort
  - Reduce required human reviewers from 3 to 1 + AI
- **Blockers:** Trust, liability, organizational habit, "we've always had 3 reviewers"
- **Key question:** What % of code review comments are things AI could catch? (Hypothesis: 60-80%)

### QA & Testing
- **Current state:** Manual test execution, human-written test plans, human judgment
- **AI opportunity:**
  - AI generates test plans from acceptance criteria
  - AI executes exploratory testing based on code changes
  - AI identifies regression risk areas from diff analysis
  - AI-powered visual regression testing
  - AI triages bug severity
- **Blockers:** QA teams feel threatened, tooling immaturity, edge case coverage concerns
- **Key question:** Can AI reduce QA cycle from 2 weeks to 2 days for standard features?

### Requirements & Spec
- **Current state:** PMs write PRDs manually, engineers interpret them
- **AI opportunity:**
  - AI drafts PRDs from rough notes/conversations
  - AI identifies ambiguities and gaps in requirements before development starts
  - AI generates acceptance criteria from user stories
  - AI cross-references new requirements against existing system behavior
- **Blockers:** PMs worried about relevance, requirement quality is hard to evaluate programmatically
- **Key question:** Can AI turn a 30-minute PM conversation into a production-ready PRD?

### Staging & Deployment
- **Current state:** Manual deploys, human monitoring, "let it bake" with no criteria
- **AI opportunity:**
  - AI-driven canary analysis (is this deploy healthy?)
  - Auto-rollback on anomaly detection
  - AI replaces "bake for a week" with "monitor these 15 metrics for 2 hours"
  - AI-generated release notes from commit history
- **Blockers:** Risk aversion, ops team trust, regulatory requirements in some industries

---

## Category 3: AI Makes This Actively Worse

### Code Review Burden
- **Problem:** AI-assisted devs produce more code, faster. Same number of human reviewers.
- **Result:** PR queue grows. Each PR is larger. Reviewers have less context because they didn't write the code. Reviews take longer and are less thorough.
- **The math:** 1 dev now produces 3-5x the PRs. Reviewer capacity unchanged. Queue explodes.

### Knowledge Distribution
- **Problem:** When AI writes the code, fewer humans deeply understand it.
- **Result:** Reviewer understanding is shallower. Onboarding new devs is harder. Bus factor increases.
- **The irony:** AI makes individuals faster but can make teams more fragile.

### Quality Confidence
- **Problem:** AI-generated code passes tests but may have subtle logic issues that humans would have caught during the slower, more deliberate manual coding process.
- **Result:** QA catches things later (more expensive) or doesn't catch them at all.

### Process Pressure
- **Problem:** "You built it in a day, why isn't it shipped?" Management sees build speed, not pipeline speed.
- **Result:** Developers feel squeezed between AI-speed expectations and human-speed processes. Morale impact.

---

## The Fundamental Mismatch

```
+----------------------------------------------------------------+
|                                                                |
|  AI capability growth:     Exponential                         |
|  Human process adaptation: Linear (at best)                    |
|  Organizational change:    Glacial                             |
|                                                                |
|  The gap between these rates IS the problem.                   |
|                                                                |
+----------------------------------------------------------------+
```

The PDLC was designed around these assumptions:
1. **Code is expensive to write** --> Heavy upfront planning to avoid rework
2. **Code is expensive to change** --> Extensive review to catch mistakes pre-merge
3. **Deploys are risky** --> Long bake periods, manual QA, staged rollouts
4. **Build is the bottleneck** --> All process optimizations focused on developer throughput

AI has invalidated assumption #1 and largely #2. But the processes built on those assumptions remain intact.

---

## Where This Group Should Focus

### High-Impact, Achievable (Start Here)
1. **AI-assisted code review** — Reduce human reviewer count, add AI pre-review gate
2. **AI-generated test plans** — Accelerate QA kickoff, reduce ambiguity
3. **Defined "bake" criteria** — Replace time-based staging with metrics-based staging
4. **Smaller, more frequent PRs** — If build is cheap, ship incrementally

### High-Impact, Hard (Requires Organizational Change)
5. **Parallel workflows** — Run QA, review, and staging concurrently where possible
6. **AI as a first-class PDLC participant** — Not just a tool for devs, but an actor in review/QA/ops
7. **Continuous deployment with AI monitoring** — Ship on merge, AI watches for problems
8. **Requirements-to-deploy pipeline** — AI maintains traceability from requirement through production

### Research Questions (Need Data)
9. What % of code review feedback is style/format vs. logic/architecture? (AI can handle the former)
10. What's the actual defect rate of AI-assisted code vs. human-only code?
11. How much QA time is regression testing vs. new feature testing? (Regression can be automated)
12. What's the cost of a 1-week delay in shipping vs. the cost of a production bug?

---

## For Discussion

The current PDLC treats AI as a **power tool for developers** — like giving a carpenter a nail gun instead of a hammer. The house gets framed faster, but the permits, inspections, and closing process take exactly as long.

The real question isn't "how do we make developers faster?" — that's solved.

The question is: **"How do we redesign the entire process for a world where building is nearly free?"**

That's a fundamentally different question, and it requires input from everyone in the chain — not just developers.
