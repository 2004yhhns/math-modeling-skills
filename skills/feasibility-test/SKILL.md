---
name: feasibility-test
description: Test a genuinely high-risk candidate mechanism or interface with a minimum viable prototype before expensive full model building. This is a conditional gate, not a mandatory test for every candidate.
---

# Feasibility Test

## Goal

Answer one narrow question:

> Does the high-risk mechanism/interface identified during model-selection work well enough under the real data and constraints to justify keeping this candidate?

Feasibility testing produces **viability evidence**, not comparative ranking and not a final model.

## Position in the workflow

```text
candidate pool
     ↓
risk triage
 ┌───┴─────────────┐
 ↓                 ↓
LOW_RISK        HIGH_RISK
 ↓                 ↓
 │          feasibility-test
 │          MVM + sanity checks
 │                 ↓
 │      GO / GO_WITH_RISKS /
 │          HOLD / NO_GO
 └──────────┬──────┘
            ↓
      viable candidates
            ↓
        screening
```

Do not invoke this Skill for every mature standard model. Ordinary candidates whose data/interface requirements are already clear should proceed to comparable screening.

## Read

1. `project/problem_brief.md`
2. `project/data_audit.json`
3. `project/data_preparation.json`, if present
4. `project/model_selection_audit.json`
5. `project/model_spec.json`, if already initialized
6. `project/assumption_ledger.md`
7. `references/feasibility-rules.md`
8. relevant `knowledge/`/`algorithms/` only as needed.

Reuse upstream diagnosis. Do not repeat a full data audit, literature review, or model-selection exercise.

## Step 1 — Confirm the trigger

State:

- candidate;
- risk trigger;
- critical mechanism/interface;
- why ordinary screening is insufficient;
- downstream consequence if it fails.

If no genuine high-risk trigger exists, return `NOT_REQUIRED` and route the candidate to screening.

## Step 2 — Write the Core Feasibility Question (CFQ)

Use:

```text
Candidate:
...
Critical mechanism/interface:
...
Why it may fail:
...
CFQ:
Under the actual data, information and constraints, can [mechanism/interface]
produce [required result] well enough to justify screening/full investment?
Success evidence:
- ...
Failure evidence:
- ...
```

## Step 3 — Readiness/interface check

Check only quantities needed by the CFQ:

- availability and semantics;
- units/resolution/group/time alignment;
- information legality/leakage;
- remaining invalid/missing values;
- sensitivity to critical preparation decisions;
- input/output interface mapping.

Route upstream defects back to their owner. Do not silently clean or reinterpret data inside feasibility code.

## Step 4 — Minimum Viable Model (MVM)

Build the smallest executable prototype that preserves the defining risk. Reduce scale, not the mechanism whose failure is being tested.

Record:

- included mechanism/constraints;
- intentionally excluded noncritical components;
- inputs/outputs;
- success criteria;
- what success proves;
- what success does not prove.

## Human Gate

Before substantial prototype implementation, show a compact Feasibility Card:

```text
CANDIDATE
RISK TRIGGER
CFQ
REQUIRED DATA / INTERFACE
PROPOSED MVM
SUCCESS / FAILURE EVIDENCE
HUMAN APPROVAL: PENDING
```

## Step 5 — Prototype and checks

After approval, run only the checks needed to answer the CFQ, plus universal sanity checks:

- execution and runtime;
- output shape/range/unit sanity;
- hard-constraint satisfaction;
- numerical stability/repeatability;
- leakage/information legality;
- small perturbation sanity;
- dependence on questionable preparation choices.

Use family-specific checks from `references/feasibility-rules.md` only when relevant.

## Step 6 — Decision

Return one:

- `GO` — candidate can proceed to screening;
- `GO_WITH_RISKS` — proceed, carrying explicit warnings;
- `HOLD` — evidence/data/approval is insufficient;
- `NO_GO` — remove or redesign the candidate;
- `NOT_REQUIRED` — no genuine feasibility gate was needed; proceed to screening.

Create/update `project/feasibility_test.json`. Multiple high-risk candidates may have separate entries/tests.

## Boundaries

Feasibility test does **not**:

- select the best model;
- compare every candidate exhaustively;
- replace screening;
- replace full model building;
- replace model validation;
- repeat literature research;
- prove that a toy success guarantees full-model success.

Do not hide failures or obtain a false `GO` by simplifying away the defining difficulty or by silently changing data/preparation rules.
