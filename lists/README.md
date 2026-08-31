# The three lists

Three SharePoint lists on one site. Create each by importing its CSV (SharePoint: New, List, From
CSV), then fix the column types listed below, because an import types everything as text unless it
is told otherwise. The Title column that SharePoint creates on every list is the key: the shipment
reference on Shipments and Document log, the contract reference on Document requirements.

## Shipments (`shipments.csv`)

One row per shipment. A person creates it: this is the shipping instruction, and the values on it
are what every document has to agree with. The workflow writes only the worklist columns.

| Column | Type | Who writes it | What it is |
|---|---|---|---|
| Title | text | person | shipment reference, and the name of the folder on OneDrive |
| contract_ref | text | person | key into Document requirements |
| commodity, origin_country | text | person | expected values |
| consignee, notify_party | text | person | expected values |
| vessel, voyage | text | person | expected values |
| port_of_loading, port_of_discharge | text | person | expected values |
| net_weight_kg, bags | number | person | expected values |
| marks | text | person | expected values |
| counterparty_email | text | person | who is asked for originals and chased |
| status | choice or text | workflow | awaiting drafts, drafts incomplete, discrepancy, ready for approval, drafts approved, drafts rejected, awaiting originals, complete, attention |
| next_step | text | workflow | the worklist line |
| missing, discrepancies | multiple lines of text | workflow | |
| approved_by | text | workflow | the name typed on the approval form |
| approved_at, last_chased, last_checked | date and time | workflow | |
| chase_count | number | workflow | |
| ai_generated | multiple lines of text | workflow | the EU AI Act Article 50 envelope, present only while `discrepancies` quotes values a model read. See COMPLIANCE.md |

## Document requirements (`document-requirements.csv`)

One row per document per contract. Which documents must be there, whether an original is needed,
and which Shipments columns each document has to agree with.

| Column | Type | What it is |
|---|---|---|
| Title | text | contract reference |
| doc_type | choice or text | bill_of_lading, phytosanitary, certificate_of_origin, ico_certificate, commercial_invoice, packing_list, weight_certificate, quality_certificate, fumigation, insurance |
| required | yes/no | |
| original_required | yes/no | |
| check_fields | text | comma separated Shipments column names |
| tolerance_pct | number | for numeric fields, percent |

Adding a document type to a contract is a row. It is never a change to a node.

## Document log (`document-log.csv`)

Append only. The seed row exists so the import types the columns; delete it afterwards.

| Column | Type |
|---|---|
| Title | text (shipment reference) |
| event | text |
| detail | multiple lines of text |
| actor | text |
| at | date and time |

## Where the ids come from

The site id: open any SharePoint node in the workflow, choose the site from the list, and copy the
id it resolves to; or open `https://<tenant>.sharepoint.com/sites/<site>/_api/site/id` in a browser.
Lists are addressed by their display name, which is why the Configuration defaults are the names
above; rename a list and change the setting.
