# Agentic Loop Notation (ALN) v0.1

Agentic Loop Notation is a small, readable notation for describing verified AI-agent loops.

Many practical loop examples now exist: documentation sweeps, repair loops, UI scoring loops, eval loops, migration loops, optimization tournaments, and recurring operational checks. Most of them are still written as prose prompts. Prose is useful, but it hides the structure that makes a loop controllable.

ALN makes that structure visible:

- `goal`: what success means
- `context`: what the agent must inspect first
- `authority`: what the agent may and may not change
- `cycle`: observe, act, verify, decide
- `verifier`: the fixed check that turns repetition into control flow
- `stop`: success, no-op, stall, budget, and blocked rules
- `return`: the evidence and artifacts the agent must hand back

ALN is not a runtime, safety system, or authorization layer. It is a notation for making agent loops easier to teach, compare, audit, repair, and export into tools such as Codex goals, skills, worktrees, subagents, automations, AGENTS.md guidance, and hooks.

## Why ALN

The first wave of AI coding was mostly linear prompting:

```text
prompt -> output -> correction -> output -> correction
```

Agentic work changes shape when the human defines a loop:

```text
goal -> observe -> act -> verify -> decide -> repeat/stop
```

The key is the verifier. Without a verifier, a loop is just repetition. With a verifier, the check becomes the branch condition: keep, revert, escalate, or stop.

## ALN v0.1 Skeleton

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

See [docs/ALN-v0.1.md](docs/ALN-v0.1.md) for the notation and [docs/examples.md](docs/examples.md) for examples.

## Examples

This repository includes four initial examples:

- [DocsSweep](examples/docs-sweep.aln): repair documentation drift one bounded section at a time.
- [CompletionContract](examples/completion-contract.aln): require proof before declaring work complete.
- [UXScoreRepair](examples/ux-score-repair.aln): improve a user flow against a repeated rubric.
- [QueryOptimizationTournament](examples/query-optimization-tournament.aln): test competing strategies in isolation and select by evidence.

## Loop Doctor

Loop Doctor is a review checklist for ALN loops. It flags common failures:

- no fixed verifier
- vague success condition
- unsafe authority
- no rollback rule
- no evidence artifact
- no blocked/stall/no-op distinction
- excessive scope

See [docs/loop-doctor.md](docs/loop-doctor.md).

## Codex Mapping

Codex primitives map naturally onto ALN:

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

See [docs/codex-mapping.md](docs/codex-mapping.md) and [skills/aln-loop-designer/SKILL.md](skills/aln-loop-designer/SKILL.md).

## Acknowledgements

ALN v0.1 was motivated in part by the growing body of practical loop examples in the [Forward Future Loop Library](https://signals.forwardfuture.ai/loop-library/), which collects repeatable AI-agent workflows with checks and stopping conditions.

Several examples in this repository are ALN translations or adaptations of Loop Library examples. See [docs/examples.md](docs/examples.md) for lineage and acknowledgements.

## Citation

If you use ALN in writing, tooling, research, videos, or examples, please cite this repository. See [CITATION.cff](CITATION.cff) and [NOTICE.md](NOTICE.md).

## Contributing Examples

Good ALN examples should be small, inspectable, and honest about authority. Prefer one clear target, one fixed verifier, explicit stop rules, and concrete return artifacts.

When adding an example, include:

1. The loop name and target.
2. The verifier.
3. The authority boundary.
4. The success and stop rules.
5. The evidence returned to the human.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution checklist.

## Safety Note

ALN describes control flow. It does not grant permission.

An ALN loop must not be treated as authorization to deploy, delete, spend money, contact third parties, expose private data, alter production systems, or override repository rules. Human gates, sandboxing, approvals, and local policies still apply.

To report unsafe loop wording or examples, see [SECURITY.md](SECURITY.md).

## License

ALN is licensed under [Creative Commons Attribution 4.0 International](LICENSE). You can share and adapt it freely, including for commercial use, as long as you credit the project and link back when reasonably possible.

If ALN is useful to you, please consider starring the repository so others can find it. Starring is appreciated, not required by the license.
