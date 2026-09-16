# Representation examples

Rust type notation illustrates the storage choices below; apply the consumer and layout requirements using the target language's representations.

## A recursive grammar does not always need a stored tree

Requirement: consume nested segments such as `(ab(cd))` once, emitting execution records in sequence. Nested groups inherit the current interpretation state, and a closing delimiter returns control to the parent. No consumer edits or revisits syntax.

Bad design: allocate `Node::Group(Vec<Node>)`, finish building the whole tree, walk it to create another `Vec<Instruction>`, then iterate that vector once. This stores two intermediate representations with no consumer that needs them. Splitting opening-delimiter detection and closing-delimiter detection into separate one-line functions does not improve the grammar model.

Good design: the parser's existing recursive group operation consumes a shared byte iterator and emits the final records as it goes. Each recursive invocation owns the state needed for its group; it returns at the closing delimiter. Do not flatten recursion so aggressively that the nested state disappears. Perform global finalization only for requirements that genuinely depend on the complete input.

Different requirement: a source editor must preserve group boundaries, comments, and original spelling while rewriting expressions. A syntax tree now has a real consumer. Streaming away that information would be wrong. Select storage from the consumer, not from a blanket AST prohibition.

## Keep generator invariants separate from consumer restrictions

Requirement: generate paths within a configured range, then let an optional policy exclude protected destinations.

Bad: generate paths outside the range, collect them, and immediately filter them to repair the generator's own contract. Good: stop or reject those paths inside generation.

The later protected-destination restriction still belongs to its policy component. Folding that policy into generic path generation would move meaning to the wrong owner. A lazy downstream filter is not inherently a violation; identify who owns the condition and whether staging is necessary.

## One schema, no generated convenience framework

Requirement: represent a compact record and a presence mask for partial updates. Adding one field must update both layouts consistently.

Bad: manually list each field in `Record`, `ChangedFields`, setters, conversion helpers, and a generated builder API. This creates synchronization points and methods with no current consumers.

Good: declare authoritative fields and widths once; derive the record layout and the mechanically corresponding presence bits. Keep the existing merge/application behavior handwritten on its real owner. Generate a method only if it depends on the schema and has a concrete consumer. Expose declared widths and report invalid widths at the declaration, rather than hiding errors in generated code.

Check actual size and alignment. A sum of field bits does not prove the size of the Rust structure. A small static mask array is shared storage, not allocation per instance; a table of every mask combination is a different tradeoff and must be disclosed and measured.
