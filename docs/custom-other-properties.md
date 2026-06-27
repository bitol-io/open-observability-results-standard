---
title: "Custom & Other Properties"
description: "Extending OORS with custom properties."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Custom & Other Properties

OORS can be extended with custom properties for vendor- or organization-specific metadata that is not part of the standard.

[Back to TOC](README.md)

## Custom Properties

`customProperties` is available at the top level of an OORS message and on each [result](results.md). The structure is shared across the Bitol standards and follows the [ODCS Custom & Other Properties structure](https://bitol-io.github.io/open-data-contract-standard/latest/custom-other-properties/).

| Key                             | Type   | UX label          | Required | Description                                                                                                                            |
| ------------------------------- | ------ | ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| customProperties                | array  | Custom Properties | No       | A list of key/value pairs for custom properties.                                                                                     |
| customProperties[].**property** | string | Property          | Yes      | The name of the key. Names should be in camel case—the same as if they were permanent properties in the standard.                    |
| customProperties[].**value**    | any    | Value             | Yes      | The value of the key.                                                                                                                |
| customProperties[].description  | string | Description       | No       | Optional description of the custom property.                                                                                          |
| customProperties[].id           | string | ID                | No       | Optional unique identifier for the custom property, for stable references.                                                            |

## Example

```yaml
customProperties:
  - property: environment
    value: production
  - property: orchestrator
    value: airflow
    description: The scheduler that triggered this run.
```

Custom properties may also be attached to an individual result, for example to record the engine that produced an observation:

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
```
