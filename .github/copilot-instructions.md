# Copilot Instructions for This Repository

This repository is a skill-system repository.

When working in this repo, treat it as a structured product rather than a loose collection of prompt files.

## Core Expectations

- Preserve repository structure and naming conventions.
- Prefer extending existing patterns over inventing one-off formats.
- Keep skills, docs, hooks, tests, and platform integration artifacts aligned.
- Do not present placeholder content as complete.
- When adding new structure, be explicit about what is active now vs reserved for later.

## Skill Authoring

When writing or revising an individual skill:
- follow the repository’s existing skill-writing conventions
- maintain clear "when to use" guidance
- preserve strong workflow and quality bar language
- keep the skill actionable, not abstract
- ensure the skill’s outputs and behavior are testable

If the task is about writing or improving a single skill, prefer the repository’s skill-authoring guidance and existing meta-skills such as `writing-skills`.

## Skill-System Design

If the task is about:
- designing a new skill repository
- restructuring an existing skill repository
- defining multi-skill workflows
- planning repository architecture across skills, hooks, docs, tests, agents, and platform adapters

then use the `building-skill-systems` skill as the primary reference.

Key expectations from `building-skill-systems`:
- use progressive questioning rather than demanding a full specification up front
- clarify before generating architecture
- distinguish new-repository vs refactor mode
- produce structured outputs rather than loose advice
- include workflow, platform integration, testing, and placeholder planning
- label placeholders honestly

## Testing Expectations

When adding or changing skills:
- consider whether new prompt fixtures are needed
- consider whether expected-behavior docs should be updated
- preserve existing test organization where possible
- prefer behavior contracts over brittle exact-wording assertions

## Documentation Expectations

When adding a meaningful new skill or repository-level capability:
- update example docs or reference docs if appropriate
- keep examples realistic and architecture-relevant
- ensure docs explain not just what exists, but how it should be used

## Placeholder Discipline

Placeholders are acceptable when they preserve future structure.

Ambiguity is not acceptable.

Good:
- explicitly marked placeholder directories
- starter drafts labeled as drafts
- postponed platform integrations called out clearly

Bad:
- implied completeness
- vague "add later" notes with no structural decision
- invented domain detail presented as confirmed

## Final Rule

This repository values:
1. structure
2. workflow clarity
3. truthful staging
4. testable behavior
5. maintainable evolution

Do not optimize for short-term file generation at the cost of long-term repository coherence.