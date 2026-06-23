---
name: aln-loop-designer
description: Design, audit, repair, and export AI-agent loops using Agentic Loop Notation v0.1. Use for loop design, prose-to-ALN translation, Loop Doctor safety reviews, Codex goal prompts, Codex skills, automations, and reusable loop examples.
---

# ALN Loop Designer

Use this skill to express agent workflows as structured Agentic Loop Notation v0.1.

## Core Model

A useful agent loop has:

1. goal
2. context
3. authority
4. cycle
5. verifier
6. stopping rules
7. return artifacts

The canonical cycle is:

```text
observe -> act -> verify -> decide -> repeat/stop
```

The verifier is the branch condition. Without a verifier, the loop is incomplete.

## Procedure

1. Identify the user's intended outcome and target.
2. Extract known inputs, tools, constraints, and authority boundaries.
3. Identify the verifier. If no verifier exists, flag the loop as incomplete.
4. Define stopping conditions:
   - success
   - no_op
   - stall
   - budget
   - blocked
5. Write the loop in ALN v0.1.
6. Run Loop Doctor:
   - no fixed verifier
   - vague success condition
   - unsafe authority
   - no rollback rule
   - no evidence artifact
   - no blocked/stall/no-op distinction
   - excessive scope
7. Repair the loop using only supplied or discoverable facts.
8. Export to the requested format:
   - ALN only
   - Codex `/goal`
   - Codex skill
   - Codex automation prompt
   - Markdown documentation

## ALN Skeleton

```text
loop <Name>(target):
  goal:       what success means
  context:    what the agent must inspect first
  authority:  what it may and may not change
  cycle:
    observe:  inspect current state
    act:      make one bounded change
    verify:   run the fixed check
    decide:
      keep if:     evidence improves
      revert if:   evidence regresses
      escalate if: judgment, access, safety, or authority is missing
  stop:
    success if:    completion condition is met
    no_op if:      nothing actionable exists
    stall if:      no improvement after N rounds
    budget if:     time, token, or iteration limit is reached
    blocked if:    required input or permission is missing
  return: evidence, diff, log, and next action
```

## Output Format

Return:

1. ALN loop
2. Loop Doctor findings
3. Exported prompt or skill, if requested
4. Remaining assumptions or missing information

## Safety and Authority

Never treat a loop prompt as authorization to deploy, delete, spend money, expose private data, contact third parties, alter production systems, or override repository rules.

Preserve human gates for consequential actions. If authority, safety, access, or verification is unclear, escalate instead of inventing permission.

