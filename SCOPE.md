# Scope and limits

What this does, what it refuses to do, and where the edges are. If you are deciding whether to
put real shipments through it, read this page rather than the feature list.

## What it is

A document control loop. It checks a shipment's document pack for presence and agreement against
the shipping instruction, records a named person's approval, asks for the originals, chases them,
and keeps an append-only log of every change.

## What it is not

It does not author documents, present to a bank, file customs, book freight or track a vessel.
It does not decide whether a document is legally sufficient; it reports whether the pack is
complete against your requirement rows and whether the fields agree with your instruction, and a
person decides the rest.

The requirement rows shipped in `lists/document-requirements.csv` are an example that exercises
the checks. They are not a statement of what any contract, bank or authority requires. Set your
own rows before a real shipment depends on them.

## Where a model is used, and where it is not

One job: reading the fields a requirement row names out of a draft document, as written, null
where absent, with a confidence for the reading as a whole. Nothing the model returns can mark a
document present, absent, agreeing or approved. Presence is set arithmetic; agreement is a fixed
comparison in a Code node; approval is a person on a form. A failed model call is recorded as a
document that could not be read, never as a pass.

Reading quality is still reading quality. A value read wrongly can produce a false discrepancy or
a false agreement; the mitigation is that every discrepancy names both values side by side, and
that nothing is sent to a counterparty and nothing moves phase without a person pressing approve.

## Known limits

**The filename convention is a hard dependency.** A file whose name matches no alias counts for
nothing. It is reported to the desk by name, but until somebody renames it, the pack reads as
incomplete. This is deliberate: guessing what a document is from its content would put a model in
front of the completeness check, which is the one check that must stay free.

**Originals are checked for presence, not content.** A file in `originals/` means the original
arrived. Nothing reads it.

**One OneDrive account.** The folder tree lives on the drive the OneDrive credential signs into.
Shared drives and SharePoint document libraries are not enumerated - the OneDrive node is the
only Microsoft node that can list a folder, and it lists that account's drive.

**List reads are capped at 2,000 rows** per list per run, unpaginated. At that scale this is the
wrong tool anyway.

**The list reads are plain HTTP calls** to SharePoint's `_api/v2.0`, with the same credential the
native SharePoint node uses. The native node's `getAll` returns items without their columns on
n8n versions whose SharePoint node predates the fields option - silently. The writes use the
native node, where the behaviour is consistent.

**Graph throttles.** Every Microsoft node retries with a wait, and the scan reads each list once
per run rather than once per shipment, but a very large estate on a tight schedule will still
meet the limiter.

**The approval form is an n8n form.** Put it behind your proxy's authentication; the workflow
records whatever name is typed into it. The guard stops double approvals and unknown references,
not impersonation.

**Scanned documents are only as readable as the model finds them.** The document goes to the
endpoint as a PDF; a poor scan reads as nulls, which surface as "not found in the document"
discrepancies rather than as silence.
