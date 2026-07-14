---
title: "Fundamentals"
description: "The message envelope of an OORS observability results document."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Fundamentals

The top level of an OORS message describes *when* and *by what* the observations were made, optionally *which contract* they relate to, and carries the list of `results`.

Any string value MAY contain `${VAR_NAME}` references resolved at runtime by tooling; see [Variables](variables.md).

[Back to TOC](README.md)

## Example

### Minimal

Only the required fields: the envelope, `source.process`, and one result.

```yaml
apiVersion: v0.2.1
kind: ObservabilityResults
id: 9f1c8a52-2b4e-4a6e-9d3e-7c2a1f0b4d11
observedAt: "2026-01-15T09:30:00Z"
source:
  process: nightly-quality-suite
results:
  - id: no_dup_rows
    type: check
    name: No duplicate rows
    status: pass
```

```json
{
  "apiVersion": "v0.2.1",
  "kind": "ObservabilityResults",
  "id": "9f1c8a52-2b4e-4a6e-9d3e-7c2a1f0b4d11",
  "observedAt": "2026-01-15T09:30:00Z",
  "source": {
    "process": "nightly-quality-suite"
  },
  "results": [
    {
      "id": "no_dup_rows",
      "type": "check",
      "name": "No duplicate rows",
      "status": "pass"
    }
  ]
}
```

### Full

A full envelope: `source` with all fields, a `run` lifecycle object, a `contract` reference, results, and `customProperties`. (See [Results](results.md) for the result fields and [Run Lifecycle](run-lifecycle.md) for `run`.) The complete files are in [`examples/`](examples/README.md).

```yaml
apiVersion: v0.2.1
kind: ObservabilityResults
id: 3d2b6e90-7a1c-4f5b-8e2d-1a9c4b6f0e23
observedAt: "2026-01-15T09:30:00Z"
source:
  process: order-processor-dbt
  runID: build-4821
  vendor: soda
run:
  state: completed
contract:
  contractID: 53581432-6c55-4ba2-a65f-72344a91553a
  standard: ODCS
  version: 3.1.0
results:
  - id: dup_rows_myTable
    type: check
    name: No duplicate rows
    status: fail
  - id: freshness_myDataProduct
    type: metric
    name: Data should not be stale
    status: pass
customProperties:
  - property: environment
    value: production
```

```json
{
  "apiVersion": "v0.2.1",
  "kind": "ObservabilityResults",
  "id": "3d2b6e90-7a1c-4f5b-8e2d-1a9c4b6f0e23",
  "observedAt": "2026-01-15T09:30:00Z",
  "source": {
    "process": "order-processor-dbt",
    "runID": "build-4821",
    "vendor": "soda"
  },
  "run": { "state": "completed" },
  "contract": {
    "contractID": "53581432-6c55-4ba2-a65f-72344a91553a",
    "standard": "ODCS",
    "version": "3.1.0"
  },
  "results": [
    { "id": "dup_rows_myTable", "type": "check", "name": "No duplicate rows", "status": "fail" },
    { "id": "freshness_myDataProduct", "type": "metric", "name": "Data should not be stale", "status": "pass" }
  ],
  "customProperties": [
    { "property": "environment", "value": "production" }
  ]
}
```

## Field Descriptions

| Key                   | Type              | UX label          | Required | Description                                                                                                                | Example                                |
| --------------------- | ----------------- | ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| `apiVersion`          | string            | API Version       | Yes      | Version of the OORS schema used.                                                                                          | `v0.2.1`                               |
| `kind`                | string            | Kind              | Yes      | The type of OORS object. For OORS, always `ObservabilityResults`.                                                         | `ObservabilityResults`                 |
| `id`                  | string (UUID)     | ID                | Yes      | A unique identifier (could be a UUID) identifying the observability results.                                              | `9f1c8a52-2b4e-4a6e-9d3e-7c2a1f0b4d11` |
| `observedAt`          | string (ISO 8601) | Observed At       | Yes      | The moment in time when the observations were done (ISO 8601 format).                                                     | `2026-01-15T09:30:00Z`                 |
| `source`              | object            | Source            | Yes      | Object describing the source from where the results come (e.g., job, pipeline).                                           |                                        |
| `source.process`      | string            | Process           | Yes      | The name of the process (job, pipeline, etc.) that made the observations.                                                 | `order-processor-dbt`                  |
| `source.runID`        | string            | Run ID            | No       | An optional unique identifier for the specific run/execution of the process.                                              | `build-4821`                           |
| `source.vendor`       | string            | Vendor            | No       | An optional field indicating the vendor or tool that produced the observations.                                           | `soda`                                 |
| `run`                 | object            | Run               | No       | Run lifecycle object. Absent ⇒ the document is terminal. See [Run Lifecycle](run-lifecycle.md).                            |                                        |
| `contract`            | object            | Contract          | No       | Object describing the contract that provided the specifications for the observations. Highly recommended for traceability.|                                        |
| `contract.contractID` | string (UUID)     | Contract ID       | No       | A unique identifier identifying the specific contract (ODCS/ODPS) the observations are based on.                          | `53581432-6c55-4ba2-a65f-72344a91553a` |
| `contract.standard`   | string            | Standard          | No       | The standard followed by the contract.                                                                                   | `ODCS`                                 |
| `contract.version`    | string            | Version           | No       | The version of the contract (not the standard).                                                                          | `3.1.0`                                |
| `results`             | array             | Results           | Yes      | An array of results (at least one). Each entry is a [result](results.md) object (check or metric).                        |                                        |
| `customProperties`    | array             | Custom Properties | No       | A list of key/value pairs for custom properties. See [Custom & Other Properties](custom-other-properties.md).             |                                        |

> **Note:** When a `contract` is provided, all observations in `results` are expected to be based on the specifications defined in that contract. Observations not related to the contract should be sent in separate OORS messages (without the `contract` object).

> **Note:** An OORS document is **terminal by default** — without a `run` object it is the authoritative record of the run that produced it. Long-running producers can opt in to progressive reporting with the optional `run` object; see [Run Lifecycle](run-lifecycle.md).
