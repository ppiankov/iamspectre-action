# IAMSpectre Action

GitHub Action for [IAMSpectre](https://github.com/ppiankov/iamspectre) — audit AWS and GCP IAM for stale credentials, overprivileged roles, and policy drift.

## What it does

- Downloads the IAMSpectre binary (auto-detects latest version)
- Audits either AWS or GCP IAM based on the `cloud` input
- Checks for stale credentials, overprivileged access, and risky IAM findings
- Optionally uploads SARIF results to GitHub Security tab
- Fails the step on configuration/runtime errors (exit code 2); exposes the scan exit code as an output, and optionally fails the step on findings too via `fail-on-findings`

## What it does NOT do

- Does not configure AWS or GCP credentials for the runner
- Does not modify IAM roles, policies, users, or service accounts
- Does not audit AWS and GCP in the same action invocation

## Usage

Set up cloud credentials before running the action. For AWS, provide credentials on the runner using your standard AWS authentication flow. For GCP, authenticate with Application Default Credentials and ensure the runner can access the target project.

### AWS audit

```yaml
- uses: ppiankov/iamspectre-action@v1
  with:
    cloud: aws
    profile: production
```

### GCP audit

```yaml
- uses: ppiankov/iamspectre-action@v1
  with:
    cloud: gcp
    project: my-project-id
```

### With severity filter

```yaml
- uses: ppiankov/iamspectre-action@v1
  with:
    cloud: aws
    severity-min: high
```

### Fail the build on findings

```yaml
- uses: ppiankov/iamspectre-action@v1
  with:
    cloud: aws
    fail-on-findings: 'true'
```

### SARIF upload to GitHub Security tab

```yaml
- uses: ppiankov/iamspectre-action@v1
  with:
    cloud: aws
    format: sarif
    upload-sarif: 'true'
```

### Pin to specific version

```yaml
- uses: ppiankov/iamspectre-action@v1
  with:
    cloud: gcp
    project: my-project-id
    version: '0.1.0'
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `cloud` | Yes | — | Which cloud to audit: `aws` or `gcp` |
| `project` | No | — | GCP project ID; required when `cloud: gcp`, ignored for AWS |
| `profile` | No | — | AWS profile name; only used when `cloud: aws` |
| `stale-days` | No | `90` | Inactivity threshold in days |
| `severity-min` | No | `low` | Minimum severity: `critical`, `high`, `medium`, `low` |
| `format` | No | `text` | Output format: `text`, `report`, `json`, `sarif`, `spectrehub` |
| `timeout` | No | `5m` | Scan timeout |
| `version` | No | `latest` | IAMSpectre version (for example, `0.1.0`) |
| `args` | No | — | Additional arguments passed to `iamspectre aws` or `iamspectre gcp` |
| `upload-sarif` | No | `false` | Upload SARIF to GitHub Security tab (requires `format: sarif`) |
| `fail-on-findings` | No | `false` | Fail the step (not just warn) when findings are detected (exit code 1) |

## Outputs

| Output | Description |
|--------|-------------|
| `exit-code` | Exit code from `iamspectre` (`0=ok`, `1=findings`, `2=error`) |
| `report-path` | Path to generated report file (`report`, `json`, `sarif`, `spectrehub` formats) |

## Exit codes

| Code | Meaning |
|------|---------|
| 0 | Audit completed without findings |
| 1 | Findings detected; reported as a GitHub Actions warning by default. The step still succeeds unless `fail-on-findings: 'true'` is set, in which case it fails the step too |
| 2 | Configuration, credential, or runtime error; always fails the step |

## License

MIT
