---
name: select-architecture
description: Select and constrain architectural patterns when changing ownership, interfaces, module structure, state representation, or execution ordering. Create or reconstruct the target project's architecture-map.md when missing, and update it as accepted decisions change.
---


# Select architecture from the responsibility

Architecture is an acceptance criterion alongside behavior. A working result with misplaced policy, unnecessary concepts, or the wrong interaction model is unfinished.

Read the target project's `architecture-map.md`, normally at `.agents/architecture-map.md`. Determine the intended behavior from accepted requirements. Existing implementation and generated documentation do not establish intended ownership. Generic technique references do not override the project's explicit architectural constraints.

If `architecture-map.md` is missing, create or reconstruct it for the target project using [project architecture guidance](references/project-architecture.md), then apply it to the task. Read that guidance also when updating the map. Write the project binding into the target project, not into the reusable skills installation; another project's map is not a substitute.

For a structural change, show the concrete operation, its existing owner, the input/output contract, and any proposed type/interface delta. Use one representative example across alternatives. Compare the same behavior and show the relevant caller and implementation; names without bodies hide differences. Explain unfamiliar concepts before using them in a decision. Do not silently replace an accepted owner while fixing one technique inside it.

## Pattern selection

These rules compose at different boundaries. Apply every matching constraint; do not impose one pattern on the whole codebase.

First classify the responsibility: does the definition contribute optional behavior, or supply configuration to a core operation that exists independently of it? Owning the configuration schema does not automatically mean owning the configured operation. A state-only definition is a complete design when the existing domain owner consumes that state.

| Condition | Required design | Boundary |
|---|---|---|
| A configured component defines optional behavior through an established shared protocol | Declarative definition + protocol implementation, composed through the established dispatch mechanism | Component policy stays in its protocol implementation. Do not add a separate behavior interface or helper to evade that boundary. |
| A definition configures a core operation that exists even without that definition | State-only definition + interpretation by the operation's existing domain owner | Keep grammar and state in the definition; keep the operation, defaults, and use of that configuration with its owner. No behavior protocol is required for the state holder. |
| A family shares a lifecycle and the project explicitly adopts default no-op hooks | Uniform protocol with neutral defaults | Reuse that protocol. Do not replace it with capability markers merely to satisfy interface-segregation fashion. Each default must actually mean no contribution. |
| A separate family has a smaller responsibility and a distinct existing contract | Narrow protocol and separate composition | Do not force it into the configured family or invent a global lifecycle. |
| An operation coordinates meaningful owned stages | A stateful transaction or existing orchestration owner | The coordinator orders operations; components still define their policy. No types created merely as function containers or method for every expression. |
| Contributors may affect overlapping results and registration order must not change outcomes | Declare contributions, combine them with an explicit algebra, then commit | Read [composition.md](references/composition.md). Do not consume another contributor's half-applied mutations. |
| A consumer needs another component's stable state to implement the consumer's policy | Consumer-owned interpretation of an authorized state view | A reference dependency alone does not imply an execution dependency. Do not add consumer-specific helpers to the provider. |
| An operation belongs to an entity's representation or invariant regardless of optional components | A method on that entity or its existing owner | Keep neutral mechanics here. Do not encode optional policy or particular configuration identities. |
| Input is consumed once into an execution representation | Parse directly into the consumer's representation, with streaming recursion where the grammar is recursive | Read [representation.md](references/representation.md). Do not materialize an AST merely to walk it once and discard it. |
| A probe uses reversible execution under the accepted design | Preserve its actual transition and undo operations | Restore every affected owner on all exit paths. Do not substitute whole-state cloning to simplify the probe. |
| External input becomes trusted internal data | Validate at the boundary; preserve the established invariant inside | Identify exactly what was checked and what invalidates it. Do not invent unchecked access or skip overflow handling. |

If more than one design remains valid, prefer the one that reuses accepted owners, introduces fewer distinct concepts, and exposes fewer interfaces. Keep independently meaningful state or invariants separate even when collapsing them would reduce the type count. Performance chooses among compliant designs using evidence, not guesses. A new pattern is justified by a concrete unmet requirement, not by the chance that future work might need it.

Before adding a protocol/interface or a capability to the existing dispatch mechanism, identify the actual caller and implementations and the required composition, substitution, or external contract that direct use of the existing owner cannot satisfy. An interface for one definition created solely to relocate one core operation fails this test. Naming a responsibility or imagining future implementations is insufficient. A single implementation may fulfill a real existing contract; its count alone neither justifies nor forbids an interface.

## Structural acceptance

Before expanding a design, trace the representative example through all owners and stages. Identify state reads, writes, data dependencies, semantic dependencies, and actual ordering requirements separately. A diagram must distinguish these edges and distinguish existing components from proposed ones. Concept diagrams need not use function names; implementation diagrams must resolve to actual code. Keep the graph's scope and meaning stable when repairing layout.

For each added owner, identify its state, invariant, or distinct reusable concept and why an existing owner cannot hold it. Reject duplicate model hierarchies, feature-specific conditions in common mechanics, speculative buses/registries, and relocating functions into types with no state or invariant. A generic name does not make a feature-specific algorithm generic.

A question or naming correction does not approve a semantic redesign. Continue authorized work, and surface only a material unresolved design choice with the smallest concrete example that exposes it.
