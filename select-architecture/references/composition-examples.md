# Composition examples

## Overlapping contributions must not depend on registration order

Requirement: one component selects records affected by a request; another marks selected records inactive; a third counts the records actually removed. All may be enabled together, in any catalog order. The existing context stores candidates and partial-field patches.

Bad sequence:

```text
for each configured component:
    mutate the shared record list immediately
    inspect changes made by earlier components
```

Running the counter first misses removals. Running it after candidate creation but before the inactive patch counts records that ultimately remain. Reading another component's configuration to predict the final result merely hides that outcome dependency.

Good sequence on the existing owners:

```text
collect the complete candidate set from stable input
collect patches against those candidates
merge compatible patches and reject conflicts
commit the combined results once
let the counter consume finalized removals
```

This sequence does not require a class for every line. Reuse the existing context, composition, and transaction. Components supply policy; common mechanics combine and apply contributions without switches on component names.

For an existing partial-update model, these cases define the merge contract:

| Incoming contributions to one record | Result |
|---|---|
| `inactive = true` and `label = 4` | Apply both fields |
| `inactive = true` and `inactive = true` | One agreeing assignment |
| `inactive = true` and `inactive = false` | Explicit conflict; catalog order cannot choose a winner |
| No assignment and `inactive = false` | Assign false; absence differs from false |
| Empty patch and a label assignment | Keep the label assignment |

For a numeric counter, contributions might instead sum. Do not reuse field-conflict rules for sums, or impose this patch model on unrelated operations.

## State references, ordering, and outcome dependence

A retention component reads an already configured hold set. It decides whether to retain a record. This is a stable state reference; it does not require the hold component to run first.

A retention component reads a hold set another component is still modifying during this operation. This is an execution dependency. Define the required stage boundary or derive independent contributions; do not claim catalog order is irrelevant.

A billing component must charge only for removals that survive every patch. This is an outcome dependency. It must consume finalized removals rather than reproduce retention and hold policy in its own prediction.

## When direct mutation and default hooks are valid

A component decrements its private countdown, and no peer reads that countdown during the operation. Direct mutation through its protocol is sufficient. Creating `CountdownCandidate`, `CountdownPatch`, and `CountdownResolver` adds no useful coordination. If a peer later requires the updated countdown, reassess the dependency rather than silently relying on order.

For an existing shared lifecycle, a neutral hook such as an empty `after_commit` can correctly mean "this component has no after-commit contribution." It must not hide a required implementation. A separate family responsible only for choosing candidates can keep its narrower protocol. Do not invent capability routing to avoid a valid neutral default, or force unrelated responsibilities into one universal protocol.

Review test: vary contributor order, include overlapping changes, and trace which state each consumer reads. The intended result must remain the same where independence is required.
