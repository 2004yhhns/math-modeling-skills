---
name: model-validation
description: Validate mathematical modeling results against baselines and failure modes. Use after experiments exist to check generalization, leakage, residuals, robustness, and whether conclusions are actually supported.
---

# Model Validation

## Goal

Determine whether apparent model improvement is real and competition-defensible.

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the **skills repository root**.

The user only needs to invoke this Skill. Resource lookup is this Skill's responsibility.

Load resources in this order:

1. Read this `SKILL.md`.
2. Read local validation rules:
   - `skills/model-validation/references/validation-checklist.md`
3. Read live-project evidence:
   - `project/model_spec.json`
   - `project/baseline_solution.json`
   - `project/experiment_log.json`
   - `project/data_audit.json`
   - `project/data_preparation.json`, if present
4. Read relevant `knowledge/` only when needed to interpret model-specific failure modes, residual assumptions, calibration requirements, optimization feasibility conditions, or robustness expectations.
5. Inspect `algorithms/` only for reusable validation utilities such as grouped validation, residual diagnostics, sensitivity analysis, robustness checks, or optimization-feasibility checks.
6. Initialize `project/validation_summary.json` from `templates/validation_summary.json` when needed.
7. Write validation results to the live project only. Do not modify shared repository knowledge or templates as a substitute for fixing a failed model.

Validation must use the metrics, split strategy, and validation plan approved in `model_spec.json`. If those rules must change, record the reason and send the workflow back to model-selection rather than silently changing the evaluation after seeing results.

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
8. Does the apparent best model remain preferable after robustness and generalization checks?

## Output

Create or update `project/validation_summary.json`.

Validation may recommend a final model, but the primary candidate from model-selection is not automatically final. If a validation failure changes the modeling decision, send the workflow back to model-selection or model-building and record why.
