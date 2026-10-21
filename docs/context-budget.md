# Context budget

Audit of 2026-10-21: what an agent loads in this repo, and where each fact lives. ~tokens = bytes / 4.

## Layers

- **ALWAYS**: loaded at every session start. `CLAUDE.md`, and `NOTES-FOR-CLAUDE.md` (the map points every session to it).
- **ON DEMAND**: read only for a matching task. `docs/learner-profile.md` (teaching / reviewing), `docs/teacher-notes/lesson-<n>.md` (checking exercise results), `<lesson>/study-guide.html` (explaining a topic).
- **NEVER** (for agents): `README.md` (the instructor's course intro, shared with `upstream`; left untouched to avoid merge conflicts), `docs/archive/`, this file.

## Before / after

| File | Layer | Before (bytes / ~tok) | After (bytes / ~tok) |
|---|---|---|---|
| `CLAUDE.md` | ALWAYS | 2,803 / 700 | 4,077 / 1,019 |
| `NOTES-FOR-CLAUDE.md` | ALWAYS (force-read) | 8,341 / 2,085 | 1,812 / 453 |
| `docs/learner-profile.md` | ON DEMAND | – | 3,902 / 976 |
| `docs/teacher-notes/lesson-3.md` | ON DEMAND | – | 1,312 / 328 |
| `docs/teacher-notes/lesson-6.md` | ON DEMAND | – | 192 / 48 |
| `docs/archive/2026-10.md` | NEVER | – | 796 / 199 |
| `2 - .../study-guide.html` | ON DEMAND | 33,032 / 8,258 | unchanged |
| `3 - .../study-guide.html` | ON DEMAND | 28,228 / 7,057 | unchanged |
| `README.md` | NEVER | 2,957 / 739 | unchanged |
| **Repo total at session start** | | **11,144 / 2,786** | **5,889 / 1,472** (−47%) |

Outside the repo, also loaded every session and not changed: `~/.claude/CLAUDE.md` (228 B) and the auto-memory `MEMORY.md` (131 B).

Not found in this repo: AGENTS.md, GEMINI.md, .cursorrules, .cursor/, .github/ (`.github/instructions/` and `*.instructions.md` are gitignored but absent), .windsurfrules, CONTRIBUTING, .claude/skills, .claude/commands.

## What moved where

From `NOTES-FOR-CLAUDE.md`:
- "How I study" hint ladder and "why" question → `CLAUDE.md` (it was a teaching rule that `CLAUDE.md` already half-stated; merged into one place).
- "Study guides" list + artifact links → map in `CLAUDE.md`.
- "Lesson 3 exercise: what the teacher should know" → `docs/teacher-notes/lesson-3.md` (plus the small-grid numbers and the "Part 4, cells 100-116" note from the status).
- "Things I find hard", "From the lesson 3 exercise", lesson 6 "Understood" / "Reading scores" / CV misconception, the "$88 vs 66 fewer features" point → `docs/learner-profile.md`.
- Lesson 6 class-notebook quirk (3.4 `data_selected`) → `docs/teacher-notes/lesson-6.md`.
- Superseded lesson 3 numbers (small-grid MAE $14,283 / RMSE $20,916) and the lesson 6 git-conflict note → `docs/archive/2026-10.md`. The git note's one new fact (VS Code kernel picker writes metadata) was added to the instructor-notebook rule in `CLAUDE.md`.
- Status, open question, plans → stay in `NOTES-FOR-CLAUDE.md`, now replace-not-append.

## Duplicates and errors fixed

- Hint ladder was in both files and differed: `CLAUDE.md` said full code "when I'm stuck or short on time", `NOTES` said "only if I ask". Merged as "if I ask, or I'm stuck or short on time".
- "Each lesson folder has a `study-guide.html`" was wrong: only lessons 2 and 3 have one. Map now says so.
- Study-guide instruction stated twice (`CLAUDE.md` and `NOTES`). Now only in the map.
- Git-conflict lesson duplicated the instructor-notebook rule. Folded in.
- Stale "Where I am (2026-10-13)" header replaced by "Last updated".
- No hard rule changed meaning. Nothing deleted outright; everything moved or archived.

## Open questions

1. Lesson 3 assignment: was it finished and submitted (due ~2026-10-17)? Status still says "left as of 2026-10-13". If yes, move its block to `docs/archive/2026-10.md` and leave one line with the commit.
2. The status file keeps the name `NOTES-FOR-CLAUDE.md` instead of `docs/handover.md`, because the Claude app reads it under that name. Rename only if the app setup changes too.
3. Hint ladder step 4: is the merged wording right?
4. Study guides are ~30 KB each. Splitting them per section would cut on-demand cost further, but they're also human-facing HTML pages, so left as is.
5. No doc-vs-code test was added: the only code-facing fact (scikit-learn 1.9 in `.venv`) was checked by hand (1.9.1), and there's no test setup in this repo.
