# Changelog

All notable changes to Shipment Document Control are recorded here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project aims to follow
[Semantic Versioning](https://semver.org/spec/v2.0.0.html). Pre-1.0: the list schemas and the
Configuration keys may still change between minor versions.

## [0.1.0] - 2026-08-31

### Added

- The workflow: one importable file, 51 nodes, two flows - the scan (two triggers, filename
  classification, completeness with no model, model reading of present drafts, fixed-rule
  comparison, worklist upsert, append-only log, desk and counterparty notices) and the approval
  gate (form, guard, record, request for originals).
- Seed CSVs and column documentation for the three SharePoint lists.
- Licence, governance and EU AI Act compliance documentation.

Tested end to end on a live tenant before release: classification, completeness, readings and
comparisons, approval with double-press guard, originals request, day-5 nudge, day-9 escalation,
and completion.
