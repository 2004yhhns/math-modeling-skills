# Validation Checklist

Use this checklist on the full-built shortlist, not on screening-only prototypes.

## 1. Baseline and alternative evidence

- primary vs meaningful baseline(s);
- primary vs serious alternative(s);
- effect size / metric improvement;
- uncertainty across folds, groups, seeds, or repeated runs when relevant;
- whether a more complex model improves enough to justify added complexity.

## 2. Split, leakage, and information legality

Verify the split structure justified by data audit/model_spec:

- K-fold when independent observations justify it;
- GroupKFold / leave-one-group-out for grouped/repeated observations;
- time split for sequential/forecast settings;
- spatial/source/dataset holdout when required;
- no target/group/time/source/preprocessing leakage;
- all train-fitted preprocessing is fit only inside the legal training scope.

## 3. Generalization

Check performance across important:

- groups/subjects/entities;
- time periods/regimes;
- spatial regions;
- datasets/sources/cohorts;
- classes/targets;
- operational conditions.

A strong aggregate score is insufficient when one important regime fails.

## 4. Error / residual diagnostics

Check when applicable:

- residual bias;
- heteroscedasticity;
- subgroup failure;
- extreme-regime failure;
- calibration;
- class confusion;
- constraint violation;
- systematic error patterns that suggest missing structure.

## 5. Robustness and sensitivity

Perturb important inputs, parameters, seeds, samples, preparation choices, or assumptions when conclusions could change.

Distinguish:
- robustness of model behavior;
- sensitivity to data-preparation decisions;
- sensitivity to tuning/hyperparameters;
- sensitivity to modeling assumptions.

## 6. Interpretation discipline

- prediction importance is not causality;
- a good test score alone does not prove a mechanism;
- feature importance must respect correlation/grouping and model limitations;
- domain interpretation may use literature, but claims must stay within evidence.

## 7. Practical and downstream usefulness

When relevant compare:

- runtime and computational burden;
- numerical stability;
- interpretability;
- reproducibility;
- downstream interface quality;
- constraint satisfaction;
- whether the output is actually usable by later subproblems.

## 8. Final recommendation rule

The screening winner is not automatically final.

Recommend a final model only when full evidence supports it after baseline/alternative comparison, generalization, robustness, interpretation, and practical constraints.

It is valid to return:
- one final primary model;
- primary + retained baseline/alternative;
- justified ensemble;
- no final model yet.

## 9. Improvement routing

When validation exposes a weakness, diagnose the owner before changing the system:

```text
raw-data diagnosis        → data-audit
preparation choice        → data-preparation
wrong model family        → model-selection
unproven high-risk route  → feasibility-test when genuinely needed
implementation/tuning     → model-building
validation design         → model-validation with recorded revision
interpretation only       → limit/rewrite the claim
```

Record the evidence, proposed change, new experiment/validation identifier, and whether the change actually improved the target without introducing new problems.
