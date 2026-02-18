# PDLC in the Age of AI

## Project Purpose

Redesigning the Product Development Life Cycle for a world where building software is nearly free. AI collapsed build time by ~80%, but total delivery only improved ~20% because every other phase (review, QA, staging, signoff) is still human-gated and sequential.

This project produces artifacts, frameworks, and eventually real systems to fix that.

## Context

- **Owner:** Emerson McCuin
- **Working group:** Pittsburgh-based group of SWEs, product folks, and others from across the PDLC
- **Repo:** https://github.com/emersonmccuin-pixel/PDLC-in-the-Age-of-AI
- **Status:** Early — baseline mapping complete, solutions not yet scaffolded

## What Exists

- `docs/pdlc-standard-mapping.md` — 9-phase PDLC breakdown with roles, time estimates, bottlenecks, pre-AI vs post-AI comparison
- `docs/pdlc-mermaid-charts.md` — 5 Mermaid charts (flow, gantt, pie, compression problem, parallel possibilities)
- `docs/ai-impact-analysis.md` — Where AI helps, where it doesn't, where it makes things worse. Includes prioritized focus areas and research questions

## What's Next

Scaffold solutions to the identified bottlenecks, then flesh them out into real systems. Priority areas from the impact analysis:

### High-Impact, Achievable
1. AI-assisted code review (reduce human reviewers, add AI pre-review)
2. AI-generated test plans (accelerate QA kickoff)
3. Metrics-based staging criteria (replace time-based "bake")
4. Smaller, more frequent PRs (leverage cheap builds)

### High-Impact, Hard
5. Parallel workflows (run QA, review, staging concurrently)
6. AI as first-class PDLC participant (not just a dev tool)
7. Continuous deployment with AI monitoring
8. Requirements-to-deploy traceability pipeline

## Conventions

- All docs in `docs/`
- Mermaid for diagrams (renders natively on GitHub)
- Collaboration via GitHub (public repo)
- Push changes to GitHub after meaningful updates
- Conventional commits with descriptive messages

## Key Framing

The question is NOT "how do we make developers faster?" — that's solved. The question is "how do we redesign the entire process for a world where building is nearly free?"
