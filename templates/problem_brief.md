# Problem Brief

## Competition / Problem

- Competition:
- Year:
- Problem ID:
- Source files / statement attachments:

## 1. Problem objective

- Overall modeling goal:
- Overall final evaluation target:
- Main required decision / prediction / explanation:

## 2. Subproblem decomposition

Repeat this block for Q1...Qn.

### Q1

- Goal:
- Conceptual inputs required:
- Conceptual target:
- Required output:
- Intermediate outputs:
- Downstream consumer of each important output:
- Constraints:
- Evaluation target:
- Mathematical task type:
- Depends on:
- Dependency reason / interface:
- Later subproblems depending on Q1:
- Candidate high-risk dependency/interface:
- Data claims requiring audit verification:

### Q2

- Goal:
- Conceptual inputs required:
- Conceptual target:
- Required output:
- Intermediate outputs:
- Downstream consumer of each important output:
- Constraints:
- Evaluation target:
- Mathematical task type:
- Depends on:
- Dependency reason / interface:
- Later subproblems depending on Q2:
- Candidate high-risk dependency/interface:
- Data claims requiring audit verification:

## 3. Task dependency graph

This graph represents dependencies implied by the statement/mathematical task, not verified data availability. Do not infer edges from question numbering alone.

```text
Example only:
Q1 --result: intermediate quantity--> Q2 --decision input--> Q4
Q1 --validation target-------------> Q3
```

### Dependency edge registry

| From | To | Type | Conceptual interface carried | Why the dependency exists | Data verification needed? |
|---|---|---|---|---|---|

- Critical downstream outputs:
- Interfaces requiring later verification:

## 4. Data-audit verification requests

Record what must be checked against actual files; do not assume availability here.

| Subproblem | Required quantity/data concept | What must be verified | Why it matters |
|---|---|---|---|

Typical checks: target/label availability, concrete file/field mapping, observation unit, grouping/repeated measures, units, cross-dataset compatibility, information timing, leakage-sensitive identifiers.

## 5. Data and attachments known from the statement

- Files mentioned:
- Quantities/fields described by the statement:
- Units described by the statement:
- Required temporal resolution:
- Required spatial resolution:
- Unverified data claims:

## 6. Information availability and timing

| Quantity / information | Available when? | Observed / forecast / estimated / future-realized | Allowed for which decision? | Notes |
|---|---|---|---|---|

- Future information that must not be used early:
- Forecast issue times / horizons if relevant:
- Decision update frequency if relevant:

## 7. Hard constraints

## 8. Ambiguities

| ID | Issue | Impact | Status / resolution |
|---|---|---|---|

## 9. Deliverables

## 10. Current task taxonomy

- Primary task types:
- Secondary task types:
- Main uncertainty sources:
- Candidate critical dependencies for later feasibility testing:
