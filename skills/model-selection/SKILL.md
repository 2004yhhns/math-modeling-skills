---
name: model-selection
description: Research the modeling landscape, generate defensible candidates, triage feasibility risk, run lightweight comparable screening, and produce a shortlist/experiment contract for full model building.
---

# Model Selection

## Goal

Turn the audited problem and data regime into an evidence-backed shortlist. Do not jump from “this model sounds suitable” to a final model.

This stage owns four linked jobs:

1. literature/model research;
2. benchmark-landscape and candidate generation;
3. risk triage and optional feasibility handoff;
4. lightweight screening under comparable conditions.

The final competition model is **not** selected here. This stage normally hands a small shortlist to `model-building`; `model-validation` later supplies the evidence for the final choice.

## Position in the workflow

```text
problem-analysis
→ data-audit
→ data-preparation
→ model-selection
    ├─ literature research
    ├─ benchmark landscape
    ├─ candidate pool
    ├─ risk triage
    │    └─ high-risk route → feasibility-test → GO/NO-GO
    └─ screening experiment
→ shortlist
→ model-building
→ model-validation
→ improvement loop
```

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the skills repository root. The live competition project is separate.

Load in this order:

1. this `SKILL.md`;
2. `skills/model-selection/references/selection-rules.md`;
3. live-project state:
   - `project/problem_brief.md`
   - `project/data_audit.json`
   - `project/data_preparation.json`, if present
   - `project/assumption_ledger.md`
4. relevant reusable `knowledge/` selectively;
5. current-project literature under `paper/literature/`, if already present;
6. external literature when needed;
7. `algorithms/` only after serious candidates are known, to check reusable implementations/utilities;
8. initialize project outputs from repository templates.

Never put competition-specific papers, notes, or literature conclusions into the reusable skills repository.

## External literature policy

When external literature can materially improve candidate design or validation design, search it. Prefer peer-reviewed journal/conference papers, official dataset papers, authoritative reviews, and established benchmark studies.

All current-project literature belongs in the live project:

```text
paper/
└── literature/
    ├── downloaded/              # PDFs/files when downloading is permitted and useful
    ├── literature_matrix.md     # structured evidence extraction
    ├── benchmark_landscape.md   # field-level method map
    └── literature_notes.md      # synthesis, limitations, open questions
```

Rules:

- never save project-specific papers under `math-modeling-skills/knowledge/`, `skills/`, `references/`, or `algorithms/`;
- preserve title, authors, year, venue and source/link/identifier for traceability;
- do not infer a paper's method or result from title alone;
- distinguish what the paper actually reports from our interpretation;
- do not copy a paper's model merely because it is recent or sophisticated.

If the environment cannot download a paper, record the traceable citation/link and analyze only the evidence actually accessible.

## Step 1 — Reconstruct the current modeling contract

For the target subproblem identify:

- task and evaluation target;
- usable data and preparation status;
- observation/group/time/spatial constraints;
- leakage/information restrictions;
- interpretability/extrapolation requirements;
- upstream outputs consumed and downstream outputs required.

If preparation status is `HOLD`, do not silently continue with a candidate that requires blocked data.

## Step 2 — Research literature as evidence

Search around the intersection of:

```text
current task
+ domain/problem context
+ data regime / measurement type
+ evaluation or validation constraint
```

For each core paper extract, when available:

- research problem;
- dataset/sample size and observation unit;
- features/inputs and target;
- preprocessing/feature engineering;
- split/CV strategy and leakage controls;
- model/algorithm;
- baselines/comparison models;
- metrics;
- main result;
- limitations/failure modes;
- relevance to the current problem;
- transferable idea;
- non-transferable difference or risk.

Update `paper/literature/literature_matrix.md` and `paper/literature/literature_notes.md`.

Literature evidence may affect more than model choice. Route findings to the owning stage when they reveal:

- data semantics/leakage risk → `data-audit`;
- defensible preparation/feature handling questions → `data-preparation`;
- candidate/baseline ideas → `model-selection`;
- split/metrics/external validation requirements → validation plan;
- interpretation/domain context → final analysis/paper discussion.

## Step 3 — Build the benchmark landscape

A benchmark landscape is the map of established methodological families and comparison standards relevant to this task; it is broader than a baseline.

Organize evidence into roles such as:

```text
Benchmark landscape
├─ simple meaningful baselines
├─ established/standard benchmark methods
├─ strong modern candidates
└─ improved/problem-specific candidates
```

For each family record evidence, typical strengths, assumptions, data fit, validation practice, and known limitations. Save the synthesis to `paper/literature/benchmark_landscape.md`.

## Step 4 — Generate the candidate pool

Construct a small defensible pool, normally including:

- at least one meaningful baseline;
- one or more established strong/standard methods when appropriate;
- one or more serious improved/problem-specific candidates when justified.

For each serious candidate record:

- role and literature/repository evidence;
- fit reason and assumptions;
- exact data/preprocessing requirements;
- expected inputs/outputs;
- upstream/downstream interfaces;
- interpretability and computational cost;
- leakage/information risks;
- implementation/numerical risks;
- validation method.

Do not create novelty for novelty's sake.

## Step 5 — Risk triage

Classify each serious candidate as `LOW_RISK`, `HIGH_RISK`, or `BLOCKED` for pre-building feasibility.

High-risk triggers include a genuinely uncertain critical mechanism, complex model-to-model interface, hard-feasibility constraint, numerical stability risk, unusual implementation, severe information-timing restriction, or a data/preparation dependency whose failure would invalidate the route.

Mature standard algorithms are not sent to feasibility testing merely to prove that a library implementation runs.

For each high-risk candidate, write a feasibility target and route only that candidate/mechanism to `feasibility-test`. `feasibility-test` returns viability evidence (`GO`, `GO_WITH_RISKS`, `HOLD`, `NO_GO`), not a winner.

## Human Gate 1 — Candidate design

Before expensive experiments, present:

- benchmark landscape summary;
- literature evidence quality/gaps;
- candidate pool and roles;
- rejected ideas and reasons;
- risk triage;
- candidates requiring feasibility test;
- proposed screening protocol.

Pause when human approval is required by project governance.

## Step 6 — Lightweight screening experiment

After required feasibility gates are resolved, compare all viable candidates under a common screening protocol.

Screening asks: **which candidates deserve full modeling investment?** It is not final validation.

Keep it lightweight but fair:

- same prepared data and legal information set;
- same approved split/CV logic;
- same primary/secondary metrics;
- comparable preprocessing boundaries;
- modest/common compute budget;
- fixed seeds where applicable;
- record failures and runtime.

Compare, as relevant:

- predictive/objective performance;
- variability/stability;
- constraint satisfaction;
- runtime/computational burden;
- interpretability;
- robustness signals;
- downstream usefulness/interface quality.

Do not over-tune during screening.

## Step 7 — Produce the shortlist and experiment contract

Normally retain a small shortlist rather than one model:

```text
baseline(s)
+ primary candidate
+ serious alternative(s)
```

The shortlist may contain multiple models so full model building and validation can establish whether the apparent advantage survives tuning, robustness and generalization checks.

Create/update:

- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`

`model_spec.json` is the modeling-hand → programming-hand experiment contract. It must state approved models, data/features, forbidden predictors, split/CV, metrics, preprocessing boundaries, random seeds where needed, tuning budget/plan, validation requirements, and required outputs.

## Human Gate 2 — Full-build shortlist

Before expensive full model building, show:

- screening evidence;
- feasibility evidence where applicable;
- shortlist roles;
- rejected candidates and reasons;
- approved experiment contract.

## Forbidden behavior

- no final winner from literature prestige alone;
- no final winner from one screening run;
- no deep learning/complex model for novelty alone;
- no candidate without a meaningful comparison baseline;
- no project papers stored in the reusable skills repository;
- no accuracy-only decision when other constraints matter;
- no silent change of split, information legality, or high-impact preparation assumptions;
- no mandatory MVM for every mature candidate;
- no full-build handoff for a high-risk candidate with unresolved `HOLD`/`NO_GO` feasibility status.
