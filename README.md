# setup-sapl

GitHub Action to install the [SAPL](https://sapl.io) CLI for policy testing, evaluation, and coverage analysis.

## Usage

### Run policy tests

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
  - run: sapl test --dir ./policies
```

### Pin a specific version

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
    with:
      version: '4.0.0'
  - run: sapl test --dir ./policies
```

### Use the latest snapshot

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
    with:
      version: snapshot
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
    os: [ubuntu-latest, windows-latest, macos-latest]
runs-on: ${{ matrix.os }}
steps:
  - uses: actions/checkout@v4
  - uses: heutelbeck/setup-sapl@v1
  - run: sapl test --dir ./policies
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `version` | SAPL version: `latest`, `snapshot`, or a specific version like `4.0.0` | `latest` |
| `token` | GitHub token for downloading release assets | `${{ github.token }}` |

## Outputs

| Output | Description |
|--------|-------------|
| `version` | Version string reported by the installed binary |

## Supported platforms

| Runner | Architecture |
|--------|-------------|
| `ubuntu-latest` | x86_64 |
| `ubuntu-24.04-arm` | ARM64 |
| `windows-latest` | x86_64 |
| `macos-latest` | ARM64 |

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
