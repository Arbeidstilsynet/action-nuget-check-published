# Arbeidstilsynet/action-nuget-check-published

Action to check if a NuGet package version is already published

## Requirements

- GitHub Actions runner with Bash shell support (Linux, macOS, or Windows with Bash)
- [`jq`](https://stedolan.github.io/jq/) installed on the runner (available by default on Ubuntu runners)

## Inputs

| Name             | Description                                                                | Required | Default                  |
|------------------|----------------------------------------------------------------------------|----------|--------------------------|
| `name`           | The package name                                                           | Yes      |                          |
| `version`        | The package version                                                        | Yes      |                          |
| `source-feed`    | The source feed to check against. Must match a key in your `nuget.config`. | No       | `nuget.org`              |
| `dotnet-version` | The version of dotnet to use                                               | No       | `10.0.x`                 |
| `config-file`    | Custom nuget.config location                                               | No       | `nuget.config` (at root) |

## Outputs

| Name                      | Description                                                              |
|---------------------------|--------------------------------------------------------------------------|
| `published`               | `true` if the version is already published, `false` otherwise            |
| `last_version`            | The last stable version published on the source feed (empty if none)     |
| `last_prerelease_version` | The last prerelease version published on the source feed (empty if none) |

## Usage

```yaml
name: Check NuGet Package Version

on:
  push:
    branches:
      - main

jobs:
  check-nuget-version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - uses: Arbeidstilsynet/action-nuget-check-published@v2
        id: nuget_version_check
        with:
          name: MyPackage
          version: 1.2.3

      - name: Show results
        run: |
          echo "Published: ${{ steps.nuget_version_check.outputs.published }}"
          echo "Last version: ${{ steps.nuget_version_check.outputs.last_version }}"
          echo "Last prerelease version: ${{ steps.nuget_version_check.outputs.last_prerelease_version }}"
```

## Versioning

This repository uses a simple versioning system based on the `VERSION` file.
When you update the `VERSION` file and push to `main`, a Git tag with that version is created or updated automatically by the workflow.
If you make breaking changes to the action, bump the version and update `CHANGELOG.md`.
