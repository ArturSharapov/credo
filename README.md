# Credo

Personal engineering principles for coding agents. Covers architecture, implementation, explanations, performance, verification, and commits across projects and languages.

Credo treats code design as part of correctness. It asks agents to keep behavior with its owner, justify abstractions through actual needs, preserve meaning in names and comments, and verify both behavior and structure. Its rules include concrete examples and exceptions so they apply to the responsibility at hand.

## Install

```sh
vpx skills add ArturSharapov/credo
```

## Use

Type `$credo` in your conversation with the agent. [SKILL.md](SKILL.md) routes the current task to the relevant skills. Invoke `$issue-findings` explicitly for issue investigation reports; it is manual-only.

## Skills

| Skill | Purpose |
|---|---|
| [construct-code](construct-code/SKILL.md) | Implement with meaningful ownership and justified function extraction. |
| [select-architecture](select-architecture/SKILL.md) | Choose responsibility boundaries, interfaces, representations, and execution order. |
| [explain-code](explain-code/SKILL.md) | Preserve terminology, conditions, and intended meaning in names, comments, and explanations. |
| [measure-performance](measure-performance/SKILL.md) | Compare implementations under the same workload and distinguish algorithm changes from storage choices. |
| [verify-changes](verify-changes/SKILL.md) | Check behavior, structural constraints, and affected consumers; report verification gaps. |
| [issue-findings](issue-findings/SKILL.md) | Investigate issues and compare solutions in the Before, After, Best solution format. Manual invocation only. |
| [name-commits](name-commits/SKILL.md) | Group changes by purpose, write precise subjects, and perform authorized commit operations. |

## Project context

Before code changes, Credo reads the project's `architecture-map.md`, normally at `.agents/architecture-map.md`. The map connects the principles to the project's actual owners, contracts, and constraints.

If missing, Credo creates or reconstructs it from accepted requirements and project evidence, recording unresolved decisions explicitly. The map stays in the target project and has no prescribed document format.

Some skill references use Rust examples to explain a design choice. The principles apply through the target language's own constructs.
