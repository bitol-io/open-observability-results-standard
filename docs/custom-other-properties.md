---
title: "Custom & Other Properties"
description: "Extending OORS with custom properties."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Custom & Other Properties

OORS can be extended with custom properties for vendor- or organization-specific metadata that is not part of the standard. `customProperties` is available at the top level of an OORS message and on each [result](results.md). The structure is shared across the Bitol standards and follows the [ODCS Custom & Other Properties structure](https://bitol-io.github.io/open-data-contract-standard/latest/custom-other-properties/).

[Back to TOC](README.md)

## Example

### Minimal

A single key/value pair (only `property` and `value` are required).

```yaml
customProperties:
  - property: environment
    value: production
```

```json
{
  "customProperties": [
    {
      "property": "environment",
      "value": "production"
    }
  ]
}
```

### Full

Multiple entries using the optional `description` and `id`, plus custom properties attached to an individual result.

```yaml
results:
  - id: completeness_customer_email
    type: check
    name: customer email completeness
    status: pass
    customProperties:
      - property: checkEngine
        value: soda-core
      - property: queryDurationMs
        value: 184
customProperties:
  - property: environment
    value: production
  - property: orchestrator
    value: airflow
    description: The scheduler that triggered this run.
    id: cp-orchestrator
```

```json
{
  "results": [
    {
      "id": "completeness_customer_email",
      "type": "check",
      "name": "customer email completeness",
      "status": "pass",
      "customProperties": [
        { "property": "checkEngine", "value": "soda-core" },
        { "property": "queryDurationMs", "value": 184 }
      ]
    }
  ],
  "customProperties": [
    { "property": "environment", "value": "production" },
    {
      "property": "orchestrator",
      "value": "airflow",
      "description": "The scheduler that triggered this run.",
      "id": "cp-orchestrator"
    }
  ]
}
```

## Field Descriptions

| Key                             | Type   | UX label          | Required | Description                                                                                                       | Example          |
| ------------------------------- | ------ | ----------------- | -------- | --------------------------------------------------------------------------------------------------------------- | ---------------- |
| `customProperties`              | array  | Custom Properties | No       | A list of key/value pairs for custom properties.                                                                |                  |
| `customProperties[].property`   | string | Property          | Yes      | The name of the key. Names should be in camel case—the same as if they were permanent properties in the standard.| `environment`    |
| `customProperties[].value`      | any    | Value             | Yes      | The value of the key.                                                                                           | `production`     |
| `customProperties[].description`| string | Description       | No       | Optional description of the custom property.                                                                    | `The scheduler.` |
| `customProperties[].id`         | string | ID                | No       | Optional unique identifier for the custom property, for stable references.                                       | `cp-orchestrator`|
| `customProperties[].vendor`     | string | Vendor            | No       | Identifies the vendor, provider, or external system associated with this custom property. SHOULD be a stable, lowercase identifier matching `^[a-z0-9][a-z0-9-]*$`. Tools MUST preserve unknown vendor values. (Added in v0.2.0, [RFC 0035](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v1.0.0/0035-extensions.md).) | `zeenea`         |
