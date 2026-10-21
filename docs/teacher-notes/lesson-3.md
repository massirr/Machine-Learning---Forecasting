# Lesson 3 exercise: what the teacher (Claude) should know

For checking my work. Don't show these to me before I've run my own code.

- Split: `test_size=0.1, random_state=101` gives 2,632 train rows and 293 test rows, 273 features.
- Scaler: fit on `X_train` only, transform both.
- Grid from the lesson (`alpha` [0.1, 1, 5, 10, 50, 100], `l1_ratio` [.1, .5, .7, .9, .95, .99, 1], cv=5, neg MSE) with `ElasticNet(max_iter=1000000)`: best `alpha=100, l1_ratio=1`, test MAE about $14,195, RMSE about $20,559 (solution notebook says $14,149 / $20,532; small version differences are fine). The best model keeps 207 of 273 features.
- My small test grid gave alpha 0.1, l1_ratio 0.7, 1 feature dropped, MAE $14,283. Useful for comparing with the full grid.
- I use my class notes notebook (Part 4, cells 100-116) as the example code while I write the exercise myself.
- It's slow: about 100 s for 210 fits with `n_jobs=-1`. A `ConvergenceWarning` means `max_iter` is too low. Suggest a small grid first to test the code.
- scikit-learn 1.9: `mean_squared_error(..., squared=False)` raises a TypeError. Use `root_mean_squared_error`.
- The study guide asks me 3 questions after my run (why strong-Ridge corner is bad, best value at the edge of the grid, which penalty zeroed 66 features). Let me answer them myself.
