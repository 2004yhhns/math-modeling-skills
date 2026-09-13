# Data Audit Checklist

## Structure

- row meaning
- column meaning
- target variable
- units
- primary key / group key
- time or spatial index

## Quality

- missingness
- duplicates
- impossible values
- inconsistent units
- outliers
- class imbalance

## Leakage

Ask whether two rows share information because they come from:

- the same subject,
- the same experiment,
- the same material,
- the same machine,
- the same time window,
- the same transformed target.

If yes, prefer a grouped or temporal split.

## Output contract

At minimum record:

- `task_data_regime`
- `rows`
- `features`
- `group_structure`
- `time_structure`
- `leakage_risks`
- `recommended_split`
- `major_quality_issues`
