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

## Phase 3 — AI Safety Net: Business Requirements Test Harness (IN PROGRESS)

**Goal:** Solve the most immediate problem in AI-augmented development: how do you ensure AI-generated code doesn't break existing functionality that works?

**The thesis:** When building is nearly free, the critical investment shifts from "how to build" to "how to prove what we built doesn't break what already works." AI can write code fast, but nobody trusts it to ship without proof. Business requirements tests are the quality gate that makes AI-generated code shippable. This is the first concrete, reusable artifact the project produces — something you could hand to another team and they could use tomorrow.

**Why this comes before the context pipeline:** The context pipeline is the long-term architecture. But teams adopting AI today need a safety net now. A business requirements test harness is:
- Immediately useful (not theoretical)
- A concrete implementation of Phase 6 (Code Review) and Phase 7 (QA) concepts from the scaffolds
- A real example of "AI-enforced quality gates" — the cross-cutting theme from Phase 2
- The foundation that makes everything else trustworthy

**The deliverable:** A repeatable process that AI follows to produce a proven integration test suite from any existing codebase. The process IS the artifact. The test suite is the output of running the process.

**What exists (from Session 8, applied to HAAS Alert CIA Re-Write):**
- 12 business requirement test docs (~200+ test cases) covering auth, users, assets, events, closures, alerting zones, exclusion zones, orgs, reports, trips, hazards, API keys
- 2 executable RSpec spec files (62 passing tests) for auth and things/assets
- CI/CD integration doc for post-deploy execution
- Session 8 effectively ran steps 1–4 of the process below on one codebase. Steps 5–6 remain.

### The Process

```
Codebase Crawl --> Business Rule Extraction --> Test Doc Generation --> Executable Test Generation --> Hardening Loop --> Proven Test Suite
```

1. **Codebase crawl** — AI maps the codebase: domains, controllers, models, routes, services, policies. Identifies what the system does, not how.
2. **Business rule extraction** — For each domain, AI identifies what the code promises to users. Business behavior, not implementation details. Plain English.
3. **Test doc generation** — Each business rule becomes a Given/When/Then test case. Human-readable. Reviewable by non-engineers.
4. **Executable test generation** — Convert test docs into actual specs in the project's test framework (RSpec, Jest, Playwright, etc.).
5. **Hardening loop** — Run tests repeatedly. Random seeds. Different data states. Fix flaky tests. Fix wrong assumptions about actual codebase behavior. Iterate until every test passes reliably across many runs. This is where the AI learns what the codebase actually does vs. what it assumed.
6. **Output** — Battle-tested integration test suite that proves existing functionality works. Ready to run post-deploy.

### Tasks

**Define the process (documentation):**
- [ ] Write the process definition doc — the 6 steps above, fleshed out with decision points, inputs/outputs, and guidance for each step
- [ ] Document what we learned in Session 8: assumptions that were wrong, how we discovered them, how the hardening loop fixes this

**Prove the process (execution on HAAS Alert):**
- [ ] Harden existing 2 spec files (random-seed runs, repeated execution, edge cases) — Step 5
- [ ] Convert remaining 10 business test docs into executable specs — Step 4 for the rest
- [ ] Harden ALL specs through the full hardening loop — Step 5 at scale
- [ ] Document every assumption mismatch and how it was resolved

**Package the process (reusable artifact):**
- [ ] Write the final process doc: step-by-step, stack-agnostic, ready for any team to hand to an AI agent
- [ ] Write the PDLC framework doc: where this fits in the lifecycle, why it matters, how teams adopt it

**Output:** Process definition doc + PDLC framework doc in `phases/` | Reference implementation (HAAS Alert test suite) as proof it works

---

## Phase 4 — Context Pipeline Specification (PLANNED)

**Goal:** Produce a buildable specification for how context flows between PDLC phases — the artifact schema, agent contracts, and orchestration logic that make the Phase 2 scaffolds actually work as a connected system.

**The problem:** The Phase 2 scaffolds describe what AI does at each phase in isolation. The real challenge is connecting them. See [context-pipeline.md](../context-pipeline.md) for the full problem statement.

**Approach:** Scenario-driven. Use a real feature (the R2R heatmap from the CIA Re-Write project) to walk through a 3-phase vertical slice (Requirements → Build → Review) and define exactly what artifacts are produced, what each agent consumes, and how context flows between them.

### Tasks
- [ ] Define artifact schema (types, fields, linking rules, versioning)
- [ ] Define Requirements agent contract (inputs, outputs, decision logic)
- [ ] Define Build agent contract (inputs, outputs, decision logic)
- [ ] Define Review agent contract (inputs, outputs, decision logic)
- [ ] Define orchestration logic for each handoff
- [ ] Write end-to-end trace using R2R heatmap example

*Details refined after Phase 3.*

---

## Phase 5 — Prototype (PLANNED)

**Goal:** Build the context pipeline on a real feature. Take the spec from Phase 4 and actually wire up Requirements → Build → Review with structured artifacts on the R2R heatmap (or similar real work).

*Details TBD after Phase 4.*

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
