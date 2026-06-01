# crest-verify-action

[![CREST conformance](https://verify.crestsystems.ai/badge/cm-001.svg)](https://verify.crestsystems.ai/matrix)

Run Crest conformance verification in your CI. Get a badge.

## Quick Start

```yaml
name: Crest Verify
on: [push, pull_request]
jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: andysalvo/crest-verify-action@v1
        with:
          endpoint: https://your-service.com
```

## What it does

1. Runs `@crestdeploymentsystems/verify` against your endpoint
2. Posts results as a GitHub step summary
3. Comments on PRs with verification status
4. Optionally publishes to the [conformance matrix](https://verify.crestsystems.ai/matrix)
5. Fails the workflow if verification fails

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `endpoint` | yes | | URL to verify |
| `matrix-id` | no | | Your matrix entry ID (e.g., `cm-004`) |
| `publish` | no | `false` | Submit results to the conformance matrix |
| `fail-on-error` | no | `true` | Fail the workflow on verification failure |
| `comment` | no | `true` | Post results as a PR comment |

## Outputs

| Output | Description |
|--------|-------------|
| `status` | `pass`, `fail`, or `error` |
| `result-json` | Path to the full result JSON |
| `badge-url` | Badge URL for your matrix entry |
| `summary` | Human-readable result |

## Badge

After verification, add to your README:

```markdown
[![CREST conformance](https://verify.crestsystems.ai/badge/YOUR-ID.svg)](https://verify.crestsystems.ai/matrix)
```

## Full Example

```yaml
name: Crest Verify
on:
  push:
    branches: [main]
  pull_request:

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: andysalvo/crest-verify-action@v1
        id: crest
        with:
          endpoint: https://your-service.com
          matrix-id: cm-004
          publish: true

      - name: Check result
        run: echo "Status: ${{ steps.crest.outputs.status }}"
```

## Conformance Matrix

View all verified projects at [verify.crestsystems.ai/matrix](https://verify.crestsystems.ai/matrix).

## License

Apache 2.0
