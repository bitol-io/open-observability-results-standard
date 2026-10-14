---
title: "Run Lifecycle"
description: "Terminal by default, progressive by choice — the optional run object for reporting long-running executions."
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Run Lifecycle

An OORS document is **terminal by default**: without a `run` object, the document is the authoritative record of the run that produced it — exactly the pre-v0.2.1 semantics. Producers of long-running executions (multi-step pipelines, large batch jobs) can **opt in** to progressive reporting by adding the `run` object and emitting interim snapshots while the run is still going.

[Back to TOC](README.md)

## Example

### Interim snapshot

A pipeline that has completed step 12 of 14 reports the step's observation and marks the document as an interim snapshot of a running execution. `source.runId` is required so consumers can correlate the documents of one run.

```yaml
apiVersion: v0.8.0
kind: ObservabilityResults
id: 1c40b98e-5a7d-4d2e-9f31-8b6c2e0a4f77
observedAt: "2026-07-07T20:15:51Z"
source:
  process: imdb-silver
  runId: silver-1783398037701
run:
  state: running
  sequence: 12
  progress:
    current: 12
    total: 14
    unit: steps
results:
  - id: rows_title_alias
    type: metric
    name: title_alias
    status: pass
    dimension: volume
    target:
      resourceType: table
      resourceIdentifier: title_alias
    measure:
      metric: rows_loaded
      value: 58518276
      unit: rows
```

```json
{
  "apiVersion": "v0.8.0",
  "kind": "ObservabilityResults",
  "id": "1c40b98e-5a7d-4d2e-9f31-8b6c2e0a4f77",
  "observedAt": "2026-07-07T20:15:51Z",
  "source": {
    "process": "imdb-silver",
    "runId": "silver-1783398037701"
  },
  "run": {
    "state": "running",
    "sequence": 12,
    "progress": { "current": 12, "total": 14, "unit": "steps" }
  },
  "results": [
    {
      "id": "rows_title_alias",
      "type": "metric",
      "name": "title_alias",
      "status": "pass",
      "dimension": "volume",
      "target": { "resourceType": "table", "resourceIdentifier": "title_alias" },
      "measure": { "metric": "rows_loaded", "value": 58518276, "unit": "rows" }
    }
  ]
}
```

### Terminal document

The same run's final document — `state: completed` (or simply no `run` object at all) — carries the full summary and supersedes every interim snapshot with the same `source.runId`.

```yaml
apiVersion: v0.8.0
kind: ObservabilityResults
id: efa58843-12ec-418b-963d-40dda853f856
observedAt: "2026-07-07T20:43:31Z"
source:
  process: imdb-silver
  runId: silver-1783398037701
run:
  state: completed
  sequence: 15
  progress:
    current: 14
    total: 14
    unit: steps
results:
  - id: run
    type: check
    name: Silver build run
    status: pass
```

## Field Descriptions

| Key                    | Type    | UX label  | Required        | Description                                                                                                                                                                          | Example   |
| ---------------------- | ------- | --------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| `run`                  | object  | Run       | No              | Run lifecycle object. **Absent ⇒ the document is terminal** (authoritative record of the run — the pre-v0.2.1 behavior).                                                             |           |
| `run.state`            | string  | State     | Yes (in `run`)  | `running` — interim snapshot, more documents will follow for this `source.runId`. `completed` — terminal, the run finished. `aborted` — terminal, the run ended before producing its full results. | `running` |
| `run.sequence`         | integer | Sequence  | No              | Monotonically increasing ordinal of this document within the run. Consumers SHOULD order by `sequence` when present, falling back to `observedAt`.                                   | `12`      |
| `run.progress`         | object  | Progress  | No              | Progress indicator for the run.                                                                                                                                                       |           |
| `run.progress.current` | number  | Current   | Yes (in `progress`) | The amount of progress made so far, e.g., steps completed.                                                                                                                        | `12`      |
| `run.progress.total`   | number  | Total     | No              | The total amount expected, e.g., total steps. Omit when unknown.                                                                                                                      | `14`      |
| `run.progress.unit`    | string  | Unit      | No              | The unit of progress, e.g., `steps`, `tables`, `files`, `percent`.                                                                                                                    | `steps`   |

## Semantics

1. **Terminal by default.** A document without a `run` object, or with `run.state` of `completed` or `aborted`, is **terminal**: the authoritative record for its `source.runId`. Every pre-v0.2.1 document is therefore already valid and terminal — nothing to migrate.
2. **Interim snapshots.** A document with `run.state: running` is a partial snapshot of a run still in progress. Consumers MUST NOT treat it as the run's outcome. `source.runId` is **required** on interim documents (enforced by the JSON schema) so they can be correlated.
3. **Supersession.** A terminal document supersedes all interim documents sharing its `source.runId`. The run's *outcome* is derived from the terminal document's `results` (its pass/fail checks) — `run.state` describes the lifecycle, not the quality outcome. `aborted` means only "no full summary is coming"; producers SHOULD still include a failing `check` describing why when they can.
4. **Observations stand alone.** Each result in an interim document is a real observation that individually passed or failed — `results[].status` keeps its exact meaning. No new status values are introduced.
5. **Ordering.** Consumers SHOULD order a run's documents by `run.sequence` when present, falling back to `observedAt`. Producers SHOULD make `sequence` strictly increasing per `runId`.

> **Consumer guidance (non-normative):** a run whose latest document says `running` and that has produced no new document for an implementation-defined window may be rendered as *stale* — the wire format deliberately carries no heartbeat or timeout.
