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

Uses those fields to determine:

- whether required quantities actually exist in the data,
- whether units and meanings match,
- whether timestamps/resolution support the required task,
- whether future information is unavailable at decision time,
- whether data can be mapped to the problem quantities without ambiguity.

### Failure condition

If problem-analysis does not identify what a quantity means or when it is allowed to be known, data-audit must flag the ambiguity rather than guess.

---

## 2. data-audit → model-selection

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
- cleaning/preparation decisions,
- problem-to-data mapping,
- unresolved interface issues,
- blocking vs non-blocking quality issues.

### Consumer: `model-selection`

Uses those fields to determine:

- which model families are actually supported,
- whether a candidate requires unavailable information,
- whether candidate temporal/spatial assumptions match the data,
- whether a baseline can be constructed legally,
- which candidates carry high interface or implementation risk.

### Failure condition

A candidate model must not be proposed as primary if its required information, resolution, or data semantics conflict with the audited data unless the conflict is explicitly recorded and a defensible repair exists.

---

## 3. model-selection → feasibility-test

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
- validation method,
- candidate feasibility targets,
- evidence required before full commitment.

### Consumer: `feasibility-test`

Uses those fields together with the problem brief and data audit to derive:

- task dependency graph,
- final evaluation target,
- critical path,
- critical failure point,
- Core Feasibility Question (CFQ),
- primary/secondary families,
- required data for the CFQ,
- MVM Data Readiness & Interface Check,
- proposed MVM,
- Human Gate card.

### Important boundary

`model-selection` may suggest candidate feasibility targets, but it does **not** own the final CFQ. `feasibility-test` must independently derive the CFQ from the critical path and current evidence.

---

## 4. feasibility-test → model-building

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

The model-building stage should preserve the approved assumptions, information restrictions, interfaces, and hard constraints unless a new Human Gate is triggered.

### Failure condition

`HOLD` or `NO_GO` must not silently flow into full model implementation.

---

## 5. model-building → model-validation

### Producer: `model-building`

Must preserve:

- code/version used,
- experiment configuration,
- random seed,
- data split,
- preprocessing,
- model parameters,
- outputs and diagnostics,
- runtime,
- failed experiments,
- deviations from the approved model specification.

### Consumer: `model-validation`

Uses those records to verify:

- baseline improvement,
- correct split and leakage control,
- residual/error structure,
- group/time/spatial generalization,
- stability/robustness,
- whether conclusions are supported by evidence.

---

## Cross-stage invariant checks

These concepts should remain traceable from the first stage where they appear to every later stage that uses them:

| Concept | First owner | Later consumers |
|---|---|---|
| Final evaluation target | problem-analysis | model-selection, feasibility-test, validation |
| Hard constraints | problem-analysis | model-selection, feasibility-test, model-building, validation |
| Variable meaning / units | data-audit | model-selection, feasibility-test, model-building |
| Information availability | problem-analysis + data-audit | model-selection, feasibility-test, model-building |
| Temporal/spatial resolution | problem-analysis + data-audit | model-selection, feasibility-test, model-building |
| Model interfaces | model-selection | feasibility-test, model-building, validation |
| Feasibility risks | model-selection | feasibility-test |
| CFQ / critical failure point | feasibility-test | model-building, validation |
| Approved model specification | model-selection + feasibility-test | model-building |
| Experimental evidence | model-building | model-validation |

## Return-path rule

When a downstream stage finds a problem, send it back to the stage that owns the violated fact:

```text
problem meaning / ambiguity          → problem-analysis
missing / invalid / misaligned data → data-audit
wrong candidate / model family      → model-selection
unproven critical mechanism         → feasibility-test
implementation defect               → model-building
unsupported conclusion              → model-validation or earlier owner
```

Do not repair an upstream semantic error only inside downstream code.
