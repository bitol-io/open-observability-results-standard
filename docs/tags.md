---
title: "Tags"
description: "Lightweight, free-form classification labels attached to an OORS message or result."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Tags

Tags are lightweight, free-form classification labels that can be attached to an OORS message and to individual results. The structure is shared across the Bitol standards and follows the same shape everywhere it appears.

[Back to TOC](README.md)

## What tags are for

Tags are intended for **categorization** — making observations easier to find, filter, group, and route through tooling. Common uses include:

- **Domain or topic**: `finance`, `marketing`, `iot`
- **Sensitivity / compliance**: `pii`, `phi`, `gdpr`, `pci`, `sensitive`
- **Lifecycle**: `experimental`, `deprecated`, `terminal`
- **Audience**: `internal`, `external`, `partner`
- **Operational hint**: `hot`, `cold`, `archive`

Tags are deliberately not a controlled vocabulary: your organisation defines what each tag means. For vendor- or organisation-specific metadata that carries a value rather than a label, prefer [custom properties](./custom-other-properties.md).

## Structure

A `tags` value is always an **array of strings**. Empty arrays are allowed; duplicate strings within the same array should be avoided.

```yaml
tags: ['finance', 'sensitive']
```

The same structure applies wherever `tags` is permitted.

## Where tags can appear

`tags` is available in the following places:

| Location                  | Notes                                                                                   |
| ------------------------- | --------------------------------------------------------------------------------------- |
| Top-level (the message)   | Tags applied to the observation run as a whole. See [Fundamentals](./fundamentals.md).   |
| Result                    | Tags applied to an individual check/metric. See [Results](./results.md).                 |

## Recommendations

- **Lowercase, snake_case or single words.** Mixed casing makes filtering brittle.
- **Be consistent.** A tag `pii` in one message and `PII` in another defeats the purpose. Document your tag taxonomy organisation-wide.
- **Prefer specific over generic.** `customer_pii` is more useful than `sensitive` once you have many messages.
- **Avoid encoding hierarchy in tag strings.** `finance/regulatory` is harder for tooling than two separate tags `finance`, `regulatory`.
- **Keep tags small in number.** A handful per element is plenty; long tag lists become noise.

## Example

```yaml
apiVersion: v0.8.0
kind: ObservabilityResults
id: 9f1c8a52-2b4e-4a6e-9d3e-7c2a1f0b4d11
observedAt: "2026-01-15T09:30:00Z"
tags: ['finance', 'sox']
source:
  process: nightly-quality-suite
results:
  - id: no_dup_rows
    type: check
    name: No duplicate rows
    status: pass
    tags: ['critical-path', 'uniqueness']
```

[Back to TOC](README.md)
