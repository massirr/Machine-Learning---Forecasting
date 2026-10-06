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
- Never edit, or save after running, the instructor's notebooks. Running a notebook changes its outputs and metadata, and that alone causes conflicts.
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

## How to help me (Koze)

- I'm a student in the Machine Learning & Forecasting course (oral exam = 70% of the grade). I want to understand, not just finish.
- Guide me with hints first. Give full code only when I'm stuck or short on time, and then explain each step in one line.
- Explain with my own outputs and numbers, simply and visually (small tables, ASCII sketches).
- When something would be copy-pasted into a file, edit the file in place so I can review it.
- Any explanation text written into my notebooks: run the humanizer skill first, then the structural-humanizer skill.
- Check `NOTES-FOR-CLAUDE.md` for where I am right now.
