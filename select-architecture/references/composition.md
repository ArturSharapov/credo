# Composition and dependencies

When changing contributor interactions or choosing between direct mutation and merged effects, read [composition examples](composition-examples.md).

A behavioral contributor owns its condition, configuration interpretation, and contribution. Its composition owns dispatch; shared execution owns neutral combination and application. Moving contributions into a coordinator violates this division even when the coordinator is called a transaction. Apply this model after the parent skill's responsibility classification: a definition that only configures a core operation needs no contribution or dispatch layer.

Use the project's accepted protocol family. Broad neutral-default lifecycle protocols and narrow internal protocols can coexist. Where a catalog registers components, register each component once. Do not add a second manually synchronized catalog or duplicate dispatch cases throughout the codebase. If code generation supplies dispatch mechanics, keep semantic decisions in the component's protocol implementation. Validate declaration errors at their invocation site. Capability tokens are appropriate only when the selected contract requires explicit membership; they are not mandatory architecture.

For overlapping contributions, make the input snapshot and phase boundaries explicit:

1. Produce the complete candidate set independently of contributor enumeration order.
2. Describe changes against those candidates without prematurely applying them.
3. Combine contributions under the accepted conflict semantics.
4. Commit each combined result once. Reactions that need finalized outcomes consume those outcomes, not predictions of another feature's policy.

This is a dependency sequence, not an instruction to add four new types or four methods. Reuse existing owners and contracts. Independent mutations can stay direct when they cannot affect a peer's inputs and do not need coordinated reporting; do not route every assignment through an effect system.

For partial-field updates, distinguish an absent change from assigning the field's default value. Compatible writes to different fields combine; equal writes agree; unequal writes to the same field must follow an explicit accepted rule. Never silently introduce last-writer-wins. If no domain precedence is specified, reject conflicting writes. An empty patch contributes no field replacement and must not erase another patch. Do not assume this patch algebra applies to ordered actions, sums, or other kinds of effects: derive each combination from its meaning.

A stable state read is not an ordering dependency. A read of another contributor's transient output is. Predicting another component's final outcome is a semantic dependency even when no call or runtime ordering exists. Preserve all three distinctions when evaluating coupling.

Do not resolve circular dependence with registration priority, repeated execution until stable, or a new generic event bus unless that algorithm is itself an accepted requirement. Distinguish one bounded merge/commit from recursive side effects. Verify order-independent results with different contributor orders, overlapping writes, and a real combined behavior case.

Keep optional behavior with its component. If a shared entity property now expresses the behavior for arbitrary configurations, operate on that property rather than special-casing default names or symbols. Configuration adapters may map legacy notation to the property once at the input boundary. Shared mechanics must not reinterpret the notation later.
