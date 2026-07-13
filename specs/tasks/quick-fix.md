# Task: quick-fix

## Purpose

Fast, focused correction of a single well-defined issue in an existing material file, without running a full co-authoring session.

Equivalent to BMAD's "Quick Flow" — minimal overhead for small, targeted changes (typos, broken syntax, swapping one example, fixing a quiz answer, correcting a link).

## Inputs

- `number`: session number
- `type`: session type (`lecture` or `exercise`)
- `description`: what to fix (brief, e.g., "Typo in section 3", "Fix quiz syntax in slide 5", "Replace example for learning objective 2")
- The material document to change — resolved from `journal.md` → `## Course Context` → `__File Structure:__` (see `data/file-structure-modes.md`): `materials/{number}-{slug}/README.md` in multi-file mode, or the matching `##` chapter in root `/README.md` in single-file mode
- `journal.md` → `## Course Context` — for conventions and terminology
- `data/liascript-cheat-sheet.md` — for syntax reference if the fix involves LiaScript

## Output

- Updated material document (single targeted change only)
- Short inline confirmation of what was changed and PASS/FAIL of mini-validation

## Steps

1. **Scope confirmation:** State what will be changed and the acceptance criterion:
   - "I will [describe the change] in [material document]. The change is complete when [condition]. Correct? (Yes / Adjust scope)"

2. **Make the targeted change only** — no refactoring, no adjacent edits, no style improvements beyond the stated fix.

3. **Mini-validation of the affected section:**
   - LiaScript syntax correct in the changed area?
   - Persona/tone consistent with `journal.md` → `## Didactics`?
   - No unintended regression in surrounding content?

4. **Report result:**
   - ✅ "Fix applied and validated — done."
   - ⚠️ "The problem is larger than expected: [describe]. Should I open `:coauthor-materials {number} {type}`?"
   - If the fix changed session state (e.g., a Notes entry in the `## Sessions` overview table): run `tasks/update-dashboard.md` with `templates/project-dashboard.yaml` to update `journal.md` → `## Dashboard` in place.

5. **Escalate if scope grows:** If the fix reveals structural issues or multiple sections need rework, stop and escalate to `:coauthor-materials` — do NOT proceed silently.

---

## When to use vs. :coauthor-materials

| Situation                            | Use                   |
| ------------------------------------ | --------------------- |
| Single typo or broken syntax         | `:quick-fix`          |
| Wrong link or missing alt text       | `:quick-fix`          |
| Swap one example or code snippet     | `:quick-fix`          |
| Fix one quiz answer                  | `:quick-fix`          |
| Multiple sections need rework        | `:coauthor-materials` |
| Learning objective not covered       | `:coauthor-materials` |
| Structural or content change         | `:coauthor-materials` |
| Persona tone inconsistent throughout | `:coauthor-materials` |
