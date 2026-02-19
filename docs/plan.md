# Build Plan

## Project Arc

```
Map the Problem --> Scaffold Solutions --> Specify the Context Pipeline --> Prototype --> Test & Iterate
```

This isn't an app build. Each phase produces thinking artifacts, frameworks, and eventually process designs and tooling that address PDLC bottlenecks in an AI-native world.

---

## Phase 1 — Problem Mapping (COMPLETE)

**Goal:** Establish a shared, honest baseline of the current PDLC and where AI actually impacts it.

**Key findings:**
- Build went from 20% of cycle time to 5%. Total delivery only improved ~20%.
- Code review got *worse* — more code, less context, same reviewers.
- 45% of total cycle time is waiting for humans (queues, scheduling, availability).
- The PDLC was designed for a world where code was expensive. That assumption is now wrong.

*Artifacts archived to `archive/baseline/`.*

---

## Phase 2 — Solution Scaffolding (COMPLETE)

**Goal:** For each phase of the full 14-phase AI-native PDLC, scaffold a concrete solution approach.

**Output:** `phases/` — 14 phase docs + 1 overview, flat numbered files (00–13).

Each doc follows a three-layer structure:
- **Principle** — The universal truth driving the redesign
- **Pattern** — Platform-agnostic process design
- **Implementation example** — Org-specific example

Four domains: Strategy & Planning (Phases 0-2), Build & Deliver (3-8), Go-to-Market & Adoption (9, 11), Operate & Evolve (10, 12-13).

Cross-cutting themes: shift-left testing, parallel GTM stream, criteria gates replacing calendar gates, AI-enforced quality gates.

---

## Phase 3 — Context Pipeline Specification (IN PROGRESS)

**Goal:** Produce a buildable specification for how context flows between PDLC phases — the artifact schema, agent contracts, and orchestration logic that make the Phase 2 scaffolds actually work as a connected system.

**The problem:** The Phase 2 scaffolds describe what AI does at each phase in isolation. The real challenge is connecting them. The AI doing code review doesn't know what the requirements were. The AI doing QA doesn't know what design decisions were made. Each tool starts from scratch. See `context-pipeline.md` for the full problem statement.

**Approach:** Scenario-driven. Use a real feature (the R2R heatmap from the CIA Re-Write project) to walk through a 3-phase vertical slice (Requirements → Build → Review) and define exactly what artifacts are produced, what each agent consumes, and how context flows between them.

### Tasks
- [ ] Define artifact schema (types, fields, linking rules, versioning)
- [ ] Define Requirements agent contract (inputs, outputs, decision logic)
- [ ] Define Build agent contract (inputs, outputs, decision logic)
- [ ] Define Review agent contract (inputs, outputs, decision logic)
- [ ] Define orchestration logic for each handoff
- [ ] Write end-to-end trace using R2R heatmap example

**Output:** `docs/planning/phase-3/context-pipeline-spec.md`

---

## Phase 4 — Prototype (PLANNED)

**Goal:** Build the context pipeline on a real feature. Take the spec from Phase 3 and actually wire up Requirements → Build → Review with structured artifacts on the R2R heatmap (or similar real work).

*Details TBD after Phase 3.*

---

## Addenda — Problems to Address

### Democratized building creates fragmentation

When AI makes building nearly free, everyone becomes a "developer." Without governance, you get shadow IT on steroids.

**Solution angle: AI-enforced quality gates.** You don't restrict who can build — you restrict what can ship. The AI-enforced pipeline ensures what ships meets standards, regardless of who wrote it.

*Addressed in Phase 2 overview doc as cross-cutting theme #4.*

### The context pipeline problem

The Phase 2 scaffolds describe what AI does at each phase in isolation. The real implementation challenge: how does context flow between phases when no single AI system can hold the full lifecycle?

This is arguably the hardest unsolved problem and potentially the project's most valuable contribution. The individual phase improvements (AI code review, AI testing, etc.) already exist as products. What doesn't exist: a structured way to link artifacts across phases.

**Architecture:** Three components — Structured Artifact Store, Phase Agents, Orchestrator.

**Full analysis:** [context-pipeline.md](../context-pipeline.md)

---

## Notes

- The two-session cadence (Opus plans, Sonnet builds) applies to each phase.
- Push to GitHub after each meaningful update.
