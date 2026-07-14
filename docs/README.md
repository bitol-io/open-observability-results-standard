---
title: "Open Observability Results Standard (OORS)"
description: "A lightweight, tool-agnostic format for exchanging data quality and data observability results."
image: "https://raw.githubusercontent.com/bitol-io/artwork/main/horizontal/color/Bitol_Logo_color.svg"
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Open Observability Results Standard (OORS)

The Open Observability Results Standard (OORS) is a lightweight, tool-agnostic format for exchanging **data quality and data observability results**.

An OORS message reports the *outcomes* of checks and metrics — pass/fail, measured values, threshold breaches — optionally linked to the data contract (ODCS/ODPS) that specified them. It deliberately does **not** carry raw metrics, traces, or logs.

> **Status:** OORS is an early-stage standard (v0.2.1, draft) incubated within the Bitol TSC. Expect breaking changes until v1.0.0.

## Table of Contents

* [Fundamentals](fundamentals.md) — the message envelope (`apiVersion`, `kind`, `id`, `observedAt`) plus the `source` and optional `contract` objects.
* [Results](results.md) — the structure of an individual observation (`check` or `metric`).
* [Run Lifecycle](run-lifecycle.md) — terminal by default, progressive by choice: the optional `run` object for long-running executions.
* [Custom & Other Properties](custom-other-properties.md) — extending OORS with `customProperties`.
* [Variables](variables.md) — `${VAR_NAME}` interpolation for environment-specific values, resolved at runtime by tooling.
* [Examples](examples/README.md) — complete OORS messages.

## Design goals

* **Lightweight** — a flat, concise structure; easy to print, log, send over HTTP, or push to a queue.
* **Tool-agnostic** — can wrap results from OpenTelemetry, vendor tools, ETL pipelines, custom applications, or ad-hoc scripts.
* **Contract-aware** — optional, first-class linkage to ODCS/ODPS IDs so data products can expose their health consistently.
* **Result-centric** — carries only the outcomes of checks (pass/fail, measured values, breaches), not raw metrics or traces.

OORS is defined by [RFC-0018](https://github.com/bitol-io/tsc/blob/main/rfcs/0018-oors.md) and is part of the [Bitol](https://bitol.io) family of standards, alongside [ODCS](https://github.com/bitol-io/open-data-contract-standard) and [ODPS](https://github.com/bitol-io/open-data-product-standard).
