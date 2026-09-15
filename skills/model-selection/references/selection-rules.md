# Model Selection Rules

Use this evidence order:

1. task and executable data regime;
2. reusable repository knowledge;
3. current-project literature evidence;
4. benchmark landscape;
5. baseline and candidate pool;
6. feasibility risk triage;
7. optional feasibility test for high-risk mechanisms;
8. lightweight comparable screening;
9. shortlist and full-build experiment contract;
10. validation plan.

## Benchmark landscape vs model roles

The **benchmark landscape** is the evidence-backed map of relevant method families and established comparison practices. It is not itself a model and is broader than the baseline.

From the landscape, assign candidate roles:

- **Baseline** — simplest meaningful reference needed to quantify improvement.
- **Standard benchmark** — established method commonly used for a comparable task/data regime.
- **Strong candidate** — serious contender with good theoretical/data fit.
- **Improved/problem-specific candidate** — justified adaptation addressing a concrete limitation of existing methods or current data.
- **Rejected model** — considered but excluded with evidence/reason.

One model may occupy more than one descriptive category in a particular field, but its role in the current experiment must be explicit.

## Literature evidence extraction

For core papers record when available:

- problem/task;
- dataset and sample size;
- observation/group unit;
- features and target;
- preprocessing;
- split/CV and leakage controls;
- model;
- baseline/comparison models;
- metrics;
- result;
- limitations;
- relevance/transferability to the current problem.

Do not rank models by reported headline score across papers unless datasets, splits, metrics and protocols are genuinely comparable.

Project-specific literature goes under `paper/literature/`, never into reusable repository knowledge.

## Comparison dimensions

- theoretical fit;
- current-data fit;
- literature support;
- performance/objective potential;
- stability;
- interpretability;
- robustness;
- implementation difficulty;
- numerical stability;
- computational cost;
- information-timing legality;
- temporal/spatial/group compatibility;
- upstream/downstream interface risk;
- ease of validation;
- competition-paper explainability.

## Risk triage

Use `LOW_RISK`, `HIGH_RISK`, or `BLOCKED`.

A candidate is not `HIGH_RISK` merely because it is sophisticated. Trigger feasibility testing when a failure-prone mechanism/interface could invalidate the route and is not cheaply resolved by ordinary screening.

Typical high-risk triggers:

- uncertain model-to-model interface;
- hard constraints that may make an optimization infeasible;
- unstable PDE/simulation/numerical mechanism;
- uncertain geometry/search/localization mechanism;
- unusual or untested implementation;
- severe information-timing restriction;
- critical dependence on a questionable preparation choice;
- computational burden that may make full execution impractical.

Mature LR/SVM/RF/XGBoost-style candidates, for example, should normally go directly to screening when their data/interface requirements are already satisfied; do not run an MVM just to prove the library can execute.

## Screening rule

Screening is comparative evidence, not final validation.

All viable candidates should use a common protocol appropriate to the task: same legal data, split/CV logic, metrics, comparable preprocessing boundaries and modest compute budget. Avoid exhaustive tuning.

Screening should normally reduce the pool to:

- baseline(s),
- one primary candidate,
- one or more serious alternatives when useful.

Keep multiple models when needed for full comparison. Do not force a single model into `model-building` prematurely.

## Candidate contract

For every shortlisted model preserve:

- role;
- evidence/rationale;
- inputs/outputs;
- data and preprocessing requirements;
- forbidden information/predictors;
- hard constraints;
- upstream/downstream interfaces;
- split/CV strategy;
- metrics;
- tuning budget/plan;
- validation/robustness/interpretability requirements;
- required result artifacts.

## Evidence rule

Literature justifies what is worth trying. Feasibility evidence determines whether a risky route is executable. Screening determines what deserves full investment. Full model building plus validation determines what can be defended as the final model.
