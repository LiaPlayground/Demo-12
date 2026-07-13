# Task: promote-session

## Purpose

Converts a **Session** into a detailed **Session Material**.  
**The agent also adopts the Coauthor role from `journal.md` → `## Agents` → `### Coauthor` into its own persona, so all content is written in this voice.**

## Inputs

- number, type
- skeleton: matching `### {number}. {title}` subsection from `journal.md` → `## Sessions`
- didactics: content from `journal.md` → `## Didactics`
- agenda: content from `journal.md` → `## Agenda`
- templates: imports and usage notes from `journal.md` → `## Templates` (if present)
- **Coauthor role from `journal.md` → `## Agents` → `### Coauthor` (mandatory handoff)**
- Style, difficulty level, and didactic concept from `journal.md` → `## Didactics`
- Terminology from `journal.md` → `## Course Context`
- File Structure mode from `journal.md` → `## Course Context` → `__File Structure:__` (see `data/file-structure-modes.md`)

## Output

- **multi-file mode:** `materials/{number}-{slug}/README.md` (new folder; `{slug}` derived once from the session title — see `data/file-structure-modes.md`)
- **single-file mode:** the `##` chapter matching this session, inserted/replaced in session-number order inside root `/README.md`
- Structure based on `templates/session-material.yaml`

## Steps

1. Load the matching skeleton subsection from `journal.md` → `## Sessions`.
2. Read `journal.md` → `## Course Context` for terminology and conventions.
3. Adopt didactic concept and course type from Didactics.
4. **Agent adopts the Coauthor role from `journal.md` → `## Agents` → `### Coauthor` into its own persona.**
   - From this step, the agent writes in the tone of the Coauthor role.
   - If the Coauthor role is missing or inactive, fall back to `journal.md` → `## Didactics` → `__Professor Persona:__`, `__Teaching Style:__`, and `__Persona Voice Sample:__`, then state that the Coauthor role should be synchronized into `## Agents`.
   - All agenda descriptions reflect this style.
5. Insert agenda information.
6. Consider didactic inputs.
7. Generate planned outline.
8. Apply template.
9. If the material uses macros from `journal.md` → `## Templates`, include each required `import: {url}` line in the LiaScript metadata header (multi-file: the new file's own header; single-file: the shared root `README.md` header, if not already present).
10. Resolve the File Structure mode from `journal.md` → `## Course Context` → `__File Structure:__` (see `data/file-structure-modes.md`) and save accordingly:
    - **multi-file:** derive `{slug}` from the session title, create `materials/{number}-{slug}/README.md` (and its `assets/` subfolder on demand, not upfront).
    - **single-file:** insert or replace the `##` chapter for this session inside root `/README.md`, keeping chapters in session-number order.
11. Update the overview table in `journal.md` → `## Sessions`: set Material column to ✅ for session `{number}`.
12. Run `tasks/update-dashboard.md` with `templates/project-dashboard.yaml` to update `journal.md` → `## Dashboard` in place.
