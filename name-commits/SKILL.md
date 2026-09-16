---
name: name-commits
description: Choose commit boundaries and concise, precise subjects when drafting names, grouping changes, or making authorized commits, amendments, and history renames. Works without an existing commit history.
---

# Make each commit a clear unit of change

Derive scope and wording from the accepted task and actual changes. Repository history, a particular author, and earlier conversations are not prerequisites. Existing subjects are fallible examples, not authority to repeat a mistake.

Distinguish the requested action before proceeding. For names or a proposed split, inspect the changes and return the proposal; do not stage, commit, or rewrite history. For an authorized commit or amendment, also apply the execution checks below. An instruction to commit a specified series covers that series without repeated approval. Permission to implement, investigate, or prepare names alone does not authorize commits.

Explicit user instructions govern the title, grouping, and permitted operations. Otherwise apply documented project requirements and this skill's defaults. Choose between ordinary alternatives using the criteria below; ask only if a missing requirement or a real conflict would change the accepted result or authorized operation. A preference found in old commits is not a mandatory project convention.

## Choose boundaries by purpose and dependency

Inspect staged and unstaged changes and relevant untracked files. For each group, identify its intended result, required supporting edits, and dependencies. Keep unrelated work outside the authorized scope.

Use these decisions rather than a file-count or patch-size rule:

| Situation | Direction |
|---|---|
| Edits jointly establish one behavior, contract, or migration | Keep the implementation and required callers, configuration, tests, and docs together. Do not leave a consumer broken merely to make a smaller patch. |
| Changes have distinct results that can be reviewed or retained separately, given their prerequisites | Split them. Shared files and implementation in one session do not make them one purpose. |
| A larger change has a useful prerequisite stage | A separate refactor, compatibility layer, or setup commit is reasonable if that stage works without unfinished later edits and has its own explainable purpose. Order dependent commits accordingly. |
| Splitting would produce broken states or a fragment with no purpose except to complete the adjacent patch | Combine the tightly coupled edits. If a compatibility stage enables a useful split, use it only when that staging is part of the accepted work, not solely to manufacture more commits. |
| A behavior correction and an optimization are independently useful | Usually separate them, with the required tests for each. If the optimization is inseparable from delivering the same capability, keep it with that capability. |
| Tooling or policy changes arise alongside product work | Separate independent workflow changes. Keep supporting configuration or guidance with the product change when it is necessary to deliver or correctly use that change. |
| Formatting or local naming adjustments are incidental to an implementation change | Include them with that implementation. An independently meaningful conventions cleanup within the authorized scope may stand alone. For omitted formatting in existing commits, use the authorized amendment or follow-up route below. |

One commit for a whole working diff is valid when it has one purpose. One commit per file is valid when the files happen to represent separate purposes. Neither arrangement should result merely from file boundaries. A title that needs several unrelated actions suggests a split; related operations or an explicitly grouped task can legitimately use `and`.

In a new repository, choose a usable initial unit for the task. A build-only foundation can be one commit; a first feature can include its build setup, tests, and usage docs. Several independent capabilities can be separate ordered commits. A documentation or data repository does not need invented build machinery. Name what the initial commit actually establishes; no historical style sample is needed.

For worked decisions, read [scope and naming examples](references/commit-examples.md). Optional [naming illustrations](references/illustration-examples.md) show useful title detail without requiring project history.

## Choose a prefix for the main purpose

| Prefix | Purpose |
|---|---|
| `feat` | Add a capability or deliberately change the accepted product behavior |
| `fix` | Repair a violation of an existing intended behavior or contract |
| `refactor` | Reorganize implementation, ownership, or internal interfaces while preserving the externally intended behavior |
| `perf` | Reduce computation or storage costs |
| `build` | Change how artifacts are built, packaged, configured, or supplied with dependencies |
| `docs` | Change explanatory content, specifications, written policy, or documentation organization |
| `style` | Change presentation or conventions without changing the meaning or behavior they express |
| `dev` | Change local development tools or working procedures |
| `chore` | Usually cleanup and routine housekeeping, such as removing obsolete files, unused code, or stale configuration |
| `test` | Make independently scoped changes to coverage, fixtures, or test infrastructure |
| `ci` | Change automated integration or delivery workflows |
| `revert` | Deliberately undo an earlier change |

The list is open. Use another established or plainly descriptive prefix when none describes the work accurately. Do not create a separate commit for each prefix present among supporting edits. A manifest or regression test that supports a fix normally stays in the `fix` commit.

When purposes overlap, use the distinguishing result:

- `fix` versus `feat`: restoring the accepted contract is a fix; introducing a new contract or capability is a feature. Determine that from requirements, not from whether new code was added.
- `chore` versus `refactor` or `style`: removing obsolete remnants is cleanup; reorganizing responsibilities is refactoring; changing layout or conventions is style. Name the actual cleanup, such as `chore: remove obsolete fixtures`. Small size alone does not make a change a chore.
- `docs` versus `style`: changing what an explanation or instruction says is documentation work, even inside a source comment; changing its layout or punctuation without changing meaning can be style. A clarification necessary for a behavior fix can remain in that fix.
- `build` versus `dev` or `ci`: producing or configuring deliverable artifacts is build work; improving local developer tools is development work; changing when or how shared automation runs is CI work. Use the primary result when one change affects several of these.
- `docs` versus `dev` for agent tooling: changing written decision rules is documentation; installing or integrating a tool or workflow may be development work. The file extension does not decide.
- `test` versus `chore`: adding or changing test behavior or coverage is test work; removing obsolete test assets without changing coverage can be cleanup. A regression test and its fix normally belong together; an explicitly requested test-first series may keep the reproducer separate.
- `perf` versus `refactor`: use `perf` for a supported cost improvement. A demonstrably removed allocation or redundant operation can supply evidence; runtime-dependent speed claims need suitable measurement. Do not require a new benchmark for every obvious cost reduction, and do not claim a speedup from structure alone. If evidence remains unresolved, describe the structural change as such and report the unverified performance expectation separately.

If two prefixes remain accurate, choose the one that makes the purpose clearest in this task. Consistency with explicit project conventions can break a tie; do not block work to obtain a preference over equally accurate labels.

## Name the change at the right level

Default to one lowercase `type: short phrase` subject without a final period. Preserve actual identifier spelling and established acronyms. Prefer a direct action and concrete subject; a concise noun phrase is valid if it identifies the change just as clearly.

Check the title against the patch:

1. Does it identify the specific operation or result and what it affects? Prefer `fix: retain queued jobs after failed writes` over `fix: improve queue handling`.
2. Does it retain the condition or mechanism needed to distinguish the change? Add that detail when omission would hide the repaired case or misstate the scope. Omit incidental implementation steps.
3. Does its breadth match the commit? Name a migration's common operation instead of listing every subsystem; do not label one localized edit a redesign of the entire system.
4. Does it describe what this commit establishes, rather than a later goal or abandoned approach? For policy changes, name the changed rule. For added or reorganized documentation, name the content or organization change; do not force it into a policy-change formula.

A useful subject should let a maintainer distinguish the change without opening the patch. This is a precision test, not a fixed word or character quota. Use familiar domain terms and unambiguous abbreviations. A filename, function, algorithm, or tool can belong in the title when it is the actual subject or distinguishing mechanism; avoid inventories of incidental files and expressions.

Keep test/check lists, benchmark figures, implementation narratives, and promotional claims in the report or review. Tests or benchmarks themselves can be the subject when changing them is the task. Avoid vague continuation titles such as `more stuff`, `update code`, or `address feedback`.

The default message contains the subject only: no description body, coauthor footer, or unsolicited trailer. Add a scope, ticket marker, body, or required trailer only when the user explicitly requests it or an applicable project requirement calls for it. Follow an exact user-supplied title; do not silently restyle it. If the supplied title cannot satisfy a mandatory project requirement, identify that specific conflict and offer a concrete compliant alternative. Continue independent preparation, then ask which authorized direction to take before committing; do not silently alter the title or bypass the check.

## Execute only the authorized operation

For a new commit, stage the intended paths or hunks and inspect the staged patch for omitted counterparts and unrelated changes. For a proposed title or split, inspection alone is sufficient; do not stage merely to satisfy this procedure.

Select checks for the affected behavior and interfaces. Documentation-only work needs content/link checks where relevant, not a mandatory runtime test suite. Reuse verification while its content and dependencies remain unchanged. For a split series, assess each intermediate state for issues introduced by the split. Ordinary implementation stages should work without later repairs; an explicitly requested failing regression-test stage is a valid exception and must be identified as such. Report unavailable checks or pre-existing failures accurately; they do not authorize unrelated repairs or a claim that everything passed.

Choose the history operation from the authorization:

- A request to commit current changes normally creates a new commit. It does not automatically authorize rewriting earlier commits.
- An amendment request requires locating the owning commit by its patch and checking affected descendants. The target is not automatically the tip. For requested formatting amendments, assign the formatting hunks to their owning changes.
- When rewriting is not authorized or the accepted workflow preserves existing history, retain it and use an authorized follow-up commit. Do not make an extra formatting commit when the instruction was to amend the originals.
- A message-only rename preserves trees, authorship, and untargeted messages. Changed commit IDs and renewed signatures are expected consequences; opportunistic code edits are not.

Create signed commits using the authorized author and configured signing setup. Preserve original authorship during amendments unless explicitly told otherwise; the committer records the identity performing the operation. Do not infer a person's identity from a desired writing style or change global Git settings to make a commit succeed. If required identity or signing setup is unavailable, keep the prepared work intact and report that blocker for the commit operation. Naming and grouping can still be completed. Follow an explicit override of the signing requirement rather than silently choosing one.

After execution, verify the actual message against the chosen convention, the committed patch, author/committer, signature when required, and remaining worktree state. Report the short hashes and subjects plus material verification limits. Unrelated remaining changes are not a failure to be cleaned up; report them and claim a clean worktree only if checked.
