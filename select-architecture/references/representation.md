# Representation follows the consumer

When changing parsing, intermediate storage, or schema generation, read [representation examples](representation-examples.md).

Choose storage from the operations that consume it. Separate immutable reusable definitions from mutable instance state when they have different lifetimes. Refer to shared definitions with compact validated indices when appropriate. Avoid duplicating derived flags, cursors, caches, or alternate models unless a current consumer or measured cost justifies them.

For sequential input, use an iterator or byte stream through recursive grammar layers. Each meaningful grammar layer owns its parse and interpretation; it is not a reason to extract every token expression. Preserve intermediate state required by nested composition. Do not collect a stream only to iterate it once, build an unused AST, or parse into a second textual representation and reparse it. Use an AST when a real consumer needs syntax-preserving editing, repeated analysis, or transformation. Explain that requirement before introducing it.

Let each parser layer write the final fields it owns. Reserve global finalization for actual cross-input dependencies. Resolve local overrides before registering the used definitions; deduplicate the used runtime representation rather than accumulating every spelling.

Enforce a generator's own acceptance invariant while generating. Do not produce known-invalid intermediate results and immediately filter them away as a workaround. This does not forbid downstream component restrictions, which have a different owner, or legitimate iterator filters. Prefer lazy language constructs when eager evaluation would perform unnecessary or invalid work.

Represent mutually exclusive states and meaningful invariants explicitly. Do not wrap every scalar in a newtype or introduce typestate for a lifecycle that existing state already expresses. Conversely, do not flatten distinct meanings merely to reduce type count. Put authoritative field definitions in one place; derive mechanical companion layouts from them when needed. Generation from a schema must produce only schema-dependent material. Handwritten behavior stays outside the generated code. Do not add unused builders, getters, conversions, or methods as a convenience package.

When choosing integer widths and fixed capacities, derive them from valid bounds. For compact layouts, check actual size and alignment, not only the sum of field bits. Account for shared storage, per-instance storage, and temporaries, including stack and heap costs where applicable. Small fixed shared tables are not automatically forbidden, and tiny copies are not whole-state cloning. Measure meaningful tradeoffs. Do not introduce a table of every possible combination as an undisclosed optimization.
