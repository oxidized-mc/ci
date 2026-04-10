# Contributing to oxidized-mc CI

Thank you for your interest in contributing! This document explains the process.

---

## Code of Conduct

This project follows the [Contributor Covenant](./CODE_OF_CONDUCT.md).
Be respectful and constructive.

---

## How to Contribute

| Type | How |
|---|---|
| 🐛 Bug report | Open a [bug report issue](.github/ISSUE_TEMPLATE/bug_report.yml) |
| 💡 Feature request | Open a [feature request issue](.github/ISSUE_TEMPLATE/feature_request.yml) |
| 📖 Docs | Edit any `.md` file and open a PR |
| 🔍 Review | Review open PRs and leave constructive feedback |

---

## Development Setup

```bash
# Fork and clone
git clone https://github.com/oxidized-mc/ci.git
cd ci
```

---

## Commit Style

All commits **must** follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <short description>
```

### Types

| Type | When |
|---|---|
| `feat` | New feature or content |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `chore` | Maintenance, tooling |
| `ci` | CI/CD workflow changes |

### Scope

Use `ci` as the scope for changes to this repository.

---

## Pull Request Process

1. Create a branch: `git checkout -b feat/my-change`
2. Commit with conventional commits
3. Open a PR targeting `main`
4. At least one approving review is required before merge
5. Squash-merge preferred

---

## Continuous Improvement

We believe the project should always be getting better. If you find something
that can be improved — open an issue or PR.
