---
title: "Examples"
description: "Complete OORS message examples, minimal and full, in YAML and JSON."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Examples

Complete OORS messages, provided both as a **minimal** example (only the required fields) and a **full** example (every field). Each is available in **YAML** and **JSON**, and validates against [`schema/oors-json-schema-v0.2.0.json`](../../schema/oors-json-schema-v0.2.0.json).

| Example | YAML | JSON |
| ------- | ---- | ---- |
| Minimal — only required fields | [`minimal.oors.yaml`](minimal.oors.yaml) | [`minimal.oors.json`](minimal.oors.json) |
| Full — every field, two results, contract & custom properties | [`full.oors.yaml`](full.oors.yaml) | [`full.oors.json`](full.oors.json) |
| Variables — `${VAR_NAME}` / `${VAR_NAME:-default}` interpolation per RFC-0050 | [`variables.oors.yaml`](variables.oors.yaml) | [`variables.oors.json`](variables.oors.json) |
