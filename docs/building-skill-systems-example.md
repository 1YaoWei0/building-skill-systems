# building-skill-systems Example

This document shows what a good outcome from the `building-skill-systems` skill can look like.

It is not the only valid output shape, but it demonstrates:
- progressive questioning
- architecture-first thinking
- structured repository design
- workflow definition
- platform/test planning
- honest placeholder handling

---

## Example User Request

> I want to build a skill repository for security review and incident response work.
>
> It should help engineers and security responders with:
> - incident triage
> - root cause analysis
> - evidence gathering
> - remediation planning
> - verification before closure
>
> I don’t want just one skill — I want a real system.
>
> I’m not fully sure yet whether it should be a lightweight pack or a full workflow framework, and I also haven’t decided whether it should support only Claude Code first or also Cursor.
>
> Please guide me step by step instead of asking me for everything up front.

---

## Example Clarification Flow

A good response should not jump directly into repo generation.

A good first response would look something like:

> I’m using the building-skill-systems skill to design this skill repository as a complete system.
>
> I’ll do this in steps so you don’t need to provide everything at once. I’ll start with the minimum questions needed to determine the repository shape, then I’ll only ask follow-ups where they affect the architecture.
>
> First round:
> 1. Is this a new repository or a refactor of an existing one?
> 2. Who are the main users — mostly security engineers, mixed engineering teams, or a broader audience?
> 3. Do you want this to be a strict process-oriented workflow, or more of a lightweight advisory system?
> 4. If you had to choose today, should the first version target Claude Code only, or Claude Code + Cursor?
> 5. For this first pass, do you want architecture only, or architecture plus starter file drafts?

That is a good clarification start because it:
- uses the skill
- keeps questioning progressive
- asks architecture-shaping questions
- does not overload the user
- does not generate too early

---

## Example User Answers

For this example, assume the user replies:

> 1. New repository
> 2. Mostly security engineers and senior software engineers
> 3. Fairly strict process — I want it to guide people through a disciplined workflow
> 4. Start with Claude Code first, but leave room for Cursor later
> 5. Architecture plus starter file drafts

A good next-step follow-up would be small and targeted, for example:

> Great — that’s enough to determine the overall shape. I just need a few architecture-specific follow-ups:
>
> 1. Should the repository include hooks/bootstrap behavior from the beginning?
> 2. Do you want docs/specs/plans directories, or keep docs minimal in v1?
> 3. Should tests exist from the start, even if only as scaffolding?
> 4. Do you want reviewer/agent roles, or keep the first version skill-only?

Assume the user responds:

> 1. Yes, include hooks/bootstrap from the beginning
> 2. Yes, include docs/specs/plans
> 3. Yes, include tests from the start
> 4. Keep reviewer/agent roles as placeholders for now

---

# Example Final Design Output

## 1. Skill System Brief

**Mode:** New repository

**Purpose:** Create a skill system for security review and incident response workflows, helping technical responders move from triage through root-cause analysis, remediation planning, verification, and closure.

**Domain:** Security review, incident response, post-incident remediation

**Target users:** Security engineers and senior software engineers

**Target platforms:** Claude Code first; Cursor planned later

**Repository scope:** Full workflow framework

**Workflow philosophy:** Strict, process-oriented, evidence-driven

**Required structural components:** bootstrap skill, workflow skills, hooks, docs/specs/plans, testing, platform integration files

**Optional structural components:** agents/reviewer roles, commands, future Cursor adapter files

**Generation scope for this pass:** Architecture plus starter file drafts

**Constraints:** Start with Claude-first support, but preserve clean extension points for future multi-platform support

---

## 2. Architectural Style

This repository should be a **policy-driven, workflow-oriented skill system**.

It is not just a domain skill pack. Its value comes from turning security work into a disciplined path:

- orient the agent to the system
- triage and frame the incident
- gather evidence
- analyze root cause
- plan remediation
- verify the fix and closure conditions
- preserve documentation and repeatability

The architecture should therefore optimize for:
- workflow clarity
- high-trust verification
- explicit evidence handling
- future platform extension
- clear distinction between active content and planned structure

---

## 3. Repository Structure

```text
security-response-skills/
├── README.md
├── AGENTS.md
├── CLAUDE.md
├── LICENSE
├── CHANGELOG.md
├── package.json
├── .gitignore
├── .gitattributes
├── .version-bump.json
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── ISSUE_TEMPLATE/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── .cursor-plugin/
│   └── plugin.json
├── hooks/
│   ├── hooks.json
│   ├── session-start
│   └── run-hook.cmd
├── docs/
│   ├── testing.md
│   └── security-response/
│       ├── specs/
│       └── plans/
├── skills/
│   ├── using-security-response-skills/
│   │   └── SKILL.md
│   ├── incident-triage/
│   │   └── SKILL.md
│   ├── evidence-gathering/
│   │   └── SKILL.md
│   ├── root-cause-analysis/
│   │   └── SKILL.md
│   ├── remediation-planning/
│   │   └── SKILL.md
│   ├── verification-before-closure/
│   │   └── SKILL.md
│   └── writing-security-skills/
│       └── SKILL.md
├── agents/
│   └── review-agent.md
└── tests/
    ├── skill-triggering/
    ├── explicit-skill-requests/
    └── security-response/
```

## 4. Directory Purpose Map

### `skills/`

Core skill library. This is the main product surface.

### `skills/using-security-response-skills/`

Bootstrap/orientation skill. Teaches the agent how to use the repository and when to invoke the workflow skills.

### `skills/incident-triage/`

Handles intake, severity framing, scope clarification, and immediate next-step guidance.

### `skills/evidence-gathering/`

Defines how to gather relevant logs, indicators, timelines, impacted systems, and known unknowns before proposing solutions.

### `skills/root-cause-analysis/`

Focuses on disciplined analysis of cause, trigger, blast radius, and causal chain rather than speculative fixes.

### `skills/remediation-planning/`

Converts incident understanding into structured containment, remediation, and recovery plans.

### `skills/verification-before-closure/`

Ensures the incident is not declared resolved without fresh evidence, validation, and closure conditions.

### `skills/writing-security-skills/`

Meta skill for extending the repository with new domain-specific skills later.

### `hooks/`

Bootstrap and context injection layer for Claude Code, with future-friendly structure for cross-platform adaptation.

### `docs/security-response/specs/`

Design/spec documents describing workflow or major system changes to this repository.

### `docs/security-response/plans/`

Implementation plans and staged work plans for evolving the repository.

### `tests/`

Behavior verification for trigger behavior, explicit invocation, and domain-specific workflows.

### `agents/`

Reserved for future reviewer/specialist roles. Placeholder-backed in v1.

---

## 5. Skill Matrix

| Skill                          | Role                 | Trigger                                                    | Produces                              | Comes Before         | Comes After                 | Status        |
| ------------------------------ | -------------------- | ---------------------------------------------------------- | ------------------------------------- | -------------------- | --------------------------- | ------------- |
| using-security-response-skills | bootstrap            | session start / first relevant security task               | orientation and routing               | none                 | triage / evidence gathering | required now  |
| incident-triage                | discovery / framing  | security issue, incident alert, unclear incident situation | scoped incident understanding         | bootstrap            | evidence gathering / RCA    | required now  |
| evidence-gathering             | investigation        | incident being investigated                                | evidence inventory and known unknowns | triage               | root-cause-analysis         | required now  |
| root-cause-analysis            | analysis             | enough evidence exists to reason about cause               | root-cause summary and causal chain   | evidence gathering   | remediation planning        | required now  |
| remediation-planning           | planning             | root cause or likely cause identified                      | remediation plan and execution path   | RCA                  | verification-before-closure | required now  |
| verification-before-closure    | verification         | remediation believed complete                              | verified closure evidence             | remediation planning | finish / reporting          | required now  |
| writing-security-skills        | meta                 | repository evolution work                                  | new skill design guidance             | none                 | future skill additions      | starter draft |
| review-agent                   | future reviewer role | later phase only                                           | review feedback                       | workflow stages      | verification / refinement   | placeholder   |

---

## 6. Workflow Design

### Primary Workflow

```text
using-security-response-skills
  -> incident-triage
    -> evidence-gathering
      -> root-cause-analysis
        -> remediation-planning
          -> verification-before-closure
```

### Alternate Branches

#### Branch A: Early uncertainty

If triage shows the incident is too vague or under-instrumented:

- remain in triage/evidence mode
- do not move into remediation planning too early

#### Branch B: False positive / no incident

If triage determines there is no real incident:

- document outcome
- stop the workflow early
- do not force root-cause or remediation stages

#### Branch C: Ongoing evolving incident

If the incident is active and changing:

- iterate between evidence gathering and root-cause analysis
- keep remediation planning incremental
- delay closure verification until system state stabilizes

### Minimum Viable Workflow for v1

```text
bootstrap
  -> triage
    -> evidence gathering
      -> remediation planning
        -> verification
```

This is the shortest coherent workflow if deeper RCA content is still maturing.

---

## 7. Platform Integration Matrix

| Platform    | Discovery Method           | Bootstrap Method                | Required Files                                               | Tool Mapping Needed | Status       |
| ----------- | -------------------------- | ------------------------------- | ------------------------------------------------------------ | ------------------- | ------------ |
| Claude Code | plugin + skill discovery   | session-start hook              | `.claude-plugin/*`, `hooks/hooks.json`, `hooks/session-start` | low                 | required now |
| Cursor      | future plugin registration | likely hook/config mapping      | `.cursor-plugin/plugin.json`                                 | medium              | placeholder  |
| Gemini CLI  | not in v1                  | future context file approach    | `GEMINI.md` (future)                                         | medium              | postponed    |
| Codex       | not in v1                  | future install/symlink strategy | `.codex/INSTALL.md` (future)                                 | medium              | postponed    |

### Integration notes

- Claude-first is the only active platform integration in v1.
- Cursor structure should exist as a placeholder so future adoption does not require rethinking the repository layout.
- Tool mapping docs are not required immediately, but the design should leave room for them under a future references directory if platform spread increases.

---

## 8. Testing Matrix

| Test Suite                        | Purpose                                                      | Scope                      | Fast/Slow | Required Now or Later | Notes                                    |
| --------------------------------- | ------------------------------------------------------------ | -------------------------- | --------- | --------------------- | ---------------------------------------- |
| tests/skill-triggering            | verify the right security workflow skills trigger from common prompts | prompt -> skill activation | fast      | required now          | especially triage / evidence / RCA       |
| tests/explicit-skill-requests     | verify direct requests for specific skills are honored       | explicit invocation        | fast      | required now          | useful for debugging skill routing       |
| tests/security-response           | verify coherent workflow behavior across incident scenarios  | multi-step flow            | medium    | required now          | may start as partial/manual-guided tests |
| future platform integration tests | verify Cursor or other adapters later                        | hooks/manifests/bootstrap  | medium    | later                 | only once platform support broadens      |

### Testing note

In v1, structure and routing tests matter more than exhaustive domain simulation. But testing must still exist from the beginning.

---

## 9. Initial File Set

### Create first

- `README.md`
- `skills/using-security-response-skills/SKILL.md`
- `skills/incident-triage/SKILL.md`
- `skills/evidence-gathering/SKILL.md`
- `skills/root-cause-analysis/SKILL.md`
- `skills/remediation-planning/SKILL.md`
- `skills/verification-before-closure/SKILL.md`
- `hooks/hooks.json`
- `hooks/session-start`
- `docs/testing.md`

### Create as starter scaffolds

- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`
- `.cursor-plugin/plugin.json`
- `agents/review-agent.md`
- `tests/skill-triggering/`
- `tests/explicit-skill-requests/`
- `tests/security-response/`

### Leave for later

- Gemini/Codex/OpenCode integration files
- richer agent role library
- compatibility commands
- advanced helper scripts

---

## 10. Placeholder vs Complete Content Map

### Complete or near-complete now

- README.md
- bootstrap skill
- core v1 workflow skills
- Claude hook/bootstrap files
- testing strategy doc

### Starter drafts now

- review-agent placeholder
- Cursor plugin placeholder
- writing-security-skills
- domain test directories

### Placeholder structure only

- multi-platform expansion files
- advanced agent roles
- compatibility/deprecation command layer

### Intentionally postponed

- Gemini integration
- Codex integration
- OpenCode adapter
- full review-agent workflow
- deep regression matrix across multiple harnesses

---

## 11. Recommended Next Steps

1. Write the bootstrap skill first.
2. Draft the 5 core workflow skills at a high level before over-optimizing any one skill.
3. Add Claude hook/bootstrap support immediately so the system has a real entry path.
4. Create the first 3 trigger tests:
   - incident alert prompt
   - suspected breach prompt
   - "help me investigate this outage/security incident" prompt
5. Add one end-to-end example in `docs/security-response/specs/` showing how the workflow should behave.
6. Only after v1 feels coherent should you add Cursor support and reviewer-agent depth.

---

## 12. Draft Files

### Draft: `README.md`

Should explain:

- what the repository is for
- who it is for
- the main workflow
- supported platform(s)
- how to install/use
- repository philosophy

### Draft: `skills/using-security-response-skills/SKILL.md`

Should define:

- when this system applies
- that security work must follow evidence-driven workflow
- how to choose between triage / evidence / RCA / remediation / verification
- when to stop and ask for clarification

### Draft: `skills/incident-triage/SKILL.md`

Should define:

- how to classify incident severity
- what to clarify before deeper work begins
- when the issue is not yet ready for root-cause analysis

---

# Why This Is A Good Example

This is a good `building-skill-systems` output because it:

- starts with clarification rather than generation
- builds a real workflow instead of a list of prompts
- includes bootstrap, workflow, testing, and integration layers
- uses placeholders honestly
- keeps future platform support visible without pretending it is complete
- gives the user an actionable staged starting point