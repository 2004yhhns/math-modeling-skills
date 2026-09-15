---
name: model-building
description: Implement and run the approved shortlisted models in the live competition project, using model_spec.json as the modeling-hand to programming-hand experiment contract.
---

# Model Building

## Goal

Turn the approved shortlist into reproducible full modeling experiments without silently changing the modeling contract.

Model-building is where the majority of formal algorithm implementation, feature pipeline work, tuning and training should occur. Screening prototypes are not substitutes for these full experiments.

## Workflow position

```text
model-selection
→ risk triage / optional feasibility
→ screening
→ SHORTLIST
   ├─ baseline(s)
   ├─ primary candidate
   └─ serious alternative(s)
→ model-building
→ model-validation
```

A shortlist may contain multiple models. Do not require model-selection/screening to choose one final model prematurely.

## Repository resource loading

Load:

1. this `SKILL.md`;
2. live-project contract/evidence:
   - `project/model_spec.json`
   - `project/baseline_solution.json`
   - `project/model_selection_audit.json`
   - `project/data_preparation.json`, if present
   - relevant `project/feasibility_test.json` entries for high-risk shortlisted candidates
3. relevant shared `knowledge/` only as needed;
4. `algorithms/` for reusable tested implementations/utilities after the approved model is known;
5. initialize/update `project/experiment_log.json` from the template when needed.

Competition-specific code/results belong only to the live project:

- code → `src/`
- experiment scripts/configs → `experiments/`
- outputs → `results/`
- experiment records → `project/experiment_log.json`

## `model_spec.json` is the experiment contract

The modeling hand should specify, as applicable:

- target/problem quantity;
- shortlisted models and their roles;
- approved features/data sources;
- forbidden predictors/information;
- split/CV strategy and grouping/time rules;
- metrics;
- preprocessing boundaries;
- tuning strategy/budget;
- random seeds/reproducibility requirements;
- robustness/interpretability requirements;
- required output artifacts.

The programming hand implements this contract. If implementation reveals that the contract is impossible, ambiguous, or materially flawed, record the issue and return it to the owning modeling stage rather than silently redesigning the model in code.

## Build sequence

1. verify prepared-data paths and contract;
2. implement shared leakage-safe preprocessing pipeline;
3. implement/run baseline(s);
4. implement/run primary and alternative shortlisted models under comparable conditions;
5. tune only according to the approved tuning plan;
6. save predictions, metrics, diagnostics and model artifacts needed by validation;
7. log every meaningful experiment, including informative failures.

## Rules

- preserve approved feature set, target, split/CV, information legality and metrics;
- fit model-dependent preprocessing only within the legal training scope/fold;
- fix seeds where randomness exists;
- do not silently promote the primary candidate to final model;
- do not hide failed experiments;
- do not modify reusable repository knowledge/algorithms merely to make the current project pass.

## Experiment entry

Record at least:

- experiment id;
- model and role;
- code/config version;
- data/feature set;
- split/fold definition;
- model-dependent preprocessing;
- parameters/tuning budget;
- random seed;
- metrics;
- runtime;
- output files;
- warnings/failures;
- deviations from `model_spec.json`;
- provisional conclusion.

Do not overwrite experiment history when a new run supersedes an old one.

## Handoff to validation

Model-building provides evidence, not the final winner. Pass all shortlisted full-build results needed for baseline comparison, generalization, robustness, sensitivity, interpretability and error analysis to `model-validation`.
