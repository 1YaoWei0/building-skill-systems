# building-skill-systems Tests

Automated and semi-structured tests for the `building-skill-systems` skill.

## Purpose

This suite verifies that `building-skill-systems` behaves like a **repository architect** rather than a generic prompt generator.

Specifically, it verifies that the skill:

1. asks clarifying questions before designing when inputs are insufficient
2. uses progressive questioning instead of forcing a full intake upfront
3. produces a structured repository/system design rather than ad-hoc advice
4. supports both **new repository** and **refactor existing repository** modes
5. distinguishes **placeholder structure** from **complete content**
6. defaults toward a coherent skill-system architecture instead of just generating `skills/`

## What We Are Testing

### 1. Triggering behavior
We verify that prompts about:
- building a skill repo
- designing a multi-skill framework
- restructuring a skill repository

cause the system to use `building-skill-systems`.

### 2. Explicit invocation behavior
We verify that when the user explicitly asks for `building-skill-systems`, the agent:
- uses the skill
- still asks clarifying questions if needed
- does not skip directly to generation

### 3. Clarification behavior
We verify that:
- the skill does **not** require the user to provide everything in one message
- the first round of questions is compact and high-leverage
- follow-up questions are conditional and architecture-relevant
- questioning stops once enough information is available

### 4. Output structure
We verify that the output includes the required structured sections:
- Skill System Brief
- Architectural Style
- Repository Structure
- Directory Purpose Map
- Skill Matrix
- Workflow Design
- Platform Integration Matrix
- Testing Matrix
- Initial File Set
- Placeholder vs Complete Content Map
- Recommended Next Steps

### 5. Mode handling
We verify that:
- greenfield/new-repo prompts produce a fresh architecture design
- refactor prompts additionally include:
  - Current-State Summary
  - Gap Analysis
  - Migration Stages
  - Compatibility / Deprecation Plan

### 6. Placeholder honesty
We verify that:
- placeholder directories and files are marked honestly
- undeclared domain specifics are not fabricated
- draft content is labeled as draft, starter, or placeholder when appropriate

## Test Layout

Recommended structure:

```text
tests/
├── skill-triggering/
│   └── prompts/
│       ├── building-skill-systems-new-repo.txt
│       ├── building-skill-systems-refactor.txt
│       └── building-skill-systems-lightweight.txt
├── explicit-skill-requests/
│   └── prompts/
│       ├── please-use-building-skill-systems.txt
│       └── use-building-skill-systems-to-refactor.txt
└── building-skill-systems/
    ├── README.md
    ├── clarification/
    ├── mode-handling/
    ├── output-structure/
    ├── honesty-and-placeholders/
    └── golden/
```

## Suggested First-Wave Tests

If you are only implementing a small initial test suite, start with these:

1. **insufficient input → asks questions first**
2. **explicit skill request → uses skill and still asks questions**
3. **full design output → includes required sections**
4. **refactor mode → includes migration sections**
5. **lightweight mode → keeps future slots visible but trims structure intentionally**
6. **placeholder discipline → clearly marks draft vs placeholder vs complete**

## Pass/Fail Guidance

### A good result:
- asks a small first-round question set
- does not overwhelm the user with a huge intake form
- summarizes understanding before architecture
- produces matrices, maps, and workflow sections
- clearly separates structure from implementation
- behaves differently for new vs refactor mode

### A failing result:
- immediately generates a repo structure without clarifying
- asks 15-20 questions at once by default
- produces freeform advice instead of structured sections
- omits workflow/platform/testing matrices
- pretends placeholders are complete
- ignores migration concerns in refactor mode

## Long-Term Extension

Once the first-wave tests are in place, add:
- golden/snapshot structure checks
- regression tests for required section presence
- tests for stopping questions once enough info is available
- tests for lightweight vs full-framework branching
- tests for multi-platform design behavior