# Maintainer guidance

## Contents

- Working interpretation
- Recognize a missing workflow marker
- Identify the action in its actual context
- Original restoration request
- Logical length and structure
- Encoding examples from the maintainer
- Prefer clear developer sentences
- Preserve details and emphasis
- Preserve brackets and logical hierarchy
- Formatting section separator
- Preserve judgment instead of mechanical rules
- Preserve original intention unchanged

## Working interpretation

Treat comment layout and wording as human communication. Read the code, understand the comment, and choose the form that makes that particular thought easiest to scan.

Do not optimize for the fewest lines, uniform width, one sentence per line, or literary English. Preserve good existing comments. Decide each comment from its own meaning, not wherever a formatter reaches a column. Where the project uses `Testing` and `Formatting` separators, preserve them as semantic landmarks immediately before their sections.

After a broad edit, inspect the comments as a developer rather than trusting a length search.

The prompts below are preserved verbatim as evidence of the maintainer's preferences. Preserve their lessons about meaning, detail, emphasis, and logical grouping. Their file-specific repair requests, domain notation, and exact separators apply to the original cases; they do not prescribe those conventions for other projects.

## Recognize a missing workflow marker

Suppose a scheduling routine already has a comment about selecting available workers. After selecting them and computing their eligible work, a newly inserted block removes unavailable workers from each reservation's eligible-worker set and drops reservations whose sets are now empty. The block uses ordinary filtering operations, but performs a distinct responsibility within the larger scheduling flow.

The earlier scheduling comment does not identify this cleanup phase. Add a marker immediately before it:

```text
// Remove unavailable workers from reservations, then drop reservations with no eligible workers.
```

This preserves both parts of the block's job and their order. "Update reservations" hides that work; separate comments for every filtering assignment would add noise. The reason for the marker is that a developer should recognize this responsibility while scanning the function, even when the code itself is straightforward. Look for the same omission in the other affected functions rather than fixing only the highlighted block.

## Identify the action in its actual context

Suppose a shared update routine changes resources, subscriptions, and cached results. A cache cleanup block currently says:

```text
// An old entry cannot survive relocation, removal, or replacement of its resource.
```

The comment does not identify which entries it concerns and states a constraint rather than the cleanup action. If those are exactly the conditions the code handles, write:

```text
// Remove cached results whose resources were relocated, removed, or replaced during the update.
```

The correction names the affected concept, describes the action, and preserves each invalidating case and its place in the update flow. "Remove stale entries" would lose both the contextual distinction and the concrete conditions. Inside a dedicated cache routine, less qualification may be sufficient; read the comment in its real location instead of repeating a subsystem name mechanically.

These examples illustrate workflow visibility and contextual precision. Adapt their terms and conditions to verified behavior; they do not prescribe a scheduling or caching design.

## Original restoration request

~~~~text
There are many comments like
`// ======================================================================================== // ======================================== Testing ======================================= //`
because a poor teammate transformed all comments one day in commit 4a43a3e to one-line using an llm and it transformed also the visual comments to one line. That was stupid poor.

Please fix the `// ======================================================================================== // ======================================== Testing ======================================= //`-like comments to the original and also comments like

```
    /// The direction this color's pieces advance toward. Defines which axis and orientation "rank" maps to on the board. Standard layout: White/Red south side (forward = North), Black/Yellow north side (forward = South), Blue west side (forward = East), Green east side (forward = West).
```
to the original
```
    /// The direction this color's pieces advance toward. Defines which axis
    /// and orientation "rank" maps to on the board.
    /// Standard layout: White/Red south side (forward = North),
    /// Black/Yellow north side (forward = South), Blue west side
    /// (forward = East), Green east side (forward = West).
```
It should be usually straightforward where to break the comment line, but don't exaggerate it.
Additionally, make sure that the testing-comments block preserves before all the testing-part of the files (it's for visual clear separation).
~~~~

## Logical length and structure

~~~~text
I still see long comments like here:
// The royal itself escaping via jump: a royal queen a8 boxed by its own rook a7 and bricks b7/b8; white Nc4-b6 gives a knight check (unblockable, and the rook can't capture the knight). Casting jump on the rook a7 lets the royal queen leap over it down the a-file to a6 (unattacked) — not mate. ── freeze lifts check / averts mate ─────────────────────────────────────

You have to double-check and make sure you fix the remaining.

It is NOT about a strict length limit.
It's about common and logical sense when the line is too long.
Comments are for devs, not for computer.

Turning
```
    /// Player colors present on the board: any non-empty, non-wall square (dead pieces included, since
    /// an eliminated player's army stays on as dead blockers). Used to size per-color game rules so a shrinking turn rotation never mismaps the rule slots.
```
into
```
    /// Player colors present on the board: any non-empty, non-wall square (dead pieces included,
    /// since
    /// an eliminated player's army stays on as dead blockers). Used to size per-color game rules so
    /// a
    /// shrinking turn rotation never mismaps the rule slots.
```
is STUPID.

Turning ```
        // Capture rule only filters board move captures. Bank drops / spell casts / spare placements aren't captures.
``` into
```
        // Capture rule only filters board move captures. Bank drops / spell
        // casts / spare placements aren't captures.
```
is ALSO stupid.

Yet of course
```
        // a1  = x 0, rank 1  on 14x14 → y 13 b14 = x 1, rank 14 on 14x14 → y 0 e6  = x 4, rank 6  on 14x14 → y 8
        // a1  = x 0, rank 1  on 14x14 → y 13
        // b14 = x 1, rank 14 on 14x14 → y 0
        // e6  = x 4, rank 6  on 14x14 → y 8
```
and
```
/// Returns:
/// - `Ok(None)`        — en passant disabled ('x')
/// - `Ok(Some([]))`    — no targets right now ('-')
/// - `Ok(Some([...]))` — list of target squares in board coordinates (y 0 = top)
```
make sense and don't need to be "merged into one line" even if it can be under even 120 chars total.

As long as a comment is up to 140 chars it's fine, but if it really makes to break them because it's not one thing but two different things and it is clearly several sentences, you MAY break them into two lines (eg. one line - a few sentences about A thing, another line — 3 sentences about B thing).

Again, comments are made for human. By human.
~~~~

## Encoding examples from the maintainer

~~~~text
It makes more sense to have
```
// ^^&&*10+5#:rPwrba7:
// annotations=['^','^','&','&'], all piece modifiers, closing 'w','r'->setup, 'b','y'->hill
```
than
```
// ^^&&*10+5#:rPwrba7: annotations=['^','^','&','&'], all piece modifiers, closing 'w','r'->setup, 'b','y'->hill
```

and especially
```
// :r#*10+5P7
// same as above but in different order
```
than
```
// :r#*10+5P7: same as above but in different order
```

You have to think as a SIMPLE HUMAN, and NOT as a smart machine or a dummy line-saving nerd.
~~~~

## Prefer clear developer sentences

~~~~text
Your commenting-rules are still too english-systematic and machine-like while they have to be INSTEAD SIMPLY LOGICAL.

```
// a1 non-royal red king, c1 the royal red king: re-parse auto-royals the FIRST king in scan order (a1),
// so the royal c1 must keep its `#` or the wrong king becomes royal on round-trip.
```
is VERY STUPID.
Especially when you can simply do
```
// a1 non-royal red king, c1 the royal red king.
// Re-parse auto-royals the FIRST king in scan order (a1), so the royal c1 must keep its `#` or the wrong king becomes royal on round-trip.
```

I bare I don't need to mention that new sentence at new comment-line IS LOGICAL prioritization and the long comment phrases that are clearly AI-generated are better be split into smaller reading sentences and be clear and concise rather than being literary works (I am talking only about a few; most of the comments in the repo are human-made and are well-crafted). Following english-writing-best-practices is phrasing like a literary works is not applicable to the project. We are devs. Devs are clear, accurate, short and precise. Making comments for themselves.
~~~~

## Preserve details and emphasis

~~~~text
Go through your changes carefully and update your changes so they do not drop important details.

For EXAMPLE you changed
`Multi-player FFA also defers mate to the mated player's OWN turn — the players moving in between can still rescue (capture the checker, block);`
into
`Multi-player FFA defers mate to the mated player's own turn because intervening players can still rescue them.`
thinking it is better but you made a very, very unacceptable mistakes:
1. you dropped details/clarification `(capture the checker, block)`
2. you dropped the highlight `OWN`
~~~~

## Preserve brackets and logical hierarchy

~~~~text
Details should be preserved and brackets ARE the details — they make the comments more structured and clear, because humans can understand the levels of importance when there are brackets.
~~~~

~~~~text
More examples:
```
    /// - the active coord is either `Spare` (first duck placement, the duck
    ///   does not yet exist on the board) or `Board(duck_pos)` where
    ///   `duck_pos` is the duck's current square (board-to-board move).
```
is NOT logically for human-reading-clarity to transform to
```
    /// - the active coord is `Spare` for the first placement, before the duck exists, or
    /// - the active coord is `Board(duck_pos)` for a board-to-board move from the duck's current square.
```
You are not a literature writer. You are a concise and minimalistic doc-maker for devs.
Use common sense and logic using the perspectives given in guidance.
Example you *could* transform the comment instead applying the skill rules:
```
    /// - the active coord is either:
    ///   - `Spare` (e.g. first duck placement, when the duck does not yet exist on the board)
    ///   - `Board(duck_pos)` where `duck_pos` is the duck's current square (board-to-board move)
```
~~~~

## Line limits resolution and careful validation of preserving the original intention
~~~~text
A couple of examples of corrections:
```
    /// Scan all 8 neighbors because a diagonal Berolina-style `f2s2o` grant sets its ep target
    /// on the crossed diagonal midpoint, not same-file.
```
can be one line (one sentence = one line, only if unacceptably long from the approx limit of 140, then consider interpretation-lossless and details-lossless rephrasing to split into two separate sentences each delivering its part of the point)

```
    /// Apply one turn-leg to the FEN, returning the new FEN and ordered board changes.
    /// `last_fens`: prior position FENs (oldest to newest, not including `fen` itself)
    /// for threefold-repetition detection — pass `None` to skip the check.
```
second and third lines can be one line (or even should given breaking the like makes it possible to interpret the meaning differently)

```
    /// True if `color` may claim the win in a two-player points endgame:
    /// its lead is insurmountable even if it forfeited its whole army to the opponent.
    /// (Used by the caller's resign/timeout path.)
```
this changes the meaning interpretation given the original comment was
```
/// True if `color` may claim the win: a two-player points endgame where its lead is insurmountable even if it forfeited its whole army to the opponent. (Used by the caller's resign/timeout path.)
```
(the "a two-player points endgame where its lead is insurmountable even if it forfeited its whole army to the opponent" is clearly just an example / a specific case, but you turned it into the condition, violating the rule of "do not modify, limit, or expand the original intended interpretation".

```/// `color` resigns (or times out): it is eliminated, then the game is re-evaluated — the points leader (which may be the resigner) or the last player standing wins.```
this comment on the other hand can be rephrased and split into several sentences/lines, each delivering its point - this way it is more easy for developers to catch the points by looking briefly.

~~~~
