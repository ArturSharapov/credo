---
name: credo
description: Apply the maintainer's engineering principles when implementing, refactoring, reviewing, or explaining code, choosing architecture, comparing performance, verifying changes, or preparing commits. Route work through Credo's focused skills and the target project's architecture-map.md.
---

# Credo

Use this entry point to select the skills for the requested work. The linked skills own the decision rules; this file defines when to read them.

## Select the workflow

Read each matching skill before its part of the work. Several rows may apply to one task. Reuse skills already read in the current context, and load additional ones when the work changes. Follow their conditions for loading supporting references.

| Work | Read |
|---|---|
| Implement or refactor code | [construct-code](construct-code/SKILL.md), then [verify-changes](verify-changes/SKILL.md) for verification and completion |
| Choose or change ownership, interfaces, representation, modules, or execution order | [select-architecture](select-architecture/SKILL.md) |
| Create, reconstruct, or update the target project's `architecture-map.md` | [select-architecture](select-architecture/SKILL.md) |
| Review code or a proposed code change | [construct-code](construct-code/SKILL.md) for construction criteria and [verify-changes](verify-changes/SKILL.md) for evidence and acceptance; add the architecture, explanation, or performance skill when those concerns are in scope |
| Add, change, or review behavioral blocks for visible flow and missing or misleading comments; explain code, interpret feedback, or revise names, comments, explanations, or review records | [explain-code](explain-code/SKILL.md) |
| Compare runtime or storage costs, or evaluate an optimization | [measure-performance](measure-performance/SKILL.md) |
| Propose commit subjects or boundaries, or perform authorized commits, amendments, or history renames | [name-commits](name-commits/SKILL.md) |
| Plan checks, assess completion, or maintain and resume a task's cross-session record | [verify-changes](verify-changes/SKILL.md) |

Select by the requested activity. A question about a function calls for explanation; a request to change it also calls for construction and verification. For a review, use the linked skills' evaluation criteria without treating their implementation steps as a request to edit code. The commit skill distinguishes naming and grouping proposals from authorized repository operations.

The [issue-findings](issue-findings/SKILL.md) skill is manual-only. Read and apply it only when the user explicitly invokes `$issue-findings`; do not select it automatically through Credo.

## Resolve project and skill paths

Skill links above are relative to this file. The target project's `architecture-map.md` belongs to the project being worked on, even when Credo is installed elsewhere.

Before code changes, read that map at its established location, normally `.agents/architecture-map.md`. If it is missing, route to [select-architecture](select-architecture/SKILL.md) to create or reconstruct it before implementation. That skill owns the map's evidence, scope, and maintenance rules.
