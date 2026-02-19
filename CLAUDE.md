# PDLC in the Age of AI

## Project Purpose

Redesigning the Product Development Life Cycle for a world where building software is nearly free. AI collapsed build time by ~80%, but total delivery only improved ~20% because every other phase (review, QA, staging, signoff) is still human-gated and sequential.

This project produces artifacts, frameworks, and eventually real systems to fix that.

## Context

- **Owner:** Emerson McCuin
- **Working group:** Pittsburgh-based group of SWEs, product folks, and others from across the PDLC
- **Repo:** https://github.com/emersonmccuin-pixel/PDLC-in-the-Age-of-AI
- **Status:** Phases 1–2 complete (problem mapping + solution scaffolding). Phase 3 next (group session prep).

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
| `docs/planning/phase-N/` | Phase-specific research, notes, detailed work |

## What Exists

### Baseline Analysis (Phase 1 output)
- `docs/baseline/pdlc-standard-mapping.md` — 9-phase PDLC breakdown with roles, time estimates, bottlenecks, pre-AI vs post-AI comparison
- `docs/baseline/pdlc-mermaid-charts.md` — 5 Mermaid charts (flow, gantt, pie, compression problem, parallel possibilities)
- `docs/baseline/ai-impact-analysis.md` — Where AI helps, where it doesn't, where it makes things worse. Prioritized focus areas and research questions.

### Solution Scaffolding (Phase 2 output)
- `docs/full-pdlc-ai-native-brief.md` — Expanded 14-phase AI-native lifecycle with three big shifts and open questions. Input to Phase 2.
- `docs/planning/phase-2/overview.md` — Domain map, cross-cutting themes (shift-left, parallel GTM, criteria gates, AI-enforced governance), 17 open questions, interdependency map
- `docs/planning/phase-2/00-13-*.md` — 14 phase scaffold docs (flat numbered), one per phase. Three-layer structure: Principle → Pattern → Implementation Example.
- `docs/context-pipeline-architecture.md` — The hardest unsolved problem: how context flows between phases. Three-component architecture and build path.

### Reference Documents
- `docs/references/brownfield-swe-process.md` — Real-world brownfield SWE process for a production dashboard app. Example of a current AI-assisted PDLC in practice.
- `docs/references/claude-code-project-workflow.md` — Structured project workflow for Claude Code. The process this project itself uses.

## Conventions

- All docs in `docs/`, phase work in `docs/planning/phase-N/`
- Mermaid for diagrams (renders natively on GitHub)
- Collaboration via GitHub (public repo)
- Push changes to GitHub after meaningful updates
- Conventional commits with descriptive messages
- This is a research/framework project, not an app — "build" means producing documents, frameworks, process designs, and eventually tooling

## Key Framing

The question is NOT "how do we make developers faster?" — that's solved. The question is "how do we redesign the entire process for a world where building is nearly free?"
