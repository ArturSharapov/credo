---
name: measure-performance
description: Compare runtime or memory representations and verify hot-path optimizations with reproducible, implementation-specific measurements.
---


# Measure the implementation being chosen

Choose among architecturally compliant candidates. State the same workload and behavior for each. Show enough of the real implementation to distinguish storage, algorithm, temporary values, precomputation, and lookup tables. Do not claim a type layout caused a speedup when the compared algorithms also changed.

When comparing representations or transferring a measured design into production, read [comparison example](references/comparison-example.md).

Preserve the experiment and its source identity, toolchain and runtime versions as applicable, build settings, input cases, commands, repetitions, and results. Measure the storage costs relevant to the comparison. For compact layouts, include actual size and alignment; account for shared, per-instance, and temporary storage, including stack and heap costs where applicable. Explain bounds and capacity choices. Fixed arrays, small copies, and inlining are not automatically faster; treat optimizer behavior as a hypothesis until measured or inspected.

Use representative runtime data and consume results so constant folding or dead-code elimination cannot replace the work. Keep initialization and allocation accounting comparable. Report absolute costs, relative differences, and run variation. Separate a microbenchmark result from an end-to-end throughput claim. Re-run all candidates used in a comparison when changing the workload or algorithm, or visibly label incomparable old measurements.

If the working tree is intentionally unbuildable, a standalone experiment may compare representations, but it does not verify the repository integration. Preserve the accepted semantics in the experiment and name that limitation.

Before implementing the winner, match the production diff to the measured candidate. A different accumulator lifetime, conversion algorithm, cache, or table is a different implementation and requires renewed measurement. Do not smuggle a precomputed combinations table into an on-demand design. Prefer the simpler accepted design when measurements do not establish a useful advantage.

When benchmarking an operation whose accepted implementation uses reversible transitions, retain those transitions and verify full restoration; do not substitute whole-state cloning to simplify the benchmark. Use the user's chosen current-work record for results and explanations. If asked for code examples or a comparison table there, update that record before claiming it is ready.
