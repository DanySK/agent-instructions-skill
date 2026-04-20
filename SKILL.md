---
name: agent-instructions
description: Create or update agent-instruction files such as AGENTS.md, repository policies, or other local guidance that tells coding agents how to behave, validate changes, format code, and justify exceptions.
---

# Agent Instructions

Use this skill when the task is to create or revise instructions for agents. Target files include `AGENTS.md`, repo-local policy documents, and similar guidance files that define agent behavior.

## Workflow

1. Inspect the repository before writing instructions. Prefer rules that match the actual build, test, lint, and formatting tooling already present.
2. If the task says to update something, interpret that as a complete upgrade workflow: update the version, verify the new version in the repository, adapt the codebase to the new version if necessary, and continue until verification succeeds.
3. Write instructions as explicit actions and constraints, not general advice. Favor short rules that an agent can execute without interpretation.
4. Keep the instruction set minimal. Include only repository-specific expectations or rules that are easy for agents to violate without local guidance.
5. If the repository has multiple toolchains, state how to choose between them instead of listing every possible command.
6. When the user asks to "document how to code", prefer concrete engineering rules: how to preserve structure, when to format, how to validate, how to handle suppressions, and how to describe changes in commits.

## Required Build And Format Rules

- If Gradle is present, instruct agents to validate changes with `./gradlew build`.
- If `ktlint` is present, instruct agents to format Kotlin code with its Gradle format task, typically `./gradlew ktlintFormat`, before finishing work.
- When both rules apply, formatting should happen before the final validation run.
- Do not invent alternate validation or formatting commands when the repository already exposes these tools.
- For dependency or tool upgrades, require agents to keep fixing integration issues until the repository validates successfully with the new version.

## Warning Suppressions

- Treat warning suppressions as a last resort after attempting a real fix.
- Every suppression must include a justification near the suppression site, such as a short code comment that explains why it is necessary.
- Do not permit blanket or unexplained suppressions in agent instructions.

## Coding Guidance

- Prefer small, reviewable changes that preserve the repository's existing structure and wording style unless a real refactor is required.
- If the target repository follows a mature engineering style, mirror it in the instructions with concrete rules instead of vague quality language.
- When using Alchemist as a reference, encode its practices as explicit actions: prefer repository-defined automation over ad hoc commands, format before final validation, run the strongest available project-wide verification, and keep responsibilities separated across modules and files.
- If the repository introduces code, build logic, or automation, require agents to use the project's canonical commands rather than substituting familiar alternatives.

## Commit Guidance

- If the user asks for conventional commits, document the exact header format as `type(scope): summary`.
- Document breaking changes explicitly as `type(scope)!: summary` plus a `BREAKING CHANGE:` footer.
- If the user references `semantic-release-preconfigured-conventional-commits`, document this release-relevant mapping: `feat` for minor releases; `fix`, `docs`, `perf`, and `revert` for patch releases; `chore(api-deps)` for minor dependency releases; `chore(core-deps)` for patch dependency releases; `test`, `ci`, `build`, `style`, `refactor`, and other `chore(...)` scopes for non-release maintenance work unless the repository says otherwise.
- Tell agents to avoid vague commit summaries and to keep the subject concise, imperative, and specific.

## Writing Guidance

- Prefer imperative wording: `Run`, `Use`, `Avoid`, `Require`.
- Make escalation rules concrete. State what agents must do when validation fails, formatting changes files, or a suppression seems necessary.
- Define `update` precisely when relevant: it means version change, compatibility fixes, and successful verification, not just editing a version string.
- If a rule is conditional, say exactly what to detect, for example "if `gradlew` exists" or "if the Gradle build defines a `ktlintFormat` task".
- Avoid duplicating generic coding advice that agents already know unless the repository needs stricter behavior.

## Done Criteria

An agent-instruction update is complete when:

- the instructions match the repository's actual tooling,
- validation and formatting commands are explicit,
- update semantics require compatibility work through a passing verification,
- suppression policy is documented with the justification requirement,
- and the resulting file is concise enough to be followed reliably.
