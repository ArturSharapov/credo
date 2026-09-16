---
name: verify-changes
description: Verify implementation and refactors against intended behavior, structural constraints, affected consumers, and explicitly deferred checks.
---


# Verify the actual change

Use the requested outcome and accepted architecture as independent acceptance criteria. Existing tests are evidence, not permission to restore obsolete behavior. Do not weaken an assertion, add a compatibility shim, or change the design merely to make legacy tests pass.

Identify the changed behavior, owners, protocol boundaries, and affected consumers. Use the smallest checks that cover them. Reuse meaningful existing tests; do not add tests that merely mirror implementation or assert instruction wording. Run broader checks when a public interface or shared representation makes that necessary.

For structural edits, inspect every added function/type/module and every new cross-owner read or write. Classify each affected definition as a behavioral contributor or a state holder before checking ownership. Apply [select-architecture](../select-architecture/SKILL.md)'s protocol test to every new protocol/interface or capability added to the existing dispatch mechanism; a state holder does not fail review because its core consumer interprets configuration. Verify the function-extraction rule and declared-protocol boundary for behavioral contributions. Trace a real example through the changed path. Report a structural violation even when all behavioral tests pass. Do not use existing violations as precedents.

For behavior edits, exercise the real public entry point. Assert exact expected results when ordering or full membership is specified; containment alone can hide extra results. Cover the branch or interaction that motivated the change. For reversible execution, compare the full relevant state before and after, including side inventories, counters, and auxiliary state. Exercise early returns and failures. For order-independent composition, verify permutations and conflict cases.

A boundary check only proves its stated invariant. Reuse that proof while it remains valid; avoid checking the same condition at every internal stage. Mutations or externally supplied data can invalidate it. Maintain checked arithmetic and real bounds guarantees; an "unchecked" name does not justify undefined behavior.

A public API change requires checking the affected adapters/bindings and configured features, not only the core library. Use actual manifests and current commands. Inspect relevant diagnostics before finishing, including macro expansion failures where available. If the IDE diagnostics interface is unavailable, distinguish compiler checks from an actual IDE inspection.

For reversible execution, boundary invariants, or integration completion, read [verification examples](references/verification-examples.md).

## Deferred verification and continuity

If the maintainer explicitly suspends compilation/tests during concurrent edits, record which checks were deferred, why, and their restart point. Do not repeatedly rerun an unchanged target known to fail for that reason. At integration, resume the pending checks. "Implemented, verification pending" is accurate; "complete and verified" is not.

For substantial work spanning sessions, keep one current record at the chosen location. It holds the accepted design, unresolved choices, completed changes, remaining scope, and passed/failed/deferred checks. Update it when a decision changes. Do not maintain an independent local copy when the maintainer chose a remote record. If synchronization fails, preserve an explicitly unsynced draft and report that status.

Review the final diff against the actual requested scope. Re-run only checks invalidated by later edits, unresolved failures, or new evidence. Completion states what changed, why it meets the structural and behavioral requirements, and any remaining verification gap. Do not silently end at a convenient subset of the requested work.
