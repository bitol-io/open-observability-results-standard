---
title: "Variables"
description: "Variable interpolation in OORS: keep secrets and environment-specific values out of the message with ${VAR_NAME} references resolved at runtime by tooling."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Variables

Any string value in an OORS message MAY contain one or more variable references of the form `${VAR_NAME}`, resolved at runtime by tooling. The mechanism is shared between all Bitol standards ([RFC 0050](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v1.0.0/0050-variables.md), shared with ODCS and ODPS). It keeps environment-specific values — schema names, run identifiers, resource paths — out of the document itself.

References MAY appear as a whole value or as a substring. A reference MAY carry an inline default using the POSIX `${VAR_NAME:-default}` form: the text between `:-` and the closing `}` is used verbatim when the variable is unset or empty.

`VAR_NAME` is an identifier chosen by the author. Resolution is intentionally left to tooling; common sources include OS environment variables, `.env` files, secret managers, and CI/CD pipeline variables.

## Example

```yaml
source:
  process: nightly-quality-suite
  runID: ${CI_PIPELINE_RUN_ID}
results:
  - id: no_dup_rows
    type: check
    name: No duplicate rows
    status: pass
    target:
      resourceType: table
      resourceIdentifier: ${DB_SCHEMA:-public}.orders
```

## Syntax

| Form                    | Behavior when the variable is set | Behavior when the variable is unset or empty |
|-------------------------|-----------------------------------|----------------------------------------------|
| `${VAR_NAME}`           | Replaced by the variable's value. | Tooling SHOULD surface an error; it MUST NOT silently substitute an empty string. |
| `${VAR_NAME:-default}`  | Replaced by the variable's value. | Replaced by `default`, verbatim.             |

## Tooling behavior

* Tools MUST resolve `${VAR_NAME}` references before using the value for any purpose.
* If a referenced variable cannot be resolved (and no default is supplied), tools SHOULD surface an error and MUST NOT silently substitute an empty string.
* Tools MUST preserve unresolved `${VAR_NAME}` and `${VAR_NAME:-default}` tokens verbatim when serializing a message back to YAML or JSON (round-trip safety).
* Tools MAY define their own resolution order across sources (for example, OS environment variable before `.env` file).

## Notes

* Interpolation applies to **string** values only: a field typed as a number, boolean, or a formatted string (such as the date-time `observedAt`) cannot hold an unresolved variable reference, as the schema rejects it.
* No new section or field is added to the standard: a message using variables validates against the standard JSON schema as-is.
