# Data Preparation Rules

These rules operationalize the principle: **prepare data according to the specific problem, not according to a generic cleaning recipe.**

## 1. Decision order

For every suspicious value or record, use this order:

1. establish semantic meaning,
2. establish whether the value should exist,
3. establish when it would be available,
4. determine whether the problem treats it as signal, noise, state, constraint, or target,
5. assess whether changing it would alter the mathematical problem,
6. choose the least destructive defensible action,
7. document alternatives and remaining uncertainty.

Never start with the method (mean fill, interpolation, deletion, smoothing) and then search for a justification.

---

## 2. Missingness taxonomy

Classify missingness before action.

### A. Structural missingness

The value does not conceptually apply.

Examples:

- a variable defined only for a subset of entities,
- a measurement that is impossible outside a certain regime,
- a downstream state that does not exist before activation.

Default treatment: preserve the structural state; do not replace it with an ordinary numeric estimate unless the mathematical model explicitly requires a coded representation.

### B. Expected absence

The absence itself may be meaningful.

Examples:

- no event occurred,
- no transaction exists,
- no observation was scheduled,
- no measurement is expected in that period.

Default treatment: distinguish absence from zero unless the domain definition proves equivalence.

### C. Not observed / measurement failure

A value should exist but was not recorded correctly.

Possible actions depend on gap length, neighboring structure, measurement process, downstream use, and sensitivity. Imputation may be defensible, but is not automatic.

### D. Censored / truncated / below detection

The missing or special value encodes partial information.

Default treatment: preserve censoring information. Do not convert it to a generic mean/median without justification.

### E. Future unavailable

The value exists in the full historical dataset but would not have been known at the relevant decision time.

Default treatment: it is **not a legal input** for that decision. Never fill earlier inputs using later realized values merely because the complete dataset contains them.

### F. Unknown semantic status

The meaning cannot be established from the statement, metadata, or defensible inference.

Default treatment: preserve + flag; escalate if material.

---

## 3. Imputation gate

Before any imputation, require positive answers to all relevant questions:

- Is the value conceptually supposed to exist?
- Is the missingness mechanism sufficiently understood?
- Does the proposed method respect time/group/entity boundaries?
- Does it preserve units and physical constraints?
- Does it avoid using future or held-out information?
- Does it avoid shrinking real variability/extremes in a way that matters?
- Is the gap small/structured enough for the assumed interpolation or estimator?
- Is there a plan to assess sensitivity to the imputation choice?

If not, preserve/defer rather than fabricate precision.

### Mean / median / mode

Use only when the variable semantics and downstream task make a constant replacement defensible. These methods can distort variance, correlations, extremes, distributions, temporal structure, and group differences.

### Forward fill / backward fill

Use only when the quantity is legitimately persistent between observations and entity/regime boundaries are respected. Never carry values across resets, ownership/entity changes, regime switches, long unknown intervals, or into times where they would not yet be known.

### Interpolation

Requires a defensible continuity/smoothness assumption and a gap small enough relative to the process timescale. Check whether peaks, discontinuities, events, or physical transitions may occur inside the gap.

### Model-based imputation

Treat as model-dependent unless it is clearly part of the problem's measurement model. Fit only on legally available data, preserve uncertainty when relevant, and validate sensitivity.

### Target/label imputation

Default: do not do it. A reconstructed target can turn assumptions into fake ground truth. Only proceed when a domain-defined deterministic reconstruction is genuinely available and documented.

---

## 4. Zero versus missing

Before replacing `NA` with `0`, explicitly verify:

```text
Does zero literally mean the same real-world state as missing?
```

Examples where they may differ:

- zero production vs production not measured,
- zero demand vs demand record unavailable,
- no event vs event status unknown,
- state value zero vs state not applicable.

If they differ, preserve the distinction.

---

## 5. Outlier decision rules

Do not use z-score, IQR, winsorization, or clipping as automatic deletion rules.

For each extreme value ask:

- Is it physically or logically impossible?
- Is there evidence of entry/sensor corruption?
- Is it a real rare event?
- Is the task specifically about extremes, failures, anomalies, peaks, risk, or robustness?
- Would removing it make the model look artificially stable?

Actions may include preserve, preserve + flag, correct confirmed error, exclude confirmed invalid record, or perform a sensitivity analysis with/without the point.

---

## 6. Duplicate decision rules

Define the correct record key before deleting duplicates.

Two rows with equal values are not necessarily duplicate observations. Check entity, time, experiment, event ID, source, and repeated-measure design.

Delete only when duplication is confirmed under the correct semantic key or source metadata.

---

## 7. Resampling and aggregation

Never apply a generic `.resample(...).mean()` rule without checking variable semantics.

Typical distinctions:

- power / rate: often average over interval may be meaningful,
- energy / counts / volume: often sum may be meaningful,
- price: may require time-weighted or settlement-specific treatment,
- state variables: often endpoint/last observation has different meaning from mean,
- maxima/minima: aggregation may destroy the quantity of interest,
- event flags: `any`, count, first occurrence, or duration may matter,
- coordinates/paths: averaging may create impossible locations.

The problem statement or physical/business semantics decide the rule.

Record source resolution, target resolution, formula, and information timing.

---

## 8. Smoothing and filtering

Smoothing is not ordinary cleaning when it changes signal structure.

Before smoothing ask:

- Is high-frequency variation noise or signal?
- Are peaks or sharp transitions part of the target?
- Does the governing mechanism permit smoothness?
- Does filtering introduce phase shift or use future observations?
- Will later validation be performed on equally transformed data?

If uncertain, preserve raw and produce the smoothed representation as an additional derived field rather than replacing the original.

---

## 9. Deterministic corrections versus model-dependent preprocessing

### Usually appropriate in data-preparation

- parse known date/number formats,
- standardize confirmed units,
- correct a known timezone convention,
- deterministic coordinate conversion,
- remove confirmed exact duplicate records,
- derive documented arithmetic quantities,
- enforce documented validity constraints,
- attach quality/missingness flags,
- apply semantically justified fixed mappings.

### Usually defer to model-building pipeline

- scaling using sample mean/std,
- PCA or learned dimensionality reduction,
- learned imputation,
- target encoding,
- feature selection informed by target performance,
- transformations chosen by cross-validation,
- class rebalancing / resampling,
- learned denoising.

Reason: these operations depend on the training/evaluation protocol and can leak information if performed globally.

---

## 10. Human-review triggers

Require human review when an action can materially change conclusions, especially:

- non-deterministic imputation of a critical variable,
- deletion of a meaningful number of rows/entities/time periods,
- removal/clipping of extremes,
- ambiguous merge/join,
- changing temporal/spatial resolution,
- reconstructing unavailable measurements,
- changing target/label values,
- redefining categories or semantic states,
- assumptions about missingness mechanism that cannot be verified,
- replacing uncertainty with a single precise value.

---

## 11. Before/after validation

At minimum compare:

- row count,
- entity count,
- time coverage,
- spatial coverage,
- missingness counts and patterns,
- duplicates,
- ranges and units,
- distribution summaries,
- critical quantiles/extremes,
- class/target prevalence where relevant,
- important totals/conservation identities,
- time gaps and ordering,
- problem-required fields,
- legal information set at each decision time.

A preparation is unacceptable if it produces a technically convenient dataset while changing the substantive problem without explicit approval.

---

## 12. Required audit trail for each transformation

Record:

```text
transformation_id
source_field_or_records
issue_type
semantic_interpretation
evidence
operation
parameters
reason
rows_or_values_affected
before_summary
after_summary
reversible
information_timing_check
human_approval
remaining_risk
```

If the reason cannot be written clearly, the transformation is probably not ready to apply.
