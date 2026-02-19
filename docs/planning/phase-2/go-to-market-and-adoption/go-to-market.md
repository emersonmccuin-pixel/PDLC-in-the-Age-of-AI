# Phase 9: Go-to-Market

## Principle

**Marketing materials should be a byproduct of the development process, not a post-hoc scramble.**

Traditional GTM starts after the feature ships. Marketing scrambles to understand what was built, writes copy from scratch, and launches weeks late. In the AI-native PDLC, the GTM brief has been accumulating since Phase 2 — value propositions, design decisions, screenshots, technical specs. By launch day, AI has drafts of everything. Humans refine and approve, not research and write.

---

## Pattern

### From Post-Hoc Writing to Accumulated Refinement

**Traditional flow:**
```
Feature ships  -->  Marketing learns   -->  Marketing writes    -->  Launch materials
                    what was built          from scratch             ready (weeks late)
                    (meetings, demos)       (1-3 weeks)
```

**AI-native flow:**
```
GTM brief        -->  AI drafts all    -->  Humans refine    -->  Materials ready
accumulated            materials from        & approve             at launch
since Phase 2          brief (hours)         (hours-1 day)        (or before)
```

### Process Design

1. **GTM Brief Accumulation (Phases 2-8, automatic)**
   The GTM brief is a living document that AI builds incrementally:

   | Phase | What Gets Added |
   |-------|----------------|
   | [Phase 2](../strategy-and-planning/requirements-and-specification.md) (Requirements) | User problem, value proposition, target segments, key decisions |
   | [Phase 3](../build-and-deliver/design.md) (Design) | UX rationale, screenshots, design decisions, accessibility notes |
   | [Phase 4](../build-and-deliver/implementation.md) (Build) | Technical decisions, API docs, architecture notes, integration details |
   | [Phase 6](../build-and-deliver/qa-and-testing.md) (QA) | Known limitations, edge cases, performance characteristics |
   | [Phase 7](../build-and-deliver/staging.md) (Staging) | Final screenshots, release notes draft, migration guide |

   By launch, this brief contains everything marketing needs — organized, structured, and sourced.

2. **AI Material Generation (hours, before or at launch)**
   From the accumulated GTM brief, AI drafts:
   - Blog post / announcement (long-form)
   - Changelog entry (short-form)
   - Email announcement (segmented per customer type)
   - Social media posts (platform-appropriate)
   - Help documentation / knowledge base article
   - Sales enablement deck or one-pager
   - Support FAQ and troubleshooting guide
   - Internal announcement for customer-facing teams

3. **Human Refinement (hours-1 day)**
   - Marketing reviews and refines AI drafts for voice, tone, and strategic positioning
   - Product marketing adds competitive context and positioning that AI can't infer
   - Legal reviews claims if needed (AI can flag statements that might need legal review)
   - **Key shift:** Humans are editing, not writing. The raw material is already there.

4. **Segment-Targeted Distribution (AI-assisted)**
   - AI identifies which customer segments care about this feature (from usage data, plan tier, industry)
   - AI tailors messaging per segment: enterprise gets different framing than SMB
   - Distribution channels selected per segment: in-app notification, email, blog, social, direct sales outreach
   - Support team has FAQ and troubleshooting guide before any support tickets arrive

---

## Implementation Example

**Scenario:** Dashboard customization launched. GTM brief accumulated since Phase 2.

**What happens:**

1. GTM brief at launch contains:
   - Problem: "Users waste time scrolling past irrelevant dashboard widgets"
   - Value prop: "See the metrics that matter to you — instantly"
   - Target: Enterprise customers (team defaults), all customers (individual customization)
   - Design: 4 annotated screenshots of the edit mode flow
   - Technical: No API changes, no breaking changes, localStorage + server sync
   - Limitations: Max 12 widgets, no custom widget creation (yet)
   - Migration: No action required for existing users

2. AI drafts (generated in 2 hours):
   - **Blog post:** 600 words. Problem framing, solution, screenshots, how to use it. Ready for marketing edit.
   - **Changelog:** "You can now reorder dashboard widgets via drag-and-drop. Click 'Customize' to enter edit mode. Enterprise admins can set team-level defaults."
   - **Email (Enterprise):** Focuses on team defaults and admin controls. "Give your team a consistent starting point, while letting individuals customize their view."
   - **Email (SMB):** Focuses on individual customization. "Your dashboard, your way. Drag and drop widgets to see what matters most."
   - **Help article:** Step-by-step with screenshots. Covers: entering edit mode, reordering, saving, keyboard shortcuts, team defaults (admin).
   - **Support FAQ:** "How do I customize my dashboard?" / "Can I reset to defaults?" / "Who can set team layouts?"
   - **Sales one-pager:** "Dashboard customization — personalized views for every team member" with enterprise positioning.

3. Marketing reviews (4 hours):
   - Refines blog post voice (AI draft was too technical, marketing makes it customer-friendly)
   - Adds competitive context: "Unlike [competitor], our customization works without leaving the page"
   - Approves email drafts with minor tone adjustments
   - Help article and FAQ approved as-is

4. Launch execution:
   - Blog published at 9 AM
   - Email sent to enterprise segment at 10 AM, SMB at 2 PM
   - In-app notification for all users
   - Support FAQ live before first ticket
   - Sales team briefed with one-pager

**GTM time from feature ship to full launch: 1 day of human review.** Traditional: 2-4 weeks of writing from scratch, often launching weeks after the feature is live.

---

## Current State

From the brief:
- GTM traditionally starts AFTER the feature is built
- Marketing writes from scratch — needs demos, meetings with engineering, design screenshots
- 2-4 weeks from "feature ready" to "marketing ready"
- Support team often learns about features from customer tickets, not proactive briefing
- AI role today: Some teams use AI to draft copy, but the accumulated GTM brief pattern (building marketing content as a side effect of development) is not standard practice.

---

## What Changes

| Dimension | Traditional | AI-Native |
|-----------|-------------|-----------|
| **GTM start** | After feature ships | Phase 2 (requirements) — accumulates through build |
| **Marketing input** | Meetings with engineering to understand what was built | GTM brief with structured content, screenshots, decisions |
| **Drafting** | Marketing writes from scratch (1-3 weeks) | AI drafts from brief (hours), humans refine |
| **Segmentation** | Manual segment identification | AI identifies segments from usage data and tailors messaging |
| **Support readiness** | Support learns from first tickets | FAQ and troubleshooting guide ready before launch |
| **Time to market** | 2-4 weeks after feature ships | Same day or next day after feature ships |

### Role Shifts

| Role | Traditional | AI-Native |
|------|-------------|-----------|
| **Product Marketing** | Researches what was built, writes all materials from scratch | Refines AI drafts, adds competitive positioning, manages strategic narrative |
| **Content Writer** | Writes blog posts, help docs, emails | Edits AI drafts for voice and tone, ensures brand consistency |
| **Sales Enablement** | Creates decks/one-pagers weeks after launch | Reviews AI-generated sales materials, adds field context |
| **Support Lead** | Reacts to tickets after launch | Reviews FAQ before launch, trains team proactively |

---

## Open Questions & Risks

1. **GTM brief quality.** The accumulated brief is only as good as the AI capture at each phase. If Phase 2 didn't capture value props well, the entire downstream GTM suffers. How do we ensure quality at each accumulation point?
2. **Marketing voice.** AI-generated marketing copy tends toward generic. Brand voice, emotional resonance, and creative positioning require human expertise. How much refinement is needed, and does the time savings hold if refinement is heavy?
3. **Competitive positioning.** AI can't reliably infer competitive context (what competitors do, how customers compare products). This remains a human responsibility. Where does it fit in the process?
4. **Over-communication.** If every feature ships with a blog post, email, and social campaign, customers get fatigued. Not every feature deserves a full GTM push. How do we triage?
5. **Legal review.** Some marketing claims need legal review. If AI generates claims from technical specifications, the translation may introduce inaccuracies. Who validates?
6. **Internal alignment.** Sales, support, and customer success need to be briefed before external launch. The async approach works for marketing materials but may not work for team enablement. Do we need sync briefings for major features?

---

## How to Measure Success

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **Time to market** | Days from feature ship to full GTM launch | < 2 days (vs. 2-4 weeks) |
| **Support readiness** | % of launches where support has FAQ before first ticket | 100% (vs. ~20% today) |
| **Marketing draft time** | Hours from brief to complete drafts | < 4 hours AI + 1 day human refinement |
| **Customer awareness** | % of target segment aware of new feature within 1 week | Increase by 30%+ (faster, targeted communication) |
| **Sales enablement gap** | Days between launch and sales team having materials | 0 days (materials ready at launch) |

---

## Navigation

| Previous | Overview | Next |
|----------|----------|------|
| [Phase 8: Release](../build-and-deliver/release.md) | [Phase 2 Overview](../overview.md) | [Phase 10: Monitoring & Feedback](../operate-and-evolve/monitoring-and-feedback.md) |

### Related Documents
- [Full AI-Native PDLC Brief](../../../full-pdlc-ai-native-brief.md) — GTM as a new addition beyond the original 9-phase model
