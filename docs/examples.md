# ALN Examples

This page explains the initial examples in `examples/`. Each example is intentionally small and readable.

## Lineage and Acknowledgements

These examples were reviewed against the [Forward Future Loop Library](https://signals.forwardfuture.ai/loop-library/), a public collection of practical AI-agent loops with checks and stopping conditions.

- `DocsSweep` is an ALN translation/adaptation of Forward Future's [The docs sweep](https://signals.forwardfuture.ai/loop-library/loops/overnight-docs-sweep/) by Matthew Berman.
- `CompletionContract` is an ALN translation/adaptation of [The Codex completion-contract loop](https://signals.forwardfuture.ai/loop-library/loops/codex-completion-contract-loop/) by 3goblack (@Dis_Trackted).
- `UXScoreRepair` is an ALN translation/adaptation of [The UI/UX Score Loop](https://signals.forwardfuture.ai/loop-library/loops/ui-ux-score-loop/) by Hayden Cassar (@hcassar93).
- `QueryOptimizationTournament` is an ALN synthesis inspired by Loop Library patterns such as fixed verifiers, isolated experiments, repeatable benchmarks, and tournament/arena-style comparison. It is not presented as a direct port of one specific Loop Library entry.

The purpose of these examples is to show how ALN makes the hidden control structure of existing loop-style workflows explicit: goal, context, authority, cycle, verifier, stop rules, and return artifacts.

## Documentation Sweep

File: [`examples/docs-sweep.aln`](../examples/docs-sweep.aln)

`DocsSweep` keeps documentation aligned with implementation. It is useful when docs may be stale but product behavior should not change.

The important parts:

- Authority allows documentation and example edits only.
- The verifier runs doc checks, link checks, examples, and relevant tests.
- The loop stops differently for success, no-op, stall, and blocked states.
- The return artifact includes changed files, evidence, uncertainties, and a PR summary.

## Completion Contract

File: [`examples/completion-contract.aln`](../examples/completion-contract.aln)

`CompletionContract` prevents premature victory claims. It decomposes a task into required outcomes and requires direct evidence for each outcome before the task can be called complete.

The important parts:

- The loop maintains a requirements table.
- Each requirement needs evidence.
- Weak or indirect evidence is marked as weak, not silently treated as proof.
- The return artifact is a requirement-to-evidence table.

## UX Score Repair

File: [`examples/ux-score-repair.aln`](../examples/ux-score-repair.aln)

`UXScoreRepair` improves a user flow by repeatedly running the same task, at the same screen sizes, against the same rubric.

The important parts:

- The verifier repeats the full flow and rescoring process.
- The action focuses on the weakest safe area.
- Authority excludes authentication, billing, data model, and public API changes unless approved.
- The return artifact includes scores, screenshots, changes, and the stop reason.

## Query Optimization Tournament

File: [`examples/query-optimization-tournament.aln`](../examples/query-optimization-tournament.aln)

`QueryOptimizationTournament` tests competing optimization strategies in isolation.

The important parts:

- Each strategy runs in a separate transaction boundary.
- Every strategy uses the same benchmark and regression tests.
- Selection considers latency, behavior, complexity, and risk.
- Rejected alternatives are returned with evidence, not forgotten.

## Writing New Examples

Use this checklist before adding an example:

- Is the target concrete?
- Is the verifier fixed?
- Is the authority boundary explicit?
- Is each action bounded?
- Can the loop revert or escalate?
- Are success, no-op, stall, budget, and blocked states distinct?
- Does the loop return evidence a human can inspect?
