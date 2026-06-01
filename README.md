# crest-verify-action

[![crest verified](https://verify.crestsystems.ai/badge/cm-001.svg)](https://crestsystems.ai/verify/cm-001)

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

1. Checks your endpoint against Crest conformance fixtures
2. Posts results as a GitHub step summary
3. Comments on PRs with verification status
4. Optionally publishes to the [conformance matrix](https://crestsystems.ai/verify)
5. Fails the workflow if verification fails

## Get your project verified

1. [Submit your project](https://crestsystems.ai/verify/start) for verification
2. We review and add you to the matrix (24-48 hours)
3. You receive a matrix entry ID and submit key
4. Add this action to your CI for continuous verification

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `endpoint` | yes | | URL to verify |
| `matrix-id` | no | | Your matrix entry ID (e.g., `cm-004`) |
| `publish` | no | `false` | Submit results to the conformance matrix |
| `submit-key` | no | | Your Crest submit key (store as a GitHub secret) |
| `fail-on-error` | no | `true` | Fail the workflow on verification failure |
| `comment` | no | `true` | Post results as a PR comment |

## Outputs

| Output | Description |
|--------|-------------|
| `status` | `pass`, `fail`, `caution`, or `error` |
| `badge-url` | Badge URL for your matrix entry |
| `summary` | Human-readable result |

## Badge

After verification, add to your README:

```markdown
[![crest verified](https://verify.crestsystems.ai/badge/YOUR-ID.svg)](https://crestsystems.ai/verify/YOUR-ID)
```

## Full Example (with auto-publish)

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
          submit-key: ${{ secrets.CREST_SUBMIT_KEY }}

      - name: Check result
        run: echo "Status: ${{ steps.crest.outputs.status }}"
```

### Setting up auto-publish

1. [Get verified](https://crestsystems.ai/verify/start) to receive your matrix entry and submit key
2. Add the key as a GitHub secret: `CREST_SUBMIT_KEY`
3. Set `publish: true` and `submit-key: ${{ secrets.CREST_SUBMIT_KEY }}`
4. Every passing CI run auto-updates your matrix entry and badge

## Links

- [Verify your project](https://crestsystems.ai/verify)
- [Conformance matrix](https://crestsystems.ai/verify)
- [Submit for verification](https://crestsystems.ai/verify/start)
- [Conformance fixtures](https://github.com/andysalvo/substrate-attestation)

## License

Apache 2.0
