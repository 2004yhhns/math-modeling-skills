# Using This Repository With a Real Competition Project

## Principle

Keep reusable skills and live competition state separate.

```text
workspace/
├── math-modeling-skills/      # reusable repository
└── GMCM2026-C/                # one real project
    ├── problem/
    ├── project/
    ├── data/
    ├── src/
    ├── experiments/
    ├── results/
    ├── figures/
    └── paper/
```

## Step 1 — Create a live project

Create a new directory for the problem. Put the statement under `problem/` and raw attachments under `data/`.

Do not copy the real competition project into this skills repository.

## Step 2 — Initialize project artifacts

When a skill needs an artifact that does not yet exist, copy the corresponding clean template from this repository into the live project's `project/` directory.

Examples:

```text
math-modeling-skills/templates/problem_brief.md
        ↓ instantiate
GMCM2026-C/project/problem_brief.md
```

```text
math-modeling-skills/templates/experiment_log.json
        ↓ instantiate
GMCM2026-C/project/experiment_log.json
```

The template remains clean. Only the live-project copy is edited.

## Step 3 — Call skills in context

Recommended first-version sequence:

### A. problem-analysis

Instruction example:

> Use the problem-analysis skill. Read the current problem and attachments. Do not recommend final models. Create/update project/problem_brief.md and project/assumption_ledger.md.

### B. data-audit

> Use the data-audit skill. Audit all current data before model selection. Pay special attention to group/time leakage. Create/update project/data_audit.json.

### C. model-selection

> Use the model-selection skill for Q4 only. Read problem_brief, assumption_ledger, and data_audit. Compare meaningful baselines and 2–4 candidate models. Do not write final model code yet.

Expected artifacts:

- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

### D. model-building

After the human approves the experiment ladder:

> Use model-building. Implement only the approved models in model_spec.json. Run baselines first and append every run to project/experiment_log.json.

### E. model-validation

> Use model-validation. Verify baseline improvement, split correctness, leakage, residual/error structure, subgroup generalization, and conclusion robustness. Write project/validation_summary.json.

## Step 4 — Human decision gates

The human modeler should explicitly approve changes that alter:

- modeling objective,
- core mathematical assumptions,
- evaluation metric,
- train/test strategy,
- final model choice,
- causal interpretation.

Routine coding, experiment execution, plots, and bug fixes can usually proceed without changing these decisions.

## Step 5 — Iterate by evidence

If validation fails:

```text
validation failure
   ↓
diagnose cause
   ↓
return to model-selection or model-building
   ↓
record new experiment
   ↓
validate again
```

Do not patch a weak model only in the paper.

## Codex note

Skill discovery/installation can differ across Codex surfaces and account configurations. This repository is designed so each skill is self-contained under `skills/<name>/SKILL.md`, while the live project remains separate. Once the skills are available to your Codex environment, invoke the relevant skill by name or explicitly ask Codex to use it.
