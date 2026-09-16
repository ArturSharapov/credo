---
name: construct-code
description: Implement or refactor code while enforcing minimal functions, meaningful ownership, accepted interfaces, and direct language constructs.
---


# Construct code without structural inflation

Apply these constraints on every code change, even when the request says only "fix" or "implement". Read the target project's `architecture-map.md`, normally at `.agents/architecture-map.md`. If it is missing, use [select-architecture](../select-architecture/SKILL.md) to create or reconstruct it before implementing the change. Also use that skill when ownership, representation, interfaces, or ordering change, before expanding the edit.

## Functions and types

Keep ordinary single-use logic inline in its consumer. Extract a helper only when it has actual multiple uses AND enough nontrivial logic to remove meaningful duplication. A one-line lookup, field access, constructor expression, or renamed operator fails that test even with several callers. "It names an operation", "it is testable", and "it improves readability" alone are insufficient.

The narrow exception is a demonstrative orchestration flow: an accepted transaction coordinates meaningful stages on real owners, and those stage methods make the sequence understandable. Preserve that flow. Do not extend the exception to every branch, guard, field assignment, or conversion inside those stages. Protocol-required methods, real recursive grammar productions, and necessary public entry points are interfaces, not helper-extraction excuses; keep their responsibilities real and their number minimal.

Classify definitions using select-architecture before assigning behavior. A state-only definition needs no behavior implementation: its consuming core owner interprets the configuration of its own operation. For definitions that contribute behavior through an established protocol, keep the component-specific logic in the operation that implements that protocol. Do not add a separate behavior interface or relocate the logic into a helper function, extension, or module to evade this boundary. Use the language's normal implementation mechanism; a method that implements the protocol is valid even where the language does not distinguish it syntactically from other methods. Reuse genuinely neutral mechanics on their existing owner. If the approved protocol cannot express an actual contribution, show that blocker; apply the protocol test before proposing another interface.

This restriction applies to definition types, not every value type declared in the same file. Value/state/transaction types may have methods that own their actual representation, invariant, or lifecycle. Do not make fields private and add trivial getters without a real invariant or API constraint. Do not create new types merely to house functions.

Use built-in syntax, standard operations, and existing APIs before creating wrappers or macros. Unify genuinely repeated logic while preserving distinct meanings. Add no unused APIs or test-only production helpers. Do not keep obsolete helpers solely because obsolete tests still call them.

For function extraction, definition behavior, or transaction-stage changes, read [function and ownership examples](references/functions-and-owners.md). Apply their decision tests to the actual changed code; the example types are not a framework to reproduce.

## Implementation loop

Start from the intended behavior and accepted structural pattern. Reuse the smallest existing set of owners that can express it. A rename changes the terminology consistently through variables, constants, fields, consumers, and comments within scope; it does not authorize an algorithm change.

Inspect the diff for every added function, method, protocol implementation, type, field, and module. Each must have a concrete architectural role or meet the extraction rule above. Inline or remove those that do not. Check consumer sites before asserting reuse. Confirm that optional component contributions did not move to a shared coordinator and that core operations retained their accepted owners.

Keep the current user edits intact. Do not format or rewrite unrelated files as part of a local change. Use the project's verification workflow before claiming completion; compilation alone does not establish architectural compliance.
