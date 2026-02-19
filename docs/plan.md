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
- [x] `docs/baseline/pdlc-standard-mapping.md` — 9-phase PDLC with roles, times, bottlenecks
- [x] `docs/baseline/pdlc-mermaid-charts.md` — 5 visual charts (flow, gantt, pie, compression, parallel)
- [x] `docs/baseline/ai-impact-analysis.md` — 3-category impact analysis + prioritized focus areas

**Key findings:**
- Build went from 20% of cycle time to 5%. Total delivery only improved ~20%.
- Code review got *worse* — more code, less context, same reviewers.
- 45% of total cycle time is waiting for humans (queues, scheduling, availability).
- The PDLC was designed for a world where code was expensive. That assumption is now wrong.

---

## Phase 2 — Solution Scaffolding (COMPLETE)

**Goal:** For each phase of the full 14-phase AI-native PDLC, scaffold a concrete solution approach. Organized by domain (owner/audience), not just the delivery pipeline. Not fully fleshed out — just enough structure to discuss with the group and prioritize.

**Input:** `docs/full-pdlc-ai-native-brief.md` — expanded 14-phase lifecycle with AI-native approaches, three big shifts, and open questions.

### Structure

Four domains, each owned by a different part of the organization. Each domain gets a directory with one doc per phase, plus a top-level overview.

```
docs/planning/phase-2/
+-- overview.md
+-- strategy-and-planning/
|   +-- portfolio-prioritization.md       (Phase 0)
|   +-- ideation-and-discovery.md         (Phase 1)
|   +-- requirements-and-specification.md (Phase 2)
+-- build-and-deliver/
|   +-- design.md                         (Phase 3)
|   +-- implementation.md                 (Phase 4)
|   +-- code-review.md                    (Phase 5)
|   +-- qa-and-testing.md                 (Phase 6)
|   +-- staging.md                        (Phase 7)
|   +-- release.md                        (Phase 8)
+-- go-to-market-and-adoption/
|   +-- go-to-market.md                   (Phase 9)
|   +-- customer-success.md               (Phase 11)
+-- operate-and-evolve/
|   +-- monitoring-and-feedback.md        (Phase 10)
|   +-- operations-and-maintenance.md     (Phase 12)
|   +-- sunset-and-deprecation.md         (Phase 13)
```

### Domain Ownership

| Domain | Primary Owner(s) | Phases |
|--------|-------------------|--------|
| Strategy & Planning | Product, Leadership | 0-2 |
| Build & Deliver | Engineering | 3-8 |
| Go-to-Market & Adoption | Marketing, Sales, CS | 9, 11 |
| Operate & Evolve | DevOps/SRE, Engineering | 10, 12-13 |

### Each Phase Doc Template

Three-layer structure (universal → portable → specific):
- **Principle** — The universal truth driving the redesign for this phase
- **Pattern** — Platform-agnostic process design (how it works regardless of tooling)
- **Implementation example** — Org-specific example showing what this looks like in practice

Plus:
- Current state (from Phase 1 baseline + brief)
- What changes (process, tooling, roles)
- Open questions and risks
- How to measure success

### Overview Doc (`overview.md`)

- Domain map with handoff points between domains
- Cross-cutting themes and where they apply:
  - Shift-left everything (especially testing) — Phases 2-6
  - Parallel documentation & marketing stream — Phases 2-4, 9
  - Criteria gates replacing calendar gates — Phases 5-8
- Open questions from the brief (trust, tooling reality, resistance, metrics, risk, scale)
- Interdependencies between domains

### Tasks

**Overview:**
- [x] Create overview doc (domain map, cross-cutting themes, open questions)

**Strategy & Planning domain:**
- [x] Scaffold: Portfolio prioritization (Phase 0)
- [x] Scaffold: Ideation & discovery (Phase 1)
- [x] Scaffold: Requirements & specification (Phase 2)

**Build & Deliver domain:**
- [x] Scaffold: Design (Phase 3)
- [x] Scaffold: Implementation (Phase 4)
- [x] Scaffold: Code review (Phase 5)
- [x] Scaffold: QA & testing (Phase 6)
- [x] Scaffold: Staging (Phase 7)
- [x] Scaffold: Release (Phase 8)

**Go-to-Market & Adoption domain:**
- [x] Scaffold: Go-to-market (Phase 9)
- [x] Scaffold: Customer success & adoption (Phase 11)

**Operate & Evolve domain:**
- [x] Scaffold: Monitoring & feedback (Phase 10)
- [x] Scaffold: Operations & maintenance (Phase 12)
- [x] Scaffold: Sunset & deprecation (Phase 13)

**Output:** `docs/planning/phase-2/` — 14 phase docs + 1 overview, organized into 4 domain directories.

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

## Addenda — Problems to Address

### Democratized building creates fragmentation

When AI makes building nearly free, everyone becomes a "developer." Marketing builds automations, sales builds dashboards, ops scripts their own tools. Without a formal PDLC forcing centralization, you get shadow IT on steroids: siloed apps across systems, duplicate functionality, no shared data models, no integration, and maintenance orphans when people leave.

This is the inverse problem from the main project. The brief focuses on making the formal PDLC faster. This is about what happens when people bypass the PDLC entirely *because building is free.* The cost of building used to be a natural governance mechanism — that's gone now.

Touches: Phase 0 (portfolio governance), Phase 2 (requirements — "should this exist?"), Phase 12 (operations — orphaned apps become invisible debt).

**Solution angle: AI-enforced quality gates.** AI is both the thing that enables non-engineers to build AND the thing that can enforce standards on what they build. You don't restrict who can build — you restrict what can ship. At build time, AI writes code following project conventions by default (the builder doesn't need to know the conventions — the AI does). At review time, AI pre-review catches convention violations, architectural drift, duplicate functionality, missing tests. At merge time, AI-powered CI/CD understands intent, not just syntax — "this duplicates module X" or "this doesn't follow our data access pattern." The new governance mechanism isn't "only engineers can write code." It's "the AI-enforced pipeline ensures what ships meets standards, regardless of who wrote it."

*Addressed in Phase 2 overview doc as cross-cutting theme #4: AI-Enforced Quality Gates.*

### The context pipeline problem

The Phase 2 scaffolds describe what AI does at each phase in isolation. The real implementation challenge: how does context flow between phases when no single AI system can hold the full lifecycle?

This is arguably the hardest unsolved problem and potentially the project's most valuable contribution. The individual phase improvements (AI code review, AI testing, etc.) already exist as products. What doesn't exist: a structured way to link artifacts across phases so that the AI reviewing code in Phase 5 knows what the requirement was in Phase 2 and what the design decision was in Phase 3.

**Architecture:** Three components — Structured Artifact Store (linked artifacts with stable IDs), Phase Agents (specialized AI with curated context), Orchestrator (retrieves and compresses the right context for each task).

**Build path:** Don't build the orchestrator first. Build one vertical slice: Requirements → Build → Review, connected by structured artifacts. Validate the artifact schema and context retrieval patterns on real work, then extend.

**Full analysis:** `docs/context-pipeline-architecture.md`

---

## Notes

- Phases 1-3 are well-defined. Phases 4-6 are intentionally rough — they'll be fleshed out as we learn.
- Group input between phases is critical. This isn't a solo project.
- The two-session cadence (Opus plans, Sonnet builds) applies to each phase.
- Push to GitHub after each meaningful update so the group can follow along.
