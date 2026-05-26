---
name: auto-sdlc
description: Drive a structured software delivery lifecycle using AGENTS.override.md as the operating policy. Use when the user invokes /auto-SDLC or /auto-sdlc, asks for architecture and requirements before software work, or wants Codex to turn a software idea into requirements, architecture, implementation plan, validation, release, rollback, and production verification steps.
---

# Auto SDLC

Use this skill to start software work with requirements and architecture before implementation.

## Slash Alias

Treat `/auto-SDLC` and `/auto-sdlc` as explicit requests to use this skill.

## Required Policy Source

Read `AGENTS.override.md` from the current repository before producing the SDLC output. If a nearer `AGENTS.md` or `AGENTS.override.md` exists in the target project, read that too and reconcile the stricter applicable instruction.

## Intake Questions

Ask only the smallest set of questions needed to avoid unsafe or high-rework implementation. Cover:

- business objective and target users
- scope and non-goals
- required architecture or platform constraints
- data model and integration expectations
- security, privacy, and permission boundaries
- reliability, performance, and observability expectations
- deployment target, release strategy, and rollback needs
- acceptance criteria and validation evidence

If the request is low-risk and clear enough, proceed with explicit assumptions instead of blocking.

## Workflow

1. Summarize the problem, success criteria, actors, affected systems, constraints, and assumptions.
2. Define testable requirements and acceptance criteria.
3. Produce an architecture or design before coding when APIs, schemas, integrations, trust boundaries, or operations are affected.
4. Plan implementation in small reversible increments.
5. Map requirements to validation commands or evidence.
6. Define security checks, observability, release, rollback, and post-change verification.
7. Create or update lightweight `delivery/` artifacts when the work is multi-step, risky, cross-cutting, production-facing, or useful for handoff.

## Output

Return the SDLC package or write the relevant `delivery/` artifacts, depending on task size. Do not claim implementation, deployment, or validation happened unless it actually ran.
