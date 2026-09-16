---
name: explain-code
description: Edit names, comments, design explanations, and review records while preserving the maintainer’s terminology, meaning, and natural phrasing.
---


# Preserve meaning while making code understandable

Use the maintainer's accepted vocabulary and the project's verified concepts. Existing generated names are not permanent vocabulary. Use short names when context supplies the subject, and qualifiers when two nearby values would otherwise be confused. Do not expand conventional short names merely to sound descriptive. Functions that perform work use verbs; a noun is appropriate for a pure query returning that noun. There is no fixed approved verb list.

A naming correction applies through the relevant uses of the concept, including local variables and constants. Keep distinct concepts distinct. Do not introduce synonyms such as "score" to explain a quantity already called "points" unless a real distinction exists. Avoid generic terms such as "resolve" when they conceal the actual operation.

## Match the comment's job

| Location | Explain |
|---|---|
| Component definition | Intended observable behavior and its conditions, independent of storage or notation |
| Public interface | What the caller provides/receives and the relevant contract |
| Multi-stage function | What each non-obvious behavioral block does at that point in the sequence |
| Representation constraint | The bound or layout dependency that makes the representation valid |
| Design record | Why this design was chosen, alternatives, and unresolved choices |

For a function requiring flow comments, cover each meaningful behavioral part, not a selective few; do not narrate every assignment. Phrase flow comments as the action performed, such as "Ensure ..." where that describes the code, rather than restating a static business rule. Do not extract functions merely to avoid explaining an inline block.

Preserve qualifications, concrete examples, deliberate emphasis, and human grouping of thoughts. Sentence count and line width do not define conceptual grouping. Never strip useful content to make prose shorter, or split every sentence into a new comment by default. Follow the project's formatter for code; do not invent universal comment punctuation rules from a local correction.

When the maintainer supplies a before/after edit, compare the semantic difference and apply that difference to the requested scope. Do not rewrite pre-existing good comments during a review of comments the agent just added. Do not insert design-history explanations, claimed performance benefits, or invented terminology into usage comments.

Before adding, rewriting, reformatting, or explicitly auditing comments, read [the maintainer's comment guidance](references/comment-guidance.md) completely. It preserves concrete examples of the maintainer's preferences; project-specific syntax and section conventions illustrate those preferences. Choose line breaks by meaning, never a generic wrapper or fixed column target. Keep a parameter's compact contract together; rephrase an unwieldy thought rather than breaking it mid-clause. Preserve deliberate brackets, emphasis, and IDE line-break markers.

Preserve established section landmarks and their intended grouping. Where the project uses multi-line separators before testing or formatting sections, retain that layout and spacing; restore missing separators in edited sections when the project convention requires them.

Read every changed comment with its surrounding code before finishing. Check that no qualification, detail, or intended interpretation changed, and that the landmarks remain intact.

When revising flow comments or interpreting a naming correction, read [meaning and scope examples](references/meaning-and-scope.md).

## Understand natural language

Listen for intended outcome, preserved design, forbidden mechanism, and scope. Recover a voice reference such as "that one" from the surrounding discussion and actual code, rather than guessing. Separate a question, tentative idea, explicit choice, and request to implement. Answer a question directly before continuing authorized work. Do not turn curiosity about an alternative into approval to adopt it.

For substantial ambiguous work, briefly restate the intended behavior and architectural implication using the maintainer's words and one concrete example. Do not require the maintainer to learn pattern names or fill out a long template. Make routine implementation judgments yourself; ask only about a choice that changes the accepted result or crosses a real unresolved design boundary.

In a live review, maintain the chosen record with the relevant code context. Do not transcribe every exploratory remark as an instruction. If asked to preserve exact remarks, preserve their phrasing and sufficient context to identify the requested change. Keep proposals separate from accepted decisions. Do not replace the user's specific concern with generic agreement or a broader redesign.
