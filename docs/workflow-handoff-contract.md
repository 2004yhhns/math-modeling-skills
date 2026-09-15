# Workflow Handoff Contract

This document defines what must survive between modeling stages. Downstream skills must not reconstruct important assumptions, data semantics, interfaces, evidence, or risks from scratch.

## Human-readable workflow

```text
PROBLEM ANALYSIS
      ↓
DATA AUDIT → DATA PREPARATION
      ↓
MODEL RESEARCH
  ├─ reusable knowledge
  └─ live-project literature: paper/literature/
      ↓
BENCHMARK LANDSCAPE
      ↓
CANDIDATE POOL
      ↓
RISK TRIAGE
 ┌────┴──────────────┐
 ↓                   ↓
LOW RISK          HIGH RISK
 ↓                   ↓
 │            FEASIBILITY TEST
 │             GO / NO-GO
 └──────────┬────────┘
            ↓
      SCREENING EXPERIMENT
            ↓
          SHORTLIST
   baseline + primary + alternatives
            ↓
       MODEL BUILDING
            ↓
       MODEL VALIDATION
            ↓
      FINAL RECOMMENDATION
            ↓
       IMPROVEMENT LOOP ↺
```

The user-facing workflow can group `data-audit + data-preparation` as “data review and cleaning/preparation”. Internally they remain separate so diagnosis is not confused with irreversible cleaning decisions.

## 1. problem-analysis → data-audit

`problem-analysis` preserves subproblem definitions, conceptual inputs/targets/outputs, hard constraints, evaluation targets, task dependencies, information rules, assumptions/ambiguities, and data-verification requests.

`data-audit` verifies what actual data can support and must not silently rewrite the task logic.

## 2. data-audit → data-preparation

`data-audit` preserves dataset inventory, observation unit, grouping/independent unit, variable semantics/units, missingness/duplicates/ranges, time/spatial structure, information timing, leakage risks, dataset compatibility, question-data mapping, recommended split, unresolved issues, and preparation requirements.

`data-preparation` decides and executes only semantically defensible transformations. Missing is not automatically error or zero; mean/median filling, deletion, smoothing/resampling and future-information reconstruction are never default actions. High-impact choices require explicit justification/Human Gate under project governance.

## 3. data-preparation → model-selection

Preserve prepared-data provenance/status (`READY`, `READY_WITH_WARNINGS`, `HOLD`), transformations and rationale, rejected/deferred treatments, before/after diagnostics, unresolved issues and downstream restrictions.

Model-selection must carry those restrictions into literature interpretation, candidate design, screening and validation.

## 4. Literature evidence inside model-selection

Project-specific literature is stored only under:

```text
paper/literature/
├── downloaded/
├── literature_matrix.md
├── benchmark_landscape.md
└── literature_notes.md
```

Literature findings are cross-stage evidence:

```text
literature
├─ data semantics/leakage → data-audit
├─ preparation questions  → data-preparation
├─ baselines/candidates   → model-selection
├─ split/metrics/validation standards → model_spec/validation
└─ interpretation/domain context → final paper/discussion
```

Do not put current-project papers into reusable `knowledge/`.

## 5. model-selection → risk triage / feasibility

Model-selection builds an evidence-backed benchmark landscape and candidate pool. Each serious candidate receives a risk triage.

`LOW_RISK` mature candidates can proceed directly to screening when their data/interface requirements are satisfied.

`HIGH_RISK` candidates are routed to `feasibility-test` only when a critical mechanism/interface may invalidate the route. Feasibility returns viability evidence (`GO`, `GO_WITH_RISKS`, `HOLD`, `NO_GO`, or `NOT_REQUIRED`), not the best model.

## 6. Viable candidates → screening

Screening compares viable candidates under a common lightweight protocol: same legal data, split/CV, metrics, comparable preprocessing boundaries and modest compute budget.

Screening produces a **shortlist**, normally:

```text
baseline(s)
+ primary candidate
+ serious alternative(s)
```

It should not force one final winner before full modeling and validation.

## 7. shortlist → model-building

`project/model_spec.json` is the modeling-hand → programming-hand experiment contract.

It preserves approved models/roles, data/features, forbidden information, split/CV, metrics, preprocessing boundaries, tuning budget, reproducibility requirements, validation/robustness/interpretability plans and required outputs.

Model-building owns most full algorithm implementation, tuning and training. It writes code/results only into the live project and logs meaningful runs/failures.

## 8. model-building → model-validation

Model-building preserves code/config version, experiment configuration, random seed, split, model-dependent preprocessing, parameters, outputs/diagnostics, runtime, failed experiments and contract deviations.

Validation compares the full-built shortlist against baselines and checks leakage, generalization, error/residual structure, robustness, sensitivity, interpretation and downstream usefulness. It may recommend the final model only after this evidence exists.

## 9. validation → improvement loop

Validation diagnoses before changing the system:

```text
raw-data diagnosis problem       → data-audit
cleaning/preparation problem     → data-preparation
wrong model family/candidates    → model-selection
unproven high-risk mechanism     → feasibility-test when actually needed
implementation/tuning problem    → model-building
validation-design problem        → model-validation with transparent revision
interpretation-only problem      → limit/rewrite claim
```

Each loop records the observed weakness, evidence, owner stage, proposed change, new experiment/validation id and whether the change helped.

## Cross-stage invariant checks

| Concept | First owner | Later consumers |
|---|---|---|
| Task/evaluation target | problem-analysis | all later stages |
| Observation/group/data semantics | data-audit | all later stages |
| Cleaning/preparation rationale | data-preparation | all later stages |
| Literature evidence | model-selection/live paper | selection, validation, interpretation |
| Benchmark landscape | model-selection | candidate design, paper |
| Candidate risk triage | model-selection | feasibility/screening |
| CFQ for high-risk route | feasibility-test | screening/building/validation |
| Screening evidence | model-selection | model-building/validation |
| Experiment contract | model-selection | programming/model-building |
| Full experimental evidence | model-building | model-validation |
| Final recommendation/improvement diagnosis | model-validation | final paper or routed owner |

## Core principle

Do not repair an upstream semantic error only inside downstream code. Literature tells us what is worth trying; feasibility tells us whether a risky route is executable; screening tells us what deserves full investment; model-building produces full experimental evidence; validation determines what is defensible and where to improve.
