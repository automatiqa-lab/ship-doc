# Contributing

Thank you for looking. A few things worth knowing before you spend time on a change.

## Where changes belong

**The requirements live in lists, not nodes.** Which documents a contract needs, whether an
original must follow, which fields to check and with what tolerance - all of that is rows on the
Document requirements list. If your change is "check another document" or "compare another
field", it is data, and it needs no pull request at all.

**The workflow is generated.** It is built and tested in a private working repository and
exported here as a result, which is why every Code node carries a compiled library rather than
hand-written logic. A pull request that edits `workflows/ship-doc.json` will be overwritten by
the next release, so open an issue describing the behaviour instead. If we agree on it, the
change lands upstream and arrives here credited to you.

Documentation, the seed CSVs and the list of things that are wrong with this: ordinary pull
requests, very welcome.

## Two properties that must not regress

**The comparison is reproducible.** Given the same reading and the same instruction row,
re-running it a year later returns the same result. The engine reads no clock of its own, no
random source and no locale; the one date the chase arithmetic needs is passed in.

**A model never decides.** It copies named fields out of a document, as written. Presence is set
arithmetic, agreement is a fixed comparison, and approval is a person on a form. Any proposal
that needs one of those to bend is a different project, and worth being honest about rather than
arguing the edges.

## Accurate scope

This checks a document pack for presence and agreement and chases the originals. It does not
author documents, present to a bank or decide legal sufficiency. Wording that implies otherwise
will be edited, including in a pull request that is otherwise good.

## Reporting a bug

Most useful with the shipment row, the requirement rows for its contract, the filenames in the
folder, what you expected and what you got. A comparison that looks wrong is fully determined by
the extracted value and the instruction value; if the result does not follow from those two, that
is the bug and it is a good one to have found.
