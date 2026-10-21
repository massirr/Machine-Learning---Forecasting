# Learner profile (Koze)

Read before teaching me or reviewing my work. Add new mistakes and insights here; the hint ladder and teaching rules live in `CLAUDE.md`.

## How I learn

- I write the code myself. Give hints and explanations, don't fill in the assignment cells.
- Explanations work best simple and concrete: a named example with real numbers, no jargon pile-up.
- I don't remember a lot of the basics (MSE, MAE, RMSE and similar). Re-explain them when they come up.

## Things I find hard

- Reading a confusion matrix: rows are the truth, columns are the guess. Precision reads a column, recall reads a row.
- Metrics like MSE/MAE/RMSE: forgotten, need a refresher.
- Why a validation split affects model quality (less training data vs. honest score). Explained 2026-10-06; check it stuck.

## From the lesson 3 exercise (2026-10-13)

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
- **"The model improved".** I said this after the full grid. MAE only improved by ~$88; the real gain is the same accuracy with 66 fewer features.

Still to work on:
- Explaining what alpha and l1_ratio do with my own numbers (compare the dropped-features count, small grid vs full grid).
- What it means when the best value is at the edge of the grid (alpha=0.1 was the smallest value in my small grid).
- MAE vs RMSE in plain words: RMSE punishes big misses more.

## From lesson 6 (2026-10-20)

- Understood: why 3.1 gives 100% (memorizing), TP/FN from probabilities at a threshold, the Pipeline/ColumnTransformer recipe (5 columns → 36), scaling before the split changes mean/std (LotArea mean 10,719 train vs 10,517 all).
- Reading scores (in my Pipelines notes, after Key Takeaways): train 0.874 / CV 0.860 / test 0.866 agree, so no overfitting or leakage. Train >> CV/test means overfitting; CV/test suspiciously high (0.956 AUC in the exercise) means suspect leakage.
- Misconception fixed: I first thought CV uses "data not used in training"; it actually splits X_train into 5 folds.
