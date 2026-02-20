# PDLC in the Age of AI

## Project Purpose

Redesigning the Product Development Life Cycle for a world where building software is nearly free. AI collapsed build time by ~80%, but total delivery only improved ~20% because every other phase (review, QA, staging, signoff) is still human-gated and sequential.

This project produces artifacts, frameworks, and eventually real systems to fix that.

## Context

- **Owner:** Emerson McCuin
- **Working group:** Pittsburgh-based group of SWEs, product folks, and others from across the PDLC
- **Repo:** https://github.com/emersonmccuin-pixel/PDLC-in-the-Age-of-AI
- **Status:** Phases 1–2 complete. Phase 3: AI Safety Net — Business Requirements Test Harness (in progress).

## Session Protocol

**Every session, do this first:**
1. Read `docs/status.md` — know where we are
2. Read `docs/plan.md` — know what's next
3. Do the work for the current phase
4. Update `docs/status.md` as tasks complete (not batched at the end)
5. Log what happened in the session log

**Two-session cadence per phase:**
- **Planning session (Opus):** Research, think, write detailed plans into `docs/plan.md` for the next phase
- **Build session (Sonnet):** Execute the plan, create artifacts, update status. No freelancing beyond the plan.

**Emerson will indicate which type of session it is.** If unclear, ask.

## Project Docs

| File | Purpose |
|------|---------|
| `docs/plan.md` | Master plan — all phases, tasks, decisions |
| `docs/status.md` | Task tracker + session log — Claude's memory across sessions |
## What Exists

### PDLC Phase Scaffolds (Phase 2 output)
- `phases/overview.md` — Domain map, cross-cutting themes, 17 open questions, interdependency map
- `phases/00-13-*.md` — 14 phase scaffold docs (flat numbered), one per phase. Three-layer structure: Principle → Pattern → Implementation Example.

### Business Requirements Test Harness (Phase 3, in progress)
- Reference implementation built against HAAS Alert CIA Re-Write (`E:\Claude Code Projects\HAAS Alert\CIA Re-Write`)
- 12 business requirement test docs (~200+ test cases), 2 executable RSpec specs (62 passing), CI/CD integration doc
- Pattern: plain English business rules → Given/When/Then cases → executable specs
- Being generalized into a reusable framework for any team adopting AI

### Context Pipeline
- `context-pipeline.md` — The hardest unsolved problem: how context flows between phases. Three-component architecture and build path. (Phase 4, planned.)

## Conventions

- Project management docs in `docs/` (plan.md, status.md), phase scaffolds in `phases/`
- Mermaid for diagrams (renders natively on GitHub)
- Collaboration via GitHub (public repo)
- Push changes to GitHub after meaningful updates
- Conventional commits with descriptive messages
- This is a research/framework project, not an app — "build" means producing documents, frameworks, process designs, and eventually tooling

## Key Framing

The question is NOT "how do we make developers faster?" — that's solved. The question is "how do we redesign the entire process for a world where building is nearly free?"
