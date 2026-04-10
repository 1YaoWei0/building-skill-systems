# Expected Behavior: must include required sections

## Scenario

This test verifies that once `building-skill-systems` has enough information to produce a design, it does not respond with loose prose or partial architecture.

The skill must produce a **structured repository/system design** with the required sections.

This is not a wording test. It is a **section and structure completeness** test.

## Required Sections

A passing response should include clear equivalents of all of the following sections:

1. `Skill System Brief`
2. `Architectural Style`
3. `Repository Structure`
4. `Directory Purpose Map`
5. `Skill Matrix`
6. `Workflow Design`
7. `Platform Integration Matrix`
8. `Testing Matrix`
9. `Initial File Set`
10. `Placeholder vs Complete Content Map`
11. `Recommended Next Steps`

These do not need to use exactly these headings, but the structure must clearly cover all of these concerns.

## Required Structural Behavior

### 1. Must produce a system design, not scattered advice
The response should feel like a repository design document.

Good signs:
- explicit sections
- headings
- matrices or structured lists
- separation between architecture, workflow, platform support, and tests

### 2. Must include both structure and flow
A directory tree alone is not enough.

The response must include:
- repository structure
- skill relationships
- workflow sequence

### 3. Must include platform and testing thinking
The response must not stop at skills and folders.

It must also show:
- how the system integrates with the target platform(s)
- how the system will be tested or verified

### 4. Must include a distinction between current-generation output and future placeholders
The design must show:
- what should be built now
- what is placeholder-backed
- what is intentionally postponed

## Strong Positive Signals

These are not strictly required but indicate high-quality behavior:
- a directory tree plus explanatory mapping
- a skill matrix with roles/triggers/outputs/status
- platform support shown in matrix form
- testing support shown in matrix form
- a staged build plan or clearly prioritized initial file set

## Failure Conditions

The test should fail if any of the following happen:

### Failure A: prose-only answer
The model gives general advice about building skill repositories, but does not produce a concrete structured design

### Failure B: partial architecture
The model gives only:
- a repo tree
or only:
- a skill list
or only:
- a workflow
without covering the rest of the required system dimensions

### Failure C: no integration/testing layer
The model omits platform integration or testing strategy entirely

### Failure D: no placeholder/completion distinction
The model treats all suggested files and directories as equally complete, with no distinction between real, draft, placeholder, or postponed content

### Failure E: no actionable next steps
The model ends at description and does not guide what to build first

## Pass Criteria Summary

This test passes if:
1. the output is clearly structured
2. all required design dimensions are present
3. the result looks like a repository/system design rather than freeform advice
4. the output includes actionable build-next guidance
