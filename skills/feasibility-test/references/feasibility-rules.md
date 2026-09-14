# Feasibility Test Rules

This file separates universal feasibility checks from model-family-specific checks. Run the universal layer for every candidate; load only the special section relevant to the current problem.

## A. Universal layer — every modeling problem

### A1. End-to-end executability

The prototype must accept its intended input and produce the central output without hidden manual intervention.

### A2. Sanity and units

Check ranges, signs, dimensions, units, monotonicity or conservation relationships when applicable. Flag outputs that are mathematically possible but physically or operationally implausible.

### A3. Constraint integrity

List the core hard constraints from the problem statement and verify them numerically. A solution that violates a defining constraint is not feasible merely because code runs.

### A4. Runtime and scaling

Record prototype runtime and identify the main scaling dimension: samples, time steps, grid cells, nodes, scenarios, targets, or integer variables. Estimate whether the full instance is plausibly solvable during the competition.

### A5. Minimal perturbation test

Change one important input or parameter by a small, reasonable amount. The response should be explainable. Large discontinuities or nonsensical invariance are warning signs unless theoretically expected.

### A6. Baseline comparison

Whenever meaningful, compare against the simplest valid benchmark. The feasibility test asks whether the method adds useful capability, not whether it is already final-best.

### A7. Information discipline

Check that every decision uses only information available at that decision time. This is mandatory for forecasting, scheduling, control, sequential decision, and time-series problems.

### A8. Explainability checkpoint

The team should be able to explain the prototype using: inputs → mechanism → outputs → validation signal. If the result cannot be explained, mark at least a moderate risk.

---

## B. Optimization / scheduling

Minimum viable instance: a reduced horizon or representative scenario that retains the main objective and hard constraints.

Check:

- solver returns a valid status,
- objective is computed from the intended quantities,
- equality and inequality residuals are within tolerance,
- state transitions are correct,
- bounds are never violated,
- no forbidden simultaneous actions occur unless justified,
- removing a useful resource should not mysteriously improve the objective,
- full-scale variable/constraint counts are plausible.

Typical blockers: infeasibility from contradictory constraints, wrong sign conventions, unbounded objective, integer-variable explosion, look-ahead leakage.

---

## C. Prediction / classification

Minimum viable instance: leakage-safe baseline on a representative subset or simple split.

Check:

- target and features are correctly separated,
- split respects time/group/spatial structure,
- preprocessing is fitted only on training data,
- naive baseline is recorded,
- chosen metric matches the task,
- predictions have valid range/class structure,
- errors are not concentrated in an obvious subgroup/regime,
- training/inference time is plausible at full scale.

Typical blockers: leakage, target proxy features, severe class imbalance without an evaluation plan, too few independent samples, invalid temporal split.

---

## D. Geometry / localization / search

Minimum viable instance: one or a few synthetic targets with known ground truth, using the real observation/error model.

Check:

- coordinate system and angle convention are explicit,
- feasible regions/intersections are computed correctly,
- known ground truth lies in the inferred region when assumptions hold,
- localization uncertainty shrinks when informative observations are added,
- degenerate geometries are identified,
- boundary cases are tested,
- geometric output can be visualized and inspected.

For search strategies additionally check:

- coverage gaps,
- termination condition,
- transition from search to localization/action,
- path length/time accounting,
- sensitivity to target placement.

Typical blockers: angle/sign convention errors, unstable intersection geometry, uncovered regions, strategy depending on unknown ground truth.

---

## E. Simulation / mechanism / PDE

Minimum viable instance: coarse grid / short horizon / simplified geometry while retaining the governing mechanism and boundary conditions.

Check:

- initial and boundary conditions are implemented correctly,
- conserved quantities or balances behave as expected when applicable,
- state variables remain in physically meaningful ranges,
- coarse time-step/grid refinement does not completely change the qualitative result,
- numerical method satisfies known stability requirements or uses an appropriate stable solver,
- singular points and moving boundaries are handled explicitly when present,
- full-resolution runtime/memory appears feasible.

Typical blockers: blow-up, negative concentrations/probabilities, boundary-condition errors, severe grid dependence, stiffness, moving-boundary complexity beyond available time.

---

## F. Graph / routing / path planning

Minimum viable instance: small graph or reduced set of locations with known/inspectable solution structure.

Check:

- graph construction matches physical reachability,
- edge weights have correct units and meaning,
- path obeys all constraints,
- route cost recomputes correctly from edges,
- disconnected cases are handled,
- baseline such as nearest-neighbor or shortest path is available,
- complexity at full node count is plausible.

Typical blockers: incorrect graph abstraction, hidden infeasible edges, combinatorial explosion, objective not matching actual travel cost.

---

## G. Stochastic / Monte Carlo

Minimum viable instance: small but repeatable scenario batch with fixed seed.

Check:

- randomness represents an explicit uncertainty source,
- seed is fixed and recorded,
- output distribution rather than one lucky run is inspected,
- mean/quantiles or task-relevant risk measures are reported,
- results are not dominated by a tiny number of pathological samples,
- increasing scenario count gives reasonably stable conclusions,
- simulation cost scales acceptably.

Typical blockers: uncalibrated uncertainty model, conclusions from one realization, excessive variance, scenario count too expensive for the competition window.

---

## H. Rapid topic-choice comparison

When comparing A/B/C-style contest problems, evaluate each surviving candidate using the same dimensions:

- core prototype success,
- time to first credible result,
- data readiness,
- mathematical clarity,
- validation availability,
- downstream dependency risk,
- computational risk,
- implementation/debug risk,
- room for extension after baseline,
- team familiarity.

Scores may summarize evidence but must not replace written blocker analysis. A candidate with a fatal blocker cannot win merely through a high weighted score.
