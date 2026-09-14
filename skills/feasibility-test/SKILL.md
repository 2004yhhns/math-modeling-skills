---
name: feasibility-test
description: Run a fast minimum-viable-model test after model selection and before full model building. Use to decide whether a candidate problem/model is actually executable under competition time constraints. Requires a core-feasibility question, data/interface readiness check, runnable prototype, sanity checks, failure logging, and a go/hold/no-go decision.
---

# Feasibility Test

## Goal

Turn theoretical model selection into evidence about practical executability.

This skill is intentionally narrower than model-building and lighter than model-validation. It answers:

> Can we make the core mathematical mechanism run correctly, quickly, and reproducibly enough to justify committing competition time to this path?

Use it for final topic choice, model-path screening, or early risk detection.

## Position in the workflow

```text
problem-analysis
→ data-audit
→ model-selection
→ feasibility-test
→ model-building
→ model-validation
```

If multiple contest problems remain viable, run one feasibility test per candidate problem before final selection.

## Read

1. `project/problem_brief.md`
2. `project/data_audit.json`
3. `project/assumption_ledger.md`
4. `project/model_selection_audit.json`
5. `project/model_spec.json`
6. `references/feasibility-rules.md`
7. Relevant files under `knowledge/` and `algorithms/` only as needed for the tested model family.

## Core principle

Build the smallest executable version of the central modeling mechanism.

Do not try to solve the full problem. Prefer one representative subproblem, one baseline, one small dataset slice or synthetic case, and a few decisive checks.

Reduce size, not the defining difficulty. A prototype is invalid if it succeeds only by removing the mechanism, information restriction, hard constraint, uncertainty source, or interface that makes the original problem difficult.

## Step 1 — Build the task dependency graph

Before choosing a minimum viable model, map how the problem's important quantities depend on one another.

Represent the main chain explicitly, for example:

```text
raw/cleaned data
→ upstream model or transformation
→ intermediate output
→ downstream model or decision
→ final evaluation target
```

For every node and interface, record:

- input,
- output,
- who consumes the output next,
- whether later steps fail if this step fails,
- whether the information is available at the required time,
- whether a model-to-model interface must be validated.

Do not assume that the first technically difficult subproblem is the true bottleneck.

## Step 2 — Identify the final evaluation target and critical path

State what ultimately determines success in the problem: cost, error, coverage, stability, profit, physical consistency, service level, or another explicit target.

Then identify the critical path: the sequence of steps that must work for that final target to be obtained.

Distinguish supporting work from critical work. Visualization, feature engineering, tuning, or interpretability may be useful without being on the critical path.

## Step 3 — Identify critical failure points

Inspect critical nodes and interfaces and ask where the modeling route is most likely to break.

Assess each candidate failure point qualitatively using:

- impact: how badly final success is damaged if it fails,
- uncertainty: how little evidence we currently have that it works,
- downstream dependency: how much later work depends on it,
- validation difficulty: how hard it is to detect a false success.

Prefer feasibility tests that attack high-impact, high-uncertainty, high-dependency failure points.

Model-to-model interfaces are first-class failure points. Examples include forecast → optimizer, detector → localizer, estimated parameter → simulator, and simulation output → decision rule.

## Step 4 — Write the Core Feasibility Question (CFQ)

The CFQ must not be a vague question such as "can the model run?". Derive it from the final target, critical dependency, failure risk, and evidence required for continuation.

Use this structure:

```text
Final target:
...

Critical dependency or mechanism:
...

Why it may fail:
...

Core Feasibility Question:
Under the information, data, constraints, and operating conditions that really apply, can [critical mechanism/interface] produce [required downstream result] well enough to justify continuing?

Success evidence:
- ...

Failure evidence:
- ...
```

A useful CFQ should make it possible to say what a successful MVM proves and what it does not prove.

## Step 5 — Select primary and secondary model families

Choose model families only after the CFQ is defined.

- `primary family`: the family containing the mechanism whose failure would most directly invalidate the route.
- `secondary family`: another family whose output, uncertainty, or interface materially affects the CFQ.

A problem may require more than one family-specific check. Do not classify by keywords alone.

Supported families include:

- optimization / scheduling,
- prediction / classification,
- geometry / localization / search,
- simulation / mechanism / PDE,
- graph / routing / path planning,
- stochastic / Monte Carlo models.

## Step 6 — Derive required data from the CFQ

Do not start the MVM from whatever columns are easiest to use. Starting from the CFQ, list the exact data, parameters, states, labels, constraints, timestamps, forecasts, or boundary/initial conditions required to test it.

For every required item record:

- source,
- semantic meaning,
- unit,
- temporal/spatial resolution when relevant,
- availability time when relevant,
- whether it is observed, forecast, estimated, synthetic, or derived,
- where it enters the mathematical model.

## Step 7 — Run MVM Data Readiness & Interface Check

This is not a second full data-audit. Reuse `project/data_audit.json` and the cleaned/prepared data produced upstream. Check only the data needed by the current CFQ.

Before implementation, verify:

1. **Availability** — all indispensable variables/parameters exist or have a defensible derivation.
2. **Temporal/spatial alignment** — timestamps, sampling frequency, forecast issue time, forecast horizon, decision interval, coordinate system, or grid resolution are compatible.
3. **Units and semantics** — units and meanings are explicit; do not silently mix power/energy, rates/totals, local/global coordinates, probabilities/scores, etc.
4. **Missing/invalid values** — blocking missingness or impossible values are handled explicitly; do not silently fill with convenient constants.
5. **Information availability** — every decision uses only information available at that decision time; future actual values must not leak into a decision unless the problem explicitly grants them.
6. **Model interface mapping** — state how each data field maps into mathematical symbols, model inputs, constraints, states, or outputs.

If a blocking data defect is discovered, return to `data-audit` or the relevant upstream stage instead of hiding the repair inside the feasibility prototype.

## Step 8 — Propose the minimum viable model (MVM)

Choose the smallest executable instance that preserves the CFQ and its central difficulty.

Possible reductions:

- one representative day instead of a full year,
- one target instead of many targets,
- coarse spatial/temporal grid instead of production resolution,
- one train/validation split instead of full cross-validation,
- one baseline model instead of the full candidate ladder.

The proposal must state:

- tested subproblem,
- minimum viable instance,
- included core constraints/mechanisms,
- intentionally excluded components,
- expected inputs and outputs,
- success criteria,
- what success would prove,
- what success would not prove.

## Step 9 — Human Gate before implementation

Before substantial feasibility code is written, present a short feasibility card and wait for human approval when interactive approval is available.

The card must include:

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

DATA READINESS / INTERFACE ISSUES
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

The human gate is specifically for checking whether the CFQ attacks the real bottleneck, whether the family classification is appropriate, whether required data are legally and semantically usable, and whether the MVM preserves the defining difficulty.

Do not ask the human to review routine implementation details at this gate.

## Step 10 — Implement the prototype

After approval, create temporary or clearly marked prototype code under the live project's `src/` or `experiments/` directory.

Requirements:

- runnable end to end,
- fixed random seed when randomness exists,
- no fabricated data unless a synthetic test is explicitly documented,
- no hidden manual corrections,
- preserve units and key constraints,
- save the main numerical result and at least one diagnostic artifact when useful.

## Step 11 — Run universal checks

Always check:

1. execution success,
2. runtime and computational burden,
3. output shape/range/unit sanity,
4. core constraint satisfaction,
5. numerical stability or repeatability,
6. obvious data leakage or information-timing violations,
7. whether the result changes sensibly under a small perturbation,
8. whether the team can explain why the output is reasonable.

## Step 12 — Run family-specific checks

Use only the relevant sections of `references/feasibility-rules.md` for the primary and secondary families.

These checks supplement the universal checks; they do not replace them.

## Step 13 — Run counterfactual review

Before declaring success, attack the prototype with three questions:

1. If this MVM succeeds completely, what could still make the full modeling route fail?
2. If this MVM fails, does the evidence indicate a model/concept failure, a data/interface failure, or only an implementation bug?
3. Did the MVM obtain success by simplifying away a defining difficulty, information restriction, uncertainty source, or hard constraint?

Record remaining risks explicitly.

## Step 14 — Record blockers and repair cost

Classify every issue as:

- minor: routine implementation or parameter issue,
- moderate: requires model reformulation or nontrivial debugging,
- major: core method may be unsuitable,
- fatal: problem/model should not be selected under current competition constraints.

Estimate repair cost qualitatively, not with fake precision.

## Step 15 — Make a phase-gate decision

Return exactly one status:

- `GO`: core mechanism runs, outputs are plausible, and remaining risks are manageable.
- `GO_WITH_RISKS`: runnable, but one or more moderate/major risks must be handled early.
- `HOLD`: evidence is insufficient; another focused feasibility experiment or upstream data/interface repair is required before commitment.
- `NO_GO`: fatal blocker, unacceptable instability, infeasible computation, invalid information assumptions, or inability to validate the central mechanism.

Do not select the final contest problem solely from a score. The human modeler owns the final decision.

## Outputs

Create or update in the live project:

- `project/feasibility_test.json`
- optional prototype code under `experiments/feasibility/` or `src/`
- optional diagnostics under `results/feasibility/`

Recommended `project/feasibility_test.json` fields:

```json
{
  "problem_or_model": "",
  "final_target": "",
  "dependency_chain": [],
  "critical_path": [],
  "critical_failure_point": "",
  "cfq": {
    "question": "",
    "why_it_may_fail": [],
    "success_evidence": [],
    "failure_evidence": []
  },
  "primary_family": "",
  "secondary_families": [],
  "required_data": [],
  "data_readiness": {
    "availability": {},
    "alignment": {},
    "units_semantics": {},
    "missing_invalid": {},
    "information_availability": {},
    "model_interface_mapping": {}
  },
  "tested_subproblem": "",
  "minimum_viable_instance": "",
  "baseline_or_candidate": "",
  "data_scope": "",
  "success_criteria": [],
  "what_success_proves": [],
  "what_success_does_not_prove": [],
  "human_gate_status": "PENDING | APPROVED | REVISE",
  "universal_checks": {},
  "family_specific_checks": {},
  "counterfactual_review": {},
  "runtime_notes": "",
  "blockers": [],
  "repair_cost": "",
  "status": "GO | GO_WITH_RISKS | HOLD | NO_GO",
  "reason": "",
  "next_action": ""
}
```

If comparing several contest problems, keep one record per problem and add a short comparison table or array. Do not overwrite evidence from earlier tests.

## What this skill must not do

- do not solve every subproblem,
- do not tune for final leaderboard/performance,
- do not replace model-validation,
- do not declare a sophisticated model superior from one toy run,
- do not hide prototype failures,
- do not silently repair upstream data defects inside the prototype,
- do not simplify away the defining difficulty,
- do not use future/unavailable information to manufacture feasibility,
- do not choose the final contest problem without human approval.
