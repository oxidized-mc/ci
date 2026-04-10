# Security Policy

## Supported Versions

| Version | Supported |
|---------|----------|
| `main` (latest) | ✅ |
| older releases | ❌ |

## Reporting a Vulnerability

**Please do not open a public GitHub issue for security vulnerabilities.**

Report security issues by using
[GitHub's private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability)
on this repository.

Include:
- A description of the vulnerability and its impact
- Steps to reproduce (proof-of-concept if possible)
- Affected versions/commits

We will acknowledge your report within **72 hours** and aim to release a fix
within **7 days** for critical issues.

## Scope

This project provides CI/tooling infrastructure. Relevant
vulnerability classes:

- Supply chain attacks via compromised workflow definitions
- Secret exposure in workflow logs or artifacts
- Privilege escalation via overly permissive workflow permissions

Out of scope: game logic bugs, upstream dependency issues.
