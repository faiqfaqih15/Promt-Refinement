---
name: prompt-refinement-loop
description: Iteratively evaluates and refines prompts through a bounded, traceable quality loop. Use when a user asks to improve, evaluate, harden, or quality-check a prompt through repeated evaluation and revision.
---

# Prompt Refinement Loop

Evaluate prompt instructions, refine from evidence, verify fixes, detect regressions, and retain the best version. Defaults: `mode=Review`, output format Full, `QUALITY_THRESHOLD=9.0` (strictly greater than 9.0), and `MAX_ITERATIONS=5`. Execution mode and output format are separate inputs; use Compact only when requested.

## Input contract and modes

Before evaluation, record the objective, target model if known, expected output, hard constraints, and success criteria. Infer these from available context; ask only for missing information that materially affects correctness. Mark unknown target details explicitly. Keep success criteria and hard constraints fixed throughout the run; do not weaken them to make a revision pass.

- **Review:** the default. Evaluate instructions without executing task cases. A passing report states “Lolos evaluasi instruksi; belum diuji melalui eksekusi.” Translate faithfully if another reporting language is requested.
- **Tested:** only when requested. Read [references/tested-workflow.md](references/tested-workflow.md) before starting. Establish six cases, execute the baseline and revisions on the agreed target, and require both rubric passage and six-case passage. If the target, required tools, or fresh-context execution is unavailable, terminate as `VALIDATION_UNAVAILABLE`; do not silently fall back to Review or substitute models.

## Responsibilities and independence

- **Evaluator:** scores instructions, diagnoses, assigns stable issue IDs, verifies prior fixes, and detects regressions. It never rewrites the prompt. Receive the prompt, fixed contract, methodology references, and prior issue descriptions/statuses needed to verify fixes, without previous scores or the refiner's rationale.
- **Refiner:** produces one complete replacement prompt and an issue-to-change mapping from active findings. Preserve intent, constraints, target, output, and methodology roles. It never decides passage. In Tested, receive development findings only; keep holdout inputs, outputs, and diagnostics hidden until termination.
- **Orchestrator:** owns sequencing, state, evidence, gates, and termination. In Tested, arrange isolated runners and graders following the Tested reference. Supplementary methods never acquire these responsibilities.

Review permits a distinct evaluator pass in the same conversation. Report isolation as `SAME_CONTEXT` or `FRESH_CONTEXT` according to actual execution; procedural separation is not fresh-context independence. Tested requires fresh execution contexts for each case and version, grading independent of the refiner, and holdout-free evaluator/refiner contexts separate from the orchestrator history; if any required separation is unavailable, terminate VALIDATION_UNAVAILABLE. Do not claim isolation that was not provided.

## Execution

1. Validate the prompt and capture the input contract. In Tested, freeze the six-case set and execution configuration and verify capability before making any revision.
2. Evaluate the initial prompt before changing it, using every dimension in [references/evaluation-rubric.md](references/evaluation-rubric.md). Record evidence, priorities, stable IDs (`I-001`, ...), Critical findings, and recommendations. The initial evaluation is iteration 1; the default five iterations include it and at most four replacement evaluations.
3. In Tested, execute the initial prompt on all six cases once. Store holdout evidence privately with the orchestrator. Release development findings to the evaluator/refiner. Failed task cases cannot be cured by a higher instruction score.
4. If the mode-specific gate fails and another iteration is allowed, give the refiner active issues and require exactly one replacement with material changes mapped to issue IDs. Do not invent requirements or rewrite solely for aesthetics.
5. Independently re-evaluate each replacement; from iteration 2, include prior-issue verification and current findings. In Tested, run that version once on the same four development cases and configuration. Keep resolved issues in history.
6. Only evaluator evidence changes issue statuses: `OPEN`, `PARTIALLY_IMPLEMENTED`, `IMPLEMENTED`, `VERIFIED_RESOLVED`, `REGRESSED`, or `REOPENED`. Apply state, selection, and stagnation rules in [references/refinement-rules.md](references/refinement-rules.md).
7. Review passes only through the rubric gate. In Tested, select the highest-scoring valid version that passes the rubric and all four development cases, then check its two holdout cases once (reuse initial-version evidence when that exact version is selected). Accept only if all six pass and no hard constraint is violated. If either holdout fails, stop `TEST_FAILED`; do not use it to revise again in this run.
8. Terminate using the table below. Return the best valid instruction version and, separately, any fully validated `best_tested_prompt`; never label an untested fallback as execution-validated. If no valid instruction version exists, report the best fields as null and return the original only as an unresolved, unvalidated artifact, following the refinement rules.

## Gate and termination

The rubric gate requires `overall_score > QUALITY_THRESHOLD`, no unresolved Critical issue, and no critical dimension below 8.5. At the default threshold exactly 9.00 fails. Tested adds passage of all six fixed cases without hard-constraint violations; use case results, not a blended rubric/test score.

Apply these decisions in order at a decision boundary:

| Condition | Status |
|---|---|
| Tested cannot obtain valid target/configuration, required tools, isolation, or execution evidence | `VALIDATION_UNAVAILABLE` |
| The final selected version fails either holdout | `TEST_FAILED` |
| The mode-specific gate is satisfied | `PASSED` |
| Evaluation count reaches MAX_ITERATIONS without satisfying the gate | `MAX_ITERATIONS_REACHED` |
| Two consecutive replacement evaluations improve neither the best valid score nor the development pass count | `STOPPED_NO_PROGRESS` |

When the cap and stagnation coincide, report `MAX_ITERATIONS_REACHED` and note stagnation. Review has no development metric: stagnation depends only on score. Otherwise continue only within the remaining iteration limit. Report the actual termination condition; do not report the cap when execution stopped for another reason.

## Methodology sources

Preserve `wollfoo/codex-cli` prompt-engineering for evaluation and `nidhinjs/prompt-master` for refinement. They inform their assigned roles without controlling this loop. Read [references/external-skill-methodologies.md](references/external-skill-methodologies.md) for source alignment and the supplementary fixed-criteria, clarification, isolated-check, and baseline/holdout methods.

Keep source names, URLs, assignments, and orchestration metadata in reports/internal state, outside the final refined prompt unless explicitly requested. Supplementary methods are self-contained guidance here, not dependencies requiring installation or invocation of other skills.

## Output

Apply [references/report-templates.md](references/report-templates.md). Full includes iteration reports, final/best prompt, dimension scores, issue history, regression and source checks, recommendations, termination reason, and available test evidence. Compact retains mode, rubric score, test status, best versions, remaining/blocking issues, source preservation, regressions, and termination reason without raw case outputs. Even a pass reports non-blocking findings and limitations.
