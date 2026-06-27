<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Results

Each entry in the `results` array is a single observation — either a `check` (a pass/fail assertion) or a `metric` (a measured value).

## Result fields

| Key                          | UX label            | Type           | Required | Description                                                                                                   |
| ---------------------------- | ------------------- | -------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| `id`                         | Result ID           | string         | Yes      | A unique identifier for the specific check/metric.                                                          |
| `type`                       | Type                | string         | Yes      | The type of observation: `check` (pass/fail) or `metric` (measured value).                                  |
| `name`                       | Name                | string         | Yes      | A human-readable name for the check/metric.                                                                 |
| `status`                     | Status              | string         | Yes      | The outcome of the observation: `pass` or `fail`.                                                           |
| `severity`                   | Severity            | string         | No       | The severity level, e.g., `critical`, `high`, `medium`, `low`, `warning`.                                   |
| `dimension`                  | Dimension           | string         | No       | The data quality dimension, e.g., `accuracy`, `completeness`, `consistency`, `timeliness`, `validity`, `uniqueness` (following ODCS data quality dimensions). |
| `target`                     | Target              | object         | No       | Object describing the resource being observed.                                                              |
| `target.resourceType`        | Resource Type       | string         | No       | The type of resource, e.g., `table`, `column`, `dataset`.                                                   |
| `target.resourceIdentifier`  | Resource Identifier | string / array | No       | The identifier(s) of the resource, e.g., table or column name(s). A single string or an array of strings.   |
| `measure`                    | Measure             | object         | No       | Object describing the measurement details.                                                                  |
| `measure.metric`             | Metric              | string         | No       | The name of the metric being measured, e.g., `rowCount`, `age`, `latency`.                                  |
| `measure.value`              | Value               | number / string| No       | The observed value of the metric.                                                                          |
| `measure.threshold`          | Threshold           | object         | No       | The threshold criteria, e.g., `mustBe`, `mustBeLowerOrEqual` (following ODCS operator definitions).         |
| `measure.unit`               | Unit                | string         | No       | The unit of measurement, e.g., `rows`, `seconds`, `percent`.                                                |
| `message`                    | Message             | string         | No       | A human-readable message providing additional context about the observation.                                |

## Example result

```yaml
- id: dup_rows_myTable
  type: check
  name: No duplicate rows
  status: fail
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
```
