# Using This Repository With a Real Competition Project

## Principle

Keep reusable skills and live competition state separate.

```text
workspace/
├── math-modeling-skills/      # reusable repository
└── COMPETITION-PROJECT/       # one real project
    ├── problem/
    ├── project/
    ├── data/
    │   ├── raw/
    │   └── processed/
    ├── paper/
    │   └── literature/
    │       ├── downloaded/
    │       ├── literature_matrix.md
    │       ├── benchmark_landscape.md
    │       └── literature_notes.md
    ├── src/
    ├── experiments/
    ├── results/
    └── figures/
```

Project-specific papers, notes, code, data, and results belong to the live project, not the reusable skills repository.

## Human-readable workflow

```text
problem-analysis
      ↓
data-audit
      ↓
data-preparation
      ↓
model-selection
  ├─ literature research
  ├─ benchmark landscape
  ├─ candidate pool
  ├─ risk triage
  │    ├─ LOW_RISK  ──────────────┐
  │    └─ HIGH_RISK → feasibility-test
  │                         ↓
  │                   GO / HOLD / NO_GO
  └─────────────────────────┬──────┘
                            ↓
                     screening experiment
                            ↓
                         SHORTLIST
              baseline + primary + alternatives
                            ↓
                     model-building
                            ↓
                    model-validation
                            ↓
                  final recommendation
                            ↓
                    improvement loop ↺
```

Feasibility testing is conditional, not mandatory for every model.

## Step 1 — Initialize the live project

Put the original statement under `problem/`, raw data under `data/raw/`, and current-project literature under `paper/literature/`.

Do not copy the real competition project into this skills repository.

## Step 2 — Initialize project artifacts from templates

When a Skill needs an artifact that does not exist, instantiate the corresponding clean template into the live project.

Examples:

```text
math-modeling-skills/templates/problem_brief.md
        ↓
COMPETITION-PROJECT/project/problem_brief.md
```

```text
math-modeling-skills/templates/experiment_log.json
        ↓
COMPETITION-PROJECT/project/experiment_log.json
```

The reusable template remains clean.

## Step 3 — Recommended Skill usage

### A. problem-analysis

> Use `skills/problem-analysis/SKILL.md` on the current problem. Build the conceptual subproblem specification and task dependency graph. Do not select final models.

Expected:
- `project/problem_brief.md`
- `project/assumption_ledger.md`

### B. data-audit

> Use `skills/data-audit/SKILL.md`. Verify observation unit, grouping, leakage, dataset compatibility, question-data mapping, and executable dependencies.

Expected:
- `project/data_audit.json`

### C. data-preparation

> Use `skills/data-preparation/SKILL.md`. Apply only semantically defensible model-independent preparation. High-impact changes require explicit review.

Expected:
- prepared data under `data/processed/`
- `project/data_preparation.json`

### D. model-selection

> Use `skills/model-selection/SKILL.md` for the target question. Research relevant external literature when useful; save project-specific papers/notes only under `paper/literature/`. Build the benchmark landscape and candidate pool, perform risk triage, run required high-risk feasibility gates, then run lightweight screening and stop at the shortlist Human Gate.

Expected:
- `paper/literature/literature_matrix.md`
- `paper/literature/benchmark_landscape.md`
- `paper/literature/literature_notes.md`
- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

### E. feasibility-test — only when triggered

> Use `skills/feasibility-test/SKILL.md` only for a candidate/mechanism marked HIGH_RISK. Stop at the Feasibility Human Gate before substantial prototype implementation.

Expected when triggered:
- `project/feasibility_test.json`

If no genuine high-risk trigger exists, feasibility may return `NOT_REQUIRED`.

### F. model-building

> Use `skills/model-building/SKILL.md`. Implement the approved shortlist exactly as specified by `project/model_spec.json`. Do not silently change split, metrics, feature legality, or high-impact preparation rules.

Expected:
- code under `src/` and/or `experiments/`
- outputs under `results/`
- `project/experiment_log.json`

### G. model-validation

> Use `skills/model-validation/SKILL.md`. Compare the full-built shortlist against baselines/alternatives, verify generalization/leakage/robustness/sensitivity/interpretability, recommend the defensible final model, and route weaknesses into the improvement loop.

Expected:
- `project/validation_summary.json`

## Human decision gates

Human review is especially important when changing:

- modeling objective;
- core mathematical assumptions;
- high-impact data cleaning/preparation;
- evaluation metric;
- train/test/CV strategy;
- candidate shortlist for expensive full building;
- final model recommendation;
- causal interpretation.

Routine coding, ordinary debugging, plots, and diagnostic generation may proceed autonomously when they do not alter those decisions.

## Modeling hand → programming hand

The key interface is `project/model_spec.json`.

The modeling hand defines:
- approved shortlist;
- data/features and forbidden information;
- split/CV;
- metrics;
- preprocessing boundary;
- tuning budget;
- validation/robustness/interpretability plan;
- required outputs.

The programming hand implements and logs the experiment contract.

## Improvement loop

If full validation exposes a weakness:

```text
raw-data diagnosis        → data-audit
preparation choice        → data-preparation
wrong model family        → model-selection
high-risk mechanism       → feasibility-test when genuinely needed
implementation/tuning     → model-building
validation design         → model-validation with recorded revision
interpretation only       → limit/rewrite the claim
```

Do not patch a weak model only in the paper.

## Codex note

Each Skill is self-contained under `skills/<name>/SKILL.md`. The user should normally invoke the Skill and target; the Skill is responsible for loading its own relevant references, reusable knowledge, templates, and live-project artifacts.
