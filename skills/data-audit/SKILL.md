---
name: data-audit
description: Audit competition data before modeling. Verify dataset structure, observation units, grouping, variable semantics, quality, leakage, cross-dataset compatibility, question-data interfaces, and executable dependencies.
---

# Data Audit

## Goal

Determine what the available data can and cannot support, map actual data to the conceptual requirements in `problem_brief.md`, and produce data constraints/interfaces that downstream preparation, model-selection, and feasibility-test must respect.

Preserve the original `task_dependency_graph`. Data audit adds evidence about implementation; it does not rewrite the statement's logic.

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the skills repository root.

1. Read this `SKILL.md`.
2. Read `skills/data-audit/references/data-audit-checklist.md`.
3. Read `project/problem_brief.md` and `project/assumption_ledger.md` when relevant.
4. Read raw data, documentation, and attachments.
5. Initialize `project/data_audit.json` from `templates/data_audit.json` when absent.
6. Use shared `knowledge/` only as needed to interpret semantics, units, measurement meaning, time/spatial structure, or domain definitions.
7. Do not use `algorithms/` for model selection here. Small deterministic inspection scripts are allowed.

## Core concepts

### Observation unit
The real-world/statistical entity represented by one row or record, such as one subject, one recording, one subject-time measurement, one city-day, or one machine run. `rows` are not automatically independent samples.

### Grouping
A structure in which multiple observations belong to the same higher-level entity or experimental unit. Examples: recordings nested within a subject, measurements nested within a machine, or days nested within a city. Record group keys and nesting when identifiable.

### Subject group
When the higher-level entity is a person/participant/patient, all observations belonging to that same subject form a subject group. Subject groups commonly define the independence/splitting unit: records from one subject should not cross train/test boundaries unless the task explicitly requires and justifies that design.

### Executable dependency graph
A data-verified implementation graph derived from the task dependency graph plus actual dataset availability, field/label mappings, observation/group structure, cross-dataset compatibility, and constraints. It states which conceptual interfaces are supported, partially supported, blocked, or require assumptions/external data. It must never silently erase the original task graph.

## Audit procedure

### 1. Dataset inventory
For every relevant file/table/dataset record:
- dataset ID and source file;
- rows/columns/features;
- observation unit;
- unique higher-level units such as subjects when applicable;
- target/label candidates;
- identifiers/group keys;
- repeated-measure, temporal, spatial, or hierarchical structure;
- likely subproblem usage, marked as verified or tentative.

### 2. Variable and quality audit
Check variable semantics, roles, units, missingness, duplicates, suspicious ranges/outliers, imbalance, timestamps/coordinates, and semantic conflicts. Roles may include input, target, state, parameter, constraint, identifier, grouping, metadata, covariate, derived, potential_leakage, or other.

### 3. Structure and leakage audit
Check observation independence, repeated measurements/groups, target leakage, identifier leakage, subject leakage, preprocessing leakage, source/dataset leakage, and future/look-ahead leakage. Recommend split units/strategy from the actual structure.

### 4. Cross-dataset compatibility
When multiple datasets may interact, audit common fields/features, semantic definitions, units, measurement protocols, target definitions, cohort/population differences, and whether direct concatenation/transfer/external validation/comparison is defensible. Do not equate same names with same meanings.

### 5. Problem-to-data and question-to-data mapping
For every important conceptual quantity in `problem_brief.md`, classify its actual mapping as `clear`, `derived`, `ambiguous`, or `missing`.

For every Q1...Qn record:
- datasets actually available to it;
- required conceptual inputs/target from problem-analysis;
- verified fields/derivations;
- observation and grouping requirements;
- missing/ambiguous inputs;
- upstream outputs it consumes;
- outputs that can actually be produced;
- downstream consumers;
- support status: `SUPPORTED`, `PARTIALLY_SUPPORTED`, `BLOCKED`, `REQUIRES_ASSUMPTION`, or `REQUIRES_EXTERNAL_DATA`.

### 6. Dependency graphs
Produce and keep distinct:

1. `task_dependency_graph`: copied/referenced from problem-analysis; statement-level logic.
2. `data_dependency_graph`: actual datasets, fields, labels, groups, and constraints feeding each question.
3. `executable_dependency_graph`: task graph reconciled with verified data support. Mark unsupported edges/nodes explicitly and explain the blocking evidence.

Do not infer that a task dependency is executable merely because the statement requests it.

## Time- and decision-aware audit

For sequential/forecasting/scheduling/control problems distinguish observation time, availability/publication time, forecast issue time, target horizon, decision time, update interval, and future-realized unavailable values.

## Cleaning/preparation boundary

Data audit detects and characterizes quality issues and records preparation requirements/constraints. It should not automatically perform or prescribe irreversible cleaning merely because a value is missing, duplicated, extreme, or unusual.

Examples:
- acceptable: `missingness detected; may be structural; semantics must be verified before imputation`;
- not acceptable by default: `fill all missing values with the mean`.

Concrete cleaning/imputation/filtering decisions belong to `data-preparation`, except trivial lossless normalization needed to inspect the data. Never hide transformations inside later model code.

## Output

Create/update `project/data_audit.json`. It must include dataset inventory, variable catalog, observation/group structure, quality findings, leakage risks, split constraints, cross-dataset compatibility, problem-data mapping, question-data mapping, task/data/executable dependency graphs, unresolved issues, and downstream preparation requirements.

## Forbidden behavior

- Do not default to random splitting without checking independence/group/time/spatial structure.
- Do not count repeated rows/recordings as independent subjects without justification.
- Do not use identifiers/group keys as predictors merely because they are numeric.
- Do not silently concatenate datasets with incompatible semantics/protocols.
- Do not treat future-realized values as decision-time inputs.
- Do not silently resolve unit/timestamp/coordinate/semantic mismatches.
- Do not automatically impute/delete/clip data without semantic justification.
- Do not declare a final model from data audit alone.
