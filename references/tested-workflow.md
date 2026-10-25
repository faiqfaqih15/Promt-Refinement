# Tested Workflow

Read only for `mode=Tested`. This file owns case construction, execution evidence, and empirical acceptance; the orchestrator retains decision authority. No live messages, deployment, deletion, or other external mutations are authorized by a prompt test.

## Freeze the test contract

Before revisions, establish exactly six representative cases with criteria derived from the user's objective and constraints, not from candidate answers. Use supplied cases when sufficient; otherwise construct task-relevant examples and ask only when the expected behavior cannot be inferred reliably. Cover normal input, input variation, missing information, instruction conflict, output limits, and a relevant disturbance. For a category that does not apply, document why and use a meaningful task variation instead.

Label four cases `DEVELOPMENT` and two `HOLDOUT` before execution. Holdout should include variations that check transfer rather than duplicate development inputs. Freeze the inputs, assertions, constraints, case grouping, target and execution configuration for the run. Do not lower thresholds, change expectations, or replace a failing case to obtain a pass. A requested contract change ends this run; start a new run with a new baseline.

Each case records:

| Field | Meaning |
|---|---|
| case_id | Unique stable identifier within the run |
| group | DEVELOPMENT or HOLDOUT |
| input | Complete task input and necessary raw fixtures |
| pass_criteria | Observable assertions with expected behavior |
| hard_constraints | Required constraints that must not be violated |
| required_tools | Tools/fixtures needed, or none |

Record agreed target model identity (snapshot when known), prompt placement, decoding settings when exposed, tools, fixtures, and input-loading rules. Unknown settings stay unknown. Compare only like-for-like executions; if an observable configuration changes, invalidate the comparison instead of silently accepting mismatched results.

## Capability, execution, and evidence

Verify a matching target backend and fresh execution contexts are available before revisions. Also require holdout-free evaluator/refiner contexts: the orchestrator may see baseline holdout evidence, but evaluator/refiner contexts must start without its conversation history, holdout inputs, outputs, or diagnostics. Merely changing roles in the orchestrator conversation does not isolate holdout. If that separation is unavailable, terminate VALIDATION_UNAVAILABLE rather than continuing a contaminated Tested run. Use an available subagent/backend only when it genuinely runs the agreed target with the same prompt placement and configuration; another model or a different instruction layer is not an equivalent test. If a required capability cannot be obtained, preserve any evidence and terminate `VALIDATION_UNAVAILABLE` with the specific limitation. Do not infer model output from reading the prompt or treat simulated outcomes as real executions.

Run each case in a fresh context for each prompt version, containing only that version, the case input, the agreed tools/configuration, and necessary platform instructions. Never pass earlier conversations, scores, findings, expected answers, or refinement rationale to runners. One invocation per case/version is the default. Do not retry unfavorable outputs to select a better sample. Infrastructure failure produces unavailable evidence, not an assumed pass or a zero-valued output score.

Use fixture data and mocked tools for actions that would send messages, deploy, delete, or modify external systems. State this testing scope; mocked execution does not demonstrate real integration success. A case requiring real side effects beyond existing authorization is unavailable until an authorized isolated environment exists.

For each run store `prompt_version`, `case_id`, `actual_model`, relevant available settings, raw `output`, actual tool events when relevant, per-criterion results, hard-constraint results, case result, and failure reason. Store artifacts under the task's scratch workspace, outside the installed skill package. Version IDs must identify exact prompt text; never combine development results from one version with holdout results from another.

Grade using fresh-context graders independent of the refiner, receiving the case raw input/fixtures, actual output/tool evidence, and frozen criteria, without earlier scores, improvement claims, or the refiner's reasoning. Mechanically check objective constraints when possible; graders handle remaining semantic assertions. Keep execution failure distinct from task failure. A case passes only when every pass criterion is met and every hard constraint is satisfied. An ungradable required criterion is unavailable evidence, not a pass.

## Baseline and candidates

Run the initial version once on all six cases. Keep holdout inputs, outputs, and failure diagnostics private with the orchestrator. The evaluator and refiner may use only the four development cases and their findings during refinement. The baseline is iteration 1, not a replacement.

Run each replacement once on the same four development cases. Preserve every output and score, including regressions. Link task failures to stable evaluator issue IDs; a task failure affecting the objective can remain Critical despite a high instruction score. Refine the root cause without adding unrelated requirements.

At a passing rubric/development checkpoint, select the highest-scoring valid candidate with all four development cases passing; ties choose the earlier version. Run that candidate on the two frozen holdout cases once. If it is exactly the initial version, reuse its existing evidence rather than rerunning cases. Terminate `TEST_FAILED` if either holdout fails; reveal diagnostics only in the terminal report, not for another revision in this run. A new run must use new unseen holdout cases.

`PASSED` in Tested requires the rubric gate plus six of six cases passing for the same exact version. Test Status describes execution evidence separately from the rubric: use `PASSED` when all six cases pass even if the rubric still fails, `PARTIAL` while required cases remain unevaluated, `FAILED` for observed case failure, and `UNAVAILABLE` for missing required execution/grading capability. An execution-only Test Status of PASSED does not imply overall Status PASSED or populate best_tested_prompt when the rubric fails. Report baseline and candidate pass counts for the comparable slice and note whether behavior improved, regressed, or was equal. If baseline already passes all six and the final candidate also passes, report equal observed task performance; an increased rubric score alone is not proof of improved execution.

One sample per case cannot establish run-to-run consistency, statistical significance, or performance on other models or untested inputs. The gate is a bounded six-case result, not a universal reliability guarantee.
