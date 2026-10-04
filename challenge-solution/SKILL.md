---
name: challenge-solution
description: Challenge a proposed solution against its contract, credible alternatives, affected cases, and evidence before implementing or accepting it. Manual invocation only.
disable-model-invocation: true
---

# Challenge the solution

Apply only when explicitly invoked as `$challenge-solution`. Use it throughout the requested task, including revisions, without requiring another invocation for each correction. A review request authorizes assessment; implementation and repository operations retain their existing authorization boundaries.

Read [Credo](../SKILL.md) and its relevant focused skills. They own architecture, construction, explanations, verification, and commit rules. This skill governs when to reconsider a choice and what evidence makes it ready. Use the current project's vocabulary and accepted requirements. Keep the process proportional to the decision; routine work can satisfy it with a short source comparison and final inspection.

Before substantive edits, establish the contract and challenge the choice below. For work already underway, evaluate the current solution as the candidate; preserve valid work and revisit only decisions affected by the findings.

## Establish the contract

Identify the intended result, behavior to preserve, responsible owner, affected consumers, and current user constraints. Separate accepted requirements from observations, hypotheses, and proposals. Keep these distinctions in the existing task record when one is needed; a new report is not required.

For a regression or migration, inspect the relevant earlier behavior before choosing the replacement. Compare the public operation, its inputs and outputs, and meaningful variant or lifecycle differences. Current requirements override obsolete behavior. If historical evidence is unavailable, state the specific uncertainty and use the strongest available contract evidence.

Determine what the reported example represents. Inspect the actual catalog, callers, or shared path to find the other cases governed by the same contract. Bound that set by the requested outcome. A shared symptom alone does not justify grouping unrelated causes or expanding into a redesign.

Proceed when the intended result and scope are supported, and any remaining uncertainty is either immaterial to the decision or explicitly identified for resolution.

## Challenge the first choice

Treat the first plausible solution as a candidate. Investigate the strongest credible alternative that could improve correctness, ownership, simplicity, or effort. Look for an existing operation, a correction at the producing boundary, removal of redundant state, or a more direct expression of the same behavior. A small wording task needs comparison of meanings, not an architecture exercise.

Compare candidates on the same representative input and required result. Determine which owner changes, which consumers must change, and which assumptions each candidate introduces. Reject a smaller patch if it leaves the contract inconsistent; reject a broader design if its extra machinery serves no accepted requirement. When no credible alternative exists, establish why the existing contract determines the change rather than inventing a weaker option for comparison.

Choose a discriminating case: an affected input, consumer, or transition for which the leading candidate could give the wrong result. Inspect or check that case using permitted methods. Resolve the assumption that changes the decision, rather than accumulating unrelated evidence. A passing check of the reported example alone does not establish coverage of its siblings.

If the choice depends on unresolved domain meaning, present the smallest concrete example and ask the question that decides it. Continue independent work. Routine implementation choices remain the agent's responsibility. When the correct fix belongs outside the authorized scope, explain the owning defect and proposed change; obtain the needed decision instead of silently compensating in another layer.

Select the solution when it meets the contract, survives the discriminating case, and the credible alternatives provide no material advantage under the accepted constraints. Repeat this comparison when evidence changes the decision. Stop when further investigation would not change the choice or resolve a material uncertainty.

## Carry corrections through the work

When the user corrects a result, identify which assumption failed and the intended scope of the correction. Apply that correction to every affected item in the authorized scope, including later work in the same task. Distinguish a general preference from a local exception or a newly changed requirement. If scope remains materially ambiguous after reading the context, ask one precise question.

Revisit decisions that depended on the rejected assumption. Remove superseded work introduced for it where authorized. Reuse evidence that remains valid and repeat checks whose conclusions changed. Apply accepted commit and amendment preferences through [name-commits](../name-commits/SKILL.md); this invocation grants no additional history-rewriting permission.

For names and prose, establish the intended meaning before searching for shorter words. Compare phrasing against the actual operation and established terminology. Retain the subject, action, and conditions that distinguish the change. Read the result without the surrounding conversation: the intended reader should understand the decision without supplying missing context. Follow the requested format and reader's expertise rather than adding explanatory ceremony.

## Accept the actual result

Review the final artifact against the contract, not just the patch or latest example. Account for the affected cases, consumers, ownership, and user constraints identified above. Inspect completed check results before committing or reporting completion. Honor restrictions on tests, reproduction, tools, and edits; use permitted evidence and state exactly what remains unverified.

For each material completion claim, identify its supporting observation. Type checking supports type correctness; behavior and architectural claims require their own evidence. Separate what was proposed, implemented, inspected, and exercised. Correct a known in-scope defect before declaring success, or state the concrete blocker and remaining work.

Return the outcome, the decisive reason for the choice when useful, and material verification limits. Keep discarded exploration out of the final answer unless it explains a real tradeoff. Completion rests on satisfied requirements and evidence, not a self-assigned quality score.
