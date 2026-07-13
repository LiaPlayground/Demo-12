# Task: assemble-bundle

## Purpose

Combines the project memory, materials, and assets into a complete, distributable package for handoff, archiving, or offline use.
Produces a structured `course-bundle/` folder with an auto-generated index and all relevant artifacts.

## Inputs

- `journal.md` — canonical project memory containing course context, outline, didactics, agenda, sessions, session status, validation, reviews, and notes backup
- File Structure mode from `journal.md` → `## Course Context` → `__File Structure:__` (see `data/file-structure-modes.md`)
- **multi-file mode:** `materials/` — full session materials (primary content), each with its own nested `assets/`
- **single-file mode:** root `README.md` (primary content) and `assets/` (if exists)
- `journal.md` → `## Validation` → `### Latest Validation Summary` — latest QA gate (**required, must show `Mode: course` and `Result: PASS`**)

## Output

**multi-file mode:**
```
course-bundle/
├── bundle-index.md          ← auto-generated index
├── journal.md               ← canonical project memory
└── materials/
    └── {n}-{slug}/
        ├── README.md
        └── assets/          ← if exists
```

**single-file mode:**
```
course-bundle/
├── bundle-index.md          ← auto-generated index
├── journal.md               ← canonical project memory
├── README.md                ← the whole course
└── assets/                  ← if exists
```

## Steps

1. **Pre-flight check:** Confirm `journal.md` → `## Validation` → `### Latest Validation Summary` exists and shows `Mode: course` and `Result: PASS`.
   - If missing, not `Mode: course`, or not `Result: PASS`: block bundling. State: "⛔ Please run `:validate-course` first and resolve all issues before creating the bundle."

2. Read course title and abstract from `journal.md` → `## Outline`.

3. Scan all source folders and collect files, according to `journal.md` → `## Course Context` → `__File Structure:__`:
   - **multi-file:** `journal.md`, all files in `materials/` (including each session's nested `assets/`)
   - **single-file:** `journal.md`, root `README.md`, and `assets/` (if exists)

4. Generate `bundle-index.md`:

   ```markdown
   # Course Bundle: [Course Title]

   Generated: YYYY-MM-DD
   Course type: [type from `journal.md` → `## Course Context`]
   File Structure: [single-file | multi-file]
   Validation: PASS (see `journal.md` → `## Validation` → `### Latest Validation Summary`)

   ## Contents

   | File                              | Description                              |
   |------------------------------------|------------------------------------------|
   | journal.md                        | Project memory: context, outline, didactics, agenda, skeletons, sessions, validation, reviews, notes |
   | README.md _(single-file)_ / materials/{n}-{slug}/README.md _(multi-file)_ | Session N: [title from `journal.md` → `## Agenda`] |
   | assets/ _(single-file, root)_ / materials/{n}-{slug}/assets/ _(multi-file, per session)_ | Visual assets and prompts, if present |

   ## Quick Start

   - **Instructor handoff:** Start with `journal.md` → `## Outline` and `journal.md` → `## Didactics`
   - **LiaScript publish:** Use `README.md` (single-file) or the files in `materials/` (multi-file) directly
   - **Quality audit:** See `journal.md` → `## Validation`
   ```

5. Copy `journal.md` and the mode-appropriate content (`README.md` + `assets/` for single-file, or `materials/` for multi-file) into `course-bundle/`, preserving subfolder structure.

6. Run `tasks/update-dashboard.md` with `templates/project-dashboard.yaml` to update `journal.md` → `## Dashboard` in place.

7. Confirm completion:
   > "Bundle created in `course-bundle/`. Contains `journal.md`, [N] material files, and [assets/ ✅ / no assets]."
   > "Next step: `:agent development` → `:create-project` to publish the course."
