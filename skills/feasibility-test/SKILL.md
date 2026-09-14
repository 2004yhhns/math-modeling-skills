---
name: feasibility-test
description: Run a fast minimum-viable-model test after model selection and before full model building. Requires a Core Feasibility Question, prepared-data/interface readiness check, runnable prototype, sanity checks, failure logging, and a go/hold/no-go decision.
---

# Feasibility Test

## Goal

Turn theoretical model selection into evidence about practical executability.

Core question:

> Can we make the critical mathematical mechanism or interface run correctly, quickly, and reproducibly under the data, information, constraints, and preparation rules that actually apply?

## Position in the workflow

```text
problem-analysis
→ data-audit
→ data-preparation
→ model-selection
→ feasibility-test
→ model-building
→ model-validation
```

## Read

1. `project/problem_brief.md`
2. `project/data_audit.json`
3. `project/data_preparation.json`, if data-preparation has been run
4. `project/assumption_ledger.md`
5. `project/model_selection_audit.json`
6. `project/model_spec.json`
7. `references/feasibility-rules.md`
8. relevant files under `knowledge/` and `algorithms/` only as needed.

If `project/data_preparation.json` is `HOLD`, do not bypass the blocked issue by silently cleaning or reconstructing data inside the feasibility prototype. If it is `READY_WITH_WARNINGS`, carry the warnings and downstream usage restrictions into the Human Gate and MVM.

## Core principle

Build the smallest executable version of the central modeling mechanism. Reduce size, not the defining difficulty.

A prototype is invalid if it succeeds only by removing a defining mechanism, hard constraint, uncertainty source, information restriction, data-quality limitation, or model-to-model interface.

## Step 1 — Build the task dependency graph

Map:

```text
raw/prepared data
→ transformation/upstream model
→ intermediate output
→ downstream model/decision
→ final evaluation target
```

For every important node/interface record inputs, outputs, downstream consumer, dependency, information availability, and whether the interface requires validation.

## Step 2 — Identify final target and critical path

State the final quantity that determines success and identify the unavoidable path required to obtain it. Distinguish supporting work from critical work.

## Step 3 — Identify critical failure points

Assess candidate failure points by:

- impact,
- uncertainty,
- downstream dependency,
- difficulty of detecting false success.

Treat model-to-model and data-to-model interfaces as first-class failure points.

## Step 4 — Write the Core Feasibility Question (CFQ)

Use:

```text
Final target:
...

Critical dependency or mechanism:
...

Why it may fail:
...

Core Feasibility Question:
Under the information, prepared data, constraints, and operating conditions that really apply, can [critical mechanism/interface] produce [required downstream result] well enough to justify continuing?

Success evidence:
- ...

Failure evidence:
- ...
```

## Step 5 — Select primary and secondary model families

Choose families after the CFQ is defined.

- primary family: contains the mechanism whose failure most directly invalidates the route;
- secondary family: materially affects that mechanism through output, uncertainty, or interface.

Supported families include optimization/scheduling, prediction/classification, geometry/localization/search, simulation/mechanism/PDE, graph/routing/path planning, and stochastic/Monte Carlo models.

## Step 6 — Derive required data from the CFQ

For every required item record:

- source,
- semantic meaning,
- unit,
- temporal/spatial resolution,
- availability time,
- observed/forecast/estimated/synthetic/derived status,
- where it enters the mathematical model,
- whether data-preparation changed, imputed, excluded, aggregated, smoothed, or reconstructed it.

For a critical prepared variable, inspect the corresponding preparation rationale rather than treating the processed value as unquestioned ground truth.

## Step 7 — Run MVM Data Readiness & Interface Check

This is not another full data audit or cleaning stage. Reuse the audit and preparation report.

Check:

1. availability of indispensable quantities;
2. temporal/spatial alignment;
3. units and semantics;
4. remaining missing/invalid values;
5. information availability at decision time;
6. data-field → mathematical-symbol/model-interface mapping;
7. preparation sensitivity: whether the CFQ result could change materially under a defensible alternative treatment of important missing/outlier/resampling decisions.

If a blocking raw-data diagnosis is wrong or incomplete, return to `data-audit`. If the issue is how audited data should be cleaned/imputed/aligned, return to `data-preparation`. Do not repair it silently inside feasibility code.

## Step 8 — Propose the Minimum Viable Model (MVM)

State:

- tested subproblem,
- minimum viable instance,
- included mechanisms/constraints,
- intentionally excluded components,
- expected inputs/outputs,
- prepared-data subset and restrictions,
- success criteria,
- what success proves,
- what success does not prove.

## Step 9 — Human Gate before implementation

Before substantial feasibility code, present:

```text
FINAL TARGET
...

DEPENDENCY CHAIN
...

CRITICAL FAILURE POINT
...

CFQ
...

PRIMARY FAMILY
...

SECONDARY FAMILY
...

REQUIRED DATA
...

DATA PREPARATION / READINESS / INTERFACE ISSUES
...

PROPOSED MVM
...

SUCCESS CRITERIA
...

WHAT SUCCESS WOULD PROVE
...

WHAT SUCCESS WOULD NOT PROVE
...

HUMAN APPROVAL
PENDING
```

The human checks whether the CFQ attacks the real bottleneck, family classification is appropriate, prepared data are legally/semantically usable, and the MVM preserves the defining difficulty.

## Step 10 — Implement the prototype

After approval:

- use runnable end-to-end code,
- fixed seed when randomness exists,
- no fabricated data unless explicitly documented synthetic testing is appropriate,
- no hidden manual corrections,
- no unrecorded re-cleaning or imputation,
- preserve units, information restrictions, and core constraints,
- save main result and useful diagnostics.

If new cleaning becomes necessary during implementation, stop and route the decision through `data-preparation` when it is semantically material.

## Step 11 — Run universal checks

Always check:

1. execution success,
2. runtime/computational burden,
3. output shape/range/unit sanity,
4. core constraint satisfaction,
5. numerical stability/repeatability,
6. leakage/information timing,
7. sensible response to small perturbations,
8. whether results are understandable,
9. whether important conclusions depend mainly on one questionable preparation decision.

## Step 12 — Run family-specific checks

Use only the relevant sections of `references/feasibility-rules.md` for primary and secondary families.

## Step 13 — Counterfactual review

Ask:

1. If the MVM succeeds, what could still make the full route fail?
2. If it fails, is the cause model/concept, raw-data diagnosis, data-preparation choice, interface, or implementation?
3. Did it succeed by simplifying away a defining difficulty or by benefiting from an unjustified cleaning/imputation decision?

## Step 14 — Record blockers and repair cost

Classify blockers as minor, moderate, major, or fatal and estimate repair cost qualitatively.

Route repairs to the owner stage:

```text
problem meaning                 → problem-analysis
raw data diagnosis              → data-audit
cleaning/preparation decision   → data-preparation
candidate/model family          → model-selection
MVM/critical mechanism          → feasibility-test
implementation bug              → model-building
```

## Step 15 — Phase-gate decision

Return exactly one:

- `GO`
- `GO_WITH_RISKS`
- `HOLD`
- `NO_GO`

Do not choose the final competition problem solely from a score; the human owns the decision.

## Outputs

Create/update:

- `project/feasibility_test.json`
- optional code under `experiments/feasibility/` or `src/`
- optional diagnostics under `results/feasibility/`

Recommended fields include final target, dependency chain, critical path, critical failure point, CFQ, primary/secondary families, required data, data readiness, preparation dependencies/sensitivity, MVM, Human Gate status, universal/family checks, counterfactual review, blockers, repair cost, phase status, reason, and next action.

## What this skill must not do

- do not solve every subproblem,
- do not replace model-validation,
- do not declare superiority from one toy run,
- do not hide failures,
- do not silently repair data inside the prototype,
- do not override `data_preparation.json` restrictions,
- do not simplify away defining difficulty,
- do not use future/unavailable information,
- do not obtain a false `GO` by relying on unjustified imputation, deletion, smoothing, aggregation, or resampling,
- do not choose the final contest problem without human approval.
