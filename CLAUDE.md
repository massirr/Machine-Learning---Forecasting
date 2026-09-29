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
