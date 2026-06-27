<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Open Observability Results Standard (OORS)

The Open Observability Results Standard (OORS) is a lightweight, tool-agnostic format for exchanging **data quality and data observability results**.

An OORS message reports the *outcomes* of checks and metrics — pass/fail, measured values, threshold breaches — optionally linked to the data contract (ODCS/ODPS) that specified them. It deliberately does **not** carry raw metrics, traces, or logs.

## Structure

An OORS message is composed of:

- **Fundamentals** — the top-level envelope (`apiVersion`, `kind`, `id`, `observedAt`) and the `source` and optional `contract` objects. See [Fundamentals](fundamentals.md).
- **Results** — an array of individual observations, each a `check` or a `metric`. See [Results](results.md).

See [Examples](examples/README.md) for complete messages, and the JSON Schema in the [`schema/`](https://github.com/bitol-io/open-observability-results-standard/tree/main/schema) directory.

OORS is defined by [RFC-0018](https://github.com/bitol-io/tsc/blob/main/rfcs/0018-oors.md) and is part of the [Bitol](https://bitol.io) family of standards, alongside [ODCS](https://github.com/bitol-io/open-data-contract-standard) and [ODPS](https://github.com/bitol-io/open-data-product-standard).
