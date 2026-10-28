<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Negative tests

Each file here is a **deliberately invalid** OORS document. Every one of them **must be rejected** by
[`schema/oors-json-schema-v0.8.0.json`](../../schema/oors-json-schema-v0.8.0.json) — a run in which any
of them validates is a failure.

They guard the rules the positive examples cannot: that the schema actually enforces what the standard
says. The header comment in each file names the specific violation.

| Fixture | Violation |
| ------- | --------- |
| [`invalid-status.oors.yaml`](invalid-status.oors.yaml) | `status` outside the permitted set (`pass`, `fail`, `warn`, `error`, `skip`, `unknown`). |
| [`missing-results.oors.yaml`](missing-results.oors.yaml) | Required top-level `results` array omitted. |
| [`running-without-runid.oors.yaml`](running-without-runid.oors.yaml) | `run.state: running` without the `source.runId` the schema requires for interim snapshots. |
| [`invalid-tags.oors.yaml`](invalid-tags.oors.yaml) | `tags` given as a bare string instead of an array of strings. |

Run them with the same script CI uses:

```bash
bash scripts/validate-examples.sh
```
