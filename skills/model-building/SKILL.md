---
name: model-building
description: Implement and run the approved modeling experiment ladder in a live competition project, with reproducible code and experiment logging.
---

# Model Building

## Goal

Implement approved models without silently changing the modeling contract.

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the **skills repository root**.

The user only needs to invoke this Skill. Resource lookup is this Skill's responsibility.

Load resources in this order:

1. Read this `SKILL.md`.
2. Read the live project's approved contract and prior decisions:
   - `project/model_spec.json`
   - `project/baseline_solution.json`
   - `project/model_selection_audit.json`
   - `project/data_preparation.json`, if present
3. For each approved model, read only the relevant shared model knowledge under `knowledge/` when needed for implementation details, assumptions, or failure modes.
4. Before writing a reusable implementation from scratch, inspect `algorithms/`:
   - use an index/catalog if present;
   - otherwise inspect only the relevant algorithm family;
   - reuse existing tested utilities when they match the approved contract.
5. If no suitable repository implementation exists, implement the model using appropriate established libraries or project-specific code.
6. Initialize `project/experiment_log.json` from `templates/experiment_log.json` when needed.
7. Write competition-specific code and results only to the live project:
   - code → `src/`
   - experiment artifacts → `results/` or `experiments/`
   - experiment records → `project/experiment_log.json`
8. Do not modify shared `knowledge/`, `algorithms/`, or `templates/` merely to make the current experiment pass. Reusable improvements should be proposed separately from live-project results.

## Read

- `project/model_spec.json`
- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- relevant knowledge and algorithm resources as needed.

## Rules

1. Implement baselines before or alongside advanced candidates.
2. Preserve the approved feature set, target, split strategy, and metrics unless a change is explicitly recorded.
3. Run all approved models needed by the experiment ladder under comparable conditions.
4. Fix random seeds when randomness is used.
5. Save code under the live project's `src/`.
6. Save outputs under `results/`.
7. Append every meaningful run to `project/experiment_log.json`.
8. Record failed experiments when they inform later decisions.
9. Do not silently promote the primary candidate to final model based only on one run.

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
