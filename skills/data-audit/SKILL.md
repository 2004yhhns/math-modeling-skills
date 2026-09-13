---
name: data-audit
description: Audit competition data before modeling. Use to inspect schema, units, missingness, outliers, grouping, temporal/spatial structure, leakage risk, and valid train-test strategies.
---

# Data Audit

## Goal

Determine what the data can and cannot support before model selection.

## Read

- `project/problem_brief.md`
- raw data and attachments
- `references/data-audit-checklist.md`

## Audit

Check:

- sample count and feature count,
- variable types and units,
- missing values and duplicates,
- suspicious ranges and outliers,
- categorical imbalance,
- time ordering,
- spatial dependence,
- repeated measurements / experimental groups,
- target leakage,
- preprocessing leakage,
- plausible split units,
- extrapolation requirements.

## Output

Create or update `project/data_audit.json` in the live project.

The report must include a recommended split strategy and any leakage risks that model-selection must respect.

## Forbidden behavior

Do not default to random train/test split without checking group, time, spatial, or repeated-measure structure.
