# Security Policy

Shipment Document Control (Automatiqa Lab by Aleks Sidorecs) handles shipping documents,
counterparty contact details and commercial shipment data. It runs entirely inside the deployer's
own boundary: the lab operates no service, hosts no instance and receives no data. Vulnerability
reports are taken seriously.

## Reporting a vulnerability

Please report privately - do not open a public issue for a security problem.

- Preferred: open a private advisory via GitHub Security Advisories ("Report a vulnerability" on
  the Security tab).
- Or email **aleks@automatiqa.io**.

Include what you found, how to reproduce it, the impact, and any suggested fix. If you have a
proof of concept, attach it.

Expect acknowledgement within 3 business days, and a disclosure timeline agreed with you. Credit
is given unless you prefer otherwise.

## Supported versions

Pre-1.0. Fixes land on `main` and in the next release; older tags are not patched.

## Scope

In scope:

- Any path where the approval form can be driven without whatever authentication the deployer put
  in front of it being the only gate - e.g. a way to approve a shipment through the workflow's
  own endpoints that bypasses the guard
- Prompt injection in a shipping document that changes a comparison result, alters the worklist,
  or causes data to be sent anywhere other than the configured endpoints
- Any path where a document, a worklist row or a log line is readable or writable by a party
  other than the deployer's own tenant
- Secrets recoverable from logs, error messages, the workflow file or exported artefacts

Out of scope:

- The security of the language model endpoint you configure and hold the key for
- Vulnerabilities in n8n, Microsoft Graph, SharePoint or other upstream components - report those
  upstream, though a note here is appreciated if the combination is what makes it exploitable
- Findings that require an attacker to already hold deployer credentials
- Reports produced solely by an automated scanner with no demonstrated impact
