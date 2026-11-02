# Lesson 5: What the teacher should know

For checking Koze's work. Don't show these before Koze has run their own code.

- **Dataset**: `load_digits`, 1,797 samples, 64 features (`pixel_0_0` to `pixel_7_7`), 10 balanced classes (0-9, ~180 each).
- **Constant pixels**: 3 pixels have standard deviation = 0: `pixel_0_0`, `pixel_4_0`, `pixel_4_7`.
- **Split**: `test_size=0.25`, `stratify=y`, `random_state=42` -> `X_train` (1347, 64), `X_test` (450, 64).
- **Boruta** with `RandomForestClassifier(n_jobs=-1, class_weight='balanced', max_depth=5, random_state=42)`, `BorutaPy(n_estimators='auto', max_iter=50, random_state=42)` on `X_train`:
  - Confirmed: 50 features.
  - Tentative: 0 features.
  - Rejected: 14 features (all on outer vertical boundary columns: `pixel_x_0` and `pixel_x_7`, rank 2 through 12).
- **Model Evaluation (Random Forest, n_estimators=200)**:
  - All 64 features: 5-fold CV 0.9740, Test accuracy = 0.9667 (96.7%).
  - Boruta 50 features: 5-fold CV 0.9740, Test accuracy = 0.9711 (97.1%).
- **PCA on `X_train_scaled`**:
  - PC1 explains ~11.92%, PC2 explains ~9.67%, PC1 + PC2 explains ~21.59% (~22%).
  - Components for cumulative variance:
    - 80% variance: 21 components.
    - 90% variance: 31 components.
    - 95% variance: 40 components (`n_components_95 = 40`).
    - 99% variance: 54 components.
  - Without scaling: 95% variance reached in 29 components (center pixels dominate unscaled variance).
- **Pipelines**:
  - Logistic Regression (All 64): CV 0.9688, Test = 0.9778 (97.8%).
  - Logistic Regression (PCA 40): CV 0.9547, Test = 0.9600 (96.0%).
  - Random Forest (PCA 40): CV 0.9302, Test = 0.9511 (95.1%).
- **PCA component curve for Logistic Regression**:
  - k=2: 0.5111
  - k=5: 0.7844
  - k=10: 0.8578
  - k=15: 0.9267
  - k=20: 0.9422
  - k=30: 0.9489
  - k=40: 0.9600
  - k=50: 0.9800
  - k=64: 0.9756
- **Oral exam insights**:
  - Trees suffer on PCA because axis-aligned orthogonal cuts cannot cleanly separate rotated linear combinations.
  - PCA is unsupervised; it maximizes overall variance, not class discriminability.
  - Boruta preserves domain interpretability by keeping original variables intact.
