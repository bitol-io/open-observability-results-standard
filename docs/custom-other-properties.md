---
title: "Custom & Other Properties"
description: "This section covers custom properties you may find in an OORS message."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Custom & Other Properties

This section covers other properties you may find in an OORS message.

[Back to TOC](README.md)

## Custom Properties

This section covers custom properties you can use to add non-standard properties. This block is available at the top level of an OORS message and on each [result](results.md). The structure is shared across the Bitol standards and follows the same shape as [ODCS custom properties](https://bitol-io.github.io/open-data-contract-standard/latest/custom-other-properties/).

### Example

```yaml
customProperties:
  - id: rfc_ruleset_name
    property: refRulesetName
    value: gcsc.ruleset.name
  - property: orchestrator
    value: airflow
    description: Cluster name for specific applications
  - property: catalogId
    value: cat-987
    vendor: zeenea
```

```json
{
  "customProperties": [
    {
      "id": "rfc_ruleset_name",
      "property": "refRulesetName",
      "value": "gcsc.ruleset.name"
    },
    {
      "property": "orchestrator",
      "value": "airflow",
      "description": "Cluster name for specific applications"
    },
    {
      "property": "catalogId",
      "value": "cat-987",
      "vendor": "zeenea"
    }
  ]
}
```

Custom properties can equally be attached to an individual result:

```yaml
results:
  - id: completeness_customer_email
    type: check
    name: customer email completeness
    status: pass
    customProperties:
      - property: checkEngine
        value: soda-core
```

### Definitions

| Key                             | Type   | UX label          | Required | Description                                                                                                                                                                                                                                              |
| ------------------------------- | ------ | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `customProperties`              | array  | Custom Properties | No       | A list of key/value pairs for custom properties.                                                                                                                                                                                                          |
| `customProperties[].description`| string | Description       | No       | Description for humans.                                                                                                                                                                                                                                   |
| `customProperties[].id`         | string | ID                | No       | A unique identifier for the element used to create stable, refactor-safe references. Recommended for elements that will be referenced.                                                                                                                     |
| `customProperties[].property`   | string | Property          | Yes      | The name of the key. Names should be in camel case—the same as if they were permanent properties in the standard.                                                                                                                                          |
| `customProperties[].value`      | any    | Value             | Yes      | The value of the key. It can be an array.                                                                                                                                                                                                                 |
| `customProperties[].vendor`     | string | Vendor            | No       | Identifies the vendor, provider, or external system associated with this custom property. SHOULD be a stable, lowercase identifier matching `^[a-z0-9][a-z0-9-]*$` (e.g. `confluent`, `zeenea`). Tools MUST preserve unknown vendor values. (Added in v0.2.0, [RFC 0035](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v1.0.0/0035-extensions.md).) |

## Tags

Tags are lightweight, free-form classification labels that can be attached to the message and to individual results. See the dedicated [Tags](./tags.md) page.

## Other Properties

This section covers other properties you may find in an OORS message.

### Example

```yaml
apiVersion: v0.8.1
kind: ObservabilityResults
```

### Other properties definition

| Key          | Type   | UX label    | Required | Description                                                                    |
|--------------|--------|-------------|----------|--------------------------------------------------------------------------------|
| `apiVersion` | string | API Version | Yes      | Version of the OORS schema used. See [Fundamentals](./fundamentals.md).         |
| `kind`       | string | Kind        | Yes      | The type of OORS object. For OORS, always `ObservabilityResults`.               |

[Back to TOC](README.md)
