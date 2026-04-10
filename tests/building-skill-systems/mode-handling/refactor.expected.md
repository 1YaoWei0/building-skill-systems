# Expected Behavior: refactor mode

## Source Prompt

See:
- `tests/building-skill-systems/mode-handling/refactor.md`

## Scenario

The user explicitly describes an existing repository and asks for a structured refactor.

The prompt already provides significant context:
- current structure is partially known
- several gaps are explicitly named
- compatibility matters
- the user wants staged migration instead of a rewrite

This is a **refactor-mode behavior test**.

## Required Behavior

### 1. Must recognize refactor mode
The response must clearly treat this as restructuring an existing repository, not as a greenfield design exercise.

Good signals:
- "This is a refactor of an existing skill repository"
- "I’ll treat this as a migration/architecture cleanup problem"
- "We need a target structure and a staged transition"

### 2. Must avoid restarting from zero
The response should not behave as though nothing exists.

It should acknowledge current-state facts provided by the user:
- there is already a `skills/` directory
- there are 6 existing skills
- there is no bootstrap skill
- there are no tests, hooks, or install docs
- current skill names already have users depending on them

### 3. Must not re-ask everything
Because the prompt already contains substantial information, the model should not restart with the full first-round question set.

It may ask a few targeted follow-up questions if needed, but it should not act like the request is underspecified in the same way as a greenfield prompt.

### 4. Must produce refactor-specific structural output
The final response should include refactor-aware sections such as:

- `Current-State Summary`
- `Gap Analysis`
- `Migration Stages`
- `Compatibility / Deprecation Plan`

These do not need to use those exact words if the structure is clearly equivalent, but equivalent structure is required.

### 5. Must preserve compatibility thinking
Because the prompt explicitly says existing users depend on current names, the response must address compatibility concerns.

Acceptable behaviors:
- compatibility shims
- aliases
- deprecation periods
- bridge commands/files
- migration notes

### 6. Must provide staged migration rather than "rewrite everything"
The response should propose a staged path, for example:
- add bootstrap and docs
- classify existing skills
- add tests and integration scaffolding
- introduce deprecations carefully
- clean up old names later

A direct "throw away and rebuild" answer should fail unless the user explicitly asked for that.

## Recommended Quality Signals

These are strong positives:
- distinguishes current state from target state
- identifies missing layers, not just missing files
- explains why bootstrap, workflow clarity, testing, and compatibility matter
- proposes a practical migration order

## Failure Conditions

The test should be considered failing if any of the following happen:

### Failure A: treats it like greenfield
The response ignores the existing repository and simply outputs an ideal fresh repository structure

### Failure B: no migration path
The response proposes the target state but does not explain how to get there

### Failure C: no compatibility strategy
The response ignores the explicit constraint that users already depend on current skill names

### Failure D: excessive restart questioning
The response re-asks nearly all baseline first-round questions, ignoring the context already present in the prompt

### Failure E: rewrite bias
The response implies the best answer is effectively to start over from scratch without justifying why

## Pass Criteria Summary

This test passes if:
1. the model clearly recognizes refactor mode
2. it uses the current-state information already provided
3. it avoids unnecessary re-intake
4. it includes refactor-specific output sections
5. it addresses compatibility and staged migration
