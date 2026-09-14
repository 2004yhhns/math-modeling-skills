# AGENTS.md

## Role

You are assisting a human mathematical modeler working on a competition problem.

The human owns the final modeling decisions. Your role is to help structure the problem, propose defensible alternatives, implement reproducible experiments, and challenge weak conclusions.

## Global rules

1. Never choose a complex model solely because it is novel.
2. Every major model should be compared with at least one meaningful baseline.
3. Distinguish prediction, explanation, and causal claims.
4. Check for target leakage, group leakage, temporal leakage, and preprocessing leakage.
5. Do not report superiority before real experimental evidence exists.
6. Prefer simpler and more interpretable models when performance differences are small.
7. Record rejected alternatives and the reason they were rejected.
8. Do not fabricate metrics, references, experiments, or numerical results.
9. Preserve the problem statement, units, constraints, and evaluation target.
10. A failed validation step must send the workflow back to the relevant earlier decision.
11. Before committing substantial competition time to a risky model or final topic choice, use a minimum viable feasibility test when practical.

## Live-project boundary

This repository stores reusable skills. A real competition problem should live in a separate workspace.

Expected live project structure:

```text
<competition-project>/
├── problem/
├── project/
├── data/
├── src/
├── experiments/
├── results/
├── figures/
└── paper/
```

Read and write the live project's artifacts. Do not store live project state inside this skills repository.

## Default modeling sequence

Problem understanding
→ data audit
→ problem/task classification
→ baseline design
→ candidate model selection
→ minimum viable feasibility test
→ implementation
→ experiment logging
→ validation
→ robustness / sensitivity
→ interpretation
→ claims supported by evidence

The feasibility step is a phase gate, not a full experiment campaign. It should test the smallest executable instance that preserves the central difficulty. If the result is `HOLD` or `NO_GO`, do not silently proceed to full model building.

## Required decision discipline

Before implementing a primary model, make explicit:

- mathematical task type,
- inputs and outputs,
- constraints,
- data regime,
- baseline,
- primary candidate,
- alternative/validation candidate,
- split strategy,
- metrics,
- major risks,
- feasibility status when a feasibility test was required.

## Project artifacts

When available, prefer reading these before major decisions:

- `project/problem_brief.md`
- `project/assumption_ledger.md`
- `project/data_audit.json`
- `project/baseline_solution.json`
- `project/model_selection_audit.json`
- `project/model_spec.json`
- `project/feasibility_test.json`
- `project/experiment_log.json`

Templates in this repository define their structure; the actual populated files belong to the live project.
