<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Open Observability Results Standard (OORS)

The **Open Observability Results Standard (OORS)** provides an easy, lightweight mechanism to exchange **data quality and data observability results** across systems.

OORS focuses on clarity, portability, and ease of implementation, letting teams communicate operational outcomes — such as data quality checks, freshness, latency, and SLA breaches — without requiring deep observability expertise or complex infrastructure. As an open format developed within the [Bitol](https://bitol.io) community, OORS is human-readable and follows the same philosophy as [ODCS](https://github.com/bitol-io/open-data-contract-standard) and [ODPS](https://github.com/bitol-io/open-data-product-standard), integrating naturally into existing data workflows.

> ## WORK IN PROGRESS — NOT READY FOR PRODUCTION
>
> OORS is a **draft standard**, published early so the community can see it and shape it. It is **not** a
> released standard. Expect **breaking changes in every version until v1.0.0**, and incomplete areas. Do
> not build production systems against it.

## Design goals

- **Lightweight** — a flat, concise structure; easy to print, log, send over HTTP, or push to a queue.
- **Tool-agnostic** — can wrap results from OpenTelemetry, vendor tools, ETL pipelines, custom applications, or ad-hoc scripts.
- **Contract-aware** — optional, first-class linkage to ODCS/ODPS IDs so data products can expose their health consistently.
- **Result-centric** — carries only the outcomes of checks (pass/fail, measured values, breaches), not raw metrics or traces.

## Documentation

- [Fundamentals](docs/fundamentals.md) — the top-level structure of an OORS message.
- [Results](docs/results.md) — the structure of an individual result.
- [Examples](docs/examples) — sample OORS messages.

The JSON Schema lives in [`schema/`](schema/). Files using the `.oors.yaml` extension are auto-detected as OORS.

A reader-friendly version of the standard is published at
<https://bitol-io.github.io/open-observability-results-standard/>.

## Example

```yaml
apiVersion: v0.8.0
kind: ObservabilityResults
id: 9f1c8a52-2b4e-4a6e-9d3e-7c2a1f0b4d11
observedAt: "2025-12-25T17:00:00+01:00"
source:
  process: order-preprocessor-dbt
  runId: "27a3de3d"
  vendor: inhouse-build
results:
  - id: dup_rows_myTable
    type: check
    name: No duplicate rows
    status: fail
    severity: critical
    dimension: uniqueness
    target:
      resourceType: table
      resourceIdentifier: myTable
    measure:
      metric: rowCount
      value: 3
      threshold:
        mustBe: 0
      unit: rows
    message: Multiple duplicate rows found in myTable!
```

## Contributing

OORS is governed by the Bitol Technical Steering Committee (TSC). See [CONTRIBUTING.md](CONTRIBUTING.md) and the [TSC contributing guidelines](https://github.com/bitol-io/tsc/blob/main/CONTRIBUTING.md). The defining RFC is [RFC-0018 (OORS)](https://github.com/bitol-io/tsc/blob/main/rfcs/approved/oors-v0.8.0/0018-oors.md), approved by the TSC on 2026-10-14.

## License

This project is licensed under the Apache License 2.0 — see [LICENSE](LICENSE).
