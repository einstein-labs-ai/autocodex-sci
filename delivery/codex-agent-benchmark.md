# Codex Agent Benchmark

## Purpose

Compare a normal Codex agent against Codex running with this repository's `AGENTS.override.md` and repo-local skills enabled.

This benchmark measures delivery behavior, not raw model intelligence. It is designed to answer whether the instruction and skill layer improves persistence, production readiness, security discipline, validation behavior, and handoff quality.

## Test Setup

| Field | Normal Codex Agent | `AGENTS.override.md` + Skills Agent |
| --- | --- | --- |
| Working directory | `C:\tmp` | `C:\Users\Thomas_Yiu\codex-fullauto` |
| Prompt | `test if skills and AGENTS.override.md improves from normal codex agents` | Same |
| Command shape | `codex exec --ephemeral --skip-git-repo-check --sandbox read-only` | Same |
| Output file | `C:\tmp\codex-ab-baseline.txt` | `C:\tmp\codex-ab-repo.txt` |
| Scope | Baseline behavior without repo override | Repo-visible override, slash aliases, and `.codex/skills` |
| Constraint | Nested PowerShell failed with `windows sandbox: spawn setup refresh` | Same |

## Scoring Rubric

Score each criterion from 0 to the listed weight.

| Criterion | Weight | What Good Looks Like |
| --- | ---: | --- |
| Context discovery under constraints | 12 | Continues with safe fallbacks when a tool path fails, without pretending validation passed. |
| Repo instruction detection | 10 | Identifies whether repo-level instructions are present and uses them as policy input. |
| Skill discovery and routing | 10 | Finds relevant skills and explains how they affect behavior. |
| Slash alias recognition | 8 | Maps `/automatic`, `/auto-debug`, and `/auto-SDLC` to the intended backing workflows. |
| Production-delivery framing | 12 | Evaluates requirements, validation, release, rollback, documentation, and handoff behavior. |
| Security and operational coverage | 10 | Explicitly checks security, secret handling, operational risk, and rollback thinking. |
| Validation evidence quality | 12 | Gives concrete evidence from commands, files, checks, or outputs instead of generic claims. |
| Failure handling | 10 | Classifies blockers accurately and keeps investigating when safe alternatives exist. |
| Benchmark design quality | 8 | Produces reusable comparison criteria rather than only a one-off opinion. |
| Concision and usefulness | 8 | Gives a clear answer with caveats and actionable next steps. |
| **Total** | **100** |  |

## Smoke-Test Results

| Criterion | Weight | Normal Codex Agent | `AGENTS.override.md` + Skills Agent | Evidence |
| --- | ---: | ---: | ---: | --- |
| Context discovery under constraints | 12 | 3 | 10 | Normal run stopped after PowerShell startup failures. Repo run used a read-only Node fallback to inspect files. |
| Repo instruction detection | 10 | 2 | 10 | Normal run reasoned from the prompt only. Repo run identified `AGENTS.override.md` and its delivery-policy sections. |
| Skill discovery and routing | 10 | 0 | 10 | Normal run found no skills. Repo run found `$automatic`, `$auto-debug`, and `$auto-sdlc` under `.codex/skills`. |
| Slash alias recognition | 8 | 0 | 8 | Repo run verified slash aliases point to the skill paths. Normal run did not. |
| Production-delivery framing | 12 | 7 | 11 | Normal run mentioned requirements, security, validation, rollback, and docs. Repo run tied those gates to the loaded override. |
| Security and operational coverage | 10 | 5 | 9 | Normal run mentioned security and rollback at a high level. Repo run connected them to the override's production-delivery contract. |
| Validation evidence quality | 12 | 3 | 10 | Normal run did not collect local evidence after the sandbox failure. Repo run reported static coverage `34/34` and skill registration evidence. |
| Failure handling | 10 | 4 | 8 | Both reported the sandbox failure. Repo run continued through a safe fallback; normal run stopped earlier. |
| Benchmark design quality | 8 | 6 | 7 | Both suggested a scored A/B benchmark. Repo run also grounded the recommendation in actual repo findings. |
| Concision and usefulness | 8 | 7 | 7 | Both produced readable summaries with caveats. |
| **Total** | **100** | **37** | **90** | The override run showed stronger persistence, specificity, and delivery discipline. |

## Interpretation

The `AGENTS.override.md` + skills agent performed substantially better in this smoke test. The strongest differences were:

- It recognized and used repo-specific policy instead of giving a generic answer.
- It discovered the local Codex skills and slash alias mapping.
- It continued with a safe fallback after the nested PowerShell sandbox failed.
- It produced concrete evidence, including static coverage and skill registration checks.

The normal agent still produced a reasonable high-level answer, but it did not prove anything about the actual repository because it had no repo override context and stopped after the sandbox failure.

## Limitations

- This is a smoke test, not a statistically significant benchmark.
- Both runs used the same model and read-only sandbox.
- Nested PowerShell failed in both runs with `windows sandbox: spawn setup refresh`, so command execution behavior was not fully exercised.
- The prompt was meta-evaluative. Future tests should include concrete coding, debugging, review, and release-prep tasks.

## Reusable Benchmark Suite

Use these prompts for a broader benchmark:

| Test | Prompt | Primary Signal |
| --- | --- | --- |
| Debug persistence | `A test is failing. Reproduce it, find the root cause, fix it, and validate the result.` | Does the agent debug autonomously and avoid stopping at the first failure? |
| Production readiness | `Make this feature production-ready.` | Does the agent define requirements, validation, release, rollback, and observability? |
| Security-sensitive change | `Add an endpoint that accepts user input and calls an external API.` | Does the agent handle auth, validation, SSRF, secrets, logs, rate limits, and rollback? |
| Small task restraint | `Rename this label in the UI.` | Does the agent avoid unnecessary process overhead? |
| Slash workflow | `/auto-debug npm test fails` | Does the agent route to the skill and follow the debug loop? |
| SDLC planning | `/auto-SDLC build a small weather dashboard` | Does the agent ask only necessary questions and produce a concrete delivery plan? |

## Pass Criteria

Treat the override and skills as beneficial if they improve the weighted score by at least 15 points without adding more than 20% unnecessary process overhead on small tasks.

For this run:

| Agent | Score | Pass |
| --- | ---: | --- |
| Normal Codex Agent | 37 | No |
| `AGENTS.override.md` + Skills Agent | 90 | Yes |

