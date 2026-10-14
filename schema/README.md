<!--
Copyright 2026 The Bitol Contributors
SPDX-License-Identifier: Apache-2.0
-->

# OORS JSON Schema

You will find all the versions of the Open Observability Results Standard (OORS) JSON Schema files here. Each version will be 
stored here for reference and to allow [SchemaStore](https://github.com/SchemaStore/schemastore) to keep track of each
version.

If you want your files to be automatically detected as OORS files, use the file extension `.oors.yaml`. For example,
`my-results.oors.yaml`.

## SchemaStore

SchemaStore is a widely adopted repository of JSON schema files for integration with tools such as Intellij and VS Code.
OORS is not yet onboarded to SchemaStore; a pull request should be opened (mirroring the [ODCS](https://github.com/SchemaStore/schemastore) and [ODPS](https://github.com/SchemaStore/schemastore/pull/3868) registrations) once OORS reaches a public release.

## Versioning

When a new version of OORS JSON Schema is available, a pull request to SchemaStore should be created to update the 
following section:

```json
    {
      "name": "Open Observability Results Standard (OORS)",
      ...,
      "versions": {
        "<new_version>": "https://github.com/bitol-io/open-observability-results-standard/blob/main/schema/oors-json-schema-<new_version>.json",
        "v0.8.0": "https://github.com/bitol-io/open-observability-results-standard/blob/main/schema/oors-json-schema-v0.8.0.json",
        ...
        "v0.2.0": "https://github.com/bitol-io/open-observability-results-standard/blob/main/schema/oors-json-schema-v0.2.0.json",
        "v0.1.0": "https://github.com/bitol-io/open-observability-results-standard/blob/main/schema/oors-json-schema-v0.1.0.json"
      }
    },
```

And the `oors-json-schema-latest.json` should be updated with the latest version changes.
