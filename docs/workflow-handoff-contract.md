# Workflow Handoff Contract

This document defines the minimum information that must survive between core modeling stages so that downstream skills do not have to reconstruct important assumptions, data semantics, interfaces, or risks from scratch.

## Principle

Each stage has two responsibilities:

1. solve its own stage-specific task;
2. preserve the information that the next stage needs.

If downstream work discovers that a required upstream fact was never established, return to the responsible upstream stage rather than silently inventing or repairing it downstream.

---

## 1. problem-analysis → data-audit

### Producer: `problem-analysis`

Must preserve:

- subproblem definitions,
- required outputs,
- known inputs,
- hard constraints,
- evaluation targets,
- task dependencies,
- important intermediate outputs,
- downstream consumers of those outputs,
- units stated by the problem,
- required temporal/spatial resolution,
- information-availability rules when decisions are sequential,
- ambiguities and assumptions,
- candidate high-risk interfaces.

### Consumer: `data-audit`

Uses those fields to determine whether required quantities actually exist, whether units/meanings/resolution match, whether future information is unavailable at decision time, and whether data can be mapped to problem quantities without ambiguity.

### Failure condition

If problem-analysis does not identify what a quantity means or when it is allowed to be known, data-audit must flag the ambiguity rather than guess.

---

## 2. data-audit → data-preparation

### Producer: `data-audit`

Must preserve:

- variable catalog and semantic meaning,
- units,
- missingness and invalid ranges,
- group/time/spatial structure,
- sampling frequency / spatial resolution,
- forecast issue time and horizon when relevant,
- decision interval when relevant,
- information-availability metadata,
- leakage risks,
- recommended split strategy,
- proposed cleaning/preparation decisions,
- problem-to-data mapping,
- unresolved interface issues,
- blocking vs non-blocking quality issues.

### Consumer: `data-preparation`

Uses the audit as a diagnosis, but independently decides whether each proposed transformation is semantically defensible for the concrete problem before executing it.

It must distinguish, especially for missingness:

- structural / not applicable,
- expected absence,
- measurement failure,
- censored/truncated,
- future unavailable,
- unresolved semantic status.

It may preserve data unchanged when modification would destroy or invent information.

### Failure condition

A data-audit recommendation is not automatic permission to impute/delete/smooth/resample. If the meaning is ambiguous or the transformation is high impact, data-preparation must preserve/flag or request human review.

---

## 3. data-preparation → model-selection

### Producer: `data-preparation`

Must preserve:

- raw input provenance,
- prepared output paths,
- preparation status (`READY`, `READY_WITH_WARNINGS`, `HOLD`),
- issue-by-issue semantic interpretation,
- transformations applied and their justification,
- transformations rejected/deferred,
- missingness decisions,
- before/after diagnostics,
- affected rows/values,
- high-impact human approvals,
- unresolved issues,
- downstream usage restrictions.

Prepared data must be written separately from raw data.

### Consumer: `model-selection`

Uses prepared data only when preparation status permits it, and carries unresolved restrictions forward. A preparation decision that may materially affect conclusions becomes a modeling/validation risk rather than invisible preprocessing.

### Failure condition

`HOLD` must not silently flow into model selection for candidates that depend on blocked data. High-impact imputation/deletion/resampling must not be treated as verified truth.

---

## 4. model-selection → feasibility-test

### Producer: `model-selection`

Must preserve for serious candidates:

- baseline(s),
- candidate model family,
- expected inputs and outputs,
- exact/qualitative data requirements,
- preprocessing requirements,
- upstream dependencies,
- downstream consumers,
- model-to-model interfaces,
- information constraints,
- temporal/spatial resolution requirements,
- major implementation/numerical risks,
- sensitivity to important preparation decisions,
- validation method,
- candidate feasibility targets,
- evidence required before full commitment.

### Consumer: `feasibility-test`

Uses those fields together with the problem brief, data audit, and prepared-data report to derive the dependency graph, final target, critical path, critical failure point, CFQ, required data, MVM readiness/interface check, proposed MVM, and Human Gate card.

### Important boundary

`model-selection` may suggest candidate feasibility targets, but it does **not** own the final CFQ. `feasibility-test` must independently derive the CFQ from the critical path and current evidence.

---

## 5. feasibility-test → model-building

### Producer: `feasibility-test`

Must preserve:

- final target,
- dependency chain,
- critical failure point,
- CFQ,
- primary/secondary families,
- required data,
- data readiness and interface findings,
- approved MVM,
- success/failure evidence,
- universal checks,
- family-specific checks,
- counterfactual review,
- blockers and repair cost,
- phase-gate status,
- next action.

### Consumer: `model-building`

May proceed only if the feasibility status and Human Gate permit continuation.

The model-building stage should preserve the approved assumptions, information restrictions, interfaces, hard constraints, and data-preparation restrictions unless a new Human Gate is triggered.

### Failure condition

`HOLD` or `NO_GO` must not silently flow into full model implementation.

---

## 6. model-building → model-validation

### Producer: `model-building`

Must preserve:

- code/version used,
- experiment configuration,
- random seed,
- data split,
- model-dependent preprocessing,
- model parameters,
- outputs and diagnostics,
- runtime,
- failed experiments,
- deviations from the approved model specification.

### Consumer: `model-validation`

Uses those records to verify baseline improvement, correct split/leakage control, residual/error structure, group/time/spatial generalization, stability/robustness, and whether conclusions are supported by evidence.

---

## Cross-stage invariant checks

| Concept | First owner | Later consumers |
|---|---|---|
| Final evaluation target | problem-analysis | model-selection, feasibility-test, validation |
| Hard constraints | problem-analysis | all later stages |
| Variable meaning / units | data-audit | data-preparation and all later stages |
| Information availability | problem-analysis + data-audit | data-preparation and all later stages |
| Temporal/spatial resolution | problem-analysis + data-audit | data-preparation and all later stages |
| Cleaning/preparation rationale | data-preparation | model-selection, feasibility-test, model-building, validation |
| Missingness semantics | data-preparation | model-selection, feasibility-test, model-building, validation |
| Model interfaces | model-selection | feasibility-test, model-building, validation |
| Feasibility risks | model-selection | feasibility-test |
| CFQ / critical failure point | feasibility-test | model-building, validation |
| Approved model specification | model-selection + feasibility-test | model-building |
| Experimental evidence | model-building | model-validation |

## Return-path rule

```text
problem meaning / ambiguity                 → problem-analysis
missing / invalid / misaligned data         → data-audit
cleaning / imputation / preparation choice  → data-preparation
wrong candidate / model family              → model-selection
unproven critical mechanism                 → feasibility-test
implementation defect                       → model-building
unsupported conclusion                      → model-validation or earlier owner
```

Do not repair an upstream semantic error only inside downstream code.
