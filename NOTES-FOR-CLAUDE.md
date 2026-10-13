# Notes for Claude

Shared status between Claude in VS Code and Claude in the Claude app. Update it when something changes.

## Where I am (2026-10-13)

- Lesson 1 (Intro / Recap): done. My version: `1 - Intro - Recap/1 - Recap Exercise Solution (my notes).ipynb`.
- Lesson 2 (Decision Trees & Random Forests): submitted. My version: `2 - Decision Trees - Random Forests/2 - Decision Trees & Random Forest (student notebook) (my notes).ipynb`. Submitted work is also in commit `609098bb`.
- Lesson 3 (Regularization, Cross Validation, GridSearch): in progress. My version: `3 - Regularization, Cross Validation, GridSearch/3 - Reg & CV & GS (my notes).ipynb`.

- Lesson 3 assignment: `3 - Reg & CV & GS (Exc).ipynb` (Ames housing, ElasticNet + GridSearchCV), due around 2026-10-17. I work in `3 - Reg & CV & GS (Exc) (my notes).ipynb`. Done so far: loaded the data (path `./datasets/AMES_Final_DF.csv`) and ran `describe()`. Next: separate X and y. I'm using my notes notebook (Part 4, cells 100-116) as the example code while I write it myself.

## Study guides

Each lesson folder has a `study-guide.html` that Claude built for me (open it in a browser). It's the reference for what I've been taught so far. Read it before teaching me, so explanations match and nothing is repeated or contradicted.

- Lesson 2: `2 - Decision Trees - Random Forests/study-guide.html` (online: https://claude.ai/artifact/PaTwcFG3ZJMr52F8Z7cE7c)
- Lesson 3: `3 - Regularization, Cross Validation, GridSearch/study-guide.html` (online: https://claude.ai/artifact/HFLSjNiGuV2ULiEEsFxkMP)

## Lesson 3 exercise: what the teacher (Claude) should know

These are for checking my work. Don't show them to me before I've run my own code.

- Split: `test_size=0.1, random_state=101` gives 2,632 train rows and 293 test rows, 273 features.
- Scaler: fit on `X_train` only, transform both.
- Grid from the lesson (`alpha` [0.1, 1, 5, 10, 50, 100], `l1_ratio` [.1, .5, .7, .9, .95, .99, 1], cv=5, neg MSE) with `ElasticNet(max_iter=1000000)`: best `alpha=100, l1_ratio=1`, test MAE about $14,195, RMSE about $20,559 (solution notebook says $14,149 / $20,532; small version differences are fine). The best model keeps 207 of 273 features.
- It's slow: about 100 s for 210 fits with `n_jobs=-1`. A `ConvergenceWarning` means `max_iter` is too low. Suggest a small grid first to test the code.
- scikit-learn 1.9: `mean_squared_error(..., squared=False)` raises a TypeError. Use `root_mean_squared_error`.
- The study guide asks me 3 questions after my run (why strong-Ridge corner is bad, best value at the edge of the grid, which penalty zeroed 66 features). Let me answer them myself.

## How I study

- I write the code myself. Give hints and explanations, don't fill in the assignment cells.
- Hint ladder, one step at a time: (1) a nudge in words, (2) which function or attribute to use, (3) a skeleton with `___` blanks, (4) full code only if I ask for it. After a hint, wait for my attempt.
- When I get something right, ask me one short "why" question, like the oral exam would.
- I don't remember a lot of the basics (MSE, MAE, RMSE and similar). Re-explain these in plain words when they come up, with a small number example.
- Explanations work best simple and concrete: a named example with real numbers, no jargon pile-up.

## Things I find hard

- Reading a confusion matrix: rows are the truth, columns are the guess. Precision reads a column, recall reads a row.
- Metrics like MSE/MAE/RMSE: forgotten, need a refresher.
- Why a validation split affects model quality (less training data vs. honest score). Explained 2026-10-06; check it stuck.

## Plans

- Do similar exercises where I write the code myself, to build understanding.
- Check later whether I still know the Data Science Fundamentals basics.
