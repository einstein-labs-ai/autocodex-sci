<p align="center"><strong>Codex CLI</strong> is a coding agent from OpenAI that runs locally on your computer.
<p align="center">
  <img src="https://github.com/openai/codex/blob/main/.github/codex-cli-splash.png" alt="Codex CLI splash" width="80%" />
</p>
</br>
If you want Codex in your code editor (VS Code, Cursor, Windsurf), <a href="https://developers.openai.com/codex/ide">install in your IDE.</a>
</br>If you want the desktop app experience, run <code>codex app</code> or visit <a href="https://chatgpt.com/codex?app-landing-page=true">the Codex App page</a>.
</br>If you are looking for the <em>cloud-based agent</em> from OpenAI, <strong>Codex Web</strong>, go to <a href="https://chatgpt.com/codex">chatgpt.com/codex</a>.</p>

---

## Quickstart
This is fork from Codex, this is an autonomous science SDLC(Software Development Life Cycle), Lightweight coding agent that runs in your terminal.

### Installing and running Codex CLI

Run the following on Mac or Linux to install Codex CLI:

```shell
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Run the following on Windows to install Codex CLI:

```
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

Codex CLI can also be installed via the following package managers:

```shell
# Install using npm
npm install -g @openai/codex
```

```shell
# Install using Homebrew
brew install --cask codex
```

### How to install Agent and automatic skills
- Download AGENTS.override.md file in this respository
- Download autocodex-sci/.codex/skills/, 
-- path: auto-debug , auto-sdlc, automatic folders


----
Then simply run `codex` to get started.

<details>
<summary>You can also go to the <a href="https://github.com/openai/codex/releases/latest">latest GitHub Release</a> and download the appropriate binary for your platform.</summary>

Each GitHub Release contains many executables, but in practice, you likely want one of these:

- macOS
  - Apple Silicon/arm64: `codex-aarch64-apple-darwin.tar.gz`
  - x86_64 (older Mac hardware): `codex-x86_64-apple-darwin.tar.gz`
- Linux
  - x86_64: `codex-x86_64-unknown-linux-musl.tar.gz`
  - arm64: `codex-aarch64-unknown-linux-musl.tar.gz`

Each archive contains a single entry with the platform baked into the name (e.g., `codex-x86_64-unknown-linux-musl`), so you likely want to rename it to `codex` after extracting it.

</details>

### Using Codex with your ChatGPT plan

Run `codex` and select **Sign in with ChatGPT**. We recommend signing into your ChatGPT account to use Codex as part of your Plus, Pro, Business, Edu, or Enterprise plan. [Learn more about what's included in your ChatGPT plan](https://help.openai.com/en/articles/11369540-codex-in-chatgpt).

You can also use Codex with an API key, but this requires [additional setup](https://developers.openai.com/codex/auth#sign-in-with-an-api-key).

## Docs

- [**Codex Documentation**](https://developers.openai.com/codex)
- [**Contributing**](./docs/contributing.md)
- [**Installing & building**](./docs/install.md)
- [**Open source fund**](./docs/open-source-fund.md)

## For any contributions
Please perform pull request

## Improvements
From the provided `AGENTS.override.md` instructions, the override should improve Codex behavior for production-style work by forcing clearer intake, requirements, security review, validation, rollback thinking, and documentation.

---
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


---
This repository is licensed under the [Apache-2.0 License](LICENSE).
---
## Open source
### Thanks 
OpenAI
