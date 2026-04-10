# Expected Behavior: explicit request for building-skill-systems

## Source Prompt

See:
- `tests/explicit-skill-requests/prompts/please-use-building-skill-systems.md`

## Scenario

The user explicitly asks for `building-skill-systems`.

This test verifies that:
1. the system actually uses the requested skill
2. the skill still follows its own process
3. explicit invocation does **not** justify skipping clarification

The prompt contains some useful information:
- domain: security review and incident response
- users: experienced engineers and security team members

But it also leaves important architecture-shaping decisions unresolved:
- lightweight vs full workflow framework
- single platform vs multi-platform
- exact platform scope

So this test is about **explicit invocation + incomplete input**.

## Required Behavior

### 1. Must honor the explicit skill request
The response should clearly behave as though `building-skill-systems` is active.

Good signals:
- explicit mention that it is using `building-skill-systems`
- a response style consistent with repository/system design
- repository-architecture-oriented clarification instead of generic brainstorming

### 2. Must NOT skip clarification
Even though the user explicitly asked for the skill, the model must still ask follow-up questions before designing the repository.

Explicit invocation means:
- use this skill
not:
- skip this skill's process

### 3. Must use targeted follow-up questions
The prompt already gives some useful context, so the model should not restart from zero.

It should ask a **small set of follow-up questions** focused on unresolved architecture decisions, such as:
- lightweight vs full framework
- first platform(s)
- whether hooks/tests/docs/agents are required now
- architecture-only vs starter-file generation scope

### 4. Must preserve progressive-questioning behavior
The model should not switch into giant-intake mode just because the user explicitly named the skill.

The response should still feel like:
- guided design
- staged questioning
- low-friction clarification

## Strong Positive Signals

These are not strictly required but are strong positives:
- it explicitly says something like "I’m using the building-skill-systems skill..."
- it acknowledges what the user already provided
- it only asks the missing architecture-relevant questions
- it avoids re-asking the domain and user-type if already clear

## Failure Conditions

The test should fail if any of the following happen:

### Failure A: ignores the explicit skill request
The model responds generically and does not behave like the requested skill is active

### Failure B: skips straight to generation
The model immediately produces architecture/design output without resolving the missing structural decisions

### Failure C: full restart intake
The model re-asks the full baseline question set without taking into account the useful information already present in the prompt

### Failure D: giant questionnaire
The model responds with a large all-at-once intake form by default, instead of progressive follow-up questions

### Failure E: domain/user amnesia
The model ignores the domain and user information already provided and behaves as though nothing meaningful was given

## Pass Criteria Summary

This test passes if:
1. the model clearly honors the explicit use of `building-skill-systems`
2. it still follows the skill's clarification-first process
3. it asks only targeted follow-up questions for missing architecture inputs
4. it does not jump straight to design generation
