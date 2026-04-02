# Oxidized MC — Reusable CI/CD Workflows

Centralized, modular GitHub Actions workflows for every repository in the
[Oxidized MC](https://github.com/oxidized-mc) ecosystem.

## Workflows

### Universal (all Rust crates)

| Workflow | File | Description |
|----------|------|-------------|
| **Rust CI** | `rust-ci.yml` | Lint (fmt + clippy @ MSRV), test (cross-OS), cargo-deny, MSRV check |
| **Commit Lint** | `commit-lint.yml` | Conventional commit validation |
| **Security Audit** | `security-audit.yml` | `cargo-audit` advisory scanning |

### Binary projects (server, client-headless)

| Workflow | File | Description |
|----------|------|-------------|
| **Release Please** | `release-please.yml` | Automated release PRs and changelogs |
| **Dev Release** | `dev-release.yml` | Nightly binary builds (5 targets) |
| **Release Binaries** | `release-binaries.yml` | Stable release builds on tag |
| **Docker** | `docker.yml` | Multi-arch Docker images to GHCR |

## Usage

### Library crate (minimal)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  ci:
    uses: oxidized-mc/ci/.github/workflows/rust-ci.yml@main
```

```yaml
# .github/workflows/commit-lint.yml
name: Commit Lint
on:
  pull_request:
    branches: [main]
jobs:
  lint:
    uses: oxidized-mc/ci/.github/workflows/commit-lint.yml@main
    with:
      scopes: "deps"
```

```yaml
# .github/workflows/security.yml
name: Security Audit
on:
  push:
    paths: ['**/Cargo.toml', '**/Cargo.lock']
  schedule:
    - cron: '0 8 * * 1'
jobs:
  audit:
    uses: oxidized-mc/ci/.github/workflows/security-audit.yml@main
```

### Workspace crate (server)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  ci:
    uses: oxidized-mc/ci/.github/workflows/rust-ci.yml@main
    with:
      workspace: true
      cache-hash-file: "**/Cargo.lock"
```

### Binary project (full pipeline)

```yaml
# .github/workflows/dev-release.yml
name: Dev Release
on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]
    branches: [main]
jobs:
  release:
    if: github.event.workflow_run.conclusion == 'success'
    uses: oxidized-mc/ci/.github/workflows/dev-release.yml@main
    with:
      binary-name: oxidized
      package-name: oxidized-server
```

```yaml
# .github/workflows/release-binaries.yml
name: Release Binaries
on:
  release:
    types: [published]
jobs:
  build:
    uses: oxidized-mc/ci/.github/workflows/release-binaries.yml@main
    with:
      binary-name: oxidized
      package-name: oxidized-server
```

```yaml
# .github/workflows/docker.yml
name: Docker
on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]
    branches: [main]
  release:
    types: [published]
jobs:
  docker:
    uses: oxidized-mc/ci/.github/workflows/docker.yml@main
```

## Inputs Reference

### `rust-ci.yml`

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `msrv` | string | `1.85` | Minimum supported Rust version |
| `toolchain` | string | `stable` | Rust toolchain for tests |
| `workspace` | boolean | `false` | Use `--workspace` flag |
| `cache-hash-file` | string | `**/Cargo.toml` | File glob for cache key |
| `extra-clippy-args` | string | `""` | Additional clippy arguments |
| `check-no-default-features` | boolean | `true` | Run no-default-features check |
| `test-platforms` | string | `["ubuntu-latest", "windows-latest", "macos-latest"]` | OS matrix (JSON) |
| `cargo-deny` | boolean | `true` | Run cargo-deny |

### `commit-lint.yml`

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `types` | string | `feat,fix,perf,refactor,test,docs,chore,ci` | Allowed commit types |
| `scopes` | string | `deps` | Allowed commit scopes |
| `max-header-length` | number | `100` | Max header length |

### `security-audit.yml`

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `ignore` | string | `""` | Comma-separated RUSTSEC IDs to ignore |

### `dev-release.yml` / `release-binaries.yml`

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `binary-name` | string | ✅ | Output binary name |
| `package-name` | string | ✅ | Cargo package (`-p` flag) |
| `extra-files` | string | | Extra files to include |

## License

MIT — see [LICENSE](LICENSE).
