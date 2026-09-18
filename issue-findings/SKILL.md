---
name: issue-findings
description: Investigate issues, compare solutions, and write numbered issue reports with Before, After, and Best solution blocks.
disable-model-invocation: true
---

# Issue findings

Produce a report the reader can understand without the source conversation. Read the [writing examples](references/writing-examples.md) and [report template](assets/report-template.md) before drafting. The fictional examples demonstrate phrasing and level of detail; the template defines the required Markdown structure. Derive names, paths, versions, findings, and checks from the current task. Transfer the examples' writing choices, not their technical recommendations or test claims.

Apply Unslop to the prose and Credo to engineering explanations and solution choices. Read their available skills and the focused Credo skills relevant to the task. The report rules below capture the maintainer's requirements independently of those skills. Explicit user changes take precedence over this template.

## Establish the findings

For a new investigation or changed recommendation:

1. Read the supplied messages, reports, and relevant project documentation. Treat outside diagnoses, proposed APIs, and test claims as leads. Reconstruct each issue from the code and observed behavior. Use plain issue names, not the source author's codes or reporting categories.
2. Inspect the current implementation, its callers, configuration, dependency versions, and affected consumers. Read the project's architecture map when evaluating ownership or interfaces. Distinguish accepted architecture from proposed changes recorded there.
3. Establish the concrete trigger, affected component, actual result, and intended result. Verify version-sensitive APIs against declarations, implementation, or primary documentation. If a claim cannot be confirmed, state what is missing rather than converting it into a fact.
4. Compare the first plausible fix with a simpler alternative. Look for an existing owner, native API, or update path before proposing another abstraction, dependency, configuration language, or state store. Prefer the smallest change that handles the verified cases; fewer changed lines alone do not make a fix better.
5. Challenge the leading solution with the cases it could break. Check affected consumers, optional dependencies, lifecycle, persisted data, and compatibility where relevant. Use focused reproductions, type checks, builds, or browser checks to resolve specific uncertainties. Do not invent a fixed number of alternatives or run unrelated checks to pad the report.
6. Finish with a recommendation supported by those checks, or state the unresolved choice and the exact check needed to decide it. Distinguish inspected code, simulated behavior, and actual integration results. A successful prototype is not a deployed feature.

A report request authorizes investigation and documentation, not implementation of its recommendations. Keep experimental changes isolated. For a wording or formatting edit, preserve established findings and qualifications; investigate again only if a factual change or contradiction requires it. Preserve the user's manual edits outside the requested changes.

## Required report structure

Use the template literally for heading levels, numbering, separators, label spelling, and order. Replace its placeholders with findings. Repeat the issue block for the actual number of issues. The investigation's paragraph order and length are flexible; arrange the material so each finding is easy to understand. Include comparisons where they explain the chosen recommendation, without manufacturing alternatives to fill a paragraph.

- One sentence-case H1 names the subject. A short introduction identifies the inspected revision and relevant versions. State whether changes are proposed or implemented in a bold sentence. Include only facts established for this report; omit unavailable metadata or state that it was not recorded.
- Place `---` before the first issue and between issues. Number issue H2 headings consecutively: `## 1. Concrete problem`.
- Start each issue with description and investigation paragraphs. Explain the example or trigger, mechanism, comparison results, and remaining verification limits. Use a flat list when comparing alternatives is easier to scan.
- Place `---` between that description and the final three labeled blocks.
- Use exactly `**Before:**`, `**After:**`, and `**Best solution:**`, each on its own line, in that order. These are bold labels, not H3 headings. Separate blocks with blank lines. Put a blank line before a list or code block.
- Everything after `**Best solution:**` belongs to the solution, including affected-file links. End the issue there; do not append another analysis or evidence section.
- After the last issue, use the unnumbered H2 `## Verification and sources`. Record shared verification context and report-wide outstanding checks, then link primary references and source material. Keep issue-specific results in their issues.

Do not add executive summaries, status tables, severity taxonomies, evidence codes, or extra issue subsections unless requested. Ordinary issue numbers are navigation, not identifiers such as R1, D2, or E3.

## Give each part one job

**Description** explains what the investigation established. Name the relevant component and the conditions that expose the issue. Describe why a rejected alternative fails with its actual result, rather than an abstract argument about design quality. State precisely what a check proved and what remains untested.

**Before** states the current behavior and its concrete consequence. Include the affected subject and condition so the reader does not need the source messages to understand it. Keep test history and proposed implementation out of this block.

**After** states the observable result if the recommendation is implemented. Include material compatibility or behavior limits. Do not imply the change already exists, promise results beyond the checks, or repeat implementation steps.

**Best solution** specifies what to change and where. Name the responsible module, existing API to use, input/output changes, lifecycle or ordering requirements, and error handling where they affect correctness. Preserve necessary compatibility, data preservation, and configuration details. Use paragraphs for independent requirements and a numbered list for ordered steps. Link files at the point of use or in a final affected-files sentence.

Before and After may briefly restate the behavior explained above so each block remains understandable. Keep detailed evidence and comparison results in the description, and implementation instructions in Best solution. Repeating a component name for clarity is useful; repeating the full explanation is not. A cross-reference may identify a shared proposal, but must name the capability being reused so the reader understands the dependency.

For reports opened in the workspace viewer, use absolute filesystem targets for local file links. Resolve each target from the actual report directory and verify that it exists; the viewer may otherwise resolve it from a different starting directory. Derive the checkout path at runtime rather than copying a path from an older report. Use relative links only when the user requests a portable report for another renderer. Keep web references as full URLs.

## Language and phrasing

- Write as a colleague explaining a specific problem. Use plain English, active verbs, stable technical names, and sentence-case headings. Keep the tone friendly without chatty introductions or praise.
- Start with the concrete behavior or example that makes the problem understandable. Introduce implementation details as they explain that behavior. Keep each paragraph focused on one finding or one implementation concern; let its length follow the information it needs to convey.
- Name the actor and action: "The editor changes the same object the preview reads." Replace vague claims such as "state handling needs improvement" with the actual data flow or failure.
- State concrete causes and results. "The older request replaces the results of the current search" explains the issue. "This creates architectural complexity" does not explain which code or behavior changes.
- In recommendations, use direct verbs such as "Add", "Keep", "Pass", "Validate", and "Remove". Name the file, API, value, or condition instead of saying "handle this properly" or "make it robust".
- Preserve exact technical identifiers in backticks. Explain unfamiliar terms through what they do. Use the project's vocabulary consistently instead of cycling through synonyms or inventing compound labels.
- Give sentences one main point. Split dense clauses, but retain all conditions, exceptions, failure behavior, and verification limits needed to understand the finding. Brevity must not remove a detail that changes the recommendation.
- Use natural prose and flat lists where they help. Avoid abstract reasoning, slogans, sales language, formulaic transitions, forced groups of three, and generic conclusions. The required Before/After labels are an explicit format requirement, not permission to add more bold-label lists.
- Use straight quotes. In prose, separate thoughts with periods or commas rather than em dashes, en dashes, parenthetical asides, or hyphens used as dashes. Preserve punctuation inside code identifiers and Markdown links.
- Report uncertainty specifically: "The parser rejected the malformed record; importing a complete file remains untested." Do not dilute facts with repeated hedging or conceal missing checks behind broad claims of verification.
- Keep only comparisons that explain a real choice. Replace "the simplest consistent mechanism" with the existing method being reused and the concrete extra work another option would require.

## Check before saving

Read the report as someone who has not seen the conversation. For every issue, confirm that the reader can identify the problem, current behavior, intended result, exact proposed change, and verification limits without decoding labels or opening the source messages.

Check that every material finding and qualification survived the rewrite, facts agree across sections, and recommendations remain distinguishable from completed work. Check that comparison counts match the results described. Remove repeated explanations, vague nouns, and sentences that could fit any project unchanged.

Check consecutive issue numbers, separators, the three exact labels at the end of every issue, file-link targets, and removal of template placeholders. Include only checks actually performed and sources actually consulted. Save the requested Markdown file and return its link with a short description of the change.
