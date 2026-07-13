---
title: "Changelog: Open Observability Results Standard (OORS)"
description: "Home of Open Observability Results Standard (OORS) documentation."
image: "https://raw.githubusercontent.com/bitol-io/artwork/main/horizontal/color/Bitol_Logo_color.svg"
---

This document tracks the history and evolution of the **Open Observability Results Standard**.

# v0.2.0 - DRAFT

* **Adds** `customProperties` — a list of key/value pairs (shared structure across the Bitol standards: `property`, `value`, optional `description` and `id`) at the top level and on each result, for vendor- or organization-specific extensions.
* **Adds** optional `vendor` string on `customProperties` items, associating a custom property with a specific vendor, provider, or external system. SHOULD be a stable, lowercase identifier (`^[a-z0-9][a-z0-9-]*$`); not enforced, and tools MUST preserve unknown vendor values — [RFC 0035](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v1.0.0/0035-extensions.md), shared with ODCS v3.2.0 and ODPS v1.1.0 (slated for OORS v1.0.0).
* **Adds** `relatesTo` on result objects — an optional reference to the element this result relates to in the linked contract (e.g., a path to a data quality rule or property in the referenced ODCS/ODPS contract).
* **Adds** variable interpolation — any string value MAY contain `${VAR_NAME}` references resolved at runtime by tooling, with the POSIX `${VAR_NAME:-default}` form for inline defaults. Tools MUST resolve references before use, MUST NOT silently substitute empty strings, and MUST preserve unresolved tokens verbatim on round-trip. No new section or field — [RFC 0050](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v1.0.0/0050-variables.md), shared with ODCS v3.2.0 and ODPS v1.1.0 (slated for OORS v1.0.0).
* Default `apiVersion` raised to `v0.2.0`.
* Documentation expanded to match the ODCS/ODPS structure: per-section pages with front matter, a table-of-contents landing page, and a Custom & Other Properties page.

# v0.1.0 - DRAFT

Initial draft of the Open Observability Results Standard, based on [RFC-0018](https://github.com/bitol-io/tsc/blob/main/rfcs/0018-oors.md).

* Top-level fundamentals: `apiVersion`, `kind` (`ObservabilityResults`), `id`, `observedAt`.
* `source` object: `process` (required), `runID`, `vendor`.
* Optional `contract` object linking results to an ODCS/ODPS contract: `contractID`, `standard`, `version`.
* `results` array (at least one) of result objects, each with `id`, `type` (`check`/`metric`), `name`, `status` (`pass`/`fail`), and optional `severity`, `dimension`, `target` (`resourceType`, `resourceIdentifier`), `measure` (`metric`, `value`, `threshold`, `unit`), and `message`.
* JSON Schema (`oors-json-schema-v0.1.0.json`) and example messages.
