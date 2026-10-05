# Security Policy

## Overview

FlowFi takes security seriously. We welcome security researchers and users to report security vulnerabilities responsibly. This document outlines our security policy and reporting guidelines.

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| latest  | ✅ Security fixes  |
| < latest| ⚠️ Best effort     |

## Reporting a Vulnerability

We appreciate responsible disclosure of security vulnerabilities. Please follow these guidelines when reporting:

### Contact Methods

- **Preferred**: Open a private vulnerability report via GitHub's [Security Advisories](https://github.com/LabsCrypt/flowfi/security/advisories/new)
- **Alternative**: Email security issues to the maintainers via the contact information in the repository issues

### What to Include

When reporting a vulnerability, please include:

1. **Description** - Clear description of the vulnerability
2. **Impact** - Potential impact and attack vectors
3. **Reproduction** - Steps to reproduce the issue (if possible)
4. **Remediation** - Suggested fix or workaround (if known)

### What to Avoid

- Do not exploit vulnerabilities beyond what is necessary to demonstrate them
- Do not access or modify user data without authorization
- Do not disrupt production services

### Response Timeline

| Severity | Initial Response | Target Resolution |
| -------- | ---------------- | ----------------- |
| Critical | 24-48 hours      | 7 days           |
| High     | 48-72 hours      | 14 days          |
| Medium   | 1 week          | 30 days          |
| Low      | 2 weeks         | 60 days          |

## Security Scanning Pipeline

This repository implements an automated security scanning pipeline that runs on all pull requests and weekly on a schedule.

### Scanning Tools

#### Rust Dependency Auditing
- **Tool**: `cargo-audit`
- **Purpose**: Scans Rust crate dependencies for known vulnerabilities (CVEs)
- **Configuration**: Blocks on high/critical severity issues

#### Node.js Dependency Auditing
- **Tool**: `npm audit`
- **Purpose**: Scans npm dependencies for known vulnerabilities
- **Configuration**: `--audit-level=high`

#### Secret Scanning
- **Tool**: TruffleHog
- **Purpose**: Scans git history for exposed secrets, API keys, and credentials
- **Configuration**: Only verified findings, scans full history

#### Static Code Analysis (SAST)
- **Tool**: Semgrep
- **Purpose**: Identifies security anti-patterns and common vulnerabilities
- **Rulesets**:
  - `p/security-audit` - Security audit rules
  - `p/secrets` - Secret detection
  - `p/typos` - Typo squatting detection
  - `p/rust` - Rust-specific security rules
  - `p/nodejs` - Node.js security rules
  - `p/react` - React security rules

#### CodeQL Analysis
- **Tool**: GitHub CodeQL
- **Purpose**: Deep static analysis for Rust and JavaScript/TypeScript
- **Configuration**: `security-extended` query suite

### CI/CD Integration

The security workflow:
1. Runs on every pull request to main/master branches
2. Runs on every push to main/master branches
3. Runs weekly (Monday 00:00 UTC) to detect newly published CVEs
4. Generates SARIF reports uploaded to GitHub Security tab
5. Blocks PRs with high/critical severity vulnerabilities

## Security Architecture

### Soroban Smart Contracts

The FlowFi protocol uses Stellar Soroban smart contracts for financial operations. Key security considerations:

1. **Access Control**: All sensitive operations require appropriate authorization
2. **Input Validation**: All inputs are validated before processing
3. **Reentrancy Protection**: State changes precede external calls where applicable
4. **Integer Overflow**: Uses checked arithmetic operations
5. **Authorization**: Proper authentication checks on all state-modifying functions

### Continuous Security Practices

- Dependencies are monitored for vulnerabilities
- Code is reviewed for security issues before merging
- Automated scanning runs on all changes
- Weekly scans detect new CVEs in existing dependencies

## Disclosure Policy

When a vulnerability is reported:

1. We will confirm receipt within the response timeline
2. We will investigate and validate the issue
3. We will develop and test a fix
4. We will coordinate disclosure with the reporter
5. We will publish the fix and advisory together

## Security Best Practices for Contributors

1. **Never commit secrets** - Use environment variables or secret management
2. **Keep dependencies updated** - Regularly update and audit dependencies
3. **Follow least privilege** - Request only necessary permissions
4. **Validate all inputs** - Never trust unvalidated input
5. **Use secure coding patterns** - Follow security best practices for Rust/Soroban

## Acknowledgments

We thank the security researchers who have helped improve FlowFi's security through responsible disclosure.

---

*Last updated: 2026-01-01*
