# External Methodology Sources

These are preserved methodology references, not independent agents or controllers.

These references are internal implementation metadata. Preserve and use them for evaluation, refinement, and source-alignment checks, but do not insert their names, URLs, or role descriptions into the final refined prompt unless the user explicitly requests that disclosure.

| Source | URL | Assigned role | Use |
|---|---|---|---|
| prompt-engineering | https://github.com/wollfoo/codex-cli/tree/main/skills/prompt-engineering | Evaluator methodology | Clarity, specificity, structure, completeness, ambiguity, constraints, data/instruction separation, format gaps, contradictions |
| prompt-master | https://github.com/nidhinjs/prompt-master | Refiner methodology | Target detection, intent extraction, construction, constraint handling, output definition, success alignment, token efficiency |

The orchestrator owns sequencing, state, scoring, issue lifecycle, acceptance, and termination. Do not remove, weaken, generalize away, or reassign these references without explicit user approval. If one changes, log the previous role, new role, reason, whether requested, and impact; normally reject the change when it is not requested.

## Integration boundary

Use the mapping below for every iteration:

```text
prompt-engineering → evaluation methodology
prompt-master      → refinement methodology
prompt-refinement-loop → orchestration, state, verification, gate, and termination
```

The evaluator must not rewrite the prompt. The refiner must not judge passage. Neither source methodology may independently run the loop or replace the orchestrator. The refiner receives the evaluator's issue IDs as its implementation checklist; the next evaluator independently verifies the result.

## Adding a future source

Add a source only when it supplies a concrete reusable method. Record its name, stable URL, assigned responsibility, integration point, and preservation constraints. Do not let a new source duplicate evaluator, refiner, or orchestrator authority. Any change to an existing source assignment requires an explicit source-skill change log and user approval.

## Supplementary methods added by CHG-002

These methods support existing owners; no additional skill installation or invocation is required. Preserve the two required source assignments above. Names, source locators, and adaptation notes remain metadata outside the final refined prompt.

| Source | Stable locator | Supporting method | Integration boundary |
|---|---|---|---|
| constraint-driven-development, installed agent-skills 0.6.12 | `C:/Users/Formulatrix/.codex/plugins/cache/agent-skills/agent-skills/0.6.12/skills/constraint-driven-development/SKILL.md` | Freeze observable success criteria and prevent lowering the bar | Supports intake and acceptance; does not own scoring or install project quality tools |
| doubt-driven-development, installed agent-skills 0.6.12 | `C:/Users/Formulatrix/.codex/plugins/cache/agent-skills/agent-skills/0.6.12/skills/doubt-driven-development/SKILL.md` | Isolate artifacts and criteria for an independent check of assumptions/failure cases | Supports Tested isolation and grading; does not create a second loop or require cross-model review |
| interview-me, installed agent-skills 0.6.12 | `C:/Users/Formulatrix/.codex/plugins/cache/agent-skills/agent-skills/0.6.12/skills/interview-me/SKILL.md` | Read available facts before focused clarification of material ambiguity | Supports the input contract; does not require a full interview or extra confirmation for clear tasks |
| getsentry/skills prompt-optimizer | https://github.com/getsentry/skills/blob/main/skills/prompt-optimizer/SKILL.md and https://github.com/getsentry/skills/blob/main/skills/prompt-optimizer/references/meta-optimization-loop.md | Same-case baseline comparison, recorded actual outputs, hidden holdout, plateau detection | Supports Tested evidence and bounded stopping; keep one replacement per iteration rather than its multi-candidate search |

Source-assignment change log: the original evaluator and refiner assignments are unchanged. The user-requested supplementary sources add support methods only. Local source paths identify the inspected provenance; the workflow itself is written into this package and continues to work if those source skills are absent.
