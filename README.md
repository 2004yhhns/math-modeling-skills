# Math Modeling Skills for Codex

A reusable skills repository for mathematical modeling competitions.

This repository is **not** a place to store real competition projects. It stores reusable workflows, mathematical knowledge, templates and algorithm guidance. Competition data, papers, code and results belong in a separate live project.

## Design goals

1. Keep the human modeler in charge of high-impact/final modeling decisions.
2. Separate problem meaning, data diagnosis, data preparation, model research, implementation and validation.
3. Make model choice evidence-driven: literature → benchmark landscape → candidate pool → screening → full validation.
4. Require meaningful baselines before claiming improvement.
5. Preserve raw evidence and make cleaning problem-driven rather than recipe-driven.
6. Treat leakage, grouping, information timing, robustness and interpretability as first-class constraints.
7. Use explicit handoff contracts between the modeling hand and programming hand.

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

- `skills/`: reusable stage workflows.
- `skills/*/references/`: local SOPs for a skill.
- `knowledge/`: reusable mathematical/model knowledge, not current-project literature.
- `templates/`: clean reusable output templates.
- `algorithms/`: reusable implementation guidance/code.
- `docs/`: cross-stage architecture and handoff contracts.

## Live competition project

Recommended separation:

```text
workspace/
├── math-modeling-skills/
└── COMPETITION-PROJECT/
    ├── AGENTS.md
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

Current-project papers and literature notes stay under the live project's `paper/literature/`; never put them into reusable `math-modeling-skills/knowledge/`.

## Human-readable workflow

```text
1. PROBLEM ANALYSIS
        ↓
2. DATA REVIEW & PREPARATION
   data-audit → data-preparation
        ↓
3. MODEL RESEARCH & CANDIDATE GENERATION
   reusable knowledge + external literature
        ↓
   literature matrix
        ↓
   benchmark landscape
        ↓
   baseline + standard + strong/improved candidates
        ↓
4. MODEL FILTERING
   risk triage
   ├─ low risk ───────────────────────┐
   └─ high risk → feasibility-test ──┤
                                      ↓
                              screening experiment
                                      ↓
                                  SHORTLIST
                         baseline + primary + alternatives
                                      ↓
5. MODEL BUILDING
   full pipeline + tuning + reproducible experiments
                                      ↓
6. MODEL VALIDATION & EVALUATION
   generalization + robustness + sensitivity + interpretation
                                      ↓
                              FINAL RECOMMENDATION
                                      ↓
7. IMPROVEMENT LOOP
   diagnose → route to owner stage → rebuild/revalidate ↺
```

The repository still uses 7 core Skills, but user-facing stages do not have to map one-to-one to Skill directories. `data-audit` and `data-preparation` are separate internally for safety. `feasibility-test` is conditional, not a mandatory gate for every model. The improvement loop is primarily driven by validation rather than a separate Skill.

## What the benchmark landscape means

The benchmark landscape is the evidence-backed map of relevant methodological families and established comparison practices. It is broader than a baseline.

```text
Benchmark landscape
├─ simple meaningful baseline(s)
├─ established / standard benchmark(s)
├─ strong candidate(s)
└─ improved / problem-specific candidate(s)
```

Literature tells us what is worth trying; it does not determine the winner. Scores reported by different papers are not directly comparable unless data and evaluation protocols match.

## Literature research policy

When model-selection needs external evidence, it should search literature relevant to the intersection of the current task, domain/data regime and evaluation constraints. Prefer traceable peer-reviewed papers, official dataset papers, authoritative reviews and established benchmark studies.

For each core paper extract, when available: problem, dataset/sample, observation/group unit, features, target, preprocessing, split/CV/leakage controls, model, baselines, metrics, results, limitations, relevance and transferability.

Literature findings can route to multiple stages:

```text
literature
├─ data semantics / leakage → data-audit
├─ preparation questions    → data-preparation
├─ baseline / candidates    → model-selection
├─ split / metrics / external validation → validation plan
└─ domain interpretation    → final paper/discussion
```

Reusable templates: `templates/literature_matrix.md` and `templates/benchmark_landscape.md`.

## Model filtering: feasibility vs screening

These answer different questions.

**Feasibility test:** “Can this genuinely high-risk mechanism/interface work well enough to continue?” It is triggered only for high-risk candidates and returns `GO`, `GO_WITH_RISKS`, `HOLD`, `NO_GO`, or `NOT_REQUIRED`.

**Screening:** “Among viable candidates, which deserve full modeling investment?” It compares candidates under the same legal data, split/CV, metrics and modest compute budget.

Normally screening outputs a shortlist, not one final model:

```text
baseline(s)
+ primary candidate
+ serious alternative(s)
```

Full model-building and model-validation provide the evidence for the final recommendation.

## Modeling hand → programming hand

`project/model_spec.json` is the experiment contract. The modeling hand specifies the approved shortlist, data/features, forbidden information, split/CV, metrics, preprocessing boundaries, tuning plan, robustness/interpretability requirements and required outputs.

The programming hand should implement that contract in `src/`/`experiments/`, save results under `results/`, and log runs in `project/experiment_log.json`. If implementation reveals a semantic/modeling defect, route it back rather than silently redesigning the model in code.

Most formal algorithm implementation and tuning belong to `model-building`; most full evaluation/diagnostic code belongs to `model-validation`. Earlier stages may still run small audit, preparation, feasibility or screening scripts when needed for decisions.

## Improvement loop

Validation diagnoses the owner before changing anything:

```text
raw-data diagnosis      → data-audit
preparation choice      → data-preparation
wrong model family      → model-selection
high-risk mechanism     → feasibility-test when needed
implementation/tuning   → model-building
validation design       → model-validation
interpretation only     → limit/rewrite the claim
```

## Invocation contract

The user should normally name the Skill and target; the Skill loads its own relevant references, knowledge, templates and live-project state. Do not require the user to enumerate every dependency manually.

Examples:

```text
使用 `math-modeling-skills/skills/model-selection/SKILL.md` 对 Q1 做模型研究与筛选。
根据当前 problem/data 状态搜索必要的外部文献；项目相关论文和文献分析只写入当前项目 `paper/literature/`，不要写入 math-modeling-skills。
先建立 benchmark landscape 和 candidate pool，做 risk triage；只有 high-risk candidate 才进入 feasibility-test；其余进入统一 screening。到 Human Gate 暂停。
```

```text
使用 `math-modeling-skills/skills/feasibility-test/SKILL.md` 对已标记 HIGH_RISK 的候选机制做最小可行性检验，到 Human Gate 暂停并给我 Feasibility Card。
```

```text
使用 `math-modeling-skills/skills/model-building/SKILL.md`，严格按照 `project/model_spec.json` 实现 shortlist，不要自行改变 split、metrics、feature legality 或高影响数据处理规则。
```

See `docs/workflow-handoff-contract.md` for the detailed producer/consumer contract and return paths.
