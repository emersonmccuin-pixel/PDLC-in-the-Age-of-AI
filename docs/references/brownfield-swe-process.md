# Brownfield AI-Assisted SWE Process

*A brownfield AI-assisted SWE process for production applications (Rails API + React frontend), adapted from the greenfield Claude Code SWE Process.*

---

## Quick Reference

```
UNDERSTAND EXISTING → DEFINE SCOPE → PATTERN RESEARCH → PHASED PLAN → BUILD LOOP
```

**Key Difference from Greenfield:** You're not designing from scratch. You're extending an existing system with established patterns, conventions, and constraints.

**Required Artifacts:**

| Phase | Artifact | Purpose |
|-------|----------|---------|
| 1 | `SCOPE.md` | Define what you're building (replaces PROBLEMS.md) |
| 2 | `PATTERNS.md` | Document existing patterns to follow |
| 3 | `PHASE-N-*.md` | Detailed plan per phase |
| 4 | `STATUS.md` | Track progress, enable session handoff |

**External Systems:**
- Jira tickets - required for all work
- Pull requests - required for merging
- CI/CD pipeline - must pass before merge

**Skills Available:**
- `/jira-ticket-creator` - Interactive ticket creation following project conventions
- `/start-feature` - Creates Jira ticket + branches in both repos
- `/commit-feature` - Commits with Jira reference
- `/finish-feature` - Push, create PRs, update Jira

---

## Phase 0: Understand Existing System

**Purpose:** Before building anything, understand what already exists.

### When Starting a New Feature Area

1. **Explore the codebase** (use Explore agent or manual search)
   - How is similar functionality implemented?
   - What patterns are used for controllers, policies, services?
   - What's the test coverage like for similar features?

2. **Document what you find** in `PATTERNS.md`

3. **Identify constraints**
   - What can't you change? (APIs in production, DB schema dependencies)
   - What must you use? (Existing auth, existing UI components)

### Codebase Research Questions

**Backend (Rails):**
```
- How are similar controllers structured?
- What's the policy pattern? (ActionPolicy)
- How are services organized?
- What serializers exist?
- How are routes namespaced?
- What's the test pattern? (RSpec)
```

**Frontend (React):**
```
- How are similar pages structured?
- What state management pattern? (Redux Toolkit)
- What UI components exist?
- How is routing handled?
- How are API calls made?
- What's the test pattern? (Jest/RTL)
```

### Output: `PATTERNS.md`

```markdown
# Existing Patterns

## Backend Patterns

### Controllers
- Inherit from `Api::ApiController`
- Use `doorkeeper_authorize!` (automatic via inheritance)
- Use `authorize!` for policy checks
- Return JSON via serializers

### Policies
- Inherit from `ApplicationPolicy`
- Use `permissioned?(['permission:name'])` for checks
- Convention: `{Model}Policy` with `{action}?` methods

### Services
- Located in `app/services/`
- Pattern: [describe what you find]

### Tests
- RSpec in `spec/`
- Request specs for API endpoints
- Use `authenticated_header(user)` helper

## Frontend Patterns

### Pages
- Located in `src/pages/`
- Pattern: [describe what you find]

### API Calls
- Using [Redux Toolkit Query / axios / etc]
- Pattern: [describe what you find]

### Components
- Located in `src/components/`
- Pattern: [describe what you find]

## Authentication/Authorization

### Permissions
- Format: `domain:action` (e.g., `metrics:read`)
- Checked via: `permissioned?(['permission:name'])`
- Assigned to: Roles

### Existing Relevant Permissions
- [List permissions related to your feature]

## Database

### Relevant Tables
- [List tables you'll interact with]

### Migration Patterns
- [How are migrations structured in this codebase]
```

---

## Phase 1: Define Scope

**Purpose:** Clearly define what you're building and what you're NOT building.

### Create: `SCOPE.md`

```markdown
# Feature Scope: [Feature Name]

## Summary
[1-2 sentences: What are we building?]

## Background
[Why is this needed? Link to existing systems being replaced/extended]

## Requirements

### Must Have (MVP)
1. [Requirement - specific and testable]
2. [Requirement - specific and testable]

### Nice to Have (Future)
1. [Enhancement for later]

### Non-Goals (Explicitly Out of Scope)
- [What we're NOT doing]
- [Related feature we're deferring]

## Affected Systems

### Backend Changes
- [ ] New endpoints: [list]
- [ ] New policies: [list]
- [ ] Database changes: [yes/no, describe]
- [ ] New services: [list]

### Frontend Changes
- [ ] New pages: [list]
- [ ] New components: [list]
- [ ] State management changes: [describe]
- [ ] Navigation changes: [describe]

## Dependencies
- [Other features this depends on]
- [External systems this integrates with]

## Success Criteria
1. [Measurable outcome]
2. [Measurable outcome]

## Constraints
- Must follow existing [X] pattern
- Must use existing [Y] component
- Cannot modify [Z] because [reason]
```

---

## Phase 2: Pattern Research

**Purpose:** Confirm how to implement within existing conventions.

### When to Do Deep Research

- Touching unfamiliar parts of the codebase
- Adding new integration (new data source, new external API)
- Implementing something with no existing precedent

### When to Skip

- Copying an existing pattern exactly
- Simple CRUD following established conventions
- Small changes to existing code

### Research Output

Add findings to `PATTERNS.md` or create `Research/` folder for complex topics.

---

## Phase 3: Phased Implementation Plan

**Purpose:** Break work into reviewable, mergeable chunks.

### Brownfield Phase Rules

1. **Each phase should be one PR** (or one PR per repo if touching both)
2. **Each phase should be testable independently**
3. **Each phase should pass CI**
4. **Each phase needs a Jira ticket**

### Phase Structure

Create `PHASE-N-[name].md` for each phase:

```markdown
# Phase N: [Name]

**Jira:** PROJ-XXXX
**Branch (BE):** `PROJ-XXXX-description`
**Branch (FE):** `feature/PROJ-XXXX-description`
**Status:** [Planning / In Progress / Ready for Review / Merged]

---

## Objective
[What this phase accomplishes]

## Prerequisites
- [ ] [Previous phase merged]
- [ ] [Required data/permission exists]

---

## Backend Changes

### Files to Create
- `app/controllers/api/[name]_controller.rb` - [purpose]
- `app/policies/[name]_policy.rb` - [purpose]

### Files to Modify
- `config/routes.rb` - add routes
- [other files]

### Database
- [ ] Migration needed: [yes/no]
- Migration: [describe changes]

### Tests Required
- `spec/requests/api/[name]_spec.rb`
  - [ ] Test authenticated access returns 200
  - [ ] Test unauthenticated returns 401
  - [ ] Test unauthorized returns 403
  - [ ] Test [specific behavior]

---

## Frontend Changes

### Files to Create
- `src/pages/[Name]/index.tsx` - [purpose]
- `src/components/[Name].tsx` - [purpose]

### Files to Modify
- `src/routes.tsx` - add route
- `src/components/Navigation.tsx` - add nav item
- [other files]

### Tests Required
- `src/pages/[Name]/__tests__/[Name].test.tsx`
  - [ ] Test renders correctly
  - [ ] Test [specific behavior]

---

## Testing Instructions

### Developer Testing (Before PR)

**Backend:**
```bash
bundle exec rspec spec/requests/api/[name]_spec.rb
rubocop app/controllers/api/[name]_controller.rb
```

**Frontend:**
```bash
yarn test src/pages/[Name]
yarn lint
```

### Manual Testing (After Deploy to Dev)
1. [Step-by-step manual test]
2. [Expected result]

---

## PR Checklist

### Backend PR
- [ ] Tests pass (`bundle exec rspec`)
- [ ] Lint passes (`rubocop`)
- [ ] No security issues (see SECURITY-GUARDRAILS.md)
- [ ] PR description references PROJ-XXXX
- [ ] Ready for code review

### Frontend PR
- [ ] Tests pass (`yarn test`)
- [ ] Lint passes (`yarn lint`)
- [ ] Build passes (`yarn build`)
- [ ] PR description references PROJ-XXXX
- [ ] Ready for code review

---

## Rollback Plan
[If this breaks something, how do we revert?]
```

---

## Phase 4: Implementation Loop

**Purpose:** Build, test, get user approval, then merge - one phase at a time.

### The Brownfield Loop

```
+-----------------------------------------------------------------------------------+
|  1. TICKET > 2. BRANCH > 3. BUILD > 4. TEST > 5. UAT > 6. PR > 7. MERGE         |
|       ^                                          |                  |              |
|       |                              +-----------+                  |              |
|       |                              v                              |              |
|       |                         [User tests]                        |              |
|       |                              |                              |              |
|       |                    +----NO---+---YES----+                   |              |
|       |                    v                    v                   |              |
|       |              [Fix & retest]      [Approved]                 |              |
|       |                    |                    |                   |              |
|       |                    +--------------------+                   |              |
|       +-------------------------------------------------------------+              |
|                              (repeat per phase)                                    |
+-----------------------------------------------------------------------------------+
```

**UAT is a gate.** Phase doesn't proceed to PR until user explicitly approves.

### Step 1: Create Jira Ticket

Before writing code, create a Jira ticket.

**Title format:**
```
[Area Prefix]: [Description]

Examples:
- Dashboard BE: Add analytics:read permission and AnalyticsPolicy
- Dashboard FE: Add Analytics nav item with permission gate
- API: Add customer search endpoint
```

Area prefixes: `Dashboard FE:`, `Dashboard BE:`, `Admin:`, `API:`, `DB:`

### Step 2: Create Branches

**Branch naming conventions:**
- **Backend:** `PROJ-XXXX-description` (no prefix)
- **Frontend:** `feature/PROJ-XXXX-description` or `fix/PROJ-XXXX-description`

### Step 3: Build

- Follow the phase plan
- Match existing patterns (reference PATTERNS.md)
- Write tests as you go
- Commit frequently with ticket reference

**Commit format:**
```
feat: add metrics endpoint (PROJ-XXXX)
fix: correct permission check (PROJ-XXXX)
test: add metrics controller specs (PROJ-XXXX)
```

### Step 4: Test

**Before creating PR:**
```bash
# 1. Sync with latest dev
git fetch origin && git merge origin/dev
# Resolve conflicts if any

# 2. Run tests (Backend)
bundle exec rspec           # All tests pass
rubocop                     # No lint errors

# 2. Run tests (Frontend)
yarn test                   # All tests pass
yarn lint                   # No lint errors
yarn build                  # Build succeeds
```

### Step 5: User Acceptance Testing (UAT)

**This is a gate.** No PR until you've manually tested and approved.

Walk through every acceptance criterion from the Jira ticket:
1. Does the feature work as specified?
2. Do edge cases behave correctly?
3. Do error states display properly?
4. Is the UI acceptable?

### Step 6: Create PR

**PR Title:** `PROJ-XXXX: [Brief description]`

### Step 7: Review & Merge

- Address code review feedback
- **Re-sync with dev** if review takes more than a day
- CI must pass
- Get approval
- Merge to dev
- Verify deployment

### Step 8: Update Status

After merge, update `STATUS.md`.

---

## Two-Repo Coordination

### When Both Repos Change

**Order matters:**
1. **Backend first** - API must exist before frontend calls it
2. **Frontend second** - Can call new API

### API Contract

When adding new endpoints, document the contract with request/response shapes.

---

## Adapting for Task Size

### Small Task (< 1 day, single PR)
- Skip SCOPE.md (describe in Jira ticket)
- Skip PATTERNS.md (you already know the pattern)
- Single phase plan (or just work from ticket)
- Still update STATUS.md

### Medium Task (1-3 days, 1-2 PRs)
- Brief SCOPE.md
- Reference PATTERNS.md if exists
- 1-2 phase plans
- STATUS.md for tracking

### Large Task (1+ week, multiple PRs)
- Full SCOPE.md
- PATTERNS.md with research
- Multiple phase plans
- STATUS.md religiously maintained

---

## Testing Requirements

**Rule: If a file type has an existing test, new changes to that file type must be tested.**

### Backend (Rails)

| File Type | Test Required | Location | Pattern |
|-----------|---------------|----------|---------|
| Controllers | Yes | `spec/requests/api/` | Request specs with auth scenarios |
| Policies | Yes | `spec/policies/` | ActionPolicy DSL (`describe_rule`) |
| Models | Yes | `spec/models/` | Unit tests for validations, scopes |
| Services | Yes | `spec/services/` | Unit tests for business logic |
| Migrations | No | - | Tested implicitly via model/integration tests |
| Routes | No | - | Tested implicitly via request specs |

### Frontend (React)

| File Type | Test Required | Location | Pattern |
|-----------|---------------|----------|---------|
| Components (new) | Yes | `__tests__/` adjacent | Basic render test minimum |
| Constants (permissions) | Yes | `__tests__/constants.test.js` | Assert string values match |
| Helpers/utils | Yes | `__tests__/` adjacent | Unit tests for logic |
| Route config | No | - | No existing test pattern |
| Translation files | No | - | No existing test pattern |

---

## Anti-Patterns for Brownfield

| Anti-Pattern | Why It's Bad | What to Do Instead |
|--------------|--------------|---------------------|
| Inventing new patterns | Inconsistent codebase | Follow existing patterns |
| Big-bang PRs | Hard to review, risky | Small, focused PRs |
| Skipping tests | CI fails, bugs ship | Write tests first |
| No Jira ticket | No traceability | Always create ticket first |
| Mixing refactoring with features | Confuses reviewers | Separate PRs |
| Not checking existing code | Duplicate or conflict | Research first |
| Ignoring CI failures | Broken builds | Fix before merge |

---

*This process is designed for extending existing production systems while maintaining consistency, traceability, and quality.*
