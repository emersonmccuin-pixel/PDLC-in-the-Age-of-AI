# Repo Restructure Plan

**For:** Sonnet build session
**Goal:** Reorganize the repo so a GitHub visitor can navigate it without needing project history context.

---

## Target Structure

```
README.md
CLAUDE.md
context-pipeline.md              # Top-level — the core unsolved problem
phases/                          # The 14 PDLC phase scaffolds
+-- overview.md
+-- 00-portfolio-prioritization.md
+-- 01-ideation-and-discovery.md
+-- 02-requirements-and-specification.md
+-- 03-design.md
+-- 04-implementation.md
+-- 05-code-review.md
+-- 06-qa-and-testing.md
+-- 07-staging.md
+-- 08-release.md
+-- 09-go-to-market.md
+-- 10-monitoring-and-feedback.md
+-- 11-customer-success.md
+-- 12-operations-and-maintenance.md
+-- 13-sunset-and-deprecation.md
docs/                            # Project management only
+-- plan.md
+-- status.md
archive/                         # Phase 1 baseline, references, earlier drafts
```

---

## Steps (execute in order)

### 1. Move files

```bash
# Move phase scaffolds from docs/planning/phase-2/ to phases/
mkdir phases
mv docs/planning/phase-2/* phases/

# Move context pipeline to root
mv docs/context-pipeline-architecture.md context-pipeline.md

# Clean up empty directories
rmdir docs/planning/phase-2
rmdir docs/planning
```

### 2. Update internal links in phase docs

All 14 phase docs + overview have navigation footers with relative links to each other. These links are just filenames (e.g., `03-design.md`) which won't change since the files stay in the same directory relative to each other. **These should still work — verify, don't blindly edit.**

Links that DO need updating in phase docs:
- `overview.md` line ~235: `[Back to Plan](../../plan.md)` → `[Back to Plan](../docs/plan.md)`
- `overview.md`: any remaining link to `../../context-pipeline-architecture.md` → `../context-pipeline.md`
- Each phase doc: any link to `../../context-pipeline-architecture.md` → `../context-pipeline.md`

Search pattern to find all links that need updating:
```bash
grep -rn '\.\./\.\.' phases/
```

All `../../` paths are now wrong because the files moved from `docs/planning/phase-2/` (3 levels deep) to `phases/` (1 level deep).

### 3. Update context-pipeline.md

Internal links currently use paths relative to `docs/`:
- `planning/phase-2/overview.md` → `phases/overview.md`
- `planning/phase-2/02-requirements-and-specification.md` → `phases/02-requirements-and-specification.md`
- `planning/phase-2/04-implementation.md` → `phases/04-implementation.md`
- `planning/phase-2/05-code-review.md` → `phases/05-code-review.md`
- `planning/phase-2/10-monitoring-and-feedback.md` → `phases/10-monitoring-and-feedback.md`
- `plan.md` → `docs/plan.md`

### 4. Update README.md

- `docs/planning/phase-2/overview.md` → `phases/overview.md`
- `docs/planning/phase-2/` → `phases/`
- `docs/context-pipeline-architecture.md` → `context-pipeline.md`
- Repo structure diagram: update to match target structure

### 5. Update CLAUDE.md

- `docs/planning/phase-2/overview.md` → `phases/overview.md`
- `docs/planning/phase-2/00-13-*.md` → `phases/00-13-*.md`
- `docs/context-pipeline-architecture.md` → `context-pipeline.md`
- `docs/planning/phase-N/` → remove or update (no longer applicable)
- Update conventions section to reflect new structure

### 6. Update docs/plan.md

- `docs/planning/phase-2/` → `phases/`
- `context-pipeline-architecture.md` → `../context-pipeline.md`
- Any remaining `docs/planning/` references

### 7. Update docs/status.md

- No link updates needed (status.md contains historical text, not navigational links)
- Session 6 log references `archive/plans/` which is correct

### 8. Verify no broken links

```bash
# Find all markdown links and check they resolve
grep -rn '](.*\.md)' phases/ context-pipeline.md docs/ README.md CLAUDE.md | grep -v archive | grep -v node_modules
```

Manually verify each link target exists.

### 9. Commit and push

```bash
git add -A
git commit -m "refactor: restructure repo for GitHub navigation

- Move phase scaffolds from docs/planning/phase-2/ to phases/
- Move context pipeline doc to repo root
- docs/ now contains only project management (plan, status)
- Update all internal cross-references"

git push origin main
```
