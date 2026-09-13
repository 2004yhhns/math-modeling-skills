# Model Selection Rules

Use this order:

1. task type,
2. data regime,
3. assumptions,
4. baseline,
5. candidate families,
6. comparison,
7. experiment order,
8. validation plan.

## Comparison dimensions

- theoretical fit,
- data fit,
- accuracy potential,
- interpretability,
- robustness,
- implementation difficulty,
- computational cost,
- ease of validation,
- competition-paper explainability.

## Model roles

Every important subproblem should distinguish:

- **Baseline** — minimum meaningful benchmark.
- **Primary candidate** — model expected to solve the task best.
- **Validation/alternative model** — independent family used to test whether conclusions depend on one method.
- **Rejected model** — considered but excluded, with a reason.

## Evidence rule

A primary model becomes the selected model only after experimental evidence shows a meaningful advantage without violating validation constraints.
