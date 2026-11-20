# CLAUDE.md

Script name alias in prompts: `TID`

## The box and the TLE API belong together

TID is the one script that changes the tracklist AFTER the TLE API has answered about it: the
`?` gap tracks whose cue sits too close to a neighbour are dropped, and the **Toggle** puts them
back. A tracklist that is incomplete only because of those rows is valid AND complete without
them - https://trackid.net/audiostreams/aka-aka-pres-rhythm-prism-radio-053 is the reported
case - so text and verdict drift apart the moment one of them moves alone.

**Every path that changes the box's text after an answer arrived either sends the new text and
prints the new answer, or swaps in an answer already asked for exactly that text.** Never leave
the previous answer standing over text it does not describe - it is not only the colour that is
then wrong, the row count in the corner counts the version that is gone.

The paths, all covered:

- the `?` removal after the first answer: a second `apiTracklist()` call, and ITS feedback is
  the one handed to `fixTLbox()`
- the **Toggle**: both verdicts are stashed on the textarea when it is built
  (`mdbTlFeedback` / `mdbTlFeedbackCandidate`) and swapped with the text, so no third call is
  needed - the memo the blur update compares against (`mdbTlboxKnown`) moves with them
- the cue format switch (`[059]` <-> `[0:59]`): no call needed, verified against the API - it
  keeps whichever format it is given and answers both identically, rows and status included

Rendering another verdict rebuilds the feedback box's CONTENT, which takes our own rows in it
with it, so the "? tracks removed" notice goes back afterwards - that is what
`showInfoCuesRemoved()` is for, and why its button is bound through a delegated handler rather
than to the node that happened to be there. The cue format switch re-attaches by itself; it
waits for `ul#tlEditor-feedback-topInfo` to turn up.

## Cues come from td.startTime / td.endTime of the COPIED table

`funcTidTables()` copies TID's MUI grid into `.mdb-tid-table`, one `<td>` per grid cell, classed
by the cell's `data-field`. Every cue reader (tracklist builder, gap measuring, `?` row checks)
asks for `.startTime` / `.endTime`. Since ~Nov 2026 TID shows ONE `time` column
("0:07:11 – 0:07:46"), and `mdbTid_splitTimeCell()` splits it back into the two cells. If the
tracklist box suddenly has `#` rows and no cues, check the grid's `data-field` names first – a
renamed or merged column is the usual cause, not the builder.
