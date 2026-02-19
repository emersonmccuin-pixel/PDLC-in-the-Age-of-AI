# Phase 1: Ideation & Discovery

## Principle

**Discovery should start with synthesis, not a blank page.**

Traditional ideation begins with "let's schedule a brainstorming session" — which means aligning calendars, sitting in a room, and generating ideas from scratch. In an AI-native PDLC, the AI has already done the research before any human meeting happens. Humans react to synthesized insights, not raw data. The creative work shifts from "what's the problem?" to "is this the right framing of the problem?"

---

## Pattern

### From Calendar-Gated Research to AI-Primed Discovery

**Traditional flow:**
```
Schedule meeting  -->  Brainstorm  -->  Research  -->  Synthesize  -->  Align
    (days)            (1 meeting)      (1-2 weeks)    (days)          (more meetings)
```

**AI-native flow:**
```
AI pre-research     -->  Humans react     -->  AI refines    -->  Validated
(automatic,              & redirect            (same day)        problem statement
 hours)                  (1 session)                              + test hypotheses
```

### Process Design

1. **AI Pre-Research (automatic, triggered by portfolio approval)**
   - AI scans existing user feedback (support tickets, NPS comments, survey responses) related to the initiative
   - AI analyzes competitor approaches to the same problem space
   - AI pulls usage data around the problem area (where do users drop off? What workarounds exist?)
   - AI identifies conflicting stakeholder needs from prior meeting notes and documents
   - AI generates 2-3 initial problem statement drafts with supporting evidence
   - **Output:** A pre-research brief that humans review before any meeting

2. **Human Discovery Session (focused, 1-2 hours)**
   - Team reacts to AI-generated problem statements — validate, redirect, or reject
   - Focus on judgment: "Is this actually the problem, or a symptom?"
   - AI captures decisions in real-time, updates the brief live
   - Stakeholders who can't attend review the async brief and comment

3. **AI-Generated Hypotheses and Acceptance Criteria**
   - From the validated problem statement, AI generates testable hypotheses
   - AI drafts initial acceptance criteria: what does "solved" look like?
   - **Shift-left testing starts here:** Before anyone writes a requirement, we know what "done" looks like
   - AI flags hypotheses that are untestable or too vague

4. **Synthetic User Research (augments, doesn't replace real research)**
   - AI runs synthetic interviews against personas built from real behavioral data
   - AI identifies likely edge cases and user segments that would react differently
   - Human researchers validate with real users where the stakes are high
   - For lower-stakes features, synthetic research may be sufficient

---

## Implementation Example

**Scenario:** Product team at a B2B SaaS company wants to explore adding a dashboard customization feature. Initiative was approved through Phase 0 continuous prioritization.

**What happens:**

1. AI pre-research runs overnight after portfolio approval:
   - Pulls 47 support tickets mentioning "dashboard," "customize," or "layout" from the last 6 months
   - Finds 3 competitor screenshots showing their customization approaches
   - Shows usage data: 68% of users only look at 2 of the 8 default dashboard widgets
   - Surfaces a Slack thread from 4 months ago where 3 customers asked for this in a sales call
   - Generates problem statement: "Users waste time scrolling past irrelevant widgets to find the 2-3 metrics they actually monitor daily"

2. Product team meets for 90 minutes:
   - Reviews AI brief, agrees the problem statement is right
   - Adds nuance: "Enterprise customers want team-level defaults, not just individual customization"
   - Decides to scope down: drag-and-drop widget reordering first, full customization later
   - AI captures all decisions, updates the brief

3. AI generates acceptance criteria:
   - "User can reorder widgets via drag-and-drop"
   - "Widget order persists across sessions"
   - "Admin can set team-level default layouts"
   - "User customization overrides team defaults"
   - AI generates test hypotheses: "We expect dashboard engagement time to decrease by 20%+ as users find relevant data faster"

**Total time:** 1 day (AI pre-research overnight, human session next morning, acceptance criteria by afternoon). Traditional: 2-3 weeks.

---

## Current State

From Phase 1 baseline:
- **Typical time:** 1-4 weeks
- **Bottlenecks:** Scheduling alignment meetings, consensus-building, unclear ownership
- **Key roles:** Product Manager, UX Researcher, Business Stakeholders
- **AI role today:** Minimal. Some teams use AI to summarize meeting notes, but discovery is overwhelmingly human-driven and calendar-gated.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Starting point** | Blank page, brainstorming session | AI-generated pre-research brief with data and draft problem statements |
| **Research time** | 1-2 weeks of human research | Hours of AI synthesis + targeted human validation |
| **Meeting count** | 3-5 meetings over weeks | 1 focused session reacting to AI brief |
| **Acceptance criteria** | Written later, during requirements | Generated here, during discovery — shift-left |
| **User research** | All human-conducted, weeks to schedule | AI synthetic interviews for initial signal, human research for validation |
| **Stakeholder alignment** | Multiple sync meetings | Async review of AI brief + 1 focused sync session |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Product Manager** | Conducts research, writes problem statements, schedules alignment meetings | Reviews AI pre-research, directs discovery focus, makes scoping decisions |
| **UX Researcher** | Plans and executes all user research from scratch | Designs research strategy, validates AI synthetic findings, conducts high-stakes interviews |
| **Stakeholders** | Attend multiple meetings, provide input piecemeal | Review async AI brief, attend 1 focused session, comment on acceptance criteria |

---

## Open Questions & Risks

1. **Synthetic user research quality.** AI personas built from behavioral data are only as good as the data. For novel problem spaces with no historical data, synthetic research is unreliable. Where's the line?
2. **Over-reliance on existing data.** AI pre-research biases toward problems that already have data signals. Truly innovative features (that users haven't asked for yet) won't show up in support tickets or usage analytics.
3. **Premature convergence.** If AI generates compelling problem statements, teams may accept them too quickly without exploring alternative framings. The AI brief should provoke thinking, not replace it.
4. **Stakeholder engagement.** Will stakeholders actually read the async AI brief? Or will they still demand live meetings? Change management is the hard part.
5. **Acceptance criteria too early?** Generating acceptance criteria at discovery means they may change during requirements. Is that wasted work or useful scaffolding?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Discovery cycle time** | Days from initiative approval to validated problem statement | < 3 days (vs. 1-4 weeks) |
| **Meetings per discovery** | Number of sync meetings needed | 1-2 (vs. 3-5) |
| **Requirement rework rate** | % of requirements that change significantly after discovery | Decrease by 30%+ (better discovery = fewer surprises) |
| **Acceptance criteria coverage** | % of features with testable acceptance criteria at discovery exit | 100% (vs. ~30% today) |
| **Problem statement accuracy** | % of shipped features where the original problem statement held true | > 80% |
