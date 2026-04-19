---
name: agent-instructions
description: Create or update agent-instruction files such as AGENTS.md, repository policies, or other local guidance that tells coding agents how to behave, validate changes, format code, and justify exceptions.
---

# Agent Instructions

Use this skill when the task is to create or revise instructions for agents. Target files include `AGENTS.md`, repo-local policy documents, and similar guidance files that define agent behavior.

## Workflow

1. Inspect the repository before writing instructions. Prefer rules that match the actual build, test, lint, and formatting tooling already present.
2. Write instructions as explicit actions and constraints, not general advice. Favor short rules that an agent can execute without interpretation.
3. Keep the instruction set minimal. Include only repository-specific expectations or rules that are easy for agents to violate without local guidance.
4. If the repository has multiple toolchains, state how to choose between them instead of listing every possible command.

## Required Build And Format Rules

- If Gradle is present, instruct agents to validate changes with `./gradlew build`.
- If `ktlint` is present, instruct agents to format Kotlin code with its Gradle format task, typically `./gradlew ktlintFormat`, before finishing work.
- When both rules apply, formatting should happen before the final validation run.
- Do not invent alternate validation or formatting commands when the repository already exposes these tools.

## Warning Suppressions

- Treat warning suppressions as a last resort after attempting a real fix.
- Every suppression must include a justification near the suppression site, such as a short code comment that explains why it is necessary.
- Do not permit blanket or unexplained suppressions in agent instructions.

## Writing Guidance

- Prefer imperative wording: `Run`, `Use`, `Avoid`, `Require`.
- Make escalation rules concrete. State what agents must do when validation fails, formatting changes files, or a suppression seems necessary.
- If a rule is conditional, say exactly what to detect, for example "if `gradlew` exists" or "if the Gradle build defines a `ktlintFormat` task".
- Avoid duplicating generic coding advice that agents already know unless the repository needs stricter behavior.

## Done Criteria

An agent-instruction update is complete when:

- the instructions match the repository's actual tooling,
- validation and formatting commands are explicit,
- suppression policy is documented with the justification requirement,
- and the resulting file is concise enough to be followed reliably.
