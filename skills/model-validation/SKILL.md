---
name: model-validation
description: Validate mathematical modeling results against baselines and failure modes. Use after experiments exist to check generalization, leakage, residuals, robustness, and whether conclusions are actually supported.
---

# Model Validation

## Goal

Determine whether apparent model improvement is real and competition-defensible.

## Read

- `project/model_spec.json`
- `project/baseline_solution.json`
- `project/experiment_log.json`
- `project/data_audit.json`
- `references/validation-checklist.md`

## Minimum checks

1. Does the primary model beat the baseline on the approved metrics?
2. Was the approved split strategy actually used?
3. Is there any sign of leakage?
4. Are residuals / errors systematically structured?
5. Does performance hold across important groups, time periods, or regimes?
6. Are main conclusions stable under reasonable perturbations?
7. Are interpretation claims consistent with what the model can establish?

## Output

Create or update `project/validation_summary.json`.

If a validation failure changes the modeling decision, send the workflow back to model-selection or model-building and record why.
