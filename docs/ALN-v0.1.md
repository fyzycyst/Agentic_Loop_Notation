# Agentic Loop Notation v0.1

ALN v0.1 is a readable notation for verified agent loops. It is meant to expose the structure that prose prompts often hide: goal, context, authority, cycle, verifier, stop rules, and return artifacts.

It is intentionally small. It is not a programming language, runtime, scheduler, or permission system.

## Core Shape

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

## Required Blocks

### `loop <Name>(target)`

Names the loop and the thing it works on.

Use a short PascalCase name such as `DocsSweep`, `CompletionContract`, or `UXScoreRepair`. The target should be concrete enough to inspect: `repo`, `task`, `flow`, `query`, `dataset`, `issue`, or another bounded object.

### `goal`

Defines what success means.

Weak:

```text
goal: make the docs better
```

Stronger:

```text
goal: documentation accurately reflects current implementation
```

### `context`

Lists what the agent must inspect before acting.

Context can include files, docs, tests, logs, screenshots, issue text, product requirements, examples, policies, or prior outputs. A loop should not act before it has loaded the context required to make a bounded decision.

### `authority`

Defines what the agent may and may not change.

Authority is not permission by itself. It is an explicit boundary inside the loop. External approvals, repository policy, tool sandboxing, and human review still control what is actually allowed.

### `cycle`

Defines one iteration.

The canonical ALN cycle is:

```text
observe -> act -> verify -> decide
```

The `act` step should be bounded. A good loop changes one coherent thing per round so the verifier can attribute improvement or regression to that change.

### `verify`

Defines the fixed check.

The verifier is the central idea in ALN. It turns repetition into control flow. It may be a test command, benchmark, lint pass, screenshot review, rubric score, eval suite, human approval gate, or evidence table.

If the verifier changes every round, the loop becomes hard to trust. If no verifier exists, the loop is incomplete.

### `decide`

Defines what happens after verification.

Common decisions:

- `keep if`: evidence improved and constraints still hold.
- `revert if`: evidence regressed or the change violated authority.
- `escalate if`: the loop needs human judgment, access, safety review, or permission.

### `stop`

Defines the reasons the loop can end.

Use distinct stop reasons:

- `success if`: the goal has been met.
- `no_op if`: there is nothing actionable to do.
- `stall if`: the loop is running but no longer improving.
- `budget if`: time, token, cost, or iteration budget is exhausted.
- `blocked if`: required input, access, permission, or environment is missing.

These states are different. ALN should preserve the difference.

### `return`

Defines what the loop gives back.

Prefer evidence over claims. A good return artifact includes changed files, diffs, logs, checks run, scores, screenshots, benchmark results, unresolved risks, and the recommended next action.

## Optional Blocks

ALN v0.1 allows small optional blocks when useful:

```text
state:
  working memory or tables the loop maintains

before_act:
  setup that happens once before the repeated cycle

transaction:
  isolation boundary, such as a worktree or branch

strategies:
  candidate approaches for a tournament

select:
  rule for choosing a winner among strategies
```

Keep optional blocks rare. If a loop needs many custom sections, it may be too broad for v0.1.

## Tournament Shape

Some loops explore multiple strategies in isolation.

```text
tournament <Name>(target):
  goal: what success means
  strategies:
    A: first bounded approach
    B: second bounded approach
  transaction:
    how strategies are isolated
  verify:
    same check applied to every strategy
  select:
    how the winner is chosen
  stop:
    success if: one strategy meets the threshold
    reject if: no strategy improves enough
    escalate if: selection needs human judgment
  return: comparison table, winning artifact, rejected alternatives, evidence
```

Use tournaments when independent strategies can be tested fairly against the same verifier.

## Design Principles

- Prefer plain language over clever syntax.
- Make the verifier fixed and visible.
- Bound each action so progress can be attributed.
- Preserve authority limits.
- Return evidence, not vibes.
- Distinguish success from no-op, stall, budget exhaustion, and blocked work.
- Keep v0.1 loops small enough for a human to audit.

