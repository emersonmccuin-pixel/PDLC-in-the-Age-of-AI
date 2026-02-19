# Phase 2: Requirements & Specification

## Principle

**Requirements should be generated from conversation, validated against the codebase, and expressed as testable contracts — not written from scratch in isolation.**

The traditional PRD is a PM writing alone for days, producing a document that engineers interpret differently. In an AI-native PDLC, the AI drafts the PRD from discovery output, cross-references it against the existing system, generates test specifications as the acceptance contract, and flags ambiguities before development begins. The PM's job shifts from writing to directing and validating.

---

## Pattern

### From Document Writing to Contract Generation

**Traditional flow:**
```
PM writes PRD  -->  Engineering reviews  -->  Feedback loop  -->  Tickets created
   (1-2 weeks)      (days to schedule)       (days-weeks)        (more days)
```

**AI-native flow:**
```
AI drafts PRD       -->  PM validates    -->  AI generates     -->  AI creates
from discovery           & refines            test specs +          tickets with
output (hours)           (same day)           GTM brief seed        full context
                                              (hours)               (automatic)
```

### Process Design

1. **AI-Drafted PRD (automatic, hours)**
   - AI synthesizes discovery output: validated problem statement, acceptance criteria, meeting notes, stakeholder decisions
   - AI drafts full PRD: user stories, acceptance criteria, technical context, edge cases
   - AI cross-references requirements against existing codebase: "this conflicts with how module X currently works," "this requires changes to the data model"
   - AI identifies ambiguities, contradictions, and gaps: "user story 3 assumes real-time data, but user story 7 implies batch processing"
   - **Output:** Draft PRD with flagged issues for human review

2. **PM Review & Refinement (human, hours-1 day)**
   - PM reviews AI-drafted PRD for accuracy, completeness, and strategic alignment
   - PM resolves flagged ambiguities — makes decisions, not research
   - PM adds business context AI can't infer: political considerations, timing constraints, strategic sequencing
   - Engineering lead reviews technical cross-references for accuracy

3. **Test Specification Generation (AI, automatic)**
   - AI generates test specifications from each acceptance criterion
   - Not test code yet — test specifications: "given X, when Y, then Z"
   - **This is the contract.** If the requirement can't produce a clear test spec, the requirement isn't clear enough.
   - Test specs are attached to each user story — they ship with the requirement, not after implementation
   - AI flags acceptance criteria that are untestable: "how do we verify 'the UI should feel intuitive'?"

4. **GTM Brief Seed (AI, automatic)**
   - AI extracts user-facing value propositions from the PRD
   - AI captures problem/solution framing in marketing-friendly language
   - AI notes key decisions and their rationale
   - **This starts the parallel marketing stream.** The GTM brief begins here and grows through Build.

5. **Ticket Creation (AI, automatic)**
   - AI creates Jira epics/stories from the PRD
   - Each ticket includes: user story, acceptance criteria, test specifications, technical context from codebase cross-reference
   - AI suggests story point estimates based on codebase complexity analysis
   - AI identifies dependencies between tickets and sequences them

---

## Implementation Example

**Scenario:** Continuing the dashboard customization feature from Phase 1. Discovery produced a validated problem statement, scoping decisions, and initial acceptance criteria.

**What happens:**

1. AI drafts PRD within 2 hours of discovery completion:
   - Converts acceptance criteria into full user stories with edge cases
   - Cross-references against codebase: "Dashboard component uses a fixed grid layout in `src/components/Dashboard/Grid.tsx`. Drag-and-drop requires replacing this with a dynamic grid library."
   - Flags: "Acceptance criterion says 'admin can set team defaults' but the current user model has no team/admin concept. This requires a data model change."
   - Generates 6 user stories with 23 acceptance criteria

2. PM reviews in 2 hours:
   - Confirms the data model gap — decides to add a simple team concept scoped to dashboard only (not a full RBAC system)
   - Adds business context: "Enterprise customers are the target. SMB customers get individual customization only — no team features."
   - Resolves 3 ambiguity flags

3. AI generates test specifications:
   - "Given a user with a customized widget order, when they log in on a new device, then their custom order is preserved"
   - "Given a team admin, when they set a default layout, then new team members see the default layout on first login"
   - "Given a user with a custom layout and an admin who changes the team default, then the user's custom layout is unchanged"
   - 23 acceptance criteria produce 41 test specifications

4. AI seeds the GTM brief:
   - Value prop: "See the metrics that matter to you — instantly"
   - Problem framing: "Users waste time scrolling past irrelevant data"
   - Key decision: "Enterprise gets team defaults; SMB gets individual customization"

5. AI creates 8 Jira tickets:
   - Epic: "Dashboard Widget Customization"
   - Stories include technical context, test specs, and sequencing: "Story 3 (team defaults) is blocked by Story 1 (data model change)"

**Total time:** 1 day. Traditional: 1-3 weeks.

---

## Current State

From [Phase 1 baseline](../../baseline/pdlc-standard-mapping.md):
- **Typical time:** 1-3 weeks
- **Bottlenecks:** PRD review cycles, requirement ambiguity, ticket review/commenting (can take DAYS just for PM review)
- **Key roles:** Product Manager, Tech Lead, Designer, Engineering Manager
- **AI role today:** Some teams use AI to draft PRDs, but cross-referencing against the codebase, test spec generation, and automated ticket creation are rare.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **PRD creation** | PM writes from scratch (days-weeks) | AI drafts from discovery output, PM validates (hours) |
| **Codebase awareness** | Engineer reads PRD, mentally maps to codebase | AI cross-references requirements against actual code, flags conflicts |
| **Ambiguity detection** | Found during development ("what does this mean?") | AI flags before development starts |
| **Test specifications** | Written by QA after build, or not at all | Generated from acceptance criteria before build — the contract |
| **Marketing input** | Starts after feature ships | GTM brief seeds here, grows through build |
| **Ticket creation** | PM manually creates, engineers add context | AI generates with full context (stories, acceptance criteria, test specs, codebase references) |
| **Estimates** | Planning poker, gut feel | AI suggests based on codebase complexity analysis |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Product Manager** | Writes PRD from scratch, creates tickets, runs review cycles | Directs AI, validates PRD, makes strategic decisions, resolves ambiguities |
| **Tech Lead** | Reviews PRD for feasibility, estimates effort | Validates AI codebase cross-references, reviews technical recommendations |
| **QA Engineer** | Writes test plans later ([Phase 6](06-qa-and-testing.md)) | Reviews test specifications now (Phase 2) — shift-left |
| **Designer** | Receives requirements, designs from spec | Co-reviews PRD, ensures requirements are designable |

---

## Open Questions & Risks

1. **Codebase cross-reference accuracy.** AI can misunderstand codebase architecture. A wrong cross-reference is worse than no cross-reference — it creates false confidence. How do we validate?
2. **Test spec granularity.** Too granular and they're brittle (change with every design tweak). Too high-level and they don't serve as contracts. What's the right level?
3. **PM skill evolution.** If AI writes the PRD, what skills do PMs need? The role shifts from writing to directing, validating, and making judgment calls. Is this a net positive or does it deskill the role?
4. **Ticket quality vs. quantity.** AI can generate many well-structured tickets fast. But more tickets doesn't mean better planning. How do we avoid ticket proliferation?
5. **Cross-reference scope.** Should AI cross-reference against the entire codebase or just the relevant domain? Full codebase is more thorough but slower and noisier.

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Spec cycle time** | Days from discovery completion to approved PRD + tickets | < 2 days (vs. 1-3 weeks) |
| **Ambiguities found pre-build** | % of requirement ambiguities caught before development | > 80% (vs. ~30% today) |
| **Test spec coverage** | % of acceptance criteria with corresponding test specifications | 100% at requirements exit |
| **Requirement stability** | % of requirements unchanged through build | > 85% (better upfront spec = less churn) |
| **Ticket completeness** | % of tickets with acceptance criteria, test specs, and technical context | 100% (vs. ~40% today) |
| **Rework rate** | % of build work that requires re-implementation due to requirement issues | Decrease by 40%+ |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 1: Ideation & Discovery](01-ideation-and-discovery.md) | [Phase 2 Overview](overview.md) | [Phase 3: Design](03-design.md) |

### Related Documents
- [Context Pipeline Architecture](../../context-pipeline-architecture.md) — how requirements flow to downstream phases
