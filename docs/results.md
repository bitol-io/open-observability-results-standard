---
title: "Results"
description: "The structure of an individual observability result (check or metric)."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Results

Each entry in the `results` array is a single observation — either a `check` (a pass/fail assertion) or a `metric` (a measured value). A **check** reports a pass/fail outcome; a **metric** reports a measured value (its `status` still indicates whether the value met expectations).

[Back to TOC](README.md)

## Example

### Minimal

A result requires only `id`, `type`, `name`, and `status`.

```yaml
- id: no_dup_rows
  type: check
  name: No duplicate rows
  status: pass
```

```json
{
  "id": "no_dup_rows",
  "type": "check",
  "name": "No duplicate rows",
  "status": "pass"
}
```

### Full

A result using every field — a failing `check` linked to a contract rule via `relatesTo`, with a target, a measured value against a threshold, a message, and custom properties.

```yaml
- id: dup_rows_myTable
  type: check
  name: No duplicate rows
  status: fail
  relatesTo: schema/orders_tbl/quality/dq_no_duplicates
  severity: critical
  dimension: uniqueness
  target:
    resourceType: table
    resourceIdentifier: myTable
  measure:
    metric: rowCount
    value: 3
    threshold:
      mustBe: 0
    unit: rows
  message: Multiple duplicate rows found in myTable!
  customProperties:
    - property: checkEngine
      value: soda-core
```

```json
{
  "id": "dup_rows_myTable",
  "type": "check",
  "name": "No duplicate rows",
  "status": "fail",
  "relatesTo": "schema/orders_tbl/quality/dq_no_duplicates",
  "severity": "critical",
  "dimension": "uniqueness",
  "target": {
    "resourceType": "table",
    "resourceIdentifier": "myTable"
  },
  "measure": {
    "metric": "rowCount",
    "value": 3,
    "threshold": {
      "mustBe": 0
    },
    "unit": "rows"
  },
  "message": "Multiple duplicate rows found in myTable!",
  "customProperties": [
    {
      "property": "checkEngine",
      "value": "soda-core"
    }
  ]
}
```

## Field Descriptions

| Key                         | Type            | UX label            | Required | Description                                                                                                                          | Example                                  |
| --------------------------- | --------------- | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| `id`                        | string          | Result ID           | Yes      | A unique identifier for the specific check/metric.                                                                                  | `dup_rows_myTable`                       |
| `type`                      | string          | Type                | Yes      | The type of observation: `check` (pass/fail) or `metric` (measured value).                                                         | `check`                                  |
| `name`                      | string          | Name                | Yes      | A human-readable name for the check/metric.                                                                                        | `No duplicate rows`                      |
| `status`                    | string          | Status              | Yes      | The outcome of the observation: `pass` or `fail`.                                                                                  | `fail`                                   |
| `relatesTo`                 | string          | Relates To          | No       | A reference to the element this result relates to in the linked contract (e.g., a path to a data quality rule or property).         | `schema/orders_tbl/quality/dq_no_duplicates` |
| `severity`                  | string          | Severity            | No       | The severity level, e.g., `critical`, `high`, `medium`, `low`, `warning`.                                                          | `critical`                               |
| `dimension`                 | string          | Dimension           | No       | The data quality dimension (following ODCS data quality dimensions), e.g., `accuracy`, `completeness`, `timeliness`, `uniqueness`.  | `uniqueness`                             |
| `target`                    | object          | Target              | No       | Object describing the resource being observed.                                                                                     |                                          |
| `target.resourceType`       | string          | Resource Type       | No       | The type of resource, e.g., `table`, `column`, `dataset`.                                                                          | `table`                                  |
| `target.resourceIdentifier` | string / array  | Resource Identifier | No       | The identifier(s) of the resource — a single string or an array of strings.                                                        | `myTable`                                |
| `measure`                   | object          | Measure             | No       | Object describing the measurement details.                                                                                         |                                          |
| `measure.metric`            | string          | Metric              | No       | The name of the metric being measured, e.g., `rowCount`, `age`, `latency`.                                                         | `rowCount`                               |
| `measure.value`             | number / string | Value               | No       | The observed value of the metric.                                                                                                 | `3`                                      |
| `measure.threshold`         | object          | Threshold           | No       | The threshold criteria, e.g., `mustBe`, `mustBeLessThan` (following ODCS operator definitions).                                    | `{ "mustBe": 0 }`                        |
| `measure.unit`              | string          | Unit                | No       | The unit of measurement, e.g., `rows`, `seconds`, `percent`.                                                                       | `rows`                                   |
| `message`                   | string          | Message             | No       | A human-readable message providing additional context about the observation.                                                       | `Multiple duplicate rows found!`         |
| `customProperties`          | array           | Custom Properties   | No       | A list of key/value pairs for custom properties. See [Custom & Other Properties](custom-other-properties.md).                       |                                          |
