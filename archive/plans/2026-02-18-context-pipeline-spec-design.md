# Design: Context Pipeline Spec (Buildable)

**Date:** 2026-02-18
**Type:** Planning session design doc
**Drives:** Phase 3 (redefined) — Context Pipeline Specification

---

## What Changed

Phase 3 was originally "Group Session Prep" — discussion guides, debate topics, and prioritization exercises for the Pittsburgh working group's first meeting. That's been scrapped. The new Phase 3:

**Produce a buildable specification for the context pipeline's first vertical slice (Requirements -> Build -> Review), using the R2R heatmap feature from the CIA Re-Write as the concrete scenario.**

The Phase 2 scaffolds and context pipeline architecture doc remain valid as high-level foundations. This spec is the detailed, buildable layer on top.

---

## Why the R2R Heatmap

The context pipeline architecture doc used a hypothetical "dashboard customization" example. We're replacing it with a real feature: the R2R (Responder-to-Responder) near-collision heatmap being built into the HAAS Alert CIA dashboard.

**Why this works:**
- **Fully planned, zero implementation.** All the "requirements" and "design" artifacts exist as planning docs. The build hasn't started. We can trace the full pipeline without retroactive reconstruction.
- **Real complexity.** 1.4M Redshift rows, deduplication logic, two permission models (cia:read vs cia:beta), cross-repo Rails + React implementation, Redis caching strategy, deck.gl rendering.
- **The planning artifacts ARE the problem.** The R2R heatmap produced 6 unstructured planning documents:

| Artifact | File | What it contains |
|----------|------|-----------------|
| Interview notes | `R2R-HEATMAP-INTERVIEW.md` | 15 design decisions, 4 open questions |
| Data findings | `r2r-thing-alerts-findings.md` | Schema, data volume, `alert_sent` behavior, reciprocal record analysis |
| Reference research | `r2r-heatmap-reference-research.md` | Standalone app architecture, SQL, component structure |
| Implementation plan | `PHASE-7-R2R-HEATMAP.md` | 3 sub-phases, file lists, SQL, component tree, API contracts, performance strategy |
| Jira ticket draft | `JIRA-TICKET-DRAFT-7.md` | User story, 14 acceptance criteria, testing approach |
| Review action plan | `ACTION-PLAN.md` | 12 ordered fixes (2 critical, several high/medium) from code review |

These docs are thorough. They're also **completely disconnected**. The acceptance criteria in the Jira draft aren't linked to the test cases in the implementation plan. The design decisions in the interview aren't traceable to the code that implements them. The review action plan references line numbers in files but doesn't link back to which requirement or acceptance criterion is affected.

This is exactly the gap the context pipeline fills.

---

## What the Spec Covers

One document: `docs/planning/phase-3/context-pipeline-spec.md`

### Section 1: Artifact Schema

The foundation. Defines the structured format for every artifact the pipeline produces.

**Contents:**
- Core artifact fields (ID, type, status, parent links, timestamps)
- Linking rules (how artifacts reference each other — parent/child, produces/consumes)
- Versioning (how changes to requirements propagate visibility downstream)
- Artifact types for the vertical slice:
  - **Requirement** — ID, description, acceptance criteria (each with sub-ID), priority, status
  - **Test Specification** — ID, linked acceptance criterion, given/when/then, type (unit/integration/component/a11y)
  - **Design Decision** — ID, linked requirement, decision, rationale, alternatives considered
  - **Implementation Artifact** — PR reference, linked requirement, linked test specs, files changed, self-review report
  - **Review Artifact** — linked PR, linked requirement, linked test specs, findings (pass/fail per criterion), risk assessment, human reviewer notes

**R2R illustration:** Show what the R2R heatmap's artifact chain looks like in this schema — from the requirement ("R2R heatmap in CIA dashboard") through acceptance criteria, test specs, implementation PRs, to review findings. Side-by-side with what actually exists today (the 6 disconnected markdown files).

### Section 2: Requirements Phase

The first agent in the pipeline.

**Agent contract:**
- **Inputs:** Rough notes, interview transcripts, data findings, reference research (the kind of raw material that exists for R2R today)
- **Outputs:** Structured requirement artifact + test specifications + design decisions
- **Decision logic:** How the agent decides granularity (when to split requirements, when to flag ambiguity, when to generate test specs vs. flag for human input)
- **Tool access:** Codebase read (to cross-reference against existing code), artifact store write
- **Failure modes:** Ambiguous input (asks for clarification), contradictory requirements (flags conflict), untestable criteria (flags for human)

**R2R illustration — today vs. structured:**

*Today:* Interview produced 15 decisions in a markdown table. Jira ticket has 14 acceptance criteria in a numbered list. Data findings live in a separate doc. No formal links between them. A build agent would need to read all 6 files and mentally reconstruct the relationships.

*Structured:* Requirements agent consumes the interview notes + data findings + reference research. Produces:
- REQ-R2R-001 (R2R Heatmap) with 14 acceptance criteria (AC-R2R-001-01 through AC-R2R-001-14)
- Each AC linked to test specs (TS-R2R-001-01-01, etc.)
- Design decisions (DD-R2R-001 through DD-R2R-015) each linked to the requirement and the specific acceptance criteria they affect
- Ambiguity flags: "AC-04 says 'cia:read users see all orgs' but AC-05 says 'cia:beta locked to their org' — what about future permission levels?"

### Section 3: Build Phase

**What context it receives + how (orchestration for this handoff):**
- Orchestrator retrieves: requirement artifact, all linked test specs, all design decisions, relevant codebase context (existing patterns from `alerts_controller.rb`, `CiaPolicy`, `ciaApi.js`)
- Context budget: estimate token count for R2R's artifact chain, demonstrate it fits in a single context window (or show where compression is needed)
- What the orchestrator does NOT include: unrelated requirements, historical artifacts from other features, full codebase

**Agent contract:**
- **Inputs:** Requirement artifact + test specs + design decisions + codebase context (from orchestrator)
- **Outputs:** Test suite (code), implementation code, self-review report, documentation artifacts — each linked back to the requirement and specific acceptance criteria
- **Decision logic:** Test-first enforcement (generates tests from specs before implementation), PR sizing (breaks work into reviewable units), convention adherence (follows existing codebase patterns)
- **Tool access:** Codebase read/write, test runner, linter, artifact store write
- **Failure modes:** Test spec that can't be implemented (flags back to requirements), convention conflict (flags for human), dependency issue (flags blocker)

**R2R illustration — today vs. structured:**

*Today:* Implementation plan has 3 sub-phases (7A, 7B, 7C) with file lists and SQL. But the plan doesn't formally link each sub-phase to specific acceptance criteria. A developer (or AI) reading the plan has to mentally trace "step 1 of 7A covers AC-01 and AC-03, step 2 covers AC-02..." The review action plan found issues (SQL injection, wrong auth method) that could have been caught if the build agent had the acceptance criteria and codebase patterns in context.

*Structured:* Build agent receives the full requirement artifact. For each acceptance criterion, it knows exactly what test to generate and what code to write. Each PR links back to specific ACs:
- PR-7A-1: Implements AC-01 (points endpoint), AC-02 (orgs endpoint), AC-03 (caching). Test suite: 14 backend specs linked to these ACs.
- PR-7B-1: Implements AC-07 (heatmap renders), AC-08 (org selection + camera fly). Test suite: component tests linked to ACs.
- Self-review report checks each AC: "AC-04 (cia:read sees all orgs): PASS — verified against CiaPolicy#full_access? pattern"

### Section 4: Review Phase

**What context it receives + how (orchestration for this handoff):**
- Orchestrator retrieves: PR diff, original requirement artifact, linked acceptance criteria, linked test specs, design decisions, build agent's self-review report, codebase conventions
- This is the richest context package — the review agent needs to verify implementation against requirements, not just check code quality
- Context budget: demonstrate what the R2R review context looks like in practice (PR diff + requirement + ACs + test specs + design decisions + self-review)

**Agent contract:**
- **Inputs:** PR + full upstream context (from orchestrator)
- **Outputs:** Structured review artifact with pass/fail per acceptance criterion, risk assessment, flagged concerns, recommendation (approve / request changes / escalate)
- **Decision logic:** Risk triage (low/medium/high/critical based on what's changed), verification against acceptance criteria (not just code quality), identification of gaps (requirement addressed but edge case missing)
- **Tool access:** Codebase read, test runner, artifact store read/write
- **Failure modes:** Context insufficient (requests more from orchestrator), ambiguous acceptance criterion (flags back to requirements), disagreement with design decision (escalates to human)

**R2R illustration — today vs. structured:**

*Today:* The action plan review found 12 issues, 2 critical (SQL injection pattern, wrong auth method). These were caught by a human (with AI assistance) reading the code against their knowledge of project patterns. The review didn't have formal access to the acceptance criteria — it checked against general best practices and codebase conventions. The SQL injection issue was a pattern violation; the auth method issue was a requirements misunderstanding (`:access?` vs `:full_access?`).

*Structured:* Review agent receives the PR + the requirement artifact (which includes DD-R2R-002: "Authorization uses CiaPolicy"). Agent checks:
- AC-04 and AC-05 (permission behavior) against the actual controller code → catches `:access?` vs `:full_access?` immediately because the acceptance criteria specify "HAAS employees" and the agent knows `:full_access?` maps to `cia:read`
- SQL construction against codebase pattern (existing `alerts_controller.rb` uses `sanitize_sql_array`) → catches the injection pattern because it has the convention in context, not just general security knowledge
- Produces: Review artifact with per-AC verification, risk triage (Critical: SQL injection, Critical: wrong auth), and specific fix recommendations linked to the ACs they affect

### Section 5: End-to-End Trace — R2R Heatmap

The full walkthrough. Every artifact, every handoff, every orchestrator action for the R2R heatmap feature from raw inputs to completed review.

**Structure:**
1. Raw inputs enter the system (interview notes, data findings, reference research)
2. Requirements agent produces structured artifacts (show the actual artifact content)
3. Orchestrator assembles context for build agent (show what's retrieved, estimate token count)
4. Build agent produces implementation + tests (show artifact links)
5. Orchestrator assembles context for review agent (show the full chain)
6. Review agent produces review findings (show per-AC verification)
7. Summary: what the pipeline caught that the current process missed (or caught later/harder)

---

## What's NOT in the Spec

- **Phases beyond the vertical slice.** No QA agent, no monitoring agent, no GTM brief accumulation. Those extend later.
- **Runtime/platform choices.** No Agent SDK vs. Claude Code vs. API decisions. Interfaces only.
- **Artifact store implementation.** The schema is defined; whether it's YAML files, SQLite, or Postgres is a build decision.
- **The full orchestrator.** The spec shows orchestration logic for 2 handoffs (requirements->build, build->review). A general-purpose orchestrator is the end state, not the starting point.

---

## Project Documentation Updates

These happen in the build session alongside the spec:

| File | Change |
|------|--------|
| `docs/plan.md` | Redefine Phase 3 as "Context Pipeline Spec." Update Phase 4+ to account for no group session. |
| `docs/status.md` | Replace Phase 3 tasks. Log this session and the build session. |
| `CLAUDE.md` | Update status line from "Phase 3 next (group session prep)" to current state. |

The Phase 2 scaffolds, baseline docs, context pipeline architecture doc, and reference docs are unchanged.

---

## Success Criteria

The spec is done when:

1. **Someone could build each agent** from the contract alone — inputs, outputs, decision logic, and tool access are specific enough to implement without guessing
2. **The artifact schema is concrete** — you could create actual YAML/JSON files conforming to it right now
3. **The R2R walkthrough is complete** — every handoff is explicit, no "and then magic happens" gaps
4. **The "today vs. structured" comparisons are honest** — they show real value, not theoretical value, by comparing against the actual R2R planning artifacts
5. **Context budgets are estimated** — rough token counts for each orchestrator retrieval, demonstrating feasibility within current model context windows

---

## Build Session Scope

The Sonnet build session executes this design:

1. Write `docs/planning/phase-3/context-pipeline-spec.md` following the 5-section structure above
2. Reference the R2R heatmap planning artifacts from `E:\Claude Code Projects\HAAS Alert\CIA Re-Write\PLANNING\` for all concrete examples
3. Update `plan.md`, `status.md`, and `CLAUDE.md`
4. Push to GitHub
