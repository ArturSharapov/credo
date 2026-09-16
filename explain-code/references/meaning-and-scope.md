# Meaning and scope examples

## Explain the action and preserve the condition

Requirement: a move between two generic locations must remain within a safe region for its entire path, including both ends.

Bad flow comment:

```rust
// Paths are safe. Resolve endpoints.
```

This invents a vague operation and loses the intermediate-path condition.

Good flow comment:

```rust
// Ensure the path stays within the safe region, including its start and end.
```

If a following block charges a cost only after validation, give that block its own action comment when the sequence is non-obvious. Do not comment every assignment or extract `ensure_path` solely to give the condition a name. Preserve the actual project terms when adapting this example.

A component definition comment has a different job: state the component's observable effect and its conditions. Do not fill it with parser storage details or implementation history.

## Short names can be more precise

In a scope containing two coordinates, `position` hides both the type and which value it means. `coord_from` and `coord_to` identify the distinction. In a loop with a single direction, `dir` is sufficient; expanding it to `direction` solves no ambiguity. Do not standardize every variable name to the same length or grammatical pattern.

## A question is not permission to redesign

Engineer: "Why is this called commit? Does it only record the patch?"

Bad response: rename it to `apply`, move mutation into it, and update its callers to fit the new behavior.

Good response: inspect it and explain whether it records or applies. If it only records, propose or make the scoped terminology correction as authorized, including corresponding names in fields and consumers. Keep semantics unchanged. If its behavior itself violates the task contract, identify that separately instead of concealing a redesign inside the rename.
