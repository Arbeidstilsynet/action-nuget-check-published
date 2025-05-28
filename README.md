# Arbeidstilsynet/action-nuget-check-published

Action to check if a NuGet package version is already published

## Requirements

- GitHub Actions runner with Bash shell support (Linux, macOS, or Windows with Bash)
- [.NET SDK](https://dotnet.microsoft.com/download) (automatically installed by the action)
- [`jq`](https://stedolan.github.io/jq/) installed on the runner (available by default on Ubuntu runners)

## Inputs

| Name             | Description                                                                | Required | Default     |
|------------------|----------------------------------------------------------------------------|----------|-------------|
| `name`           | The package name                                                           | Yes      |             |
| `version`        | The package version                                                        | Yes      |             |
| `source-feed`    | The source feed to check against. Must match a key in your `nuget.config`. | No       | `nuget.org` |
| `dotnet-version` | The version of dotnet to use                                               | No       | `8.0.x`     |

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
      - uses: actions/checkout@v4

      - uses: Arbeidstilsynet/action-nuget-check-published@v1
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
