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

[Back to TOC](README.md)

## Example

```yaml
apiVersion: v0.2.0
kind: ObservabilityResults
id: 9f1c8a52-2b4e-4a6e-9d3e-7c2a1f0b4d11
observedAt: "2025-12-25T17:00:00+01:00"
source:
  process: order-preprocessor-dbt
  runID: "27a3de3d"
  vendor: inhouse-build
contract:
  contractID: 53581432-6c55-4ba2-a65f-72344a91553a
  standard: ODCS
  version: 3.1.0
results:
  - id: dup_rows_myTable
    type: check
    name: No duplicate rows
    status: fail
```

## Fundamental fields

| Key          | UX label    | Type              | Required | Description                                                                  |
| ------------ | ----------- | ----------------- | -------- | ---------------------------------------------------------------------------- |
| `apiVersion` | API Version | string            | Yes      | Version of the OORS schema used, e.g., `v0.2.0`.                            |
| `kind`       | Kind        | string            | Yes      | The type of OORS object. For OORS, always `ObservabilityResults`.           |
| `id`         | ID          | string (UUID)     | Yes      | A unique identifier (could be a UUID) identifying the observability results. |
| `observedAt` | Observed At | string (ISO 8601) | Yes      | The moment in time when the observations were done (ISO 8601 format).        |

## `source` object

| Key              | UX label | Type   | Required | Description                                                                                           |
| ---------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------------- |
| `source`         | Source   | object | Yes      | Object describing the source from where the results come (e.g., job, pipeline, etc.).               |
| `source.process` | Process  | string | Yes      | The name of the process (job, pipeline, etc.) that made the observations.                           |
| `source.runID`   | Run ID   | string | No       | An optional unique identifier for the specific run/execution of the process (e.g., build ID).       |
| `source.vendor`  | Vendor   | string | No       | An optional field indicating the vendor or tool that produced the observations (e.g., `soda`, `dbt`).|

## `contract` object (optional)

| Key                   | UX label    | Type          | Required | Description                                                                                      |
| --------------------- | ----------- | ------------- | -------- | ----------------------------------------------------------------------------------------------- |
| `contract`            | Contract    | object        | No       | Object describing the contract that provided the specifications for the observations. Optional but highly recommended for traceability. |
| `contract.contractID` | Contract ID | string (UUID) | No       | A unique identifier identifying the specific contract (ODCS/ODPS) the observations are based on. |
| `contract.standard`   | Standard    | string        | No       | The standard followed by the contract, e.g., `ODCS`, `ODPS`.                                     |
| `contract.version`    | Version     | string        | No       | The version of the contract (not the standard), e.g., `1.0.0`.                                   |

> **Note:** When a `contract` is provided, all observations in `results` are expected to be based on the specifications defined in that contract. Observations not related to the contract should be sent in separate OORS messages (without the `contract` object).

## Top-level collections

| Key                | UX label          | Type  | Required | Description                                                                                          |
| ------------------ | ----------------- | ----- | -------- | -------------------------------------------------------------------------------------------------- |
| `results`          | Results           | array | Yes      | An array of results (at least one). Each entry is a [result](results.md) object (check or metric).  |
| `customProperties` | Custom Properties | array | No       | A list of key/value pairs for custom properties. See [Custom & Other Properties](custom-other-properties.md). |
