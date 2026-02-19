# The Context Pipeline Problem

## The Core Insight

The Phase 2 solution scaffolds describe what AI should do at each phase in isolation. But the AI-native PDLC only works if context flows between phases. No single AI system can hold the full context of a 14-phase lifecycle spanning weeks or months. The real engineering challenge isn't "what should AI do at each phase?" — it's "how does context move between phases when every AI interaction starts from near-zero?"

This is the hardest unsolved problem in the AI-native PDLC, and arguably the project's most important contribution if we solve it.

---

## Why This Is Hard

### The Three Layers of Context

**1. Within a phase — manageable today.**
Claude Code with a good CLAUDE.md and file context handles a single phase's work. An AI agent doing code review can read the PR, the codebase, and the project conventions. This works.

**2. Between adjacent phases — hard but solvable.**
Phase 4 (build) needs context from Phase 3 (design). This is structured artifact handoff — the output of one phase becomes the input of the next. If you design artifacts to be machine-readable, an AI agent can consume them. The challenge is defining the artifact schema and ensuring every phase produces outputs the next phase can consume.

**3. Across the full lifecycle — this breaks everything.**
When Phase 10 (monitoring) detects a production issue, the AI needs to trace it back: What was the original requirement? What design decision led to this implementation? What did QA test? What edge cases were accepted as known limitations? That's a context chain spanning months and thousands of artifacts.

No AI model has a context window large enough for this. And even if it did, dumping everything in wouldn't work — the signal-to-noise ratio would be terrible.

### Why Existing Tools Don't Solve This

Individual AI tools exist for individual phases:

| Phase | Existing Tools | What They Know |
|-------|---------------|----------------|
| Code review (Phase 5) | Greptile, CodeRabbit, GitHub Copilot Review | The PR diff and the codebase |
| Testing (Phase 6) | Codium, Qodo | The code and test patterns |
| Monitoring (Phase 10) | Datadog AI, PagerDuty AI | Metrics and logs |
| Requirements (Phase 2) | Various AI writing tools | Whatever you paste in |

**The problem:** None of them talk to each other. The code review tool doesn't know the original requirement. The testing tool doesn't know the design decisions. The monitoring tool doesn't know what was tested and what wasn't. Each tool starts from scratch with whatever's immediately in front of it.

The AI-native PDLC requires these tools to share context. Not all context — the right context for the task at hand.

---

## The Architecture

### Overview

It's not one AI system. It's three components:

```
+------------------------------------------------------------------+
|                                                                  |
|  STRUCTURED ARTIFACT STORE                                       |
|  The source of truth across all phases                           |
|                                                                  |
|  Every phase produces machine-readable artifacts with            |
|  stable identifiers that link to artifacts from other phases     |
|                                                                  |
+------------------------------------------------------------------+
         |              |              |              |
         v              v              v              v
+------------+  +------------+  +------------+  +------------+
|  Phase     |  |  Phase     |  |  Phase     |  |  Phase     |
|  Agent     |  |  Agent     |  |  Agent     |  |  Agent     |
|            |  |            |  |            |  |            |
|  Gets ONLY |  |  Gets ONLY |  |  Gets ONLY |  |  Gets ONLY |
|  the       |  |  the       |  |  the       |  |  the       |
|  context   |  |  context   |  |  context   |  |  context   |
|  it needs  |  |  it needs  |  |  it needs  |  |  it needs  |
|  for THIS  |  |  for THIS  |  |  for THIS  |  |  for THIS  |
|  task      |  |  task      |  |  task      |  |  task      |
+------------+  +------------+  +------------+  +------------+
         ^              ^              ^              ^
         |              |              |              |
+------------------------------------------------------------------+
|                                                                  |
|  ORCHESTRATOR                                                    |
|                                                                  |
|  Knows the phase graph. Knows which artifacts exist.             |
|  When a phase agent needs context, the orchestrator retrieves    |
|  the RIGHT artifacts and compresses them into the agent's        |
|  context window.                                                 |
|                                                                  |
|  Context retrieval, not context storage.                         |
|                                                                  |
+------------------------------------------------------------------+
```

### Component 1: Structured Artifact Store

Every phase produces artifacts. Today, those artifacts are unstructured: Google Docs, Jira tickets, Slack threads, PRs, dashboards. The AI-native PDLC requires them to be structured and linked.

**Example artifact chain for a single feature:**

```
Requirement REQ-042: "Users can reorder dashboard widgets"
  |
  +-- Acceptance Criteria AC-042-01 through AC-042-06
  |     |
  |     +-- Test Specs TS-042-01 through TS-042-23
  |           |
  |           +-- Test Suite (code) tests/dashboard/widget-order/*.test.ts
  |
  +-- Design Decision DD-042: "Edit mode with drag-and-drop"
  |     |
  |     +-- UI Test Specs UTS-042-01 through UTS-042-08
  |
  +-- Implementation PR-1847, PR-1852, PR-1855
  |     |
  |     +-- AI Self-Review SR-1847, SR-1852, SR-1855
  |     +-- Human Review HR-1847, HR-1852, HR-1855
  |
  +-- QA Result QA-042: "Passed. 1 minor (50-widget perf), 1 UX enhancement"
  |
  +-- Deploy DEPLOY-329: "Canary 100%, all metrics green"
  |
  +-- Health HEALTH-042: "Week 3: 41% adoption, P95 680ms (P1 filed)"
  |
  +-- GTM Brief GTM-042: "Value prop, screenshots, FAQ, sales one-pager"
```

**Key properties:**
- Every artifact has a stable identifier
- Every artifact links to its parent artifacts (traceability)
- Artifacts are machine-readable (not PDFs or Google Docs)
- The store is append-only — history is preserved, nothing is overwritten

**What this could be in practice:**
- A database (Postgres, SQLite) with a defined schema
- A set of structured markdown/YAML files in a git repo
- A purpose-built tool (doesn't exist yet)
- Jira + GitHub + a linking layer (ugly but possible)

### Component 2: Phase Agents

Each phase has one or more AI agents that do the work. These are not general-purpose assistants — they're specialized for their phase and receive curated context.

**What a phase agent looks like:**
- A Claude agent (via Agent SDK, Claude Code, or API) with a specific system prompt
- Access to relevant tools (MCP servers for Jira, GitHub, analytics, etc.)
- A curated context package assembled by the orchestrator
- Produces structured output that goes back into the artifact store

**Example: Code Review Agent (Phase 5)**
- **Receives from orchestrator:** The PR diff, the original requirement (REQ-042), the acceptance criteria (AC-042-*), the test specs (TS-042-*), the design decision (DD-042), and the project conventions
- **Does:** Pre-review against all of those — not just linting, but "does this implementation match the requirement? Are all acceptance criteria addressed? Do the tests cover the specs?"
- **Produces:** Structured review artifact (SR-XXXX) with pass/fail on each criterion, risk assessment, and highlighted areas for human review
- **Does NOT receive:** Every artifact in the system, the full codebase history, unrelated features

### Component 3: The Orchestrator

This is the hard part. The orchestrator is the brain that connects everything.

**What it does:**
1. **Knows the phase graph.** Understands which phases produce which artifacts, and which phases consume them.
2. **Knows what artifacts exist.** Maintains an index of all artifacts, their relationships, and their status.
3. **Retrieves the right context.** When a phase agent needs to run, the orchestrator:
   - Identifies which artifacts are relevant to this specific task
   - Retrieves them from the artifact store
   - Compresses/summarizes them to fit the agent's context window
   - Packages them into a context payload for the agent
4. **Routes outputs.** When a phase agent produces output, the orchestrator stores it as a structured artifact and triggers downstream phases if needed.

**The context retrieval problem in detail:**

This is not generic RAG (retrieve whatever's semantically similar). This is structured retrieval:

```
Task: "Review PR-1847 for feature REQ-042"

Orchestrator thinks:
  1. PR-1847 implements REQ-042
  2. REQ-042 has acceptance criteria AC-042-01 through AC-042-06
  3. AC-042-* produced test specs TS-042-01 through TS-042-23
  4. REQ-042 has design decision DD-042
  5. The review agent needs: PR diff + requirement + acceptance criteria
     + test specs + design decision + project conventions
  6. Total context: ~8K tokens. Fits in window. No compression needed.

Orchestrator retrieves and packages context.
Review agent runs with full relevant context.
```

For simple cases, this is straightforward graph traversal. For complex cases (cross-feature impact analysis, production incident tracing across months of changes), it requires summarization and intelligent context compression.

**What this could be in practice:**
- A Python/TypeScript application using the Claude Agent SDK
- Uses the artifact store as its database
- Maintains a phase graph (which phases connect to which)
- Implements context retrieval as graph traversal + summarization
- Triggers phase agents via API calls

---

## Where to Start Building

### Don't Build the Orchestrator First

The orchestrator is the end state. Building it first means building infrastructure with no validated use case. Instead:

### Build One Vertical Slice

Pick one real feature and connect 2-3 adjacent phases with structured artifacts.

**The smallest useful prototype:**

```
Phase 2 (Requirements)  -->  Phase 4 (Build)  -->  Phase 5 (Review)
                         |                     |
                  Structured artifact:    Structured artifact:
                  - Requirement ID        - PR + requirement ref
                  - Acceptance criteria    - Test results
                  - Test specifications    - Self-review report
```

**Concrete steps:**

1. **Define a minimal artifact schema.** A requirement artifact with: ID, description, acceptance criteria (each with ID), test specifications (each linked to an acceptance criterion). Start with YAML or structured markdown — don't over-engineer the store.

2. **Build a requirements agent.** Takes rough notes / conversation transcript as input. Produces a structured requirement artifact. Uses Claude API or Claude Code.

3. **Build a test-generation agent.** Takes the requirement artifact as input. Produces test specifications and/or actual test code. Validates that every acceptance criterion has at least one test.

4. **Build a review agent.** Takes a PR diff + the original requirement artifact as input. Checks: Does the implementation address all acceptance criteria? Do tests cover the specs? Produces a structured review report.

5. **Wire them together.** The output of step 2 feeds into step 3. The output of steps 2 and 3 feeds into step 4. This wiring IS the embryonic orchestrator.

**What you learn from this:**
- What does the artifact schema actually need to contain?
- How much context compression is needed at each handoff?
- Where does the structured approach break down in practice?
- What's the right granularity for artifacts (too detailed = noise, too abstract = useless)?

### Then Extend

Once the 3-phase slice works, extend in both directions:
- Backward: Add Phase 1 (discovery) feeding into requirements
- Forward: Add Phase 6 (QA) consuming test specs and build artifacts
- Sideways: Add the GTM brief accumulation as a parallel stream

Each extension validates the artifact schema and forces you to solve new context retrieval problems.

---

## Open Questions

### Schema Design
1. **What's the right artifact format?** YAML, JSON, structured markdown, database records? Trade-off: human-readable vs. machine-parseable vs. version-controllable.
2. **How granular?** One artifact per requirement? Per acceptance criterion? Per test spec? Granularity affects both usefulness and complexity.
3. **How do artifacts version?** Requirements change. Design decisions evolve. How do we track which version of a requirement the implementation was built against?

### Context Retrieval
4. **How do we handle context that doesn't fit?** A complex feature might have 50 acceptance criteria, 200 test specs, and 30 PRs. That's too much for a context window. What's the summarization strategy?
5. **How do we handle cross-feature context?** Feature A's monitoring detects an issue that was introduced by Feature B's implementation. The context chain crosses feature boundaries.
6. **How stale is too stale?** Artifacts from 6 months ago may not reflect current system state. How do we handle context decay?

### Orchestration
7. **Push vs. pull?** Does the orchestrator push context to agents when phases trigger? Or do agents pull context when they need it?
8. **Human-in-the-loop.** At what points does the orchestrator need human approval before triggering the next phase? This maps to the trust calibration question from the solution scaffolds.
9. **Failure handling.** What happens when a phase agent produces garbage? How does the orchestrator detect and recover?

### Practical
10. **What's the MVP tool stack?** Claude API + structured files in git? Claude Agent SDK + SQLite? Full platform with API and UI?
11. **How does this integrate with existing tools?** Most teams already have Jira, GitHub, Slack, APM. The context pipeline needs to work with these, not replace them.
12. **Who maintains the artifact schema?** If the schema is wrong or incomplete, the whole pipeline breaks. Who owns it?

---

## Relationship to Phase 2 Scaffolds

The Phase 2 solution scaffolds describe **what each phase does**. This document describes **how they connect**. Both are needed:

- The scaffolds tell you what a Phase 5 (code review) agent should check and how it should triage
- This document tells you what context that agent needs, where it comes from, and how it gets there

The scaffolds are the nodes. The context pipeline is the edges. Without the edges, you just have 14 isolated AI tools that don't know about each other — which is what exists today.

---

## What This Means for the Project

This reframes the project's most valuable output. The Phase 2 scaffolds are useful for group discussion — they describe the vision. But the thing worth actually building is:

1. **The artifact schema** — a standard for how PDLC artifacts are structured and linked
2. **The context retrieval patterns** — how to assemble the right context for each phase agent
3. **A working prototype** — one vertical slice (requirements → build → review) with real context flowing between phases

The group discussion (Phase 3) should include this framing. The question for the group isn't just "which phases should we redesign?" — it's "how do we solve the context pipeline problem that makes any of this actually work?"

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
