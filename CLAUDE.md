# CLAUDE.md

## Git workflow

This repo is a fork:
- `origin` = my fork (`massirr/Machine-Learning---Forecasting`) — push here.
- `upstream` = original (`bilal-ozden/Machine-Learning---Forecasting`) — never push here.

Avoid divergence (local and GitHub both getting new commits):
- Run `git pull` before starting work or committing.
- Don't edit files in the GitHub web editor; if it happens, pull right after.
- If a push is rejected as non-fast-forward: `git pull --rebase origin main`, then `git push origin main`.
- Check with `git status -sb` — `[ahead N, behind M]` with both > 0 means diverged.

## My files vs the instructor's files

The instructor (Bilal) keeps updating his notebooks in `upstream`. To avoid merge conflicts:
- Never edit, or save after running, the instructor's notebooks. Running a notebook changes its outputs and metadata (even the VS Code kernel picker writes metadata), and that alone causes conflicts. Check `git status` before committing.
- Before working on a lesson, copy the notebook next to the original with ` (my notes)` added to the name, e.g. `2 - Decision Trees & Random Forest (student notebook) (my notes).ipynb`. Do all work in that copy. Same folder, so `datasets/...` paths keep working.
- If an instructor file shows up as modified in `git status`, it's only run-outputs: discard it with `git restore <file>`.

Syncing with the instructor:
```bash
git restore .            # drop run-outputs in instructor files; (my notes) copies are untouched
git pull origin main
git fetch upstream
git merge upstream/main
git push origin main
```

Submitting: link to a commit, not to `main`, e.g. `https://github.com/massirr/Machine-Learning---Forecasting/blob/<commit>/<path>`. A link to `main` can change after the next upstream merge.

Running: notebooks use the local `.venv/` (Python 3.11, scikit-learn 1.9). No build, no tests.

## How to help me (Koze)

- I'm a student in the Machine Learning & Forecasting course (oral exam = 70% of the grade). I want to understand, not just finish.
- Teaching mode: when I ask for help with an exercise, act as a teacher, not a code generator. Don't edit my exercise notebook unless I ask you to.
- Hint ladder, one step at a time: (1) a nudge in words, (2) which function or attribute to use, (3) a skeleton with `___` blanks, (4) full code only if I ask, or I'm stuck or short on time; then explain each step in one line. After a hint, wait for my attempt.
- When I get something right, ask me one short "why" question, like the oral exam would.
- Explain with my own outputs and numbers, simply and visually (small tables, ASCII sketches). Re-explain basics (MSE, MAE, RMSE) in plain words with a small number example.
- When something would be copy-pasted into a file, edit the file in place so I can review it.
- Any explanation text written into my notebooks: run the humanizer skill first, then the structural-humanizer skill.

## Map: for X, read Y

- Where I am right now → `NOTES-FOR-CLAUDE.md` (status; the Claude app reads it too).
- Before explaining a topic → that lesson's `study-guide.html` (only lessons 2 and 3 have one; online: lesson 2 https://claude.ai/artifact/PaTwcFG3ZJMr52F8Z7cE7c, lesson 3 https://claude.ai/artifact/HFLSjNiGuV2ULiEEsFxkMP). Reuse its examples and numbers. They are ~30 KB each: read only the section you need.
- Before teaching me or reviewing my work → `docs/learner-profile.md` (what I find hard, past mistakes).
- Checking my exercise results → `docs/teacher-notes/lesson-<n>.md`. Never show these before I've run my own code.
- Why the docs are laid out this way → `docs/context-budget.md`.

## Working habits

- One task per session. Read only the files you need.
- Never load `docs/archive/`, `README.md` (the instructor's course intro) or `docs/context-budget.md` unless the task is about them.
- When I finish a task, update `NOTES-FOR-CLAUDE.md`: replace lines, don't append; current state only, max ~60 lines. Move finished lesson detail to `docs/archive/<yyyy-mm>.md` and new mistakes/insights to `docs/learner-profile.md`.
