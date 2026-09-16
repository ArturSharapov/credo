# Naming illustrations

These selected, accepted subjects illustrate concise naming. They are optional examples, not instructions to look up a repository, diff, or conversation. Project terms belong to the examples; use the relevant vocabulary of the current task. All rules for scope, grouping, and detail are stated in the skill itself.

| Subject | Naming quality illustrated |
|---|---|
| `perf: use xxh3 hashes for repetition` | Names the chosen mechanism and its purpose |
| `refactor: use bottom-origin coordinates` | Names the convention unifying a broad change |
| `fix: normalize zombie choices` | Identifies a behavioral correction |
| `perf: pack zombie choices` | Distinguishes a storage change from the behavioral correction |
| `perf: construct fen errors lazily` | Names the operation and cost-saving mechanism |
| `refactor: inline rule slot parsers` | Names a shared refactoring without listing files |
| `build: declare metadata feature in vwasm` | Identifies build configuration and its destination |
| `docs: allow state-only rules without extra traits` | States the architectural guidance being changed |
| `docs: restore comment guidance` | Identifies the specific documentation change |
| `dev: comments skill` | Uses a short noun phrase where it identifies the work clearly |
| `fix: retain history on rejected moves` | Retains the condition that distinguishes the repaired contract |

For context, `fen` is a position notation and `zombie choices` are ordered choices in a game feature. Their domain does not constrain the skill. A different project might instead use `perf: construct parser errors lazily` or `perf: pack cache entries`.

The paired choice subjects demonstrate distinct naming purposes; they do not establish whether an unfamiliar patch must be split. Apply the skill's purpose and dependency criteria to the actual changes. Likewise, a broad title does not authorize unrelated changes, and a short title is not automatically precise.
