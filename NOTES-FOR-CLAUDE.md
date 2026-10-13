# Notes for Claude

Shared status between Claude in VS Code and Claude in the Claude app. Update it when something changes.

## Where I am (2026-10-13)

- Lesson 1 (Intro / Recap): done. My version: `1 - Intro - Recap/1 - Recap Exercise Solution (my notes).ipynb`.
- Lesson 2 (Decision Trees & Random Forests): submitted. My version: `2 - Decision Trees - Random Forests/2 - Decision Trees & Random Forest (student notebook) (my notes).ipynb`. Submitted work is also in commit `609098bb`.
- Lesson 3 (Regularization, Cross Validation, GridSearch): in progress. My version: `3 - Regularization, Cross Validation, GridSearch/3 - Reg & CV & GS (my notes).ipynb`.

- Lesson 3 assignment: `3 - Reg & CV & GS (Exc).ipynb` (Ames housing, ElasticNet + GridSearchCV), due around 2026-10-17. I work in `3 - Reg & CV & GS (Exc) (my notes).ipynb`. I'm using my notes notebook (Part 4, cells 100-116) as the example code while I write it myself.
  - Done (2026-10-13): X/y, split, scaling (`scaled_X_train` / `scaled_X_test`), `ElasticNet(max_iter=1000000)`, small test grid, then the full lesson grid, predictions on the test set, MAE ≈ $14,283 and RMSE ≈ $20,916. Those numbers came from the old small-grid model, though: the full-grid fit (210 fits) was still running when I ran the cells below it.
  - Full grid done: best alpha=100, l1_ratio=1, MAE $14,195, RMSE $20,559, 66 of 273 features dropped (small grid: alpha 0.1, l1 0.7, 1 dropped, MAE $14,283). I said "the model improved". Point to stress: MAE only improved by ~$88. The real gain is the same accuracy with 66 fewer features.
  - Next: answer the study-guide questions (best value at the edge of the grid, which penalty zeroed the 66 features), read the scatter plot, clean up the comments, then Restart & Run All and commit.
  - Claude added 2 extra cells at the end (not part of the task): a coefficient bar chart (top 15 features + how many were dropped) and a predicted-vs-real scatter plot.

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

### From the lesson 3 exercise (2026-10-13)

What I got right without help:
- Splitting X/y and train/test, and why the test set has to be split off first (data leakage).
- Scaler: fit on train only, transform both. I also noticed X_test's mean is slightly off 0 (e.g. 0.09, 0.14 per column), and that's the honest result.

What went wrong or confused me (check these again):
- **Mixing up train and test in the same call.** I made this mistake twice: `fit(scaled_X_train, y_test)` and `predict(scaled_X_train)` compared with `y_test`. The error `inconsistent numbers of samples: [2632, 293]` means the X and y are from different sets. Ask me why predicting on training data is a bad test.
- **Notebook state.** Twice the kernel held something different from the code I saw: re-running the scaler cell overwrote `X_train` (my "before" plot was already scaled), and editing `max_iter` without re-running the cell kept the old model (ConvergenceWarning). Fix: don't overwrite variables (use `scaled_X_train`), re-run cells below an edit, and use Restart & Run All before submitting.
- **What GridSearch does.** I thought it picks features or train/test splits. It only picks hyperparameters (`alpha`, `l1_ratio`) by trying every combination with 5-fold CV inside the training set. The model learns the coefficients, and Lasso's zeros are what drop features.
- **What `SalePrice` is.** I didn't see it in `df.head()` (hidden in the `...`) and didn't know what the `_AdjLand` / `_Normal` suffixes were. They're dummy variables from `get_dummies`, one 0/1 column per category.
- **The scaling boxplot.** I didn't understand it. 273 boxes, mostly dummies, can't be read. Keep visuals small (3–4 columns) or skip them.
- **Small wording errors to fix for the exam:** `l1_ratio=.1` is *mostly* Ridge, pure Ridge is `0`. `n_jobs=-1` uses CPU cores, not GPU. ElasticNet = Lasso + Ridge (I wrote "Lasso and Reg").
- **MSE vs MAE/RMSE.** I first used MSE. The task asks for MAE and RMSE because they're in dollars.

Still to work on:
- Explaining what alpha and l1_ratio do with my own numbers (compare the dropped-features count, small grid vs full grid).
- What it means when the best value is at the edge of the grid (alpha=0.1 was the smallest value in my small grid).
- MAE vs RMSE in plain words: RMSE punishes big misses more.

## Plans

- Do similar exercises where I write the code myself, to build understanding.
- Check later whether I still know the Data Science Fundamentals basics.
