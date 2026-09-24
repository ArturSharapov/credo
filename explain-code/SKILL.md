---
name: explain-code
description: Make code responsibilities visible through contextual flow comments; detect missing or misleading comments when implementing or reviewing code. Preserve meaning in names, comments, design explanations, and review records.
---


# Preserve meaning while making code understandable

Use the maintainer's accepted vocabulary and the project's verified concepts. Existing generated names are not permanent vocabulary. Use short names when context supplies the subject, and qualifiers when two nearby values would otherwise be confused. Do not expand conventional short names merely to sound descriptive. Functions that perform work use verbs; a noun is appropriate for a pure query returning that noun. There is no fixed approved verb list.

A naming correction applies through the relevant uses of the concept, including local variables and constants. Keep distinct concepts distinct. Do not introduce synonyms such as "score" to explain a quantity already called "points" unless a real distinction exists. Avoid generic terms such as "resolve" when they conceal the actual operation.

## Match the comment's job

| Location | Explain |
|---|---|
| Component definition | Intended observable behavior and its conditions, independent of storage or notation |
| Public interface | What the caller provides/receives and the relevant contract |
| Multi-stage function | The action and responsibility of each meaningful behavioral part at that point in the sequence |
| Representation constraint | The bound or layout dependency that makes the representation valid |
| Design record | Why this design was chosen, alternatives, and unresolved choices |

### Make the flow visible

Flow comments let a senior developer scan a function and recognize its responsibilities without first working through its implementation. When a function contains distinct phases or conditional paths, mark the meaningful behavioral parts where they begin. Straightforward code still needs a marker when it performs a distinct responsibility that would otherwise disappear into the surrounding work. A function-level summary or a distant comment does not label a newly inserted phase.

Cover EACH meaningful behavioral part of such a function, including nested branches that perform distinct work. Group statements that serve one responsibility under one comment; assignments, guards, and loop mechanics do not each need narration. A short function or a direct call whose name already makes its sole responsibility visible may need no flow comment. Judge by whether the reader can identify the work while scanning, not by line count or algorithmic difficulty. Do not extract functions merely to label inline blocks.

Describe the action performed here, including the relevant condition or ordering. Use concrete developer language such as "Remove expired reservations before assigning work" or "Ensure every segment stays within the boundary." A static rule or a vague label such as "Handle state" does not identify the workflow step. Explain why when that adds necessary understanding, but do not restrict comments to rationale: identifying what a block handles is itself a useful purpose.

Read the wording in its actual location. Name the affected concept when a general-purpose function handles several kinds of records, state, or operations; a word such as "entry" may be clear inside a dedicated parser and ambiguous inside a shared update routine. Use the established terminology and only the qualification needed to identify the action's subject. Keep conditions accurate to the code rather than implying a broader check or restriction.

Before adding, revising, or auditing comments, including checking whether comments are missing, read [comment guidance and examples](references/comment-guidance.md). The examples illustrate decisions, not required syntax, vocabulary, or layouts.

### Preserve meaning and structure

Preserve qualifications, concrete examples, deliberate emphasis, and human grouping of thoughts. Sentence count and line width do not define conceptual grouping. Never strip useful content to make prose shorter, or split every sentence into a new comment by default. Follow the project's formatter for code; do not invent universal comment punctuation rules from a local correction.

When the maintainer supplies a before/after edit, compare the semantic difference and apply that difference to the requested scope. Do not rewrite pre-existing good comments during a review of comments the agent just added. Do not insert design-history explanations, claimed performance benefits, or invented terminology into usage comments.

Choose line breaks by meaning, never a generic wrapper or fixed column target. Keep a parameter's compact contract together; rephrase an unwieldy thought rather than breaking it mid-clause. Preserve deliberate brackets, emphasis, and IDE line-break markers. When behavior changes, update the affected description to the new contract; preservation of meaning during a prose edit is not a reason to keep stale documentation.

Preserve established section landmarks, their grouping, and surrounding spacing. Restore missing separators in edited sections where the project's convention requires them; choose no universal separator design.

### Verify the affected flow

Before finishing implementation or review, scan each affected function in full. Can a senior developer locate its meaningful responsibilities and conditional paths from the comments and clear operation names? Check for missing markers as well as poor wording. Then read those markers against their blocks: each must identify the actual action, subject, conditions, and relevant sequence without narrating individual statements.

Check neighboring comments too, even when their text was unchanged: inserted, moved, or removed code may have changed what they appear to cover. Correct omissions and misleading scope within the affected flow; leave unrelated good comments intact. Compare every revised comment with its original for lost details, emphasis, grouping, or changed interpretation, and check that section landmarks remain intact. Formatting checks alone cannot establish this.

When revising flow comments or interpreting a naming correction or the scope of a requested change, read [meaning and scope examples](references/meaning-and-scope.md).

## Understand natural language

Listen for intended outcome, preserved design, forbidden mechanism, and scope. Recover a voice reference such as "that one" from the surrounding discussion and actual code, rather than guessing. Separate a question, tentative idea, explicit choice, and request to implement. Answer a question directly before continuing authorized work. Do not turn curiosity about an alternative into approval to adopt it.

For substantial ambiguous work, briefly restate the intended behavior and architectural implication using the maintainer's words and one concrete example. Do not require the maintainer to learn pattern names or fill out a long template. Make routine implementation judgments yourself; ask only about a choice that changes the accepted result or crosses a real unresolved design boundary.

In a live review, maintain the chosen record with the relevant code context. Do not transcribe every exploratory remark as an instruction. If asked to preserve exact remarks, preserve their phrasing and sufficient context to identify the requested change. Keep proposals separate from accepted decisions. Do not replace the user's specific concern with generic agreement or a broader redesign.
