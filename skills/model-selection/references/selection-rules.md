# Model Selection Rules

Use this order:

1. task type,
2. data regime,
3. assumptions,
4. baseline,
5. candidate families,
6. comparison,
7. feasibility-risk handoff,
8. experiment order,
9. validation plan.

## Comparison dimensions

- theoretical fit,
- data fit,
- accuracy / objective potential,
- interpretability,
- robustness,
- implementation difficulty,
- numerical stability risk,
- computational cost,
- information-timing legality,
- temporal/spatial resolution compatibility,
- upstream/downstream interface risk,
- ease of validation,
- competition-paper explainability.

## Model roles

Every important subproblem should distinguish:

- **Baseline** — minimum meaningful benchmark.
- **Primary candidate** — model expected to solve the task best.
- **Validation/alternative model** — independent family used to test whether conclusions depend on one method.
- **Rejected model** — considered but excluded, with a reason.

## Candidate contract

For every serious candidate, record enough information for downstream implementation and feasibility testing:

- expected inputs,
- expected outputs,
- exact or qualitative data requirements,
- preprocessing requirements,
- hard constraints,
- upstream dependencies,
- downstream consumer or interface,
- information that must be available at decision time,
- temporal/spatial resolution requirements,
- major implementation/numerical risks,
- validation method.

Do not treat two model stages as independent when the output of one becomes the input of another.

## Feasibility handoff

Before full model building, identify any risk that should be tested with a minimum viable prototype.

Typical feasibility targets include:

- a model-to-model interface whose semantics or timing may be invalid,
- an optimization model whose hard constraints may make it infeasible,
- a PDE/simulation whose numerical scheme may be unstable,
- a geometry/search mechanism whose uncertainty may not shrink as expected,
- a routing model whose abstraction may create infeasible paths,
- a stochastic method whose variance or scenario cost may be too large,
- a prediction pipeline whose legal information set may be too weak.

Record these as **candidate feasibility targets**. Do not write the final CFQ here; `feasibility-test` owns the critical-path analysis and final CFQ.

## Evidence rule

A primary model becomes the selected model only after experimental evidence shows a meaningful advantage without violating validation constraints.

A theoretically attractive candidate should not proceed directly to full implementation when a major feasibility risk remains unresolved.
