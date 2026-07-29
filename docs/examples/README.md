---
title: "Examples"
description: "Complete OORS message examples, minimal and full, in YAML and JSON."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Examples

Complete OORS messages: a **minimal** example (only the required fields), a **full** example (every field), and a **progressive** example (an interim snapshot of a run still in progress — see [Run Lifecycle](../run-lifecycle.md)). Each is available in **YAML** and **JSON**, and validates against [`schema/oors-json-schema-v0.2.2.json`](../../schema/oors-json-schema-v0.2.2.json).

| Example | YAML | JSON |
| ------- | ---- | ---- |
| Minimal — only required fields | [`minimal.oors.yaml`](minimal.oors.yaml) | [`minimal.oors.json`](minimal.oors.json) |
| Full — every field, two results, contract & custom properties | [`full.oors.yaml`](full.oors.yaml) | [`full.oors.json`](full.oors.json) |
| Progressive — interim snapshot (`run.state: running`) of a multi-step pipeline | [`progressive.oors.yaml`](progressive.oors.yaml) | [`progressive.oors.json`](progressive.oors.json) |
| Variables — `${VAR_NAME}` / `${VAR_NAME:-default}` interpolation per RFC-0050 | [`variables.oors.yaml`](variables.oors.yaml) | [`variables.oors.json`](variables.oors.json) |
