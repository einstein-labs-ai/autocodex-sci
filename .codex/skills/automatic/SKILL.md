---
name: automatic
description: Suggest safe automation opportunities for the current repository or workflow. Use when the user invokes /automatic, asks for automation suggestions, wants recurring Codex automations, or wants SKILLS.md or AGENTS.md based recommendations for what should be automated next without immediately making broad code changes.
---

# Automatic

Use this skill to turn a broad automation request into concrete, low-risk automation suggestions.

## Slash Alias

Treat `/automatic` as an explicit request to use this skill.

## Workflow

1. Inspect the repository structure, existing `AGENTS.md`, `AGENTS.override.md`, skills, automations, delivery docs, tests, CI, scripts, and recent user context when available.
2. Identify repetitive or failure-prone work that can be automated safely:
   - validation commands
   - dependency checks
   - test or lint workflows
   - release readiness checks
   - documentation updates
   - triage, debug, or review routines
3. Separate suggestions into:
   - immediate local improvements
   - scheduled or recurring automations
   - skills or agent-instruction improvements
   - items that need user approval, credentials, production access, or GitHub access
4. For each suggestion, state:
   - trigger
   - action
   - expected benefit
   - validation path
   - security or operational risk
   - rollback or disable path
5. Do not create, enable, schedule, or run privileged automations unless the user has explicitly asked for that specific action and the required access is available.

## Output

Provide a concise prioritized list. Prefer recommendations that are reversible, locally testable, and grounded in files that actually exist.
