# Mapping ALN to Codex

ALN is tool-agnostic, but Codex is a natural early runtime for many ALN-shaped workflows.

The mapping is an analogy, not a requirement. ALN does not depend on Codex, and Codex does not need ALN to run.

## Primitive Map

```text
Prompt       -> statement
Skill        -> function or macro
/goal        -> loop
Verifier     -> branch condition
Worktree     -> transaction
Subagent     -> fork
Automation   -> daemon
AGENTS.md    -> persistent policy memory
Hook         -> interrupt or guardrail
```

## `/goal` as Loop

Use a Codex `/goal` when the work has:

- one durable objective
- a verifiable stopping condition
- enough room for multiple bounded iterations
- clear authority limits
- progress evidence after each checkpoint

ALN helps shape the goal before the run starts.

Template:

```text
/goal Complete <objective> without stopping until <verifiable end state>.

Context to inspect first:
- <files, docs, tests, issues, logs>

Authority:
- May: <allowed changes>
- May not: <forbidden changes>

Cycle:
- Observe one current gap.
- Make one bounded change.
- Run <fixed verifier>.
- Keep, revert, escalate, or stop based on evidence.

Stop:
- Success: <condition>
- No-op: <condition>
- Stall: <condition>
- Budget: <condition>
- Blocked: <condition>

Return:
- evidence, diff, log, and next action
```

## Skills as Reusable Loops

A Codex skill packages a reusable workflow. An ALN loop can become a skill when the procedure is valuable across tasks.

Good skill candidates:

- have repeated use
- have clear triggers
- include a stable procedure
- can name their verifier
- preserve authority boundaries

See [`skills/aln-loop-designer/SKILL.md`](../skills/aln-loop-designer/SKILL.md) for a minimal example.

## Worktrees as Transactions

Use a worktree or branch when a loop needs isolation.

Examples:

- trying alternate query optimizations
- running risky migrations
- comparing UI repair strategies
- preserving a clean rollback path

In ALN, this belongs in `transaction` or `authority`.

## Subagents as Forks

Use subagents when independent work can proceed in parallel with clear inputs and outputs.

Examples:

- one subagent per optimization strategy
- one subagent per audit surface
- one subagent for implementation and another for review

Every fork still needs a verifier and return artifact.

## Automations as Daemons

Use automations for recurring loops.

Examples:

- weekly documentation drift scan
- nightly eval failure triage
- recurring dependency report
- telemetry error review

Recurring loops need especially clear budget and blocked rules so they do not silently wander.

## AGENTS.md as Policy Memory

`AGENTS.md` can hold repository-level rules that every loop must respect.

Examples:

- test commands
- coding conventions
- permission boundaries
- planning requirements
- release rules

An ALN loop should treat AGENTS.md as context and authority guidance, not as optional background.

## Hooks as Interrupts or Guardrails

Hooks can enforce checks around tool use, commits, tests, or other workflow boundaries.

In ALN terms, hooks can support:

- `verify`
- `revert if`
- `escalate if`
- `blocked if`

Hooks should not be used to hide important loop logic. The ALN should still name the control rule in readable form.

## Safety Boundary

Codex approvals, sandboxing, repository rules, and human review remain authoritative. ALN can describe what a loop should do, but it cannot grant permission to do it.

