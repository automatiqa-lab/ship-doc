# EU AI Act compliance - Shipment Document Control

Not legal advice. This file records how the workflow meets its transparency obligations, and why.
It is the project's defence file: every entry is dated, and its history is in git.

Contact for compliance and incident reports: aleks@automatiqa.io

## Role classification

**The lab is a template publisher, and whoever runs the workflow is the deployer.** The lab
publishes a workflow definition; it operates no instance and holds no key. Running it means
importing the workflow into your own n8n, pointing it at a model endpoint you contract for, and
putting your own shipment folders through it. That makes the operator the deployer under
Art. 50(4), and the lab the provider of a component rather than of a system placed on the market.
Because the template ships with marking on by default, the operator's laziest path is the
compliant one, and the Art. 50(2) obligation is met at the point of publication rather than left
to whoever installs it.

Recorded 2026-08-30.

## Obligations that apply

| Obligation | Applies | How this project meets it |
|---|---|---|
| Art. 50(1) interaction disclosure | no | No natural person is in a session with a model. The desk receives email; the approver fills a form that reads a list. Nobody converses with the system. |
| Art. 50(2) machine-readable marking | yes, narrowly | The only model output that reaches a person is the set of field values quoted in a discrepancy notice and in the `discrepancies` column. Both carry marking: the notice has a readable line (the `ai_notice` Configuration value) and an `X-AI-Generated: true` mail header; the row has an `ai_generated` column holding the `automatiqa-disclosure/1` envelope. |
| Art. 50(4) deployer disclosure | deployer-side | See "What remains the deployer's job" |
| Art. 4 AI literacy | yes | This file, the README section on where a model is used, NOTICE |
| Art. 5 prohibited practices | screened | No biometric categorisation, no emotion inference, no social scoring, no manipulation of a natural person. The system copies fields out of shipping documents and compares them with values a person typed. |
| Annex III high-risk | no | See screening below |

## Annex III screening

Considered and rejected, with the near-misses named rather than waved off.

**Essential private services (Annex III 5(b)).** The nearest miss. A discrepancy can delay a
presentation and therefore a payment to a supplier, which touches commercial access. It falls
outside because the subject is a document and a legal entity, not a natural person's
creditworthiness or entitlement; and no automated decision restricts access to anything. A
discrepancy produces a notice that names both values, and a named person decides what to do.

**Employment (Annex III 4).** Not touched, and worth stating why it could look otherwise. The
Shipments row and the log record who approved and who rejected. Those fields exist so the record
is attributable, not so anybody is scored: nothing aggregates them, ranks them or feeds them into
an evaluation of a worker. Using them that way would be a change of purpose that puts the
deployer in Annex III 4, and it is called out here so nobody drifts into it by adding a chart.

**Critical infrastructure (Annex III 2), justice, migration, law enforcement, education.** Not
touched.

The system generates no synthetic image, audio or video, so the deep-fake limb of Art. 50 does
not fire. The model produces no free text that reaches anyone: it returns named values, and those
are quoted inside sentences the engine writes by fixed rules.

Screened 2026-08-30. Re-run on any change of purpose or new modality.

## What this workflow does out of the box

The boundary is the reading step, and it is the only place a model touches the workflow. What
crosses it is a set of named values per document, which a Code node compares with the Shipments
row. The values reach a person in exactly two places, and both are marked:

- **The discrepancy notice** (Outlook, internal). The body ends with the `ai_notice` line from
  Configuration, and the message carries `X-AI-Generated: true` as an internet message header.
  Every other desk message carries `X-AI-Generated: false`, which is a true statement about a
  message no model contributed to.
- **The `discrepancies` column** on the Shipments row. While it is non-empty, the `ai_generated`
  column on the same row holds the envelope `{"value": true, "scope": ["discrepancies"],
  "schema": "automatiqa-disclosure/1", "system": "shipment-document-control", "review_state":
  "unreviewed", "ts": ...}`. When the discrepancy clears, the column is emptied, so absence stays
  truthful.

The marking is written by the same node that writes the row and the notice, so there is no path
that produces the text without the mark.

Marking schema: `automatiqa-disclosure/1`. The readable line is a Configuration value
(`ai_notice`), because a template cannot carry a config file and the operator must be able to
read the claim they are making.

## Carve-outs, and why

**Chase messages and the request for originals are not marked.** They are assembled by fixed
rules from the requirement rows and the Shipments row, both typed by people. No model wrote a
word of them. Labelling them would be a false statement of provenance.

**Ready-for-approval notices are not marked.** They name a shipment and carry a link. The model's
reading contributed to the status, but nothing the model produced appears in the message.

**`next_step` is not marked separately.** When it repeats the discrepancy text it does so on the
same row as the `ai_generated` envelope, whose scope names `discrepancies`; the deployer reads
the row as one record.

**The model is never named in visible output.** The endpoint and the model identifier live in
Configuration for the operator's own eyes and in no message, row or log line.

## What remains the deployer's job

Keep the `ai_notice` line and the `X-AI-Generated` header intact, and keep the `ai_generated`
column on the Shipments list. If you copy discrepancy text into another system, carry the marking
with it. Put the n8n form behind your own authentication; the approval form names a person and
should not be reachable by anyone. If you republish anything this workflow reads on a matter of
public interest, Art. 50(4) applies to you directly.

## If you deploy this in the EU

Running this template or tool for other people makes you a **deployer** under the EU AI Act. Two
things follow.

**Recipients must be able to tell that content is AI-generated.** The output produced here says
so in plain language and carries machine-readable marking. Do not strip either. If you reformat,
re-publish or forward the output, carry the disclosure with it.

**Marking must survive.** Marking is on by default and travels as metadata, not only as a visible
label, so it stays detectable after conversion between formats. A workflow that rebuilds the
payload and drops the `ai_generated` field breaks that, and the obligation then sits with you.

Questions: aleks@automatiqa.io

## Decision log

| Date | Decision | Why |
|---|---|---|
| 2026-08-30 | Mark only the discrepancy notice and the `discrepancies` column | They are the only surfaces that quote model output. Blanket marking of chase messages would misstate provenance and train people to ignore the label. |
| 2026-08-30 | Readable wording held as a Configuration setting | A template carries no config file, and the operator must be able to read the claim they are making without opening a Code node. |
| 2026-08-30 | `X-AI-Generated` header present on every desk message, true or false | The mail node cannot add a header conditionally; an explicit false is a true statement and simpler to audit than a header that is sometimes absent. |
