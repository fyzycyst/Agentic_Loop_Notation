# Loop Doctor

Loop Doctor is a review checklist for agent loops written in ALN or prose.

Its job is not to make a loop fancy. Its job is to make hidden risk visible.

## Quick Diagnosis

Ask these questions first:

1. What is the goal?
2. What context must be inspected before acting?
3. What may the agent change?
4. What must the agent not change?
5. What is the fixed verifier?
6. What causes keep, revert, escalate, or stop?
7. What evidence comes back?

If any answer is missing, the loop is not ready to run autonomously.

## Common Findings

### No Fixed Verifier

The loop says "improve this" or "keep trying" but does not define a repeated check.

Repair:

```text
verify: run the same test, benchmark, rubric, screenshot review, eval, or human approval gate each round
```

### Vague Success Condition

The loop cannot tell when it is done.

Repair:

```text
success if: all required checks pass and no material findings remain
```

### Unsafe Authority

The loop can change too much.

Repair:

```text
authority:
  may edit docs and examples
  may not change product behavior, secrets, deployments, or external systems
```

### No Rollback Rule

The loop can make a bad change but has no rule for backing out.

Repair:

```text
revert if: checks fail, behavior changes outside authority, or evidence regresses
```

### No Evidence Artifact

The loop can claim success without proof.

Repair:

```text
return: changed files, checks run, raw results, unresolved risks, and next action
```

### No Stop-State Distinction

The loop treats success, no-op, stall, budget exhaustion, and blocked work as the same thing.

Repair:

```text
stop:
  success if: goal is met
  no_op if: nothing actionable exists
  stall if: no improvement after N rounds
  budget if: iteration or cost limit is reached
  blocked if: required input or permission is missing
```

### Excessive Scope

The loop tries to solve a whole backlog instead of one bounded problem.

Repair:

- Narrow the target.
- Reduce authority.
- Define one verifier.
- Split broad work into separate loops.

## Severity Guide

- `P0`: unsafe authority, missing verifier for consequential work, or permission ambiguity.
- `P1`: vague success, no rollback, no evidence artifact.
- `P2`: weak stop rules, broad scope, unclear context.
- `P3`: wording cleanup or example clarity.

## Repair Rules

When repairing a loop:

- Preserve the user's intended outcome.
- Add structure only where it improves control.
- Do not invent facts, tools, approvals, tests, or policies.
- Mark assumptions explicitly.
- Escalate when the verifier, authority, or required context cannot be determined.

