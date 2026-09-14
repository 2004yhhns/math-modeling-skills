# Math Modeling Skills for Codex

A reusable skills repository for mathematical modeling competitions.

This repository is **not** a place to store real competition projects. It stores reusable capabilities, knowledge, templates, and algorithm guidance that can be called from a separate competition workspace.

## Design goals

1. Keep the human modeler in charge of final modeling decisions.
2. Separate workflow rules from mathematical knowledge.
3. Require baselines before complex models.
4. Record assumptions, model-selection rationale, feasibility evidence, and experiments.
5. Make validation, leakage checks, robustness, and interpretability first-class steps.
6. Keep claims traceable to evidence.

## Repository structure

```text
math-modeling-skills/
├── AGENTS.md
├── skills/
│   ├── problem-analysis/
│   ├── data-audit/
│   ├── model-selection/
│   ├── feasibility-test/
│   ├── model-building/
│   └── model-validation/
├── knowledge/
│   ├── prediction/
│   ├── classification/
│   ├── optimization/
│   └── evaluation/
├── templates/
├── algorithms/
└── examples/
```

## How the layers differ

- `skills/`: reusable task workflows — what job Codex should perform.
- `skills/*/references/`: local SOPs for one skill — how that job should be performed.
- `knowledge/`: shared mathematical-modeling knowledge — what methods mean, when they fit, and how they fail.
- `templates/`: clean reusable output templates. Do not store live competition state here.
- `algorithms/`: reusable implementation guidance or code.
- `examples/`: optional demonstrations of how the skills behave on example problems.

## Recommended usage

Keep a real competition in a separate directory, for example:

```text
workspace/
├── math-modeling-skills/
└── GMCM2026-C/
    ├── AGENTS.md
    ├── problem/
    ├── project/
    ├── data/
    ├── src/
    ├── experiments/
    ├── results/
    ├── figures/
    └── paper/
```

Typical interaction:

```text
real problem
  ↓
problem-analysis
  ↓
data-audit
  ↓
model-selection
  ↓
feasibility-test
  ↓
model-building
  ↓
model-validation
```

The skills repository provides the reusable method; the live project stores the actual state and outputs.

## First-version workflow

### 1. Problem analysis

Use `problem-analysis` to convert the statement into:

- subproblems,
- inputs and outputs,
- constraints,
- mathematical task types,
- dependencies,
- assumptions and ambiguities.

Expected project outputs:

- `project/problem_brief.md`
- `project/assumption_ledger.md`

### 2. Data audit

Use `data-audit` before model selection.

Check:

- schema,
- missing data,
- outliers,
- units,
- group/time/spatial structure,
- possible leakage,
- train/test split constraints.

Expected project output:

- `project/data_audit.json`

### 3. Model selection

Use `model-selection` only after the current subproblem and data regime are clear.

It must produce:

- a simple baseline,
- a stronger standard baseline where useful,
- 2–4 candidate models,
- rejected models with reasons,
- a recommended experiment ladder,
- a validation plan.

Expected project outputs:

- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

### 4. Feasibility test

Use `feasibility-test` before committing to full model building, especially when choosing among contest problems or when the selected model has substantial implementation/numerical risk.

It should:

- define the smallest decisive prototype,
- run the central mechanism end to end,
- check constraints, units, runtime, stability, leakage/information timing, and basic sensitivity,
- load only the relevant family-specific checks for optimization, prediction, geometry/search, PDE/mechanism, routing, or stochastic simulation,
- classify blockers and return `GO`, `GO_WITH_RISKS`, `HOLD`, or `NO_GO`.

Expected project output:

- `project/feasibility_test.json`

### 5. Model building

Use `model-building` to implement only the approved experiment ladder after feasibility is established.

Expected project outputs:

- code under `src/`,
- artifacts under `results/`,
- experiment entries in `project/experiment_log.json`.

### 6. Model validation

Use `model-validation` to test whether the apparent improvement is real.

At minimum check:

- baseline improvement,
- correct split strategy,
- leakage,
- residual structure,
- generalization,
- robustness of the main conclusion.

## Design references

This repository is an original synthesis inspired by three public projects:

1. **Hjdd14/math-modeling**  
   Main lessons used here: workflow governance, problem brief, assumptions, baselines, model-selection audit, model specification, validation artifacts, and phase-gate thinking.

2. **dreamnight16 / sixtdreanight MCM-Resource**  
   Main lesson used here: organize shared modeling knowledge by problem family (prediction, classification, optimization, evaluation, statistics, simulation/mechanism) rather than by a flat list of algorithms.

3. **chengziyue1222/math-model-agent**  
   Main lessons used here: split a large modeling agent into practical skills such as model selection and solving; distinguish primary model, baseline, validation model, and rejected models; keep algorithm implementations separate from skill instructions.

This repository does **not** copy those repositories verbatim. It adapts the useful design ideas into a smaller Codex-oriented competition workflow.

## Version 0.2 scope

The workflow now contains 6 core skills, including a rapid feasibility gate between model selection and full implementation. Useful future additions include:

- sensitivity-analysis,
- robustness-analysis,
- interpretability,
- judge-review,
- paper-handoff,
- experiment-ranking scripts,
- JSON schemas and automated validation.
