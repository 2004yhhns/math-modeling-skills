# Validation Checklist

## Baseline evidence

- primary vs baseline
- effect size / metric improvement
- uncertainty across folds or repeated runs

## Generalization

Use the split structure justified by the data audit:

- K-fold,
- GroupKFold,
- time split,
- leave-one-group-out,
- spatial holdout,
- other task-specific holdout.

## Error diagnostics

Check when applicable:

- residual bias,
- heteroscedasticity,
- subgroup failure,
- extreme-regime failure,
- calibration,
- class confusion,
- constraint violation.

## Robustness

Perturb important inputs, parameters, seeds, or samples when the conclusion could change.

## Interpretation discipline

Prediction importance is not causality. A good test score alone does not prove a mechanism.
