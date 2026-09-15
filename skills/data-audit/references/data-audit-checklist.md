# Data Audit Checklist

## Dataset inventory and observation structure

For every relevant dataset/file:
- source file and dataset ID;
- rows, columns, feature count;
- observation unit: what exactly does one row represent?;
- unique higher-level units such as subjects/devices/locations;
- primary identifier and group keys;
- repeated-measure/nested/hierarchical structure;
- target/label candidates;
- intended question usage.

Never equate row count with independent sample count without checking the observation unit and grouping.

## Variable semantics and roles

Record column meaning, units, source, availability, resolution, and role. Useful roles include `input`, `target`, `state`, `parameter`, `constraint`, `identifier`, `grouping`, `metadata`, `covariate`, `derived`, `potential_leakage`, and `other`.

An identifier may need to be retained for grouping while being forbidden as a predictor.

## Quality

Check missingness, duplicates, impossible/suspicious values, inconsistent units, outliers, imbalance, timestamp/coordinate inconsistencies, and duplicated/conflicting records across files. Characterize first; do not automatically repair.

## Grouping and leakage

Ask whether rows share information because they belong to the same subject, experiment, material, machine, location, time window, source dataset, or target-derived transformation.

Check target leakage, identifier leakage, subject/group leakage, preprocessing leakage, dataset-source leakage, future/look-ahead leakage, and post-outcome variables. Recommend the split unit and strategy that preserves independence appropriate to the task.

## Temporal / sequential semantics

When relevant record observation timestamp, sampling frequency, availability/publication timestamp, forecast issue time, target horizon, decision time, update interval, timezone/clock convention, and future-realized fields unavailable at decision time.

## Spatial semantics

When relevant record coordinate system, origin/axis/angle convention, spatial resolution, boundaries, and coordinate transformations.

## Cross-dataset compatibility

For every dataset pair that may be combined, transferred across, or used for validation/comparison, check:
- common features/fields;
- semantic equivalence;
- units/scales;
- measurement/extraction protocol;
- target definition;
- cohort/population differences;
- observation-unit compatibility;
- whether direct concatenation is defensible;
- safer alternatives such as common-feature comparison or external validation.

## Problem-to-data mapping

For every important conceptual quantity in `project/problem_brief.md`, determine whether it is directly present, derivable, ambiguous, or missing. Record concrete fields/derivations and semantic mismatches.

## Question-to-data mapping

For every Q1...Qn record:
- conceptual requirements inherited from problem-analysis;
- datasets/fields actually supporting them;
- target/label availability;
- observation/group structure;
- upstream outputs consumed;
- outputs actually supportable;
- downstream consumers;
- missing/ambiguous requirements;
- support status: `SUPPORTED`, `PARTIALLY_SUPPORTED`, `BLOCKED`, `REQUIRES_ASSUMPTION`, or `REQUIRES_EXTERNAL_DATA`.

## Dependency reconciliation

Keep three layers separate:

- `task_dependency_graph`: what should depend on what according to the statement;
- `data_dependency_graph`: what actual data/fields/groups/constraints feed each subproblem;
- `executable_dependency_graph`: which task interfaces remain implementable after data verification.

Unsupported task edges must remain visible and be marked with the evidence causing the gap.

## Cleaning/preparation contract

Audit records detected issues and preparation requirements/constraints. High-impact repair decisions such as imputation, interpolation, deduplication, filtering, clipping, aggregation, resampling, or unit conversion should be passed to `data-preparation` for justified execution and validation. Do not silently bury them in model code.

## Output contract

At minimum record:
- `task_data_regime`;
- `dataset_inventory`;
- `variable_catalog`;
- `group_structure`;
- `time_structure`;
- `spatial_structure`;
- `information_timing`;
- `problem_data_mapping`;
- `question_data_mapping`;
- `dataset_compatibility`;
- `leakage_risks`;
- `recommended_split`;
- `task_dependency_graph`;
- `data_dependency_graph`;
- `executable_dependency_graph`;
- `preparation_requirements`;
- `unresolved_interface_issues`;
- `major_quality_issues`.
