---
name: model-selection
description: Select defensible models for a mathematical modeling subproblem. Use after the problem and data regime are understood. Requires baselines, candidate comparison, rejected-model reasons, and a validation plan.
---

# Model Selection

## Goal

Build a defensible model ladder rather than naming one sophisticated model.

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
- interpretability requirement,
- extrapolation requirement.

## Step 2 — Design baselines

Choose:

- Baseline 0: simplest meaningful benchmark.
- Baseline 1: standard competition-grade benchmark when useful.

## Step 3 — Generate candidates

Propose 2–4 candidates from distinct methodological families.

For each candidate give:

- why it fits,
- assumptions,
- data requirements,
- strengths,
- weaknesses,
- interpretability,
- computation cost,
- overfitting risk,
- validation method.

## Step 4 — Compare and reject

Explicitly identify:

- primary candidate,
- baseline(s),
- independent validation / alternative candidate,
- rejected models and reasons.

## Step 5 — Experiment ladder

Recommend an order such as:

```text
Baseline 0
→ Baseline 1
→ Candidate A
→ Candidate B
```

Do not declare a winner before experiments.

## Outputs

Create or update:

- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

## Forbidden behavior

- no deep learning for novelty alone,
- no candidate without a baseline,
- no accuracy-only evaluation when other risks matter,
- no causal interpretation from predictive performance alone.
