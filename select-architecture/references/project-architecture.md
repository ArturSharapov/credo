# Create and maintain the project's architecture-map.md

A project's `architecture-map.md` tells a future contributor which responsibilities belong where, which interactions are allowed, and which constraints a change must preserve. It carries the accepted decisions needed to apply these skills without reconstructing past conversations. Its organization follows the project's needs; no particular table or section structure is required.

## Create the missing project binding

Locate `architecture-map.md` in the project these skills are being applied to. Use its established location when known; otherwise create `.agents/architecture-map.md` under that project's root. Resolve this path from the target project, even when the skills are installed globally or in a separate repository.

If the file is missing, create it using the investigation and standards below. Recover a previous version from available history when possible, checking it against later accepted decisions and current project context. For a new map, examine the project's meaningful architectural boundaries and supported constraints, not only the immediate edit. Scale the coverage to the actual project; do not invent decisions to fill a template. Record unresolved areas explicitly and continue with supported material. The output is a written project map, not merely a task-local summary or a proposal to document it later.

## Recover the decisions and their scope

Find existing guidance before creating another document. Consult the maintainer's explicit requirements and corrections, adopted design records, current task specification, and available history of a missing map. Use the latest applicable accepted decision when an earlier one has been explicitly superseded. A question, exploratory alternative, generated summary, or bug report does not become a requirement merely because it mentions a design.

Inspect the actual owners, callers, state, and affected consumers for each area being mapped. Trace representative operations far enough to distinguish configuration, component policy, shared mechanics, and orchestration. Check absent-configuration behavior and boundary cases where they determine ownership. Names and directory placement alone do not establish these responsibilities.

Distinguish accepted design from observed implementation. Code and tests establish what exists and what currently happens; they may still violate an accepted constraint. Preserve that distinction when recording migration needs. Conversely, a pattern suggested by this skill does not prove that the project adopted it. State which source supports a non-obvious decision, with a nearby reference when available; retain enough of the decision in the map to apply it without reading the whole discussion.

If evidence is missing or conflicting, record the specific uncertainty separately from definite constraints. Continue documenting supported decisions. Ask about an unresolved choice only when it changes ownership, behavior, or another material boundary. Describe the competing interpretations with a concrete operation rather than asking the maintainer to choose a pattern name. A plausible replacement is not an accurate recovery of a lost decision; say which parts remain unrecovered.

## Write constraints that decide real changes

For each accepted decision, make its application and consequences clear in the form that reads best:

- Identify the area or condition that activates it, using verified project terms and actual owners where known.
- State the responsibility, invariant, or interaction that must be preserved. Include the condition and expected outcome when behavior determines the architecture.
- Preserve qualifications and neighboring cases that would otherwise be mistaken for the same rule. Explain a rejected mechanism when it exposes a real boundary; pair it with the required behavior.
- Include the reason or a concrete example when the rule would otherwise be ambiguous or easy to overextend.

These are questions the writing must answer, not mandatory fields for every entry. A short paragraph may settle several of them. Keep related qualifications together instead of scattering exceptions across the document.

Bind general principles to project decisions rather than copying the skills. "Keep policy with its owner" is insufficient where a contributor could reasonably choose either the configurable component or the core operation. Identify which owns the operation and why. Preserve independently meaningful responsibilities even when one type or file currently holds several of them. Naming an owner in the map does not authorize introducing a new type.

Record execution order only where outcomes require it. Distinguish a stable state read from consumption of intermediate output or a finalized outcome. Where contributions overlap, state their actual combination and conflict semantics, including absence and no-op behavior when meaningful. Do not transfer one operation's merge rules to every interaction.

Scope representation and performance constraints to the operations and bounds that justify them. For example, an accepted make/unmake probe must restore all affected state on every exit path; its whole-state-copy prohibition does not automatically cover small values, test snapshots, or unrelated operations. Do not introduce reversible mutation merely because another project's probes use it.

## Preserve meaning through exceptions

Suppose an accepted design says a batch writer flushes even when optional settings are absent. Settings hold a limit; the writer owns flushing and the absent-settings default. A faithful mapping preserves those facts and any accepted restriction against adding a separate settings behavior interface. It does not infer that every optional component is only configuration: a retention component may contribute behavior through an established protocol.

Likewise, preserving meaningful stages on an existing transaction does not license a helper for each expression. A neutral default hook can be valid in an adopted shared lifecycle without requiring unrelated components to implement that lifecycle. Include such distinctions where the project's decisions depend on them, rather than reproducing every example in this reference.

## Keep one maintained account

Maintain `architecture-map.md` at the location identified above. Where architectural decisions already have authoritative records, summarize their implications for this project binding and link to those records instead of creating competing versions. Keep durable constraints separate from proposals, task progress, temporary verification deferrals, and migration status. A required target design may be documented before migration, but documenting it does not authorize that migration.

Include project-specific verification boundaries when they prevent false completion claims: which external consumers share a changed contract, which feature combinations matter, or which prerequisite limits a check. Look up commands and manifests in the project instead of inventing or needlessly duplicating them. Commit conventions and other workflow rules may remain in an existing combined document; they are not prerequisites for an architectural mapping.

When updating, preserve unrelated accepted decisions and their qualifications. Replace superseded claims rather than leaving contradictory layers. Update the project's existing discovery pointer if needed so future contributors can find the maintained guidance.

## Check the result against decisions and examples

Review every definite constraint against its supporting requirement and actual project context. For each mapped boundary, trace a representative change and, where an exception matters, a nearby case that should be treated differently. Check that the map identifies the intended owner, preserves the required outcome and ordering, and neither permits the rejected design nor prohibits the legitimate neighboring case.

Confirm that every accepted decision within the requested scope is represented, directly or through a clear reference. Verify named code locations and documentation links. Mark a partial reconstruction as partial; do not fill gaps with inferred policy to make the document look complete. Confirm that `architecture-map.md` exists in the target project and contains the recovered constraints, with remaining uncertainties visible. A contributor must be able to apply it without conversation history. The map's length or resemblance to another project's format is not a completion criterion.
