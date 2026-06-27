---
title: "Changelog: Open Observability Results Standard (OORS)"
description: "Home of Open Observability Results Standard (OORS) documentation."
image: "https://raw.githubusercontent.com/bitol-io/artwork/main/horizontal/color/Bitol_Logo_color.svg"
---

This document tracks the history and evolution of the **Open Observability Results Standard**.

# v0.1.0 - DRAFT

Initial draft of the Open Observability Results Standard, based on [RFC-0018](https://github.com/bitol-io/tsc/blob/main/rfcs/0018-oors.md).

* Top-level fundamentals: `apiVersion`, `kind` (`ObservabilityResults`), `id`, `observedAt`.
* `source` object: `process` (required), `runID`, `vendor`.
* Optional `contract` object linking results to an ODCS/ODPS contract: `contractID`, `standard`, `version`.
* `results` array (at least one) of result objects, each with `id`, `type` (`check`/`metric`), `name`, `status` (`pass`/`fail`), and optional `severity`, `dimension`, `target` (`resourceType`, `resourceIdentifier`), `measure` (`metric`, `value`, `threshold`, `unit`), and `message`.
* JSON Schema (`oors-json-schema-v0.1.0.json`) and example messages.
