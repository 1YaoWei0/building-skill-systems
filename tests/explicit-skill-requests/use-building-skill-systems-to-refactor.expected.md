# Expected Behavior: explicit request for building-skill-systems in refactor mode

## Scenario

The user explicitly requests `building-skill-systems` for a refactor scenario.

This test verifies that:
1. the requested skill is used
2. refactor mode is correctly recognized
3. the model does not fall back to generic greenfield planning
4. clarification remains targeted and limited to what affects the migration architecture

## Required Behavior

### 1. Must honor the explicit skill request
The response should clearly act like `building-skill-systems` is active.

### 2. Must recognize refactor mode
The response must treat the problem as:
- existing repository cleanup
- architecture restructuring
- migration planning
not:
- build a fresh ideal repo from scratch with no regard for existing state

### 3. Must use targeted clarification only where needed
If the refactor prompt already contains meaningful current-state information, the model should avoid re-running the whole intake process.

Appropriate follow-up questions would be about:
- compatibility requirements
- allowed renames
- whether migration should be staged conservatively or aggressively
- which platforms matter now vs later

### 4. Must eventually produce refactor-specific sections
Once enough info is available, the response should move toward sections equivalent to:
- Current-State Summary
- Gap Analysis
- Migration Stages
- Compatibility / Deprecation Plan

## Strong Positive Signals

These are strong positives:
- explicit acknowledgment that the user asked for `building-skill-systems`
- direct recognition that this is a refactor/migration problem
- use of staged migration thinking rather than replacement thinking
- compatibility discussion if existing names/paths/users are involved

## Failure Conditions

The test should fail if any of the following happen:

### Failure A: generic response
The model ignores the requested skill and responds as generic design assistance

### Failure B: greenfield reset
The model acts like the best answer is simply to design a new repository from scratch without considering the existing repository

### Failure C: unnecessary full re-intake
The model asks the complete first-round question set as though it knows nothing, despite the prompt already including refactor-specific context

### Failure D: no migration mindset
The model describes the ideal target repository but says nothing about how to move from current state to target state

### Failure E: no compatibility thinking
The model ignores the reality that existing structures may already be in use

## Pass Criteria Summary

This test passes if:
1. the model clearly uses `building-skill-systems`
2. it recognizes refactor mode rather than greenfield mode
3. it asks only focused follow-up questions when needed
4. it preserves migration and compatibility thinking
