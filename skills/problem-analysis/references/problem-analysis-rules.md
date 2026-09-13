# Problem Analysis Rules

For every subproblem, answer:

- What is being asked?
- What is known?
- What must be predicted / ranked / optimized / explained?
- What are the hard constraints?
- What data is relevant?
- What output format is required?
- Does this subproblem depend on an earlier result?
- What ambiguity could change the mathematical formulation?

Prefer a compact dependency graph such as:

```text
Q1 → Q2
 ↓
features
 ↓
Q3 → Q4
```

An assumption is acceptable only when its impact is explicit and it has a planned validation or sensitivity check when material.
