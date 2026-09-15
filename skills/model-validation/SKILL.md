---
name: model-validation
description: Validate full modeling results against baselines, alternatives and failure modes; recommend the defensible final model and route diagnosed weaknesses into an explicit improvement loop.
---

# Model Validation

## Goal

Determine whether apparent improvement is real, generalizable and competition-defensible, then diagnose what should be improved when it is not.

Validation is where the shortlist is judged with full evidence. The screening winner is not automatically the final model.

## Workflow position

```text
shortlist
   ↓
model-building
   ↓
model-validation
   ├─ baseline/alternative comparison
   ├─ generalization & leakage
   ├─ robustness/sensitivity
   ├─ error/residual analysis
   └─ interpretation
   ↓
final recommendation
   ↓
if needed: IMPROVEMENT LOOP
   ├─ data diagnosis issue      → data-audit
   ├─ preparation issue        → data-preparation
   ├─ wrong model family       → model-selection
   ├─ implementation/tuning    → model-building
   └─ validation-design issue  → model-validation redesign with recorded approval
```

## Repository resource loading

Load:

1. this `SKILL.md`;
2. `skills/model-validation/references/validation-checklist.md`;
3. live-project evidence:
   - `project/model_spec.json`
   - `project/baseline_solution.json`
   - `project/model_selection_audit.json`
   - `project/experiment_log.json`
   - `project/data_audit.json`
   - `project/data_preparation.json`, if present
   - relevant feasibility evidence, if applicable
   - `paper/literature/benchmark_landscape.md` and `literature_matrix.md` when literature established validation standards or interpretation limits
4. relevant reusable `knowledge/`/validation utilities only as needed;
5. initialize `project/validation_summary.json` from the template when needed.

Use the split, metrics and validation plan approved in `model_spec.json`. Do not change evaluation after seeing results without recording the reason and approval.

## Minimum validation questions

1. Does each serious model improve meaningfully over the baseline on approved metrics?
2. Was the approved split/CV/group/time strategy actually used?
3. Is there leakage or illegal information use?
4. Are residuals/errors systematically structured?
5. Does performance generalize across important groups, times, datasets or regimes?
6. Are conclusions stable under reasonable perturbations and defensible alternative preparation choices?
7. Are uncertainty/calibration/constraint checks satisfactory when relevant?
8. Are interpretation claims supported by the model and evidence?
9. Does the apparent best model remain preferable after robustness, complexity, interpretability and downstream usefulness are considered?
10. Are differences large/stable enough to justify preferring a more complex model?

## Final recommendation

Validation may recommend:

- one final primary model;
- a primary model plus a simpler baseline/alternative retained for comparison;
- an ensemble only when justified by evidence;
- no final model yet when blocking evidence remains.

Record why the recommendation is preferable, not only its headline score.

## Improvement diagnosis

When validation exposes weakness, classify the root cause before changing anything:

- `DATA_DIAGNOSIS` → return to `data-audit`;
- `DATA_PREPARATION` → return to `data-preparation`;
- `MODEL_FAMILY` → return to `model-selection` and, if needed, literature/candidate research;
- `IMPLEMENTATION_OR_TUNING` → return to `model-building`;
- `VALIDATION_DESIGN` → revise validation protocol transparently;
- `INTERPRETATION_ONLY` → limit/rewrite the conclusion rather than changing the model unnecessarily.

Each improvement cycle must record:

- observed failure/weakness;
- evidence;
- diagnosed owner stage;
- proposed change;
- whether a Human Gate is required;
- new experiment/validation id;
- whether the change actually improved the target without creating new problems.

Do not perform endless optimization. Stop when gains are negligible, evidence is stable, competition constraints/time make further work unjustified, or the remaining limitation should simply be reported.

## Output

Create/update `project/validation_summary.json` with:

- model comparison;
- baseline comparison;
- validation checks;
- final recommendation/status;
- blocking issues;
- improvement diagnosis and routing;
- improvement history;
- next action.

Validation results and improvement evidence belong to the live project, not reusable repository knowledge.
