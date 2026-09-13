# Design Sources and Rationale

This repository is an original synthesis. It borrows architectural ideas, not copied content, from three public repositories.

## 1. Hjdd14/math-modeling — workflow governance

Primary files reviewed:

- `SKILL.md`
- `references/workflow.md`
- `references/templates.md`

Ideas adapted:

- a short main skill that delegates detail to references,
- `problem_brief.md` as a stable source of truth,
- explicit assumption tracking,
- a baseline before advanced modeling,
- structured model-selection audit,
- `model_spec.json` as a contract between modeling, coding, and validation,
- validation artifacts and phase-gate thinking,
- validation failure should return the workflow to the relevant earlier stage.

Mapped into this repository:

| This repository | Main inspiration |
|---|---|
| `AGENTS.md` | global workflow/governance constraints |
| `problem-analysis` | problem brief + assumptions + ambiguity discipline |
| `data-audit` | data schema/leakage/quality thinking |
| `templates/problem_brief.md` | fixed project artifact concept |
| `templates/assumption_ledger.md` | assumption ledger |
| `templates/baseline_solution.json` | explicit baseline artifact |
| `templates/model_selection_audit.json` | candidate/rejection audit |
| `templates/model_spec.json` | model contract |
| `model-validation` | evidence and validation gates |

We intentionally do not reproduce its full Phase 0–5 or large artifact set in v0.1. This repository starts smaller.

## 2. dreamnight16/MCM-Resource — shared knowledge organization

Primary file reviewed:

- `README.md`, especially the directory structure and problem-type quick reference.

Ideas adapted:

- organize model knowledge by **problem family**, not a flat algorithm list,
- separate conceptual model documentation from runnable algorithms,
- route a modeling problem to the relevant model family before choosing a specific algorithm.

Mapped into this repository:

| This repository | Main inspiration |
|---|---|
| `knowledge/prediction/` | problem-type model organization |
| `knowledge/classification/` | problem-type model organization |
| `knowledge/optimization/` | problem-type model organization |
| `knowledge/evaluation/` | problem-type model organization |
| `algorithms/` | separate reusable implementation layer |

Future expansions may add `statistics/`, `simulation/`, `mechanism/`, `time-series/`, and `auxiliary/`.

## 3. chengziyue1222/math-model-agent — practical Codex skill decomposition

Primary files reviewed:

- `skills/select-model/SKILL.md`
- repository `README.md`

Ideas adapted:

- decompose one giant modeling assistant into practical reusable skills,
- model selection is a reasoning workflow, not an algorithm name,
- compare 2–4 candidates,
- distinguish a **primary route**, **simple baseline**, and **independent validation route**,
- explicitly record rejected candidates and reasons,
- keep detailed method catalogs and implementations outside the main skill instructions.

Mapped into this repository:

| This repository | Main inspiration |
|---|---|
| `model-selection/SKILL.md` | primary / baseline / validation roles |
| `model-selection/references/` | thin skill + detailed references |
| `model-building/SKILL.md` | separate implementation from selection |
| `algorithms/` | implementations separate from skill text |

## Why the repository does not clone all three structures

The three repositories optimize for different goals:

- Hjdd14: rigorous end-to-end workflow and evidence governance,
- MCM-Resource: broad knowledge-base coverage,
- math-model-agent: practical skill decomposition and implementations.

For a first personal competition repository, copying all three would create too many files and too much governance before one real problem has been run.

Version 0.1 therefore keeps only:

1. problem analysis,
2. data audit,
3. model selection,
4. model building,
5. model validation,
6. four shared knowledge families,
7. reusable project templates.

After one complete historical or real problem, add new skills only when an actual workflow gap appears.
