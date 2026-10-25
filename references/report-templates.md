# Report Templates

Execution mode (`Review`/`Tested`) and output format (`Full`/`Compact`) are separate. Full is the default; Compact is used only when requested. Keep method names, source URLs, role assignments, and orchestration metadata outside Final Prompt unless the user explicitly asks for them.

## Common fields

Include these in both formats and every final result:

- Status: PASSED, MAX_ITERATIONS_REACHED, STOPPED_NO_PROGRESS, TEST_FAILED, or VALIDATION_UNAVAILABLE.
- Execution Mode and Output Format.
- Instruction Score, configured threshold, and rubric-gate result. Keep test results separate from this score.
- Test Status: NOT_RUN (Review), PARTIAL (required cases are not yet all evaluated), PASSED (all six pass independently of the rubric), FAILED (observed case failure), or UNAVAILABLE (required execution/grading cannot be obtained). Overall Status and best_tested_prompt still require both rubric and task gates.
- Evaluator Isolation and actual runner/grader isolation for Tested; disclose SAME_CONTEXT where used in Review.
- Input Contract: objective, known/unknown target, output, hard constraints, and fixed success criteria; concise in Compact without dropping material requirements.
- Evaluations/iterations completed; initial prompt counts as iteration 1.
- Best instruction version: best_prompt, best_score, best_iteration, and why selected; use null and an explicit no-valid-version explanation when no source/intent/constraint-preserving version exists.
- Best tested version: best_tested_prompt, best_tested_score, best_tested_iteration, or explicitly none; never imply that instruction fallback passed tests.
- Remaining issues, regressions, source-preservation summary, decision, and actual reason for stopping.

Review uses NOT_RUN and a null best tested version. A passing Review report states “Lolos evaluasi instruksi; belum diuji melalui eksekusi.” Translate faithfully when the requested reporting language differs. Same-context evaluation is procedural separation, not fresh-context independence.

Tested additionally includes agreed versus actual target/configuration, baseline/final comparable pass counts, whether observed task performance improved/equaled/regressed, hard-constraint violations, any unavailable evidence, and final holdout outcome. Holdout results become visible only at termination. A high instruction score never cancels a task failure. Always disclose that one run per case does not establish repeatability or performance on other models.

## Full iteration

```text
# Prompt Refinement — Iteration N
## Execution Mode, Output Format, Isolation, and Input Contract
## 1. Prompt Under Evaluation
## 2. Overall Instruction Score and Rubric Gate
## 3. Dimension Scores
## 4. Previous Issue Implementation Report
## 5. Current Issue Findings
## 6. Critical Issues
## 7. Regression and Best-Version Check
## 8. Source Skill Preservation Check
## 9. Recommendations
## 10. Test Status and Development Evidence (Tested only)
## 11. Decision and Stop/Continue Reason
```

Issue findings table: `Issue ID | Issue Finding | Priority | Evidence | Reason`.
From iteration 2: `Issue ID | Previous Issue Finding | Previous Priority | Implementation Status | Verification Result`.
Implementation status: Not Implemented, Partially Implemented, Implemented. Verification result: Still unresolved, Partially resolved, Verified resolved, Regressed, Reopened. Keep the canonical issue lifecycle in internal state; a refiner implementation claim alone does not resolve an issue.

In Tested, Full includes available actual development outputs, criterion results, tool events when relevant, and failures. Do not expose holdout inputs, outputs, or diagnostics in ongoing iteration reports; include them only after the run terminates. Review includes NOT_RUN, not invented case evidence.

## Full final result

```text
# Prompt Refinement Result
## Status, Execution Mode, Output Format, and Validation Level
## Input Contract and Target/Isolation Disclosure
## Final Instruction Score and Rubric Gate
## Iterations
## Best Instruction Version
## Best Tested Version (or None)
## Final/Best Prompt
## Final Dimension Scores
## Previous Issue Implementation Report and Issue History
## Test Status, Baseline Comparison, and Case Evidence
## Remaining Issues and Regressions
## Source Skill Preservation Summary
## Recommendations and Validation Limitations
## Reason for Stopping
```

Label the exact version returned in Final/Best Prompt and associate its scores and test evidence with that version. If the last evaluation differs from the returned best version, report both clearly rather than attributing the last score to the returned text. Full includes raw outputs for all executed cases in the terminal evidence section, including baseline and regressions, plus per-criterion results and failure reasons. Do not fabricate absent evidence.

## Compact final result

Include all common fields, the final/best prompt, concise blocking/non-blocking findings, best-version rationale, target/isolation and validation limitations, source preservation, regressions, and stopping reason. In Tested include baseline/final pass counts for the same slice, hard-constraint failures, unavailable evidence, and holdout outcome. Omit raw case outputs, detailed traces, and repeated iteration prompts; do not omit the existence or consequence of any failure.

## Termination reporting

- PASSED: state which mode-specific gate passed. Review explicitly discloses no execution testing; Tested identifies the exact version passing the rubric and all six cases.
- MAX_ITERATIONS_REACHED: return best valid instruction version and remaining blockers; state the cap was reached before the mode-specific gate passed.
- STOPPED_NO_PROGRESS: give the two consecutive non-improving replacement evaluations and applicable metrics; do not claim the iteration cap was reached.
- TEST_FAILED: identify the selected version's failed holdout and constraints; state it was not used for another revision in this run.
- VALIDATION_UNAVAILABLE: name the missing target/tool/isolation/evidence capability; report any completed instruction review and partial task results separately without claiming Tested passed.

Never omit unresolved Critical findings, regressions, changed source assignments, unvalidated fallback, failed holdout, or unavailable checks. A successful run still reports non-blocking findings and recommendations.
