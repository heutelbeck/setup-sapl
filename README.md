# setup-sapl

GitHub Action to install the [SAPL](https://sapl.io) CLI for policy testing, evaluation, and coverage analysis.

> **Preview:** SAPL 4.0 is in preview. Only `version: snapshot` is available. Stable version pinning will be supported once 4.0.0 is released.

## Usage

### Run policy tests

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
  - run: sapl test --dir ./policies
```

### Enforce coverage quality gates

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
  - run: >
      sapl test --dir ./policies
      --policy-hit-ratio 100
      --condition-hit-ratio 80
```

### Generate SonarQube coverage report

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
  - run: sapl test --dir ./policies --sonar --output sapl-coverage
  - uses: SonarSource/sonarqube-scan-action@v5
    with:
      args: >
        -Dsonar.coverageReportPaths=sapl-coverage/sonar/sonar-generic-coverage.xml
```

### Multi-platform matrix

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
runs-on: ${{ matrix.os }}
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
  - run: sapl test --dir ./policies
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `version` | `snapshot`, `latest` (once stable releases exist), or a specific version like `4.0.0` | `snapshot` |
| `token` | GitHub token for downloading release assets | `${{ github.token }}` |

## Outputs

| Output | Description |
|--------|-------------|
| `version` | Version string reported by the installed binary |

## Supported platforms

| Runner | Architecture | Snapshot | Stable (future) |
|--------|-------------|----------|------------------|
| `ubuntu-latest` | x86_64 | Yes | Yes |
| `ubuntu-24.04-arm` | ARM64 | Yes | Yes |
| `windows-latest` | x86_64 | Yes | Yes |
| `macos-latest` | ARM64 | No | Yes |

## About SAPL

SAPL (Streaming Attribute Policy Language) is an attribute-based access control (ABAC) policy language and engine. The `sapl` CLI provides:

- **`sapl test`** -- Run `.sapltest` files against `.sapl` policies with coverage reporting
- **`sapl decide-once`** -- Evaluate a single authorization decision
- **`sapl decide`** -- Stream authorization decisions (reactive)
- **`sapl check`** -- Exit-code-based decisions for shell scripts
- **`sapl bundle`** -- Create, sign, verify, and inspect policy bundles

Learn more at [sapl.io](https://sapl.io) and [the documentation](https://sapl.io/docs).

## License

Apache License 2.0
