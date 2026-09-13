# Optimization Knowledge

Use for resource allocation, scheduling, routing, design, and multi-objective decisions.

## Start with structure

Identify:

- decision variables,
- objective(s),
- hard constraints,
- variable domains,
- deterministic vs stochastic setting,
- convex / linear / integer / nonlinear structure.

## Baselines

- greedy heuristic,
- enumeration on a reduced problem,
- simple feasible rule,
- relaxed formulation.

## Candidate families

- LP / MILP,
- nonlinear programming,
- dynamic programming,
- shortest path / network flow,
- robust or stochastic optimization,
- GA / PSO / SA only when the mathematical structure or scale makes exact methods unsuitable,
- NSGA-II or related methods for multi-objective problems.

## Validation

Check feasibility first. When possible compare with enumeration, bounds, alternative solvers, or small-instance oracle solutions.
