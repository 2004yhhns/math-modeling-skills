# Problem Analysis Rules

For every subproblem answer:

- What is being asked?
- What conceptual inputs are required in principle?
- What is the conceptual target?
- What outputs/intermediate quantities must be produced?
- What are the hard constraints and evaluation targets?
- Does it depend on an earlier result, and why?
- Which later subproblem consumes each important output?
- What ambiguity could change the mathematical formulation?
- Which claims require later verification against actual data?

## Task dependency graph

Build dependencies from wording and mathematical logic, not question numbering. For every edge record the reason and, when identifiable, the conceptual interface.

Example:

```text
Q1 --result: estimated parameter--> Q2
Q1 --validation target-----------> Q3
Q2 and Q4 may remain parallel if the statement gives no dependency.
```

Useful edge types include `result`, `validation`, `knowledge_or_interpretation`, `decision`, and `shared_requirement`.

## Conceptual versus actual data

At this stage `input` means a conceptual requirement, not a verified CSV column. A statement may require symptom labels, time series, coordinates, or measurements that later prove missing or incompatible. Record that requirement and send it to `data-audit` for verification rather than inventing availability.

Request data-audit verification when relevant for:

- required labels/targets;
- concrete dataset/file mapping;
- observation unit;
- grouping and repeated measurements;
- units and semantic equivalence;
- cross-dataset compatibility;
- information timing;
- leakage-sensitive identifiers.

An assumption is acceptable only when its impact is explicit and it has a planned validation or sensitivity check when material.
