---
name: model-building
description: Implement and run the approved modeling experiment ladder in a live competition project, with reproducible code and experiment logging.
---

# Model Building

## Goal

Implement approved models without silently changing the modeling contract.

## Read

- `project/model_spec.json`
- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- relevant knowledge and algorithm resources as needed.

## Rules

1. Implement baselines before or alongside advanced candidates.
2. Preserve the approved feature set, target, split strategy, and metrics unless a change is explicitly recorded.
3. Fix random seeds when randomness is used.
4. Save code under the live project's `src/`.
5. Save outputs under `results/`.
6. Append every meaningful run to `project/experiment_log.json`.
7. Record failed experiments when they inform later decisions.

## Experiment entry

Record:

- experiment id,
- model,
- feature set,
- split,
- parameters,
- metrics,
- output files,
- warnings,
- conclusion.

Do not overwrite history when a new run supersedes an old one.
