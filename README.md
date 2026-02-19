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
| Phase 3 — Context Pipeline Spec | Buildable spec for structured artifacts + agent contracts | In Progress |
| Phase 4 — Prototype | Build and test the context pipeline on a real feature | Planned |

## What's Been Built

### PDLC Phase Scaffolds

- [`phases/overview.md`](phases/overview.md) — Domain map, cross-cutting themes, 17 open questions, interdependency map
- [`phases/`](phases/) — 14 phase scaffold docs, one per phase. Each follows a three-layer structure: Principle → Pattern → Implementation Example

### Context Pipeline

- [`context-pipeline.md`](context-pipeline.md) — The core problem: how context flows between phases when no single AI can hold the full lifecycle. Three-component architecture (artifact store, phase agents, orchestrator) and build path.

## Repo Structure

```
context-pipeline.md        # The core unsolved problem: context flow between phases
phases/                    # 14 PDLC phase scaffolds + overview
docs/
+-- plan.md                # Master plan across all phases
+-- status.md              # Task tracker and session log
archive/                   # Phase 1 baseline, references, earlier drafts
```

## Key Ideas

**Shift-left everything.** Testing, documentation, GTM prep — start them in requirements, not after build.

**Criteria gates, not calendar gates.** Code moves to staging when it's ready, not when the sprint ends.

**AI-enforced governance.** The new guard against fragmentation isn't "only engineers can build" — it's "the AI-enforced pipeline decides what ships, regardless of who wrote it."

**The context pipeline.** The individual phase tools (AI code review, AI testing) already exist. What doesn't exist: a structured way to link artifacts across phases so the AI in Phase 5 knows what the requirement was in Phase 2.

---

*Work in progress. Context pipeline specification is next.*
