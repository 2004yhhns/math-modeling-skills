---
name: problem-analysis
description: Structure a mathematical modeling competition problem before choosing models. Use to decompose subproblems, identify conceptual inputs, outputs, constraints, task types, task dependencies, ambiguities, assumptions, information timing, and downstream interfaces.
---

# Problem Analysis

## Goal

Convert the original statement into a method-neutral project brief that expresses what each subproblem requires conceptually and how subproblems depend on one another according to the statement. Do not claim that required data actually exist; that is verified by `data-audit`. Do not select final models here.

## Repository resource loading

Treat the directory containing `skills/`, `knowledge/`, `templates/`, and `algorithms/` as the skills repository root.

1. Read this `SKILL.md`.
2. Read `skills/problem-analysis/references/problem-analysis-rules.md`.
3. Read the live project's problem statement and attachments.
4. Read existing `project/problem_brief.md` and `project/assumption_ledger.md` when present.
5. Initialize from `templates/problem_brief.md` and `templates/assumption_ledger.md` when needed.
6. Do not scan unrelated `knowledge/` or `algorithms/` by default.

## Core distinction: conceptual requirements vs verified data

Problem analysis may use the statement's descriptions of supplied data to understand the task, but it must not treat uninspected files as verified evidence.

For every subproblem distinguish:

- `conceptual_inputs`: quantities/information the task requires in principle;
- `conceptual_target`: quantity to predict, estimate, rank, optimize, explain, or otherwise determine;
- `expected_outputs`: results the subproblem must produce;
- `task_dependencies`: dependencies implied by the wording or mathematical structure of the problem.

Do not replace these with concrete column names, row counts, label availability, observation units, or split rules unless they have already been verified by an upstream audit. Those belong to `data-audit`.

## Steps

1. Split the statement into explicit subproblems.
2. For each subproblem record goal, conceptual inputs, conceptual target, expected outputs, constraints, deliverables, and evaluation target.
3. Classify the mathematical task only at a high level: prediction, classification, evaluation, optimization, statistics/factor analysis, graph/network, simulation, mechanism/differential equation, or mixed.
4. Build a `task_dependency_graph` from the statement. Record why every edge exists and, when identifiable, the conceptual output-to-input interface carried by the edge.
5. Distinguish dependency types when useful: result dependency, validation dependency, knowledge/interpretation dependency, decision dependency, or shared conceptual requirement.
6. Do not infer a dependency merely because Q2 follows Q1 numerically. Parallel subproblems are allowed.
7. Identify final evaluation targets and overall downstream targets.
8. Record information timing for sequential decisions.
9. List ambiguities, unit issues, temporal/spatial resolution requirements, assumptions, and missing information that could change the formulation.
10. Flag interfaces that `data-audit` must verify: required targets/labels, dataset availability, field mapping, observation unit, grouping/repeated measures, cross-dataset compatibility, and information availability.
11. Flag candidate critical dependencies for later feasibility testing, but do not declare the final CFQ here.

## Handoff to data-audit

`problem-analysis` produces the task-level specification: what should flow between questions according to the problem. `data-audit` verifies what can actually flow through the available data.

The task dependency graph must be preserved even if later data audit finds an interface unsupported. Downstream audit should add a data dependency graph and an executable dependency graph rather than silently rewriting the original task graph.

## Output

Create or update:

- `project/problem_brief.md`
- `project/assumption_ledger.md`

The problem brief must preserve:

- subproblem specifications;
- conceptual inputs and targets;
- expected and intermediate outputs;
- `task_dependency_graph` with edge reasons/interfaces;
- final evaluation targets;
- hard constraints and information-availability rules;
- `data_audit_verification_requests` for claims that require inspection of actual data;
- candidate high-risk interfaces.

## Forbidden behavior

- Do not jump directly to a sophisticated algorithm.
- Do not silently resolve high-impact ambiguities.
- Do not invent missing data, labels, fields, or requirements.
- Do not claim that a conceptual input is actually available merely because the statement requires it.
- Do not treat question order as proof of dependency.
- Do not treat future-realized information as available earlier unless explicitly allowed.
