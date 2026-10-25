# Refinement Rules

Preserve the original objective, explicit requirements, valid constraints, target tool/model, required output, acceptance criteria, correct existing text, source references, and evaluator/refiner/orchestrator separation.

Preserve source references internally for methodology and audit purposes. Do not treat internal source names, URLs, role assignments, or orchestration metadata as content that belongs in the user's final prompt unless explicitly requested.

Improve only from evaluator findings: clarity, specificity, structure, completeness, consistency, actionability, output definition, robustness, maintainability, efficiency, testability, and source integration clarity. Make the smallest effective change. Do not invent requirements, change scope, add unrelated features, or treat rewritten text as automatically better.

## Preserve / improve / avoid

**Preserve:** user intent, explicit constraints, required technologies or formats, valid acceptance criteria, target alignment, and all required source references and role assignments.

**Improve:** remove ambiguity, expose missing inputs, separate instructions from data, define outputs and edge cases, resolve contradictions, improve testability, and reduce duplication only when meaning is unchanged.

**Avoid:** speculative requirements, silent intent changes, removal of valid constraints, unrelated features, needless verbosity, target substitution, role reassignment, and reintroducing resolved issues.

For every material change, record:

| Issue ID | Change made | Prompt section | Expected effect |
|---|---|---|---|
| I-001 | ... | ... | ... |

After each revision, compare scores and prior improvements. A lower score is a regression: identify its cause, preserve the best prior prompt, and target the lost improvement. A version that removes required source references is never a valid best version; report the source-preservation failure even when no valid alternative exists.

## Source-skill change log

If a source reference or responsibility changes, record it separately:

| Source skill | Previous role | New role | Reason | Explicitly requested? | Impact |
|---|---|---|---|---|---|
| ... | ... | ... | ... | Yes/No | ... |

Reject unrequested removal or reassignment of a required source. If a material intent change is unavoidable, stop and request user direction.

## Best-version rule

Maintain `best_prompt`, `best_score`, and `best_iteration`. Replace the best version only when the current version scores higher and preserves required source references. At the iteration limit, return the best valid version, not automatically the last version, together with remaining issues and the reason the gate was not met.

## Separate instruction quality from tested quality

Keep existing best_prompt, best_score, and best_iteration for valid instruction versions. Validity requires preserving intent, hard constraints, required references, and assignments; a high score does not rescue an invalid version. On ties retain the earlier version. Keep these fields even when the rubric gate has not yet passed. If no version preserves intent, hard constraints, and required sources, initialize/leave best_prompt, best_score, and best_iteration as null; return the original prompt only as an explicitly unresolved, unvalidated artifact rather than a valid best version.

Maintain best_tested_prompt, best_tested_score, and best_tested_iteration separately; initialize them to null for this run. Set them only for an exact version that passes the rubric and all six frozen cases without hard-constraint violations. Four passing development cases are insufficient. At any unsuccessful termination, return best_prompt as an instruction-only fallback and separately disclose whether a fully tested version exists. Do not imply that choosing the instruction fallback proves its task performance.

In Tested, holdout selection uses the highest rubric score among valid versions that pass both the rubric and all four development cases, not automatically best_prompt if it failed development. Use that selected version's own development and holdout evidence. Review never populates best_tested_prompt.

## Progress and bounded termination

The initial prompt is evaluation 1 and counts toward MAX_ITERATIONS; at the default cap of 5, permit at most four replacement evaluations. Track the best valid score so far and, in Tested, the highest development case pass count among valid versions (0 through 4), initialized from the baseline's development result only if that version is valid. If no valid version exists, the corresponding best metrics are null; the first valid evaluated version establishes them and counts as new progress.

For each replacement, compare its score and development pass count to the pre-replacement best values. Reset no_progress_count to zero when either metric strictly increases for a valid version; otherwise increment it. Review compares only the best valid score. Ties, invalid candidates, score increases below an earlier best, and oscillations that recover only an already observed result do not constitute new progress.

At no_progress_count=2, stop STOPPED_NO_PROGRESS unless the gate has passed or the cap has already been reached. Apply the entrypoint's termination precedence. A holdout failure ends TEST_FAILED even if more iterations remain; unavailable required execution ends VALIDATION_UNAVAILABLE. Never continue indefinitely, weaken the frozen criteria, reuse exposed holdout cases in another run, or relabel a stopped run as PASSED.
