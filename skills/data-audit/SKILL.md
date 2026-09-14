---
name: data-audit
description: Audit competition data before modeling. Use to inspect schema, units, missingness, outliers, grouping, temporal/spatial structure, information timing, leakage risk, model-interface semantics, and valid train-test strategies.
---

# Data Audit

## Goal

Determine what the data can and cannot support before model selection, and preserve enough data semantics for downstream feasibility tests to verify model interfaces without repeating the full audit.

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the **skills repository root**.

The user only needs to invoke this Skill. Do not require the user to separately list repository resources.

Load resources in this order:

1. Read this `SKILL.md`.
2. Read this Skill's local reference:
   - `skills/data-audit/references/data-audit-checklist.md`
3. Read live-project state:
   - `project/problem_brief.md`
   - `project/assumption_ledger.md`, when relevant
4. Read the live project's raw data and attachments.
5. Initialize `project/data_audit.json` from `templates/data_audit.json` when it does not exist.
6. Use shared `knowledge/` only when needed to interpret domain-specific data semantics, units, time structure, or measurement meaning. Do **not** use shared knowledge here to perform final model selection.
7. Do not use `algorithms/` for modeling in this phase. Small deterministic inspection scripts are allowed when needed for auditing.

The data audit records **facts about the current data**. General statements such as “XGBoost fits tabular data” belong to shared model knowledge or model-selection, not to `data_audit.json`.

## Read

- `project/problem_brief.md`
- raw data and attachments
- `references/data-audit-checklist.md`

## Audit

Check:

- sample count and feature count,
- row meaning and variable semantics,
- variable types and units,
- missing values and duplicates,
- suspicious ranges and outliers,
- categorical imbalance,
- time ordering and sampling frequency,
- forecast issue time and forecast horizon when relevant,
- decision/update interval when defined by the problem,
- spatial coordinate system and resolution when relevant,
- repeated measurements / experimental groups,
- target leakage,
- preprocessing leakage,
- future-information / look-ahead leakage,
- plausible split units,
- extrapolation requirements,
- whether important fields can be mapped unambiguously to the quantities required in `problem_brief.md`.

## Time- and decision-aware audit

For sequential, forecasting, scheduling, control, or time-dependent problems, explicitly distinguish:

- observation timestamp,
- availability/publication timestamp when different,
- forecast issue time,
- forecast target/horizon,
- decision time,
- future-realized values that are unavailable when the decision is made.

Do not assume that a timestamped value was known at all earlier times merely because it exists in the dataset.

## Output

Create or update `project/data_audit.json` in the live project using the repository template.

The report must include:

- recommended split strategy and leakage risks that model-selection must respect,
- variable-level units and semantics,
- temporal/spatial resolution metadata when relevant,
- information-availability metadata when relevant,
- unresolved data-to-problem mapping issues,
- major quality issues and whether they block modeling or only require preprocessing.

The audit should describe cleaning/preparation decisions explicitly. Do not hide important transformations inside later model code.

## Forbidden behavior

- Do not default to random train/test split without checking group, time, spatial, or repeated-measure structure.
- Do not treat future-realized values as decision-time inputs.
- Do not silently resolve unit, timestamp, coordinate, or semantic mismatches.
- Do not declare a final model from data audit alone.
