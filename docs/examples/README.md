---
title: "Examples"
description: "Complete OORS message examples."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Examples

Complete OORS message examples. Each file uses the `.oors.yaml` extension and validates against [`schema/oors-json-schema-v0.2.0.json`](../../schema/oors-json-schema-v0.2.0.json).

- [`single-result.oors.yaml`](single-result.oors.yaml) — a single check, with no contract reference.
- [`multiple-results-with-contract.oors.yaml`](multiple-results-with-contract.oors.yaml) — multiple results (a check and a metric) linked to an ODCS contract.
- [`with-custom-properties.oors.yaml`](with-custom-properties.oors.yaml) — using `relatesTo` and `customProperties` (top level and per result).
