# PDLC in the Age of AI

**AI cut software build time by ~80%. Total delivery improved by ~20%. This project is about fixing that gap.**

When building is nearly free, the bottleneck moves — to code review, QA, staging, signoff, and every other human-gated step that was designed for a world where writing code was expensive. Those processes haven't changed. This project redesigns them.

---

## The Problem

The traditional PDLC was built on one assumption: code is expensive to write. AI broke that assumption, but the process didn't adapt.

- Implementation is now ~5% of total cycle time — it's no longer the bottleneck
- Code review got *worse* with AI — more code, less context, same reviewers
- ~45% of total cycle time is waiting for humans (queues, scheduling, availability)
- The PDLC has 14 phases. AI meaningfully accelerated maybe 2 of them.

## What This Project Does

This is a research and framework project, not a software product. The outputs are documents, frameworks, process designs, and eventually tooling.

The question we're answering: **How do you redesign the entire PDLC for a world where building is nearly free?**

Not "how do we make developers faster" — that's solved. The question is everything else.

## Who's Involved

A Pittsburgh-based working group of software engineers, product managers, and practitioners from across the PDLC. The work is collaborative: this repo seeds the research; the group debates, prioritizes, and drives it forward.

## Status

| Phase | Focus | Status |
|-------|-------|--------|
| Phase 1 — Problem Mapping | Baseline PDLC, AI impact analysis, visualization | Complete |
| Phase 2 — Solution Scaffolding | Concrete solution approaches for all 14 phases | Complete |
| Phase 3 — Group Session Prep | Discussion guide, debate topics, prioritization exercise | In progress |
| Phase 4 — Solution Deep Dives | Full treatment of top-priority solutions | Planned |
| Phase 5 — Prototype & Pilot | Design and build testable prototypes | Planned |
| Phase 6 — Test & Iterate | Run pilots, collect data, learn | Planned |

## What's Been Built

### Phase 1 — Baseline

- [`docs/baseline/pdlc-standard-mapping.md`](docs/baseline/pdlc-standard-mapping.md) — 14-phase PDLC breakdown with roles, time estimates, bottlenecks, and pre/post-AI comparison
- [`docs/baseline/pdlc-mermaid-charts.md`](docs/baseline/pdlc-mermaid-charts.md) — 5 diagrams: flow, gantt, pie, the compression problem, parallel possibilities
- [`docs/baseline/ai-impact-analysis.md`](docs/baseline/ai-impact-analysis.md) — Where AI helps, where it doesn't, where it makes things worse

### Phase 2 — Solution Scaffolds

- [`docs/full-pdlc-ai-native-brief.md`](docs/full-pdlc-ai-native-brief.md) — The expanded 14-phase AI-native lifecycle: three big shifts, open questions, and the framing that drives Phase 2
- [`docs/planning/phase-2/overview.md`](docs/planning/phase-2/overview.md) — Domain map, cross-cutting themes, 17 open questions, interdependency map
- [`docs/planning/phase-2/`](docs/planning/phase-2/) — 14 phase scaffold docs, one per phase. Each follows a three-layer structure: Principle → Pattern → Implementation Example

### Architecture Research

- [`docs/context-pipeline-architecture.md`](docs/context-pipeline-architecture.md) — The hardest unsolved problem: how context flows between phases when no single AI can hold the full lifecycle. Defines a three-component architecture and identifies the right place to start building.

## Repo Structure

```
docs/
+-- baseline/              # Phase 1: PDLC mapping, charts, AI impact analysis
+-- planning/
|   +-- phase-1/           # Phase 1 summary
|   +-- phase-2/           # Phase 2: 14 solution scaffolds + overview
|   +-- phase-3/           # Phase 3: group session materials (in progress)
+-- references/            # Real-world process docs used as reference material
+-- full-pdlc-ai-native-brief.md   # The 14-phase AI-native brief
+-- context-pipeline-architecture.md  # Context pipeline design
+-- plan.md                # Master plan across all phases
+-- status.md              # Task tracker and session log
```

## Key Ideas

**Shift-left everything.** Testing, documentation, GTM prep — start them in requirements, not after build.

**Criteria gates, not calendar gates.** Code moves to staging when it's ready, not when the sprint ends.

**AI-enforced governance.** The new guard against fragmentation isn't "only engineers can build" — it's "the AI-enforced pipeline decides what ships, regardless of who wrote it."

**The context pipeline.** The individual phase tools (AI code review, AI testing) already exist. What doesn't exist: a structured way to link artifacts across phases so the AI in Phase 5 knows what the requirement was in Phase 2.

---

*Work in progress. Group sessions and deep dives are next.*
