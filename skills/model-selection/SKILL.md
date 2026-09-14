---
name: model-selection
description: Select defensible models for a mathematical modeling subproblem. Use after the problem and data regime are understood. Requires baselines, candidate comparison, rejected-model reasons, interface/data requirements, feasibility risks, and a validation plan.
---

# Model Selection

## Goal

Build a defensible model ladder rather than naming one sophisticated model, and preserve enough information for `feasibility-test` to identify the real execution bottleneck without reconstructing model assumptions from scratch.

## Read

1. `project/problem_brief.md`
2. `project/data_audit.json`
3. `project/assumption_ledger.md`
4. `references/selection-rules.md`
5. Relevant shared files under `knowledge/` only as needed.

## Step 1 — Classify the subproblem

Identify:

- mathematical task,
- inputs,
- outputs,
- constraints,
- evaluation target,
- upstream dependencies,
- downstream consumers of important outputs,
- information-availability restrictions,
- temporal/spatial resolution requirements,
- interpretability requirement,
- extrapolation requirement.

## Step 2 — Design baselines

Choose:

- Baseline 0: simplest meaningful benchmark.
- Baseline 1: standard competition-grade benchmark when useful.

Each baseline should state what data it requires and what downstream output it produces.

## Step 3 — Generate candidates

Propose 2–4 candidates from distinct methodological families.

For each candidate give:

- why it fits,
- assumptions,
- exact data requirements,
- required preprocessing or transformations,
- expected inputs and outputs,
- upstream dependency,
- downstream consumer / interface,
- strengths,
- weaknesses,
- interpretability,
- computation cost,
- overfitting risk,
- information-timing or leakage risks,
- implementation/numerical risks,
- validation method.

If a candidate depends on an upstream model output, explicitly describe the interface. Examples: forecast → optimizer, estimated parameter → simulator, detector → localizer, simulation output → decision rule.

## Step 4 — Compare and reject

Explicitly identify:

- primary candidate,
- baseline(s),
- independent validation / alternative candidate,
- rejected models and reasons.

Do not compare models only by expected predictive accuracy. Include data readiness, information legality, downstream interface risk, computational feasibility, and ease of validation.

## Step 5 — Identify feasibility targets

Before handing off to `feasibility-test`, record candidate risks that deserve a minimum viable test.

For the primary candidate and any serious alternative, identify:

- the most important mechanism or model-to-model interface that may fail,
- why failure would matter downstream,
- what evidence would increase confidence,
- what data are needed for that test,
- whether a special information-timing or resolution constraint must be preserved.

These are **candidate feasibility targets**, not the final CFQ. The `feasibility-test` skill owns the final critical-path analysis and CFQ formulation.

## Step 6 — Experiment ladder

Recommend an order such as:

```text
Baseline 0
→ Baseline 1
→ Candidate A
→ Candidate B
```

Do not declare a winner before experiments.

If a candidate has substantial implementation, numerical, interface, or information-timing risk, mark it as requiring feasibility testing before full model building.

## Outputs

Create or update:

- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

The outputs should preserve enough information for downstream feasibility testing to recover:

- candidate data requirements,
- model inputs and outputs,
- upstream/downstream interfaces,
- information restrictions,
- resolution requirements,
- major risks,
- candidate feasibility targets,
- experiment/validation plan.

## Forbidden behavior

- no deep learning for novelty alone,
- no candidate without a baseline,
- no accuracy-only evaluation when other risks matter,
- no causal interpretation from predictive performance alone,
- no candidate that silently requires data unavailable at the relevant decision time,
- no handoff to model-building when a major feasibility risk has been identified but not tested.
