---
name: problem-analysis
description: Structure a mathematical modeling competition problem before choosing models. Use to decompose subproblems, identify inputs, outputs, constraints, task types, dependencies, ambiguities, and assumptions.
---

# Problem Analysis

## Goal

Convert the original statement into a reliable project brief. Do not select final models in this skill.

## Read

1. Problem statement and attachments available in the current project.
2. `references/problem-analysis-rules.md`.
3. Existing `project/problem_brief.md` and `project/assumption_ledger.md`, if present.

## Steps

1. Split the problem into explicit subproblems.
2. For each subproblem identify inputs, outputs, constraints, required deliverables, and evaluation target.
3. Classify the mathematical task at a high level only: prediction, classification, evaluation, optimization, statistics/factor analysis, graph/network, simulation, mechanism/differential equation, or mixed.
4. Draw dependencies between subproblems.
5. List ambiguities, unit issues, hidden assumptions, and missing information.
6. Record only necessary assumptions and mark whether each is verified, unverified, or imposed for tractability.

## Output

Create or update in the live project:

- `project/problem_brief.md`
- `project/assumption_ledger.md`

Use repository templates when initializing the files.

## Forbidden behavior

- Do not jump directly to a sophisticated algorithm.
- Do not silently resolve high-impact ambiguities.
- Do not invent missing data or requirements.
