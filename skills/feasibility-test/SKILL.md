---
name: feasibility-test
description: Run a fast minimum-viable-model test after model selection and before full model building. Use to decide whether a candidate problem/model is actually executable under competition time constraints. Requires a runnable prototype, sanity checks, failure logging, and a go/hold/no-go decision.
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

## Step 1 — Define the feasibility question

State explicitly:

- what must be demonstrated,
- which subproblem is being tested,
- which model or algorithm is being tested,
- what is intentionally excluded,
- what would count as a fatal blocker.

Examples:

- optimization: can a simplified LP/MILP solve and satisfy all core constraints?
- geometry/search: can one simulated target be localized from the required observations?
- PDE/mechanism: can a coarse-grid solver remain stable and produce physically plausible fields?
- prediction: can a leakage-safe baseline train and outperform a naive benchmark?

## Step 2 — Choose the minimum viable instance

Reduce the problem while preserving the central difficulty.

Possible reductions:

- one representative day instead of a full year,
- one target instead of many targets,
- coarse spatial/temporal grid instead of production resolution,
- one train/validation split instead of full cross-validation,
- one baseline model instead of the full candidate ladder.

Do not simplify away the mechanism that makes the problem difficult.

## Step 3 — Implement the prototype

Create temporary or clearly marked prototype code under the live project's `src/` or `experiments/` directory.

Requirements:

- runnable end to end,
- fixed random seed when randomness exists,
- no fabricated data unless a synthetic test is explicitly documented,
- no hidden manual corrections,
- preserve units and key constraints,
- save the main numerical result and at least one diagnostic artifact when useful.

## Step 4 — Run universal checks

Always check:

1. execution success,
2. runtime and computational burden,
3. output shape/range/unit sanity,
4. core constraint satisfaction,
5. numerical stability or repeatability,
6. obvious data leakage or information-timing violations,
7. whether the result changes sensibly under a small perturbation,
8. whether the team can explain why the output is reasonable.

## Step 5 — Run family-specific checks

Use only the relevant section of `references/feasibility-rules.md`.

Supported families include:

- optimization / scheduling,
- prediction / classification,
- geometry / localization / search,
- simulation / mechanism / PDE,
- graph / routing / path planning,
- stochastic / Monte Carlo models.

These checks supplement the universal checks; they do not replace them.

## Step 6 — Record blockers and repair cost

Classify every issue as:

- minor: routine implementation or parameter issue,
- moderate: requires model reformulation or nontrivial debugging,
- major: core method may be unsuitable,
- fatal: problem/model should not be selected under current competition constraints.

Estimate repair cost qualitatively, not with fake precision.

## Step 7 — Make a phase-gate decision

Return exactly one status:

- `GO`: core mechanism runs, outputs are plausible, and remaining risks are manageable.
- `GO_WITH_RISKS`: runnable, but one or more moderate/major risks must be handled early.
- `HOLD`: evidence is insufficient; another focused feasibility experiment is required before commitment.
- `NO_GO`: fatal blocker, unacceptable instability, infeasible computation, or inability to validate the central mechanism.

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
  "tested_subproblem": "",
  "minimum_viable_instance": "",
  "baseline_or_candidate": "",
  "data_scope": "",
  "success_criteria": [],
  "universal_checks": {},
  "family_specific_checks": {},
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
- do not simplify away the defining difficulty,
- do not choose the final contest problem without human approval.
