# Cross-Link All Documentation — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add navigation and cross-references between all 26 markdown files so any reader can traverse the full project from any starting point.

**Architecture:** Three types of links: (1) structural navigation (prev/next/up), (2) semantic cross-references (prose mentions of other phases become links), (3) hub indexes (plan.md, overview.md list all their children as links). All paths are relative from the linking file's location.

**Tech Stack:** Markdown only. Relative links that work on GitHub.

---

## Reference: File Inventory

All paths relative to repo root.

**Hub / Meta:**
- `docs/plan.md` — master plan
- `docs/status.md` — task tracker + session log
- `CLAUDE.md` — project instructions

**Foundational:**
- `docs/full-pdlc-ai-native-brief.md` — expanded 14-phase brief (input to Phase 2)

**Baseline (Phase 1 output):**
- `docs/baseline/pdlc-standard-mapping.md` — 9-phase PDLC mapping
- `docs/baseline/pdlc-mermaid-charts.md` — Mermaid visualizations
- `docs/baseline/ai-impact-analysis.md` — AI impact analysis

**References:**
- `docs/references/brownfield-swe-process.md` — real-world brownfield SWE process
- `docs/references/claude-code-project-workflow.md` — Claude Code project workflow

**Phase 1 Summary:**
- `docs/planning/phase-1/summary.md`

**Phase 2 Overview:**
- `docs/planning/phase-2/overview.md`

**Phase 2 Phase Docs (14 total, lifecycle order):**

| Phase | File (relative to `docs/planning/phase-2/`) | Domain |
|-------|----------------------------------------------|--------|
| 0 | `strategy-and-planning/portfolio-prioritization.md` | Strategy & Planning |
| 1 | `strategy-and-planning/ideation-and-discovery.md` | Strategy & Planning |
| 2 | `strategy-and-planning/requirements-and-specification.md` | Strategy & Planning |
| 3 | `build-and-deliver/design.md` | Build & Deliver |
| 4 | `build-and-deliver/implementation.md` | Build & Deliver |
| 5 | `build-and-deliver/code-review.md` | Build & Deliver |
| 6 | `build-and-deliver/qa-and-testing.md` | Build & Deliver |
| 7 | `build-and-deliver/staging.md` | Build & Deliver |
| 8 | `build-and-deliver/release.md` | Build & Deliver |
| 9 | `go-to-market-and-adoption/go-to-market.md` | Go-to-Market & Adoption |
| 10 | `operate-and-evolve/monitoring-and-feedback.md` | Operate & Evolve |
| 11 | `go-to-market-and-adoption/customer-success.md` | Go-to-Market & Adoption |
| 12 | `operate-and-evolve/operations-and-maintenance.md` | Operate & Evolve |
| 13 | `operate-and-evolve/sunset-and-deprecation.md` | Operate & Evolve |

**Architecture:**
- `docs/context-pipeline-architecture.md` — context pipeline problem

---

## Task 1: Fix stale paths in Phase 1 summary

**File:** `docs/planning/phase-1/summary.md`

The artifacts table has wrong paths (files were moved to `baseline/` during repo reorganization).

**Replace** the artifacts table:

```markdown
| Document | What It Covers |
|----------|---------------|
| [PDLC Standard Mapping](../../baseline/pdlc-standard-mapping.md) | 9 PDLC phases with roles, times, bottlenecks, pre/post-AI comparison |
| [Mermaid Charts](../../baseline/pdlc-mermaid-charts.md) | 5 Mermaid charts: flow, gantt, pie, compression, parallel possibilities |
| [AI Impact Analysis](../../baseline/ai-impact-analysis.md) | 3-category analysis (helps / doesn't help / makes worse), focus areas, research questions |
```

**Also add** at the bottom of the file:

```markdown
---

## Navigation

- [Back to Plan](../../plan.md)
- **Next:** [Phase 2 Overview](../phase-2/overview.md)

### Related Documents
- [Full AI-Native PDLC Brief](../../full-pdlc-ai-native-brief.md) — expanded the 9-phase model to 14 phases based on these findings
- [Context Pipeline Architecture](../../context-pipeline-architecture.md) — the implementation challenge that emerged from Phase 2
```

**Commit:** `docs: fix stale paths and add navigation to phase 1 summary`

---

## Task 2: Add navigation footer to all 14 Phase 2 phase docs

For each of the 14 phase docs, append a navigation footer at the end of the file. The footer has three sections: lifecycle navigation (prev/next), link back to overview, and related documents.

### Navigation Footer Template

```markdown
---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase N-1: Title](relative-path) | [Phase 2 Overview](../overview.md) | [Phase N+1: Title](relative-path) |

### Related Documents
- [link to baseline doc if relevant]
- [link to brief section if relevant]
- [links to other phase docs referenced in the body text]
```

### Exact Navigation Table

Use this table to build each footer. Paths are relative from the phase doc's own directory.

| Phase | Title | Prev Link | Next Link |
|-------|-------|-----------|-----------|
| 0 | Portfolio Prioritization | *(none — first phase)* | [Phase 1: Ideation & Discovery](ideation-and-discovery.md) |
| 1 | Ideation & Discovery | [Phase 0: Portfolio Prioritization](portfolio-prioritization.md) | [Phase 2: Requirements & Specification](requirements-and-specification.md) |
| 2 | Requirements & Specification | [Phase 1: Ideation & Discovery](ideation-and-discovery.md) | [Phase 3: Design](../build-and-deliver/design.md) |
| 3 | Design | [Phase 2: Requirements & Specification](../strategy-and-planning/requirements-and-specification.md) | [Phase 4: Implementation](implementation.md) |
| 4 | Implementation | [Phase 3: Design](design.md) | [Phase 5: Code Review](code-review.md) |
| 5 | Code Review | [Phase 4: Implementation](implementation.md) | [Phase 6: QA & Testing](qa-and-testing.md) |
| 6 | QA & Testing | [Phase 5: Code Review](code-review.md) | [Phase 7: Staging](staging.md) |
| 7 | Staging | [Phase 6: QA & Testing](qa-and-testing.md) | [Phase 8: Release](release.md) |
| 8 | Release | [Phase 7: Staging](staging.md) | [Phase 9: Go-to-Market](../go-to-market-and-adoption/go-to-market.md) |
| 9 | Go-to-Market | [Phase 8: Release](../build-and-deliver/release.md) | [Phase 10: Monitoring & Feedback](../operate-and-evolve/monitoring-and-feedback.md) |
| 10 | Monitoring & Feedback | [Phase 9: Go-to-Market](../go-to-market-and-adoption/go-to-market.md) | [Phase 11: Customer Success](../go-to-market-and-adoption/customer-success.md) |
| 11 | Customer Success | [Phase 10: Monitoring & Feedback](../operate-and-evolve/monitoring-and-feedback.md) | [Phase 12: Operations & Maintenance](../operate-and-evolve/operations-and-maintenance.md) |
| 12 | Operations & Maintenance | [Phase 11: Customer Success](../go-to-market-and-adoption/customer-success.md) | [Phase 13: Sunset & Deprecation](sunset-and-deprecation.md) |
| 13 | Sunset & Deprecation | [Phase 12: Operations & Maintenance](operations-and-maintenance.md) | *(none — last phase)* |

### Related Documents per Phase Doc

For each phase doc, add these specific related document links (in addition to prev/next/overview):

**Phase 0 (Portfolio Prioritization):**
- [AI Impact Analysis — Focus Areas](../../../baseline/ai-impact-analysis.md) — prioritization criteria from Phase 1

**Phase 1 (Ideation & Discovery):**
- [AI Impact Analysis](../../../baseline/ai-impact-analysis.md) — what AI can/can't do today

**Phase 2 (Requirements & Specification):**
- [Context Pipeline Architecture](../../../context-pipeline-architecture.md) — how requirements flow to downstream phases

**Phase 3 (Design):**
- [PDLC Standard Mapping](../../../baseline/pdlc-standard-mapping.md) — traditional design phase baseline

**Phase 4 (Implementation):**
- [AI Impact Analysis](../../../baseline/ai-impact-analysis.md) — build is the phase AI collapsed most
- [Brownfield SWE Process](../../../references/brownfield-swe-process.md) — real-world implementation workflow

**Phase 5 (Code Review):**
- [AI Impact Analysis](../../../baseline/ai-impact-analysis.md) — review is the phase AI made *worse*
- [Context Pipeline Architecture](../../../context-pipeline-architecture.md) — how review context links to requirements

**Phase 6 (QA & Testing):**
- [PDLC Standard Mapping](../../../baseline/pdlc-standard-mapping.md) — traditional QA baseline

**Phase 7 (Staging):**
- [PDLC Mermaid Charts](../../../baseline/pdlc-mermaid-charts.md) — visualization of staging bottleneck

**Phase 8 (Release):**
- [PDLC Standard Mapping](../../../baseline/pdlc-standard-mapping.md) — traditional release gates

**Phase 9 (Go-to-Market):**
- [Full AI-Native PDLC Brief](../../../full-pdlc-ai-native-brief.md) — GTM as a new addition beyond the original 9-phase model

**Phase 10 (Monitoring & Feedback):**
- [Context Pipeline Architecture](../../../context-pipeline-architecture.md) — monitoring feeds back into the context pipeline

**Phase 11 (Customer Success):**
- [Full AI-Native PDLC Brief](../../../full-pdlc-ai-native-brief.md) — customer success as a new addition

**Phase 12 (Operations & Maintenance):**
- [Context Pipeline Architecture](../../../context-pipeline-architecture.md) — ops as a context consumer
- [Brownfield SWE Process](../../../references/brownfield-swe-process.md) — real-world ops workflow

**Phase 13 (Sunset & Deprecation):**
- [Full AI-Native PDLC Brief](../../../full-pdlc-ai-native-brief.md) — sunset as a new addition

### Inline Cross-References

In addition to the footer, scan each phase doc for prose references to other phases (e.g., "from the test-first suite in Phase 4", "Test specs exist since Phase 2"). Convert the **first occurrence** of each such reference to a link. Examples:

- "Phase 4" in code-review.md → "[Phase 4](implementation.md)"
- "Phase 2" in qa-and-testing.md → "[Phase 2](../strategy-and-planning/requirements-and-specification.md)"
- "Phase 3" in qa-and-testing.md → "[Phase 3](design.md)"
- "Phase 1 baseline" in any build-and-deliver doc → "[Phase 1 baseline](../../../baseline/pdlc-standard-mapping.md)"

**Rule:** Only link the first meaningful prose reference per phase per document. Don't link phase numbers in the document's own title or headings. Don't link phase numbers inside code blocks.

**Commit:** `docs: add navigation footers and cross-references to all phase 2 docs`

---

## Task 3: Update Phase 2 overview.md

Add a linked index of all phase docs at the top of the document, and link to related documents.

**After the "## Purpose" section, add:**

```markdown
## Phase Index

### Strategy & Planning
- [Phase 0: Portfolio Prioritization](strategy-and-planning/portfolio-prioritization.md)
- [Phase 1: Ideation & Discovery](strategy-and-planning/ideation-and-discovery.md)
- [Phase 2: Requirements & Specification](strategy-and-planning/requirements-and-specification.md)

### Build & Deliver
- [Phase 3: Design](build-and-deliver/design.md)
- [Phase 4: Implementation](build-and-deliver/implementation.md)
- [Phase 5: Code Review](build-and-deliver/code-review.md)
- [Phase 6: QA & Testing](build-and-deliver/qa-and-testing.md)
- [Phase 7: Staging](build-and-deliver/staging.md)
- [Phase 8: Release](build-and-deliver/release.md)

### Go-to-Market & Adoption
- [Phase 9: Go-to-Market](go-to-market-and-adoption/go-to-market.md)
- [Phase 11: Customer Success & Adoption](go-to-market-and-adoption/customer-success.md)

### Operate & Evolve
- [Phase 10: Monitoring & Feedback](operate-and-evolve/monitoring-and-feedback.md)
- [Phase 12: Operations & Maintenance](operate-and-evolve/operations-and-maintenance.md)
- [Phase 13: Sunset & Deprecation](operate-and-evolve/sunset-and-deprecation.md)
```

**At the bottom of the file, add:**

```markdown
---

## Navigation

- [Back to Plan](../../plan.md)
- [Phase 1 Summary](../phase-1/summary.md)
- [Full AI-Native PDLC Brief](../../full-pdlc-ai-native-brief.md) — the input document that shaped this phase
- [Context Pipeline Architecture](../../context-pipeline-architecture.md) — the implementation challenge that emerged from these scaffolds
```

**Commit:** `docs: add phase index and navigation to phase 2 overview`

---

## Task 4: Cross-link baseline documents

Each of the 3 baseline docs gets a navigation/related section at the bottom.

### `docs/baseline/pdlc-standard-mapping.md`

Append:

```markdown
---

## Navigation

- [Back to Plan](../plan.md)
- [Phase 1 Summary](../planning/phase-1/summary.md)

### Related Baseline Documents
- [Mermaid Charts](pdlc-mermaid-charts.md) — visual representations of this mapping
- [AI Impact Analysis](ai-impact-analysis.md) — where AI helps and hurts across these phases

### Where This Feeds Forward
- [Phase 2 Overview](../planning/phase-2/overview.md) — solution scaffolds built on this baseline
- [Full AI-Native PDLC Brief](../full-pdlc-ai-native-brief.md) — expanded from 9 to 14 phases based on gaps found here
```

### `docs/baseline/pdlc-mermaid-charts.md`

Append:

```markdown
---

## Navigation

- [Back to Plan](../plan.md)
- [Phase 1 Summary](../planning/phase-1/summary.md)

### Related Baseline Documents
- [PDLC Standard Mapping](pdlc-standard-mapping.md) — the data these charts visualize
- [AI Impact Analysis](ai-impact-analysis.md) — analysis of the patterns shown here

### Where This Feeds Forward
- [Phase 2 Overview](../planning/phase-2/overview.md) — solution scaffolds addressing the bottlenecks shown here
```

### `docs/baseline/ai-impact-analysis.md`

Append:

```markdown
---

## Navigation

- [Back to Plan](../plan.md)
- [Phase 1 Summary](../planning/phase-1/summary.md)

### Related Baseline Documents
- [PDLC Standard Mapping](pdlc-standard-mapping.md) — the phase structure this analysis covers
- [Mermaid Charts](pdlc-mermaid-charts.md) — visual representation of compression and bottlenecks

### Where This Feeds Forward
- [Phase 2 Overview](../planning/phase-2/overview.md) — solutions scaffolded from these findings
- [Phase 5: Code Review](../planning/phase-2/build-and-deliver/code-review.md) — directly addresses "review got worse"
- [Phase 4: Implementation](../planning/phase-2/build-and-deliver/implementation.md) — addresses the already-collapsed build phase
```

**Commit:** `docs: add navigation and cross-links to baseline documents`

---

## Task 5: Cross-link the brief

**File:** `docs/full-pdlc-ai-native-brief.md`

Append to the bottom:

```markdown
---

## Navigation

- [Back to Plan](plan.md)

### Source Material
- [Phase 1 Summary](planning/phase-1/summary.md) — the findings this brief expands on
- [PDLC Standard Mapping](baseline/pdlc-standard-mapping.md) — original 9-phase baseline
- [AI Impact Analysis](baseline/ai-impact-analysis.md) — where AI helps and hurts

### What This Produced
- [Phase 2 Overview](planning/phase-2/overview.md) — solution scaffolds for all 14 phases
- [Context Pipeline Architecture](context-pipeline-architecture.md) — the implementation challenge that emerged
```

**Commit:** `docs: add navigation to AI-native PDLC brief`

---

## Task 6: Cross-link context pipeline architecture

**File:** `docs/context-pipeline-architecture.md`

Append to the bottom:

```markdown
---

## Navigation

- [Back to Plan](plan.md)

### Source Material
- [Phase 2 Overview](planning/phase-2/overview.md) — the solution scaffolds that revealed this problem
- [Full AI-Native PDLC Brief](full-pdlc-ai-native-brief.md) — the lifecycle this pipeline connects

### Key Phase Connections
- [Phase 2: Requirements & Specification](planning/phase-2/strategy-and-planning/requirements-and-specification.md) — where structured artifacts originate
- [Phase 4: Implementation](planning/phase-2/build-and-deliver/implementation.md) — the vertical slice starting point
- [Phase 5: Code Review](planning/phase-2/build-and-deliver/code-review.md) — where context retrieval is most critical
- [Phase 10: Monitoring & Feedback](planning/phase-2/operate-and-evolve/monitoring-and-feedback.md) — where lifecycle traceability is tested
```

**Commit:** `docs: add navigation to context pipeline architecture`

---

## Task 7: Convert plan.md artifact references to links

In `docs/plan.md`, the Phase 1 and Phase 2 sections list artifacts as backtick code paths. Convert these to markdown links. The Phase 2 directory tree in the code block should remain as-is (it's structural), but the task list items and prose references should become links.

### Phase 1 section — replace:

- `` `docs/baseline/pdlc-standard-mapping.md` `` → `[docs/baseline/pdlc-standard-mapping.md](baseline/pdlc-standard-mapping.md)`
- `` `docs/baseline/pdlc-mermaid-charts.md` `` → `[docs/baseline/pdlc-mermaid-charts.md](baseline/pdlc-mermaid-charts.md)`
- `` `docs/baseline/ai-impact-analysis.md` `` → `[docs/baseline/ai-impact-analysis.md](baseline/ai-impact-analysis.md)`

### Phase 2 section — replace:

- `` `docs/full-pdlc-ai-native-brief.md` `` in the **Input** line → `[docs/full-pdlc-ai-native-brief.md](full-pdlc-ai-native-brief.md)`

### Addenda section — replace:

- `` `docs/context-pipeline-architecture.md` `` → `[docs/context-pipeline-architecture.md](context-pipeline-architecture.md)`

**Do not touch** the directory tree inside the code block in the Phase 2 structure section — that's a visual diagram, not a link target.

**Commit:** `docs: convert plan.md artifact paths to navigable links`

---

## Task 8: Add navigation to reference documents

These are external process docs brought in for reference. Add a small context section at the top (after the title) and a navigation footer.

### `docs/references/brownfield-swe-process.md`

Add after the title line:

```markdown
> **Project context:** This document was added as reference material for the [PDLC in the Age of AI](../../CLAUDE.md) project. It represents a real-world brownfield SWE process that the AI-native PDLC aims to improve upon. See [Phase 1 Summary](../planning/phase-1/summary.md) for how this fed into the analysis.
```

### `docs/references/claude-code-project-workflow.md`

Add after the title line:

```markdown
> **Project context:** This document was added as reference material for the [PDLC in the Age of AI](../../CLAUDE.md) project. It represents a structured AI-assisted project workflow. See [Phase 1 Summary](../planning/phase-1/summary.md) for how this fed into the analysis.
```

**Commit:** `docs: add project context to reference documents`

---

## Task 9: Update status.md

Add the following entry to the session log in `docs/status.md`:

```markdown
### Session 4 — 2026-02-18 (Sonnet, Build)
- Cross-linked all 26 markdown documents in the repo
- Fixed stale file paths in Phase 1 summary
- Added navigation footers (prev/next/overview) to all 14 Phase 2 phase docs
- Added inline cross-references (first prose mention of each phase becomes a link)
- Added phase index to Phase 2 overview
- Cross-linked baseline docs, brief, and context pipeline architecture
- Converted plan.md artifact paths to navigable links
- Added project context to reference documents
```

**Commit:** `docs: update status with session 4 cross-linking work`

---

## Task 10: Final commit and push

```bash
git push origin main
```

---

## Execution Notes for Sonnet

1. **Work through tasks sequentially** — each task is one commit.
2. **All paths are relative from the file being edited.** Double-check relative path depth (`../` count) before writing.
3. **For Task 2 (the big one):** Process the 14 phase docs in lifecycle order (0 through 13). For each doc: (a) read it fully, (b) find prose "Phase N" references, (c) convert first occurrence of each to a link, (d) append the navigation footer.
4. **Do not reformat or restructure existing content.** Only add navigation sections and convert references to links.
5. **Do not add links inside code blocks or diagram blocks.**
6. **The "From Phase 1 baseline:" prose that appears in many phase docs** should be linked as: `From [Phase 1 baseline](../../../baseline/pdlc-standard-mapping.md):` — just the first occurrence.
