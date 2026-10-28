---
title: "Changelog: Open Observability Results Standard (OORS)"
description: "Home of Open Observability Results Standard (OORS) documentation."
image: "https://raw.githubusercontent.com/bitol-io/artwork/main/horizontal/color/Bitol_Logo_color.svg"
---

This document tracks the history and evolution of the **Open Observability Results Standard**.

# v0.8.1 - DRAFT

* **Adds** optional `results[].target.filter` — the condition (a partition or a where clause) that restricted the observed data to a subset of the resource. Without it, a result that covers one partition reads as covering the whole resource. Non-breaking: the field is optional.
* **Adds** optional `results[].measure.rowCount` and `results[].measure.failedRowCount` — the number of rows evaluated and the number that did not meet the expectation, so consumers can compute the share of failing rows across tools, independent of the metric in `value`. Non-breaking: the fields are optional.
* **Adds** optional `run.startedAt` — when the run started, so its duration is not lost. Non-breaking: the field is optional.
* **Extends `results[].status`** with `unknown` — the producer cannot tell the outcome, e.g. it relays a result from another tool whose status it does not recognize. Distinct from `error` (the evaluation broke and needs fixing); like `skip`, it is counted neither as a pass nor as a failure. **Non-breaking** — this widens an enum. See [Status values](docs/results.md#status-values).
* **Adds** optional top-level `status` — the overall outcome of the document, with the same values as `results[].status`, so consumers and quality gates can read one field instead of aggregating the results. When present, it SHOULD equal the outcome derived from `results` (the first of `error`, `fail`, `warn`, `pass`, `skip` that occurs, else `unknown`); consumers SHOULD derive it with the same rule when it is absent. See [Overall status](docs/fundamentals.md#overall-status). Non-breaking: the field is optional.
* **Allows** an empty `results` array, for runs that made no observations (no checks defined, or all filtered out), instead of forcing producers to invent a placeholder result. `results` itself stays required. **Non-breaking** — this relaxes a constraint.
* Recommends the ODCS library metric names (`rowCount`, `nullValues`, `missingValues`, `invalidValues`, `duplicateValues`) for `measure.metric`. The duplicate-row examples now use `duplicateValues` instead of `rowCount`.
* Default `apiVersion` raised to `v0.8.1`. The v0.8.0 schema is unchanged.
* The `full` example gains `target.filter`, `measure.rowCount`, `measure.failedRowCount`, `run.startedAt` and the top-level `status`; the `statuses` example gains an `unknown` result.

# v0.8.0 - DRAFT

* **Approves [RFC-0018](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v0.8.0/0018-oors.md) — Open Observability Results Standard**, the founding RFC, approved by the TSC on 2026-10-14. OORS is no longer an implementation of an open draft: it is now the formal consequence of an approved RFC.
* **Adds** optional `contract.server` — the identifier of the server, as defined in the referenced contract (e.g., the `server` name in the ODCS `servers` section), against which the observations were made. A contract can be served on multiple servers (e.g., `production`, `dev`); this field records which one the results apply to. Non-breaking: the field is optional. Recreates the contribution made in [#8](https://github.com/bitol-io/open-observability-results-standard/pull/8) by [@jochenchrist](https://github.com/jochenchrist) on top of v0.8.0.
* **Adds** `tags` — the shared Bitol structure for free-form classification labels (an array of strings), available at the top level and on each result. See the new [Tags](docs/tags.md) page. Non-breaking: the field is optional.
* **Extends `results[].status`** to the five outcomes the RFC defines: `pass`, `fail`, `warn` (executed, breached a soft/warning threshold), `error` (could **not** be evaluated — verdict unknown; e.g. the query failed or the source was unreachable) and `skip` (intentionally not run). The schema previously permitted only `pass` and `fail`. `error` is deliberately distinct from `fail`: a failure is a known-bad verdict, an `error` is the *absence* of a verdict and must not be counted as a failure. **Non-breaking** — this widens an enum. See [Status values](docs/results.md#status-values).
* Documents `severity` as **orthogonal** to `status`: `status` is the runtime outcome, `severity` is the definition-time importance of the check. The two compose — a `critical`-severity check with `status: error` is the loudest possible alert.
* Default `apiVersion` raised to `v0.8.0`.
* New `statuses` example (YAML + JSON) exercising all five outcomes, and the first negative-test fixtures under `tests/negative/` — invalid documents the schema must reject — wired into `scripts/validate-examples.sh`.
* New [Tags](docs/tags.md) documentation page, mirroring the ODCS page; [Custom & Other Properties](docs/custom-other-properties.md) restructured to align with ODCS. The `full` example gains `contract.server` and `tags`.

# v0.2.2 - DRAFT

* **Renames** `source.runID` to `source.runId` and `contract.contractID` to `contract.contractId`, following the Bitol camel-case naming convention. **Breaking**: `source` and `contract` set `additionalProperties: false`, so documents still using `runID` or `contractID` no longer validate against the v0.2.2 schema.
* Default `apiVersion` raised to `v0.2.2`.

# v0.2.1 - DRAFT

* **Adds** optional top-level `run` object — run lifecycle reporting: **terminal by default, progressive by choice**. A document without `run` (every pre-v0.2.1 document) is terminal: the authoritative record of its run. Producers of long-running executions can opt in to interim snapshots with `run.state: running`, plus optional `run.sequence` (monotonic ordering per `source.runID`) and `run.progress` (`current`/`total`/`unit`). `run.state: completed|aborted` marks a terminal document; a terminal document supersedes all interim documents sharing its `source.runID`. `source.runID` is required (schema-enforced) when `run.state` is `running`. `results[].status` semantics are unchanged — lifecycle is orthogonal to observation outcomes. See the new [Run Lifecycle](docs/run-lifecycle.md) page.
* Default `apiVersion` raised to `v0.2.1`.
* New `progressive` example (YAML + JSON); `full` example extended with the `run` object.

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
