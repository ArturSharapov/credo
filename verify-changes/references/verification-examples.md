# Verification examples

## A probe must restore all state, including on failure

Requirement: an existing probe uses the real transition and its undo operation to decide whether an operation is allowed. It changes a primary collection, an inventory, a countdown, and a cached summary.

Bad test: clone only the primary collection before probing and compare it afterwards. The inventory can lose an item, and the summary can become stale, while the test passes. Replacing the actual transition with a simplified test-only transition also avoids the behavior that needs verification.

Good test: snapshot all four affected states in the test, run the real public probe, and compare all four afterwards. Exercise success, rejection after partial mutation, and an early return. Test snapshots may copy data; the production probe still uses real make/unmake, not whole-state cloning. Ensure restoration occurs before propagating an error from an intermediate stage.

Do not create a new production snapshot API solely for this test. Use the test's available state views or existing observation interface.

## A validated value has a limited proof

Requirement: an external record index must be in range before a transaction uses it repeatedly.

Bad: repeat the identical range check in every stage despite an unchanged collection. Also bad: remove all checks because the index has a type named `ValidatedIndex`.

Good: validate at the boundary, preserve the checked index and its relation to that collection, and reuse it while the collection shape remains stable. If a stage removes or reorders records, the old index may no longer identify the intended record. Revalidate or preserve stable identity as required. Do not use unchecked memory access merely because the initial range check passed.

## Passing tests and completion claims have scope

Suppose a library changes a public result type. The focused library tests pass, but its language binding still imports the old type.

Bad report: "All tests pass; the refactor is complete."

Good action: check the affected binding and configured features. Repair failures in scope. If a coordinated edit deliberately defers that integration, record the pending check and restart condition, and report "Core change implemented; binding integration remains unverified."

A passing behavioral test also does not excuse a helper that moves component-specific logic out of a protocol-only definition's protocol implementation. Review structural constraints in the diff independently of test outcomes.
