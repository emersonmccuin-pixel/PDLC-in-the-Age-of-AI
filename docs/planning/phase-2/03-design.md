# Phase 3: Design

## Principle

**Design should be explored through rapid generation and human curation, not slow iteration from a single concept.**

Traditional design is one designer producing one concept, iterating through review loops for weeks. In an AI-native PDLC, AI generates multiple design variants in hours — wireframes, interaction specs, accessibility audits — and humans curate: selecting, combining, and refining. The designer's role shifts from production to direction.

---

## Pattern

### From Linear Iteration to Parallel Exploration

**Traditional flow:**
```
Designer creates   -->  Review    -->  Revise  -->  Review  -->  Final specs
1 concept (days)       meeting        (days)       meeting      + handoff
                       (wait)                      (wait)
```

**AI-native flow:**
```
AI generates        -->  Designer curates  -->  AI generates     -->  Design specs
multiple variants        & directs              interaction +         + test specs
(hours)                  (same day)             accessibility         + GTM update
                                                specs (hours)         (automatic)
```

### Process Design

1. **AI-Generated Design Variants (hours)**
   - AI produces 3-5 wireframe-level layout options from the PRD and acceptance criteria
   - Each variant represents a different UX approach (not just color variations — structural alternatives)
   - AI checks each variant against the existing design system for consistency
   - AI flags accessibility issues per variant (contrast, touch targets, screen reader flow)
   - **Output:** Multiple options with tradeoff annotations for human evaluation

2. **Designer Curation & Direction (human, hours-1 day)**
   - Designer reviews variants, selects elements from each, directs AI to combine and refine
   - Focus on judgment: "This layout works for power users but will overwhelm new users — let's split the flow"
   - Stakeholder input is async: share variants with annotations, collect feedback in comments
   - No multi-week review loop — decisions are made in a single focused session

3. **Interaction & Accessibility Specifications (AI, hours)**
   - From the selected design direction, AI generates full interaction specs: "when user clicks X, Y happens"
   - AI produces accessibility audit: WCAG compliance check, keyboard navigation flow, screen reader behavior
   - AI checks against existing UI patterns in the codebase: "this interaction pattern exists in module Z — reuse or diverge?"

4. **Test Specification Generation (AI, automatic)**
   - AI generates UI test specifications from design specs:
     - Visual regression baselines
     - Interaction test scenarios
     - Accessibility test criteria
     - Responsive behavior expectations
   - **Shift-left continues:** These test specs are ready before a line of frontend code is written

5. **GTM Brief Update (AI, automatic)**
   - AI adds to the running GTM brief: UX rationale, key design decisions, screenshots/mockups for marketing use
   - Marketing now has visual assets and UX narrative without waiting for launch

---

## Implementation Example

**Scenario:** Dashboard customization feature. PRD specifies drag-and-drop widget reordering with team defaults.

**What happens:**

1. AI generates 4 wireframe variants:
   - **Variant A:** Drag handles on each widget, reorder in-place
   - **Variant B:** Edit mode toggle — enter edit mode, rearrange freely, save
   - **Variant C:** Sidebar panel showing available widgets, drag from panel to dashboard
   - **Variant D:** Grid-based layout with snap-to-grid positioning
   - Each variant includes tradeoff notes: "Variant A is simplest but doesn't support adding/removing widgets. Variant C supports adding/removing but adds UI complexity."

2. Designer reviews in 2 hours:
   - Selects Variant B (edit mode) as the base — cleanest UX for reordering
   - Borrows the sidebar concept from Variant C for future widget addition (scoped out for now but architecture should support it)
   - Directs AI: "Refine Variant B with edit mode. Show a clear visual distinction between view mode and edit mode. Widgets should show drag handles and a subtle grid overlay in edit mode."

3. AI generates interaction specs:
   - "User clicks 'Customize' button → dashboard enters edit mode → widgets show drag handles → grid overlay appears"
   - "User drags widget → ghost preview shows target position → drop snaps to grid → order saved on exit"
   - "User clicks 'Done' or presses Escape → edit mode exits → new order persists"
   - Accessibility: "Edit mode is keyboard-accessible via arrow keys. Screen reader announces 'Widget X, position Y of Z. Use arrow keys to move.'"

4. AI generates UI test specs:
   - "Visual: Edit mode toggle shows distinct visual state (overlay, handles visible)"
   - "Interaction: Drag widget from position 1 to position 3 → widget order updates → positions 2 and 3 shift"
   - "Accessibility: Tab to Customize button → Enter → Tab through widgets → Arrow keys reorder → Escape exits"
   - "Responsive: On mobile viewport, edit mode uses long-press instead of drag handle"

**Total time:** 1 day. Traditional: 1-3 weeks.

---

## Current State

From [Phase 1 baseline](../../baseline/pdlc-standard-mapping.md):
- **Typical time:** 1-3 weeks
- **Bottlenecks:** Design review loops, stakeholder feedback cycles, pixel-level debates
- **Key roles:** UX Designer, UI Designer, Product Manager, Frontend Engineer
- **AI role today:** Some designers use AI for image generation or layout suggestions, but AI-generated wireframes from requirements, automated accessibility audits, and test spec generation from designs are rare.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **Concept exploration** | 1 concept, iterated over weeks | 3-5 variants generated in hours, human curates |
| **Review process** | Multiple sync meetings over weeks | Async variant review + 1 focused curation session |
| **Accessibility** | Audited late (if at all) | Checked per variant during generation |
| **Design system compliance** | Manual check | AI validates against design system automatically |
| **Interaction specs** | Designer writes manually, often incomplete | AI generates comprehensive specs from design direction |
| **Test specs** | Written by QA during [Phase 6](06-qa-and-testing.md) | Generated here from design specs — shift-left |
| **Marketing assets** | Created post-launch | Screenshots and UX narrative added to GTM brief now |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **UX Designer** | Creates concepts from scratch, iterates through review | Curates AI-generated variants, directs refinement, makes UX judgment calls |
| **UI Designer** | Pixel-perfect production from approved wireframes | Ensures design system compliance, refines AI output to production quality |
| **Product Manager** | Reviews designs in meetings, provides feedback over days | Reviews variants async, makes scoping decisions in hours |
| **Frontend Engineer** | Receives final designs, interprets interaction behavior | Reviews interaction specs and UI test specs early — catches implementability issues before build |

---

## Open Questions & Risks

1. **AI design quality.** Current AI wireframe generation is improving but not production-ready for complex UIs. Is the "3-5 variants" pattern realistic today, or aspirational?
2. **Designer identity.** If AI generates the initial concepts, designers may feel their creative role is diminished. The reframe: AI handles production, designers handle direction and judgment. Will designers accept this?
3. **Design system drift.** AI checking against design systems requires the design system to be machine-readable. Most design systems are Figma files, not structured data. What's the path to machine-readable design systems?
4. **Over-generation.** More variants isn't always better. 5 mediocre options can be harder to evaluate than 1 good one. How do we balance exploration breadth with decision speed?
5. **Stakeholder variant paralysis.** Showing stakeholders 4 options instead of 1 might slow decisions ("can we combine A and C?"). How do we structure variant presentations to drive decisions, not debates?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Design cycle time** | Days from PRD approval to final design specs | < 2 days (vs. 1-3 weeks) |
| **Review iterations** | Number of design review rounds | 1-2 (vs. 3-5) |
| **Design-to-build gap** | % of design intent lost in translation to code | Decrease by 50%+ (interaction specs close the gap) |
| **Accessibility compliance** | % of designs that pass WCAG AA on first build | > 95% (vs. ~60% today) |
| **UI test spec coverage** | % of UI interactions with test specifications at design exit | 100% |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 2: Requirements & Specification](02-requirements-and-specification.md) | [Phase 2 Overview](overview.md) | [Phase 4: Implementation](04-implementation.md) |

### Related Documents
- [PDLC Standard Mapping](../../baseline/pdlc-standard-mapping.md) — traditional design phase baseline
