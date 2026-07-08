# Security Features – GitHub Advanced Security

## Overview

GitHub Advanced Security (GHAS) provides integrated security tooling that helps developers find and fix vulnerabilities without leaving their workflow. It shifts security left — catching issues in pull requests before they reach production.

---

## Feature Breakdown

### 1. Secret Scanning

**What it does:** Automatically detects credentials, API keys, tokens, and other secrets committed to your repositories.

| Feature | Description |
|---------|-------------|
| **Pattern detection** | 200+ secret patterns from providers (AWS, Azure, Slack, Stripe, etc.) |
| **Push protection** | Blocks pushes containing detected secrets before they reach the repo |
| **Alert management** | Admins receive alerts with remediation guidance |
| **Partner program** | GitHub notifies service providers to auto-revoke leaked credentials |
| **Custom patterns** | Define your own regex patterns for internal secrets |
| **Historical scanning** | Scans existing commit history, not just new pushes |

**Pricing:** $19/active committer/month (Secret Protection add-on)

---

### 2. Code Scanning (CodeQL)

**What it does:** Static analysis that finds security vulnerabilities and coding errors in your source code.

| Feature | Description |
|---------|-------------|
| **CodeQL engine** | GitHub's semantic code analysis engine |
| **Languages supported** | JavaScript, TypeScript, Python, Java, C#, C/C++, Go, Ruby, Swift, Kotlin |
| **PR integration** | Findings appear as annotations directly on pull requests |
| **Custom queries** | Write custom CodeQL queries for your specific security concerns |
| **Autofix** | AI-powered fix suggestions for common vulnerabilities |
| **Third-party tools** | Supports SARIF uploads from any static analysis tool |

**Pricing:** $30/active committer/month (Code Security add-on)

---

### 3. Dependency Review & Dependabot

**What it does:** Identifies vulnerable dependencies and automatically creates pull requests to update them.

| Feature | Description |
|---------|-------------|
| **Dependabot alerts** | Notifications when known vulnerabilities affect your dependencies |
| **Dependabot security updates** | Auto-generated PRs to update vulnerable packages |
| **Dependabot version updates** | Keep all dependencies current (configurable) |
| **Dependency review** | PR check that flags new vulnerable dependencies before merge |
| **SBOM generation** | Export Software Bill of Materials for compliance |

**Pricing:** Included free with GitHub (alerts and updates); Dependency Review enforcement requires Advanced Security

---

### 4. Security Overview Dashboard

A centralized view for security teams:
- See risk posture across all repositories
- Track alert trends and resolution times
- Filter by severity, tool, team, or repository
- Export reports for compliance

---

## How It Fits Together

```
Developer pushes code
        │
        ▼
┌─────────────────────────┐
│   Push Protection       │  ← Blocks secrets before commit lands
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│   Code Scanning (PR)    │  ← Finds vulnerabilities in changed code
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│   Dependency Review     │  ← Flags vulnerable new dependencies
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│   Merge to main         │  ← Only after all checks pass
└─────────────────────────┘
```

---

## Enabling Advanced Security

### For Enterprise Cloud:
1. Enterprise owner enables GHAS at the enterprise level
2. Organization admins enable for specific repos or all repos
3. Configure code scanning workflows (default setup or custom)
4. Enable secret scanning and push protection
5. Monitor via Security Overview dashboard

### For Enterprise Server:
1. Site admin enables GHAS in the Management Console
2. Follow same org-level configuration as Cloud
3. Ensure connectivity for vulnerability database updates (or use offline sync)

---

## Pricing Summary

| Component | Price | Billing Model | Availability |
|-----------|-------|---------------|--------------|
| Secret Protection | $19/mo | Per active committer | Team + Enterprise |
| Code Security | $30/mo | Per active committer | Team + Enterprise |
| Both combined | $49/mo | Per active committer | Team + Enterprise |
| Dependabot (alerts + updates) | Free | Included with GitHub | All plans |

**Active committer** = any user who authored a commit to a repo with GHAS enabled in the last 90 days.

---

## References

- [About GitHub Advanced Security](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [Secret Scanning Documentation](https://docs.github.com/en/code-security/secret-scanning)
- [Code Scanning with CodeQL](https://docs.github.com/en/code-security/code-scanning)
- [Dependabot Documentation](https://docs.github.com/en/code-security/dependabot)
- [Security Overview](https://docs.github.com/en/code-security/security-overview)
