# Compare storage and algorithm separately

Requirement: choose compact storage for partial-field updates processed many times. Compare the same updates, merge semantics, and result checks for every candidate.

Bad comparison:

| Candidate label | Hidden implementation difference |
|---|---|
| Six-byte record | Widens and stores an accumulator on every merge |
| Eight-byte integer | Widens once per batch and uses a precomputed combinations table |

Declaring that "eight-byte storage is faster" confuses representation with accumulator lifetime and precomputation. Reusing this result after switching to on-demand masks would compare an implementation that was never measured.

Good experiment:

1. Hold the merge algorithm, accumulator lifetime, and mask strategy constant while comparing the two storage layouts.
2. Compare algorithm alternatives separately, naming any table and its memory cost.
3. Use runtime batches with one and multiple overlapping updates; verify identical results and conflicts.
4. Preserve candidate source, commands, build settings, repetitions, absolute costs, and variation.
5. Inspect the production diff against the selected candidate before applying its performance conclusion.

No timings are supplied here: this example explains experimental control, not a claimed performance result. If the difference is within run variation, report that and prefer the simpler compliant design. Compiler simplification, constant folding, and shared static storage must be accounted for rather than assumed away.
