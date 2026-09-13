# Classification Knowledge

Use for discrete label prediction.

## Baselines

- majority class,
- simple rule-based classifier when domain structure allows,
- logistic regression,
- shallow decision tree.

## Candidates

- Random Forest,
- gradient boosting,
- SVM,
- nearest-neighbor methods,
- neural networks when scale and signal structure justify them.

## Required checks

- class balance,
- confusion matrix,
- precision / recall / F1,
- calibration when probabilities matter,
- group or temporal leakage,
- threshold sensitivity.

Accuracy alone is rarely sufficient.
