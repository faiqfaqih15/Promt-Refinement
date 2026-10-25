# Prompt Refinement Loop — Upgrade Evidence

## Current outcome

The Review/Tested workflow upgrade is applied under CHG-001 R1 and CHG-002 R2. Structural validation passed. CHG-003 R1 collected twelve original skill-invocation reports, independent per-report grades, and contract simulations. Empirical execution validation remains unavailable; the whole upgrade does not meet the original completion condition yet.

## Approved change ledger

| Unit | Scope | State |
|---|---|---|
| CHG-001 R1 | Review default, intake contract, instruction-only pass claim | VALIDATED for applied scope |
| CHG-002 R2 | Tested reference, execution gate, version tracking, reporting and source attribution | VALIDATED for applied structural/static scope; runtime behavior not proved |
| CHG-003 R1 | Workspace benchmark evidence; no active skill edits | Evidence collected; unavailable empirical validation recorded |
| CHG-003 R2 | This implementation report only | PROPOSED — NOT YET APPROVED |

This candidate is not evidence that R2 has been approved or applied. Application and document-validation receipts must record its subsequent state separately.

## Implemented behavior and preserved boundaries

- Review is the default. Passing Review reports instruction evaluation without claiming execution validation.
- Tested is explicit. It freezes six cases, compares the baseline on all six, candidates on four development cases, and the selected final candidate on two holdout cases.
- Tested success requires the rubric gate and all six cases without mandatory-constraint violations. Capability failures stop as VALIDATION_UNAVAILABLE; holdout failure stops as TEST_FAILED without tuning on that holdout.
- Best instruction and best tested versions are tracked separately. Five evaluations remain the cap; two consecutive revisions without score or development-case improvement stop for no progress.
- Full and Compact retain essential scores, version, test status, remaining issues, isolation limitations, and stopping reason.
- The skill name, purpose, stable issue IDs/history, original evaluator/refiner roles, strict score > 9.0, critical floor 8.5, and no-open-Critical requirement remain.
- The original methodology sources remain wollfoo/codex-cli prompt-engineering for evaluation and nidhinjs/prompt-master for refinement. Internal references adapt constraint-driven-development, doubt-driven-development, interview-me, and Sentry prompt-optimizer within supporting roles. No additional skill was installed.
- UI metadata, other skills, global configuration, and Python installation were not changed by this upgrade.

## Validation evidence

The official quick_validate.py returned "Skill is valid!" for the immutable old package, new snapshot, and active package. Local references and UI YAML mapping passed. PyYAML 6.0.3 was supplied in an isolated workspace directory; no autofix or global Python modification was used. Active-package hashes matched the CHG-002 R2 state before and after the evidence run.

Each request/version was dispatched once with fork_turns="none" and no model or reasoning override. Interrupted B04 old/new and B05 old invocations were resumed rather than replaced. The run spans an interruption; hidden backend settings and exact model identity are not exposed, so identical backend snapshots cannot be proved.

The following results grade reporting and preservation, not empirical task success:

| Request | Old: report / task execution | New: report / task execution |
|---|---|---|
| B01 | PASS / NOT_REQUESTED | PASS / NOT_REQUESTED |
| B02 | PASS / NOT_REQUESTED | PASS / NOT_REQUESTED |
| B03 | PASS / NOT_REQUESTED | PASS / NOT_REQUESTED |
| B04 | PASS / UNAVAILABLE | PASS / UNAVAILABLE |
| B05 | PASS / UNAVAILABLE | PASS / UNAVAILABLE |
| B06 | PASS / UNAVAILABLE | PASS / UNAVAILABLE |

The separate contract simulation matched 12/12 frozen fixture decisions across seven contract categories. It covers strict thresholds, test constraints, holdout failure, unavailable capabilities, best-version retention, stopping precedence, and essential report fields. Simulation does not establish runtime behavior on actual model outputs.

## Findings and limitations

- No observed reporting/preservation defect was established by the independent graders.

- Tested B04 and B06 could not obtain the required authenticated actual-model identity and execution isolation evidence. B05 deliberately targets an unavailable fixture model. No target task outputs were fabricated or substituted; zero validated Tested task executions are available.
- Review runs use same-context evaluator/refiner checks; independent graders reviewed their reports afterward. This does not turn Review into an executed prompt test.
- B05-new states inherited parent history, while root dispatch evidence records fork_turns="none". The original report is preserved and the contradiction is documented in dispatch-context-note.md. Requested dispatch configuration does not independently prove hidden runtime context details.
- Original self-reported rubric scores are not calibrated task-performance measurements. Missing Tested support in the old package is a feature gap; unavailable checks are not passes. No empirical performance improvement, equivalence, or repeatability is established.
- One attempt per request/version does not prove consistency across attempts or models. Actual holdout-failure and successful-Tested runtime paths remain unverified.

## Evidence locations and remaining work

Workspace: C:\Users\Formulatrix\Documents\Codex\2026-10-03\apa\outputs\prompt-refinement-upgrade\CHG-003-R1-results.

Use run-manifest.json for configuration, dispatches and hashes; B01-old.md through B06-new.md for original reports; grading.json for independent criteria and evidence; contract-decisions.json and contract-checks.json for simulation; structure.json for validator evidence; comparison.md for the paired assessment.

Completion still requires available agreed target-model identity and execution isolation, successful real Tested validation, resolution of any observed defects, and explicit approval/application/validation of the current report proposal. Further tests need their own frozen-case run specification; do not silently repeat this once-per-case evidence run or relax the target requirement.

## Historical initial implementation report

The report below is retained verbatim as historical evidence. Its missing-yaml statement and 10/10 structural-workflow count describe the initial implementation; they are not current upgrade validation results.

---

# Prompt Refinement Loop — Implementation Report

## Implemented

- Created one reusable `prompt-refinement-loop` skill.
- Added the main trigger and bounded evaluator/refiner/orchestrator workflow.
- Added evaluation rubric, methodology mapping, refinement rules, report templates, and UI metadata.
- Enforced strict `Overall Score > 9.0`, critical-dimension minimum `8.5`, unresolved-critical-issue blocking, best-version tracking, stable issue IDs, regression detection, and `MAX_ITERATIONS=5`.

## Source sections applied

- Purpose and required loop behavior
- External methodology mapping and source preservation
- Evaluator, refiner, and orchestrator responsibilities
- Input defaults and missing-input handling
- Iteration state and issue lifecycle
- Evaluation dimensions, priorities, scoring, and acceptance gate
- Refinement, traceability, regression, and best-version rules
- Full, compact, successful, and maximum-iteration output contracts
- Required behavior checklist

## Methodologies integrated

| Source | Integration |
|---|---|
| `wollfoo/codex-cli` `prompt-engineering` | Evaluation methodology |
| `nidhinjs/prompt-master` | Refinement methodology |

The sources inform their assigned roles only. The new skill retains orchestration, state, verification, scoring, and termination authority.

## Preserved

- Original objective and scope
- Required source URLs and role assignments
- Evaluator/refiner/orchestrator separation
- Stable issue IDs and resolved-issue history
- Strict acceptance and termination rules
- Best-scoring valid prompt fallback

## Unresolved

- The official validator could not run because the bundled Python runtime lacks the `yaml` package.
- Step 8 used structural workflow checks rather than a live external model execution.

## Tests performed

Structural checks passed for complete, vague, conflicting, unsafe/impossible, and long prompts, plus threshold, critical-floor, bounded-loop, best-version, and report-contract checks. Result: 10/10 checks passed.

## Step 10 decision

No split is needed. The main `SKILL.md` is compact, while the four references isolate rubric, methodology, refinement, and reporting concerns. Evaluator, Refiner, and Orchestrator remain logically separate without creating three independently triggered skills. Revisit the split only if one role becomes independently reusable or the entrypoint becomes difficult to maintain.
