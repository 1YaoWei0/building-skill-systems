# Expected Behavior: insufficient-input

## Source Prompt

See:
- `tests/building-skill-systems/clarification/insufficient-input.md`

## Scenario

The user provides almost no useful architectural information:

> "I want to build a skill repository. Help me design it."

This is a pure clarification test.

The skill must **not** jump straight into repository design.

## Required Behavior

### 1. Must use clarification-first behavior
The response should begin by indicating that the system needs to gather the minimum information required to determine the repository shape.

Acceptable patterns include language like:
- "I’ll start with a few key questions..."
- "I need to clarify the repository shape first..."
- "I’ll design this step by step..."
- "Before I design the structure, I need to understand..."

### 2. Must ask a small first-round question set
The response should ask a compact, high-leverage first round of questions.

The first round should usually cover most or all of:
- new repository vs refactor
- domain/purpose
- primary users
- lightweight pack vs full framework
- initial platform(s)

### 3. Must NOT require full upfront specification
The response should not behave as though the user must provide the entire repository contract in one answer.

Good signals:
- "You don’t need to provide everything at once"
- "I’ll ask in stages"
- "I’ll only ask follow-ups where they affect the architecture"

### 4. Must delay architecture generation
The response must **not** yet output:
- a full repository tree
- a Skill Matrix
- a Platform Integration Matrix
- draft files
- a complete architectural plan

The correct action at this stage is questioning, not designing.

## Recommended Quality Signals

These are not strictly required, but are strong positive signals:
- it mentions progressive or step-by-step questioning
- it keeps the tone collaborative and low-friction
- it groups the first-round questions clearly
- it avoids overwhelming the user

## Failure Conditions

The test should be considered failing if any of the following happen:

### Failure A: premature design
The model immediately generates:
- repository structure
- skill taxonomy
- workflow design
- file drafts
without first asking clarifying questions

### Failure B: giant intake form by default
The model responds with a huge one-shot questionnaire (roughly 10+ questions or a long wall of intake prompts) without first attempting a compact first round

### Failure C: vague non-questions
The model says it needs more information but does not ask actionable questions

### Failure D: overconfident assumptions
The model assumes:
- the domain
- the platforms
- the workflow strictness
- the scope of the repository
without asking

## Pass Criteria Summary

This test passes if:
1. the model clearly stays in clarification mode
2. it asks a compact first-round question set
3. it does not force a full upfront contract
4. it does not jump ahead into repository design
