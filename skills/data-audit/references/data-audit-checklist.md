# Data Audit Checklist

## Structure

- row meaning
- column meaning
- target variable
- units
- primary key / group key
- time or spatial index
- source file / table for each important field
- whether each important quantity is observed, forecast, estimated, derived, or future-realized

## Quality

- missingness
- duplicates
- impossible values
- inconsistent units
- outliers
- class imbalance
- inconsistent timestamps
- inconsistent coordinate conventions
- duplicated or conflicting records across files

## Temporal / sequential semantics

When time or decisions matter, record explicitly:

- observation timestamp,
- sampling frequency,
- availability/publication timestamp if different from observation time,
- forecast issue time,
- forecast target time / horizon,
- decision time,
- decision update interval,
- future-realized fields that are unavailable at decision time,
- timezone or clock convention when relevant.

A field existing in a dataset does not imply that it was available to the decision maker earlier in time.

## Spatial semantics

When spatial structure matters, record:

- coordinate system,
- origin and axis convention,
- angle convention if relevant,
- spatial resolution,
- region/boundary definition,
- transformations between coordinate systems.

## Problem-to-data mapping

For every important quantity named in `project/problem_brief.md`, determine whether it is:

- directly present in the data,
- derivable from available fields,
- ambiguous,
- missing.

Record the mapping and any transformation needed. Flag semantic mismatches even when column names look similar.

## Leakage

Ask whether two rows share information because they come from:

- the same subject,
- the same experiment,
- the same material,
- the same machine,
- the same time window,
- the same transformed target.

Also ask whether a model or decision could accidentally use:

- future actual values,
- statistics fitted on future/test data,
- post-outcome variables,
- target-derived features,
- forecasts published after the decision time.

If yes, use an appropriate grouped/temporal/spatial split or remove the invalid information path.

## Cleaning and preparation contract

Record important cleaning/preparation decisions explicitly, including:

- unit conversion,
- resampling / aggregation,
- interpolation,
- missing-value handling,
- coordinate transformation,
- deduplication,
- filtering rules.

Do not silently bury a high-impact data repair in later model code.

## Output contract

At minimum record:

- `task_data_regime`
- `rows`
- `features`
- `variable_catalog`
- `group_structure`
- `time_structure`
- `spatial_structure`
- `information_timing`
- `problem_data_mapping`
- `leakage_risks`
- `recommended_split`
- `cleaning_or_preparation_decisions`
- `unresolved_interface_issues`
- `major_quality_issues`

The output should be sufficient for `model-selection` to assess candidate data requirements and for `feasibility-test` to perform a focused MVM Data Readiness & Interface Check without repeating the entire audit.
