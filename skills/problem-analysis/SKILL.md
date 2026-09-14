---
name: problem-analysis
description: Structure a mathematical modeling competition problem before choosing models. Use to decompose subproblems, identify inputs, outputs, constraints, task types, dependencies, ambiguities, assumptions, information timing, and downstream interfaces.
---

# Problem Analysis

## Goal

Convert the original statement into a reliable project brief that downstream data-audit, model-selection, and feasibility-test can consume. Do not select final models in this skill.

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the **skills repository root**.

The user only needs to invoke this Skill. Do not require the user to separately list repository resources.

Load resources in this order:

1. Read this `SKILL.md`.
2. Read this Skill's local references:
   - `skills/problem-analysis/references/problem-analysis-rules.md`
3. Read the live project's problem statement and attachments.
4. Read existing live-project state when present:
   - `project/problem_brief.md`
   - `project/assumption_ledger.md`
5. When initializing an artifact, use the repository templates:
   - `templates/problem_brief.md`
   - `templates/assumption_ledger.md`
6. Do not scan unrelated `knowledge/` or `algorithms/` by default. Problem analysis should remain method-neutral unless a specific mathematical concept is needed to understand the statement.

Repository templates are clean source templates. Populate copies in the live project's `project/` directory; do not write live competition state back into `templates/`.

## Read

1. Problem statement and attachments available in the current project.
2. `references/problem-analysis-rules.md`.
3. Existing `project/problem_brief.md` and `project/assumption_ledger.md`, if present.

## Steps

1. Split the problem into explicit subproblems.
2. For each subproblem identify inputs, outputs, constraints, required deliverables, and evaluation target.
3. Classify the mathematical task at a high level only: prediction, classification, evaluation, optimization, statistics/factor analysis, graph/network, simulation, mechanism/differential equation, or mixed.
4. Draw dependencies between subproblems and important intermediate quantities. Record not only `Q1 → Q2`, but also important output-to-input interfaces such as `forecast → optimizer` or `estimated parameter → simulator` when present.
5. Identify the final evaluation target for each subproblem and, when multiple stages are coupled, the overall downstream target that later stages ultimately serve.
6. Record information timing when decisions are sequential: what is known at decision time, what becomes known only later, and what future information must not be used.
7. List ambiguities, unit issues, temporal/spatial resolution requirements, hidden assumptions, and missing information that could change the mathematical formulation.
8. Record only necessary assumptions and mark whether each is verified, unverified, or imposed for tractability.
9. Flag candidate critical dependencies or interfaces for later feasibility testing, but do not declare the final CFQ here.

## Output

Create or update in the live project:

- `project/problem_brief.md`
- `project/assumption_ledger.md`

Use repository templates when initializing the files.

The problem brief should preserve enough information for downstream skills to recover:

- subproblem dependencies,
- important intermediate outputs and consumers,
- final evaluation targets,
- hard constraints,
- information-availability rules,
- time/spatial resolution requirements,
- candidate high-risk interfaces.

## Forbidden behavior

- Do not jump directly to a sophisticated algorithm.
- Do not silently resolve high-impact ambiguities.
- Do not invent missing data or requirements.
- Do not treat a future-realized quantity as available at an earlier decision time unless the statement explicitly allows it.
