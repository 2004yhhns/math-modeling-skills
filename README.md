# Math Modeling Skills for Codex

A reusable skills repository for mathematical modeling competitions.

This repository is **not** a place to store real competition projects. It stores reusable capabilities, knowledge, templates, and algorithm guidance that can be called from a separate competition workspace.

## Design goals

1. Keep the human modeler in charge of final modeling decisions.
2. Separate workflow rules from mathematical knowledge.
3. Require baselines before complex models.
4. Record assumptions, data-preparation decisions, model-selection rationale, feasibility evidence, and experiments.
5. Make validation, leakage checks, robustness, and interpretability first-class steps.
6. Keep claims traceable to evidence.
7. Preserve raw evidence and make data cleaning problem-driven rather than recipe-driven.

## Repository structure

```text
math-modeling-skills/
├── AGENTS.md
├── skills/
│   ├── problem-analysis/
│   ├── data-audit/
│   ├── data-preparation/
│   ├── model-selection/
│   ├── feasibility-test/
│   ├── model-building/
│   └── model-validation/
├── knowledge/
├── templates/
├── algorithms/
├── docs/
└── examples/
```

## How the layers differ

- `skills/`: reusable task workflows — what job Codex should perform.
- `skills/*/references/`: local SOPs for one skill — how that job should be performed.
- `knowledge/`: shared mathematical-modeling knowledge — what methods mean, when they fit, and how they fail.
- `templates/`: clean reusable output templates. Do not store live competition state here.
- `algorithms/`: reusable implementation guidance or code.
- `docs/`: cross-stage contracts and architecture documentation.
- `examples/`: optional demonstrations of how the skills behave on example problems.

## Invocation contract

The user should invoke a stage by naming its `SKILL.md` and the current target. The skill is responsible for orchestrating its own dependencies.

Do **not** require the user to manually enumerate every file under `references/`, `knowledge/`, or `algorithms/` on each invocation. If the selected `SKILL.md` instructs Codex to read those resources conditionally, Codex should load only what is relevant to the current problem and model family.

`AGENTS.md` supplies global governance and decision boundaries. A `SKILL.md` supplies the stage-specific procedure. The user prompt only needs to specify the current task, scope, and any desired stopping point.

Canonical feasibility invocation:

```text
使用 `math-modeling-skills/skills/feasibility-test/SKILL.md`
对 Q1 做 feasibility test，到 Human Gate 暂停并给我 Feasibility Card。
```

Canonical data-preparation invocation:

```text
使用 `math-modeling-skills/skills/data-preparation/SKILL.md`
根据已有 data audit 对 Q1 所需数据进行准备和清洗。
严格按题意解释缺失、异常、重复、时间/空间对齐问题；
高影响处理先到 Human Gate，不要自动填补或删除。
```

If a short invocation fails because Codex cannot locate the repository, skill file, or upstream project artifacts, fix workspace/path visibility rather than copying all skill instructions into the prompt.

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
    │   ├── raw/
    │   └── processed/
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
data-preparation
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

## Core workflow

### 1. Problem analysis

Convert the statement into subproblems, inputs/outputs, constraints, dependencies, evaluation targets, information timing, assumptions, and ambiguities.

Expected outputs:

- `project/problem_brief.md`
- `project/assumption_ledger.md`

### 2. Data audit

Diagnose what the raw data can and cannot support. Check schema, semantics, units, missingness, outliers, grouping, time/spatial structure, leakage, information timing, and problem-to-data mapping.

Expected output:

- `project/data_audit.json`

### 3. Data preparation

Execute only defensible model-independent preparation after the audit.

Core rules:

- raw data are immutable,
- missingness must be interpreted before action,
- missing is not automatically zero or error,
- no blanket mean/median/mode filling,
- no automatic outlier deletion,
- no silent smoothing/resampling,
- no future-information reconstruction,
- high-impact transformations require explicit justification and human review when available,
- train/test-dependent preprocessing is deferred to the later modeling pipeline,
- every transformation is logged and checked before/after.

Expected outputs:

- prepared data under `data/processed/` or equivalent,
- `project/data_preparation.json`.

### 4. Model selection

Use the audited/prepared data regime to create a model ladder with meaningful baselines, 2–4 candidates, rejected alternatives, interface/data requirements, feasibility risks, experiment order, and validation plan.

Expected outputs:

- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

### 5. Feasibility test

Before full implementation, derive the dependency graph, final target, critical failure point, CFQ, required data, MVM readiness/interface check, and proposed minimum viable model. Stop at the Human Gate before substantial implementation. After approval, run universal and family-specific checks and return `GO`, `GO_WITH_RISKS`, `HOLD`, or `NO_GO`.

Expected output:

- `project/feasibility_test.json`

### 6. Model building

Implement the approved experiment ladder after feasibility is established. Keep model-dependent preprocessing inside the correct training/evaluation pipeline so it cannot leak held-out or future information.

Expected outputs:

- code under `src/`,
- artifacts under `results/`,
- experiment entries in `project/experiment_log.json`.

### 7. Model validation

Test whether apparent improvement is real. At minimum check baseline improvement, split/leakage correctness, residual/error structure, generalization, robustness, and sensitivity to important data-preparation assumptions when material.

## Workflow contract

See `docs/workflow-handoff-contract.md` for the producer/consumer contract between stages and the return path when a downstream stage discovers an upstream defect.

## Design references

This repository is an original synthesis inspired by public mathematical-modeling workflow projects. It adapts useful design ideas into a smaller Codex-oriented competition workflow and does not copy those repositories verbatim.

## Version 0.3 scope

The workflow now contains 7 core skills:

```text
problem-analysis
→ data-audit
→ data-preparation
→ model-selection
→ feasibility-test
→ model-building
→ model-validation
```

Useful future additions include sensitivity-analysis, robustness-analysis, interpretability, judge-review, paper-handoff, experiment-ranking scripts, JSON schemas, and automated validation.
