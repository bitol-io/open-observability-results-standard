---
title: "Contributing: Open Observability Results Standard (OORS)"
description: "How you can contribute to the Open Observability Results Standard (OORS)."
image: "https://raw.githubusercontent.com/bitol-io/artwork/main/horizontal/color/Bitol_Logo_color.svg"
---

<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# Contributing to Open Observability Results Standard

Thank you for your interest in contributing to Open Observability Results Standard (OORS). Please refer to the [TSC contributing guidelines](https://github.com/bitol-io/tsc/blob/main/CONTRIBUTING.md).

## Branching and merging

`main` is protected by a repository rule that requires changes to land through a **pull request** —
direct pushes to `main` are rejected.

Reviews are not required. Any TSC member with write access can merge their own pull request
immediately; a second reviewer is welcome but never a gate. `main` cannot be force-pushed or deleted.

The `validate-examples` workflow runs on every pull request and verifies the examples against the
schema. It is advisory: a red run should be understood, not ignored.

Documentation is published automatically from `main` to
<https://bitol-io.github.io/open-observability-results-standard/>.
