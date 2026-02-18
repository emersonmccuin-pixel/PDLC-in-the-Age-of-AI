# Build Plan

## Project Arc

```
Map the Problem --> Scaffold Solutions --> Group Input --> Deepen Solutions --> Prototype Systems --> Test & Iterate
```

This isn't an app build. Each phase produces thinking artifacts, frameworks, and eventually process designs and tooling that address PDLC bottlenecks in an AI-native world.

---

## Phase 1 — Problem Mapping (COMPLETE)

**Goal:** Establish a shared, honest baseline of the current PDLC and where AI actually impacts it.

**Artifacts produced:**
- [x] `docs/pdlc-standard-mapping.md` — 9-phase PDLC with roles, times, bottlenecks
- [x] `docs/pdlc-mermaid-charts.md` — 5 visual charts (flow, gantt, pie, compression, parallel)
- [x] `docs/ai-impact-analysis.md` — 3-category impact analysis + prioritized focus areas

**Key findings:**
- Build went from 20% of cycle time to 5%. Total delivery only improved ~20%.
- Code review got *worse* — more code, less context, same reviewers.
- 45% of total cycle time is waiting for humans (queues, scheduling, availability).
- The PDLC was designed for a world where code was expensive. That assumption is now wrong.

---

## Phase 2 — Solution Scaffolding

**Goal:** For each identified bottleneck, outline a concrete solution approach. Not fully fleshed out — just enough structure to discuss with the group and prioritize.

### Tasks
- [ ] Scaffold solution: AI-assisted code review (reduce reviewers, add AI pre-review gate)
- [ ] Scaffold solution: AI-generated test plans & QA acceleration
- [ ] Scaffold solution: Metrics-based staging (replace time-based "bake")
- [ ] Scaffold solution: Incremental delivery (smaller PRs, continuous flow)
- [ ] Scaffold solution: Parallel workflows (concurrent review + QA + staging)
- [ ] Scaffold solution: AI as PDLC participant (reviewer, tester, monitor — not just builder)
- [ ] Create solution overview document linking all scaffolds
- [ ] Identify which solutions are independent vs. interdependent

**Output:** `docs/planning/phase-2/` with one scaffold doc per solution area, plus an overview.

**Each scaffold should include:**
- Problem statement (from Phase 1 findings)
- Proposed approach
- What changes (process, tooling, roles)
- Prerequisites and dependencies
- Risks and open questions
- How to measure success

---

## Phase 3 — Group Session Prep

**Goal:** Prepare materials for the Pittsburgh working group's first session. Make the problem visceral, the solutions debatable, and the session productive.

### Tasks
- [ ] Create discussion guide / agenda for first group session
- [ ] Build "current state vs. future state" framework for collaborative input
- [ ] Prepare provocative prompts / debate topics from the solution scaffolds
- [ ] Create a prioritization exercise (what do we tackle first?)
- [ ] Package materials for sharing (GitHub + HackMD or equivalent)

**Output:** `docs/planning/phase-3/` with session materials.

---

## Phase 4 — Solution Deep Dives

**Goal:** Take the top-priority solutions (based on group input) and flesh them out into detailed designs. Each solution gets a full treatment: process design, tooling requirements, role changes, implementation plan.

### Tasks
- [ ] Incorporate group feedback into solution priorities
- [ ] Deep dive: Solution #1 (TBD based on group prioritization)
- [ ] Deep dive: Solution #2 (TBD)
- [ ] Deep dive: Solution #3 (TBD)
- [ ] Map dependencies between solutions
- [ ] Define pilot criteria (how do we test these in the real world?)

**Output:** `docs/planning/phase-4/` with detailed solution designs.

*This phase will be detailed during its planning session, after group input.*

---

## Phase 5 — Prototype & Pilot Design

**Goal:** Design concrete prototypes or pilots that can be tested in real workflows. Could be tooling (e.g., AI pre-review bot), process changes (e.g., new review protocol), or frameworks (e.g., metrics-based staging checklist).

### Tasks
- [ ] Design prototype for top solution
- [ ] Define pilot parameters (who, where, how long, what we measure)
- [ ] Build any necessary tooling or automation
- [ ] Create rollback plan (if pilot doesn't work)

**Output:** Depends on solutions selected. Could be code, process docs, configuration, or all three.

*This phase is deliberately vague — it depends entirely on what emerges from Phases 2-4.*

---

## Phase 6 — Test, Learn, Iterate

**Goal:** Run pilots, collect data, report back to the group, iterate.

*Details TBD after Phase 5.*

---

## Notes

- Phases 1-3 are well-defined. Phases 4-6 are intentionally rough — they'll be fleshed out as we learn.
- Group input between phases is critical. This isn't a solo project.
- The two-session cadence (Opus plans, Sonnet builds) applies to each phase.
- Push to GitHub after each meaningful update so the group can follow along.
