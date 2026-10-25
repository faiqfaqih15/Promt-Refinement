# Evaluation Rubric

Score each dimension from 0 to 10 and calculate the equal-weight average, rounded to two decimals. Critical dimensions are marked `*`; any one below 8.5 blocks passage.

| Dimension | What to assess |
|---|---|
| Clarity * | Understandable, unambiguous instructions |
| Specificity * | Precise requirements and constraints |
| Structure | Logical organization and execution order |
| Completeness | Context, requirements, edge cases, and acceptance criteria |
| Consistency * | No contradictions or incompatible promises |
| Actionability * | Executable without unjustified guessing |
| Output Definition * | Expected deliverable, format, and completion conditions |
| Context Sufficiency | Relevant context for the target task |
| Maintainability | Easy to update without needless restructuring |
| Efficiency | No material duplication or waste |
| Robustness * | Handles ambiguity, edge cases, and failure modes |
| Target Alignment * | Fit for the stated or inferred tool/model |
| Testability * | Requirements can be checked objectively |
| Source Skill Alignment * | Required methodology sources and role assignments are preserved |

Priorities: **Critical** can defeat the objective or explicit requirement; **High** materially harms reliability; **Medium** is a meaningful normal improvement; **Low** has limited impact.

Each finding must cite prompt evidence, explain impact, and receive a stable ID. Reuse an ID while the same issue remains; create a new ID for new issues; mark regressions explicitly.

## Acceptance threshold

The prompt passes only when all three conditions hold:

```text
Overall Score > 9.0
No unresolved Critical issue
No critical dimension below 8.5
```

An overall score of exactly `9.00` continues refinement. A high numerical score does not override an unresolved critical issue or a critical dimension below 8.5.

## Issue lifecycle

Use these statuses: `OPEN`, `PARTIALLY_IMPLEMENTED`, `IMPLEMENTED`, `VERIFIED_RESOLVED`, `REGRESSED`, and `REOPENED`. The refiner may claim implementation, but only evaluator evidence can mark an issue `VERIFIED_RESOLVED`.

At each iteration:

- Keep the same ID for an existing unresolved issue.
- Mark a fully fixed issue `VERIFIED_RESOLVED` and retain it in history.
- Mark an incomplete fix `PARTIALLY_IMPLEMENTED` and keep it active.
- Reopen or mark `REGRESSED` when a resolved issue returns.
- Assign the next available ID to a genuinely new issue.

If the overall score falls, record the cause and lost improvement as a regression, and recommend recovery from the best-scoring prompt.

## Execution-mode boundary

The dimensions and equal-weight instruction score above remain unchanged. Use the configured QUALITY_THRESHOLD (default 9.0), strict greater-than comparison, fixed 8.5 critical floor, and no-unresolved-Critical rule. Instruction passage certifies Review only. In Tested, additionally require all six frozen cases to pass for the exact selected prompt with no hard-constraint violation, following tested-workflow.md. Keep task results separate; never average them into the rubric score or let a higher score cancel a failed case.
