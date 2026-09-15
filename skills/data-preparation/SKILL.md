---
name: data-preparation
description: Prepare and clean competition data after data-audit and before model-selection. Use a conservative, problem-driven policy: preserve raw data, distinguish structural from erroneous missingness, avoid unjustified imputation/deletion, prevent leakage, record every transformation, and escalate high-impact semantic changes for human review.
---

# Data Preparation

## Goal

Convert audited raw data into a reproducible prepared dataset **without erasing information that the problem may need**.

This skill is not a generic "make the table clean" routine. Its purpose is:

> Apply only transformations that are justified by the problem meaning, data-generating process, downstream mathematical use, and information available at the relevant time.

The default stance is conservative: when the meaning of a suspicious value or missing entry is uncertain, preserve it, mark it, and investigate rather than automatically filling, clipping, deleting, smoothing, or resampling it.

## Position in the workflow

```text
problem-analysis
→ data-audit
→ data-preparation
→ model-selection
   ├─ low-risk candidates → screening
   └─ high-risk candidates → feasibility-test → screening
→ shortlist
→ model-building
→ model-validation
→ improvement loop
```

`data-audit` diagnoses and recommends. `data-preparation` executes approved model-independent cleaning/preparation. Model-dependent preprocessing such as train-fitted scaling, encoding, target-aware feature selection, or learned imputation remains inside the later modeling pipeline.

## Read

1. `project/problem_brief.md`
2. `project/data_audit.json`
3. `project/assumption_ledger.md`
4. raw data and attachments referenced by the project
5. `references/preparation-rules.md`
6. existing `project/data_preparation.json`, if present

## Non-negotiable principles

1. **Preserve raw data.** Never overwrite the original files. Write prepared outputs to a separate path such as `data/processed/`.
2. **Problem meaning comes before convenience.** A value is not an error merely because it is unusual, missing, zero, discontinuous, or inconvenient for an algorithm.
3. **Missing is not automatically wrong.** A missing value may mean not applicable, not measured, censored, unavailable at decision time, structurally impossible, sensor failure, or genuinely unknown. These cases must not be treated as equivalent.
4. **Do not impute by reflex.** Mean/median/mode/forward-fill/interpolation/model-based imputation are methods, not defaults. Use them only when their assumptions fit the specific variable and task.
5. **Do not delete by reflex.** Row/column deletion must have a defensible reason and an impact assessment. Rare/extreme observations may contain exactly the behavior the model must explain.
6. **Do not convert missing to zero unless zero has the same domain meaning.** `0`, `NA`, `not applicable`, `not observed`, and `future unavailable` are different states unless the problem explicitly makes them equivalent.
7. **Do not smooth away the phenomenon.** Filtering, winsorization, clipping, aggregation, interpolation, or resampling must not erase peaks, shocks, boundaries, regime changes, failures, rare events, or physical discontinuities that matter to the task.
8. **No leakage through preparation.** A preparation step must not use future-realized information or statistics from held-out data when the later model would not have that information.
9. **Units, coordinates, clocks, and semantics are first-class.** Conversions must be explicit and auditable.
10. **Every change must be traceable.** Record what changed, why, how many values/rows were affected, and what downstream risk remains.

## Step 1 — Define the preparation scope

Before changing data, state:

- current subproblem(s),
- downstream quantities/models the prepared data must support,
- raw input files,
- intended prepared outputs,
- variables that are critical to the problem,
- audited issues to be addressed,
- issues deliberately left unresolved.

Do not perform unrelated "cleanup" simply because it is possible.

## Step 2 — Classify every proposed issue before acting

For each missing, duplicate, suspicious, inconsistent, misaligned, or malformed item, classify its likely meaning.

Useful categories include:

- confirmed data error,
- structural / not applicable,
- expected absence,
- censored / below detection / truncated,
- unavailable at the relevant decision time,
- measurement failure,
- formatting / parsing problem,
- unit / coordinate / timestamp inconsistency,
- duplicate record,
- legitimate extreme / rare event,
- unresolved / ambiguous.

If the category is unresolved and the action could change model conclusions, do not silently repair it.

## Step 3 — Build a preparation decision table

For every issue, record:

```text
VARIABLE / RECORD
ISSUE
SEMANTIC INTERPRETATION
EVIDENCE
PROPOSED ACTION
ALTERNATIVES CONSIDERED
WHY THIS ACTION IS DEFENSIBLE
VALUES / ROWS AFFECTED
INFORMATION-TIMING RISK
DOWNSTREAM IMPACT
REVERSIBLE?
HUMAN REVIEW REQUIRED?
```

The proposed action may be:

- preserve unchanged,
- preserve + add missing/quality indicator,
- correct a confirmed format/unit/timestamp error,
- remove an exact duplicate,
- exclude a demonstrably invalid record,
- derive a variable from documented fields,
- align or resample using a justified rule,
- impute using a justified method,
- defer to a model-dependent pipeline,
- escalate / leave unresolved.

"Do nothing" is a valid and often preferable preparation decision.

## Step 4 — Missing-value decision protocol

For each important variable with missing values, answer in order:

1. **What does missing mean here?**
2. **Is the value actually supposed to exist?**
3. **Would a model/decision at that time have known the value?**
4. **Is missingness itself informative?**
5. **Is there a defensible source or deterministic rule for reconstruction?**
6. **Would imputation distort totals, conservation laws, temporal dynamics, spatial structure, extremes, labels, or uncertainty?**
7. **Would the imputation method use information unavailable at inference/decision time?**
8. **How sensitive are conclusions to preserving vs imputing it?**

Only then choose an action.

### Strong prohibitions

- Do not globally fill numeric columns with means/medians merely to remove `NA`.
- Do not forward-fill across regime boundaries, resets, long gaps, different entities, or periods where the quantity is unknown.
- Do not interpolate across gaps unless continuity/smoothness and gap length make that assumption defensible.
- Do not impute target labels merely to increase sample size unless the problem explicitly defines a valid reconstruction rule.
- Do not use future observations to fill earlier values in a real-time/sequential task unless those observations would have been available then.
- Do not replace structural missingness with an ordinary numeric estimate when the missing state has separate meaning.

When missingness may matter, prefer preserving an explicit missingness/quality indicator if compatible with the downstream method.

## Step 5 — Handle duplicates, outliers, and impossible values conservatively

### Duplicates

Before deleting, determine whether repeated rows are:

- accidental exact duplicates,
- repeated measurements,
- multiple events with equal values,
- records from different entities/times that happen to match.

Delete only confirmed duplicates under the correct key.

### Outliers

Do not remove observations merely because they are far from the mean or fail an automatic z-score/IQR rule.

First determine whether the observation is:

- physically impossible,
- inconsistent with the data definition,
- a sensor/entry failure,
- or a legitimate rare/extreme event.

If the competition problem concerns peaks, failures, extremes, anomalies, boundaries, risk, or robustness, an outlier may be central evidence.

### Impossible values

Correct or exclude only when the domain constraint is defensible. Record the rule and affected records. Do not invent domain bounds for convenience.

## Step 6 — Units, timestamps, coordinates, and resolution

For any conversion or alignment:

- preserve the original field when practical,
- record source and destination units/conventions,
- state the formula or mapping,
- verify dimensional consistency,
- distinguish timezone/clock conventions,
- avoid silent rounding,
- avoid silent temporal/spatial aggregation,
- state how aggregation affects totals, rates, peaks, uncertainty, and downstream constraints.

Resampling from one time resolution to another requires a semantic rule. For example, power, energy, counts, prices, states, and event flags generally require different aggregation/interpolation logic.

If no defensible mapping exists, leave the mismatch unresolved and return it upstream rather than manufacturing a smooth series.

## Step 7 — Separate model-independent preparation from model-dependent preprocessing

This skill may perform reproducible transformations whose correctness does not depend on the training/test split or selected predictive model, such as:

- parsing formats,
- correcting confirmed units,
- deterministic coordinate transforms,
- removing confirmed exact duplicates,
- applying documented validity rules,
- creating documented deterministic derived fields,
- conservative time alignment when the problem semantics justify it.

Normally defer the following to `model-building` or a training pipeline:

- scaling/normalization fitted from data,
- one-hot/target encoding whose categories/statistics depend on training data,
- learned or distribution-based imputation,
- target-aware feature selection,
- dimensionality reduction fitted from data,
- feature transformations tuned by validation performance.

These later operations must be fit using only legally available training information.

## Step 8 — Human Gate for high-impact preparation

Human approval is required before applying a transformation that materially changes the evidence available to later modeling, including when it:

- imputes a critical variable with a non-deterministic estimate,
- deletes a nontrivial portion of records,
- removes or clips extreme events,
- changes time/spatial resolution in a way that may alter the target or constraints,
- merges records or entities using an ambiguous key,
- redefines a variable's semantics,
- resolves an ambiguity not settled by the problem statement or audit,
- uses a strong domain assumption to reconstruct unavailable data.

Present a compact card:

```text
ISSUE
...

WHY IT MATTERS
...

EVIDENCE ABOUT ITS MEANING
...

PROPOSED ACTION
...

ALTERNATIVES
...

WHAT INFORMATION MAY BE LOST / ADDED
...

DOWNSTREAM CONSEQUENCE
...

HUMAN APPROVAL
PENDING
```

Routine deterministic corrections already established by the audit do not require repeated approval.

## Step 9 — Implement reproducibly

After decisions are approved:

- implement transformations in code or a reproducible script,
- never edit source files manually without recording the change,
- write prepared data to `data/processed/` or an equivalent live-project path,
- preserve stable identifiers/keys,
- preserve provenance back to source rows when practical,
- use explicit random seeds if any stochastic procedure is justified,
- save logs/diagnostics needed to reproduce the result.

## Step 10 — Validate the prepared data

Compare before vs after and verify:

- row/entity/time coverage,
- missingness by variable and by relevant group/time regime,
- units and ranges,
- duplicate counts,
- totals or conservation identities when applicable,
- temporal ordering and gaps,
- spatial coverage when applicable,
- distributions and important extremes,
- target prevalence / class balance when applicable,
- information-timing legality,
- critical variables required by `problem_brief.md` remain usable.

A dataset is not "better" merely because it contains fewer missing values.

## Step 11 — Record the handoff

Create or update:

- `project/data_preparation.json`
- prepared datasets under `data/processed/` or equivalent
- optional reproducible preparation code under `src/data/` or `scripts/`

The report must identify:

- exact raw inputs,
- exact prepared outputs,
- transformations applied,
- transformations rejected/deferred,
- missingness interpretation and action by important variable,
- row/value impact,
- unresolved issues,
- human approvals for high-impact changes,
- downstream usage restrictions.

`model-selection` and all later stages should prefer the prepared data when the report marks it ready, while continuing to respect unresolved restrictions. `feasibility-test` reads the prepared data only when model-selection has actually triggered a high-risk feasibility gate.

## Status

Return one of:

- `READY`: prepared data are suitable for downstream model selection under the stated restrictions.
- `READY_WITH_WARNINGS`: usable, but meaningful unresolved issues remain and must be carried downstream.
- `HOLD`: a blocking semantic/data issue requires upstream clarification or human decision.

## What this skill must not do

- do not maximize completeness at the expense of truth,
- do not treat all missing values as errors,
- do not apply one imputation method to all numeric variables,
- do not delete all outliers automatically,
- do not overwrite raw data,
- do not silently resample or smooth,
- do not use future/unavailable data to reconstruct decision-time inputs,
- do not hide uncertainty by replacing unknown values with precise-looking numbers,
- do not perform train/test-dependent preprocessing globally,
- do not change the problem definition to make the dataset easier to model,
- do not continue past a high-impact ambiguity without recording or escalating it.
