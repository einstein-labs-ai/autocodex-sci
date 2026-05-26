---
name: auto-debug
description: Autonomously reproduce, debug, fix, validate, and prepare handoff for code failures. Use when the user invokes /auto-debug, asks to automatically debug and fix code, wants root-cause analysis with regression protection, or asks Codex to create a GitHub PR when working on a non-local Git-backed repository.
---

# Auto Debug

Use this skill to run a disciplined debug-and-fix loop through validation and handoff.

## Slash Alias

Treat `/auto-debug` as an explicit request to use this skill.

## Workflow

1. Frame the failure:
   - command, workflow, or symptom
   - affected files, modules, users, or systems
   - local, CI, staging, production, or unknown environment
2. Reproduce the issue with the smallest reliable command or workflow.
3. Capture the exact failure signature, relevant logs, inputs, config, environment assumptions, and affected scope.
4. Inspect the code path and form a falsifiable root-cause hypothesis.
5. Prove or disprove the hypothesis with targeted checks.
6. Implement the smallest correct fix. Preserve unrelated user changes.
7. Add or update regression protection when practical.
8. Run targeted validation, then broader validation proportional to the blast radius.
9. Document:
   - root cause
   - fix
   - validation evidence
   - residual risk
   - release and rollback notes when production-facing

## GitHub PR Behavior

If the worktree is Git-backed and a remote is configured:

1. Confirm the current branch, remote, uncommitted changes, and whether unrelated user edits are present.
2. Create a focused branch only when needed and safe.
3. Commit only the coherent debug fix and its tests or docs.
4. Push and open a pull request when GitHub credentials and network access are available.
5. If access, network, or permissions are missing, provide the exact commands and PR body instead of pretending the PR was created.

If the directory is not a Git repository, skip PR creation and state that the PR path is unavailable.

## Safety Rules

- Do not disable failing checks to force success.
- Do not delete assertions unless they are proven wrong and replaced with correct coverage.
- Do not run destructive Git commands without explicit user approval.
- Never log or expose secrets while collecting evidence.
