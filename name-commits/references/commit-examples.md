# Scope and naming examples

These are illustrative decisions, not historical subjects or naming templates. Each example supplies its own context and works without access to another project or conversation.

## Start from the change, even with no history

A new command-line project has a settings loader, its package definition, an executable entry point, focused tests, and usage instructions. Together they implement the first usable capability. One commit named `feat: load settings from files` is appropriate. Splitting merely by manifests, source, tests, and docs would separate required parts of that capability. A build-only foundation can be a separate first commit if it is useful on its own and the feature follows as a complete second stage.

If those files only establish an empty package that builds, `build: initialize command-line package` describes the smaller result. Name the actual foundation rather than claiming a feature merely because the repository is new. A documentation-only repository could instead begin with `docs: describe deployment procedure`; it needs no invented build setup.

## Separate independent semantics and representation

A cache patch changes expiration behavior and replaces heap-allocated entries with packed storage. If both changes are independently meaningful, separate them:

- `fix: expire entries after inactivity`
- `perf: pack cache entries`

Each includes its relevant tests. If the storage bound depends on the new expiration rule, establish and verify that rule first, then apply the representation change. Shared files do not require a single commit; dependencies do require a valid order.

If the optimization instead exists solely to make one new capability feasible and cannot sensibly stand on its own, keep the necessary implementation together. The criterion is coherent purpose, not a mechanical ban on mixed kinds of edits.

## Keep one migration complete

Moving request validation into the request owner touches the implementation, twenty callers, adapter tests, and API documentation. `refactor: move validation to requests` names the common operation. One commit can be clearer than splitting by directory and leaving callers broken between commits.

A separate change to contributor instructions does not implement that migration. Give it its own documentation commit if it changes an independently meaningful policy. Documentation explaining the migrated API belongs with the migration.

## Select a prefix by purpose

| Proposed change | Suitable subject | Reason |
|---|---|---|
| Wire an optional compression feature through package manifests | `build: expose compression feature` | The purpose is feature configuration, not new compression behavior |
| Add a hash dependency while replacing repeated parsing in an existing lookup | `perf: hash lookup keys` | The dependency supports the measured optimization and stays with it |
| Install a local profiling command | `dev: add allocation profiler` | The result is a developer tool |
| State who owns plugin validation | `docs: define plugin validation ownership` | The result is written design guidance |
| Remove obsolete fixtures and their unused configuration | `chore: remove obsolete fixtures` | Cleanup is the purpose; no new behavior or structural redesign is introduced |
| Normalize import layout as a separately requested cleanup | `style: normalize imports` | The convention change is the task itself |
| Add an independent parser regression corpus | `test: cover nested expressions` | Test coverage is the substantive change |
| Run the existing checks on pull requests | `ci: run checks on pull requests` | The change is when automation runs |

Adding one regression test while fixing nested-expression parsing normally belongs in the `fix` commit. An explicitly requested test-first sequence may put the reproducer first and identify its expected failure; do not infer permission for that failing intermediate state from an ordinary request to split commits. Installing an agent tool may be `dev`, while editing its written rules may be `docs`; choose the main purpose rather than the `.md` extension.

## Keep enough detail without narrating the patch

Suppose failed disk writes currently discard queued jobs. The fix preserves those jobs for another attempt.

- Too vague: `fix: queue improvements`
- Precise: `fix: retain queued jobs after failed writes`
- Too detailed: `fix: replace drain with iteration in flush and add disk failure tests`

The precise version states the behavior and the condition needed to identify it. It neither hides the failure case nor lists implementation and test work.

For a change that shares immutable lookup tables, `perf: share lookup tables` identifies the mechanism. A measured percentage belongs in the report. If only one kind of table changes and the distinction matters, name that kind. Do not broaden a localized improvement into `perf: optimize runtime`.

## Amend the change that owns the correction

The current tip changes storage, but the requested correction concerns a parser refactor two commits earlier. Inspect the earlier patch and amend it within the requested rewrite. Preserve the later storage change. The shared filename is not evidence that the correction belongs to the tip.

When the user requests amendments for formatting omitted from several issue commits, assign each formatting hunk to its owning change. An extra `style: format changed code` commit would not fulfill that request. If instead the accepted workflow preserves published history, prepare a follow-up within the authorized scope; do not rewrite it solely to enforce a preference for tidy history.

A request to rename a vague subject changes the message only. It does not authorize opportunistic code fixes, rearranging unrelated commits, or changing their authorship.

## Respect the requested boundary

A user asks for implementation and proposed commit names. Prepare the changes and names; do not commit yet. If the user instead requests a commit per issue, that authorizes the specified sequence without another approval at each issue. A later unrelated task needs its own authorization to commit unless an existing instruction explicitly covers it. Missing signing setup blocks the actual commit, not inspection or proposing a title.

These decisions come from the task and patch. Existing history can help locate an amendment or clarify a required convention, but a short, misleading, or accidental past title is not a standard to copy.


## Route overlapping labels by the result

Correcting a misleading API explanation is `docs`, even when the explanation is a source comment. Wrapping that same explanation without changing its meaning can be `style`. If the clarification is necessary to understand an API fix being delivered, keep it with the `fix` instead of making a documentation fragment.

Removing an unused helper can be `chore`; consolidating active helpers around a new owner is `refactor`. If the supposedly unused helper was being called incorrectly and its removal repairs behavior, name the fix. Deletion alone does not determine the prefix.

A change that eliminates an allocation on every request has a directly inspectable storage-cost improvement and may use `perf`. Replacing one search algorithm with another does not establish an actual latency improvement by itself; measure the relevant workload before claiming one. If only organization is established, describe that change under `refactor` and keep the performance hypothesis in the report.

A standalone benchmark addition can be `test: add parser benchmarks`. This does not conflict with keeping measured percentages out of an implementation title: the benchmark is the work being named, not supporting evidence listed in the title.
