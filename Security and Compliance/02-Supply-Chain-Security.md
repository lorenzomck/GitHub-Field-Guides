# 02 - Supply Chain Security

## Objective

Protect the software supply chain from dependency compromise, tampered artifacts, and unauthorized build activity by combining GitHub-native visibility with provenance, signing, and pipeline hardening.

## Core Control Areas

| Control area | Primary GitHub capability | Goal |
|---|---|---|
| Dependency intelligence | Dependency graph, Dependabot alerts | Know what you ship and where risk exists |
| Change prevention | Dependency review | Catch risky dependency changes before merge |
| Artifact integrity | Artifact attestations | Prove build origin and workflow identity |
| Package provenance | npm/PyPI provenance | Link published packages to trusted CI workflows |
| Build security | GitHub Actions hardening + OIDC | Reduce credential theft and runner abuse |
| Transparency | SBOM generation | Support audits, incident response, and customer trust |

## Dependency Management Foundations

### Required Baseline

- [ ] Enable the dependency graph on all supported repositories.
- [ ] Turn on Dependabot alerts and security updates.
- [ ] Require dependency review on pull requests.
- [ ] Pin GitHub Actions to full-length commit SHAs where possible.
- [ ] Restrict who can approve workflow and package publishing changes.

### Dependabot Operating Model

| Practice | Recommendation |
|---|---|
| Alert triage | Route by severity and reachable exposure where available |
| Update cadence | Daily for internet-facing apps, weekly for internal apps |
| PR grouping | Group low-risk dependencies to reduce noise |
| Auto-merge | Limit to trusted, low-risk ecosystems with passing tests |
| Ownership | Define maintainers by service or package ecosystem |

## Dependency Review in Pull Requests

Dependency review helps teams understand what a change introduces before merge.

| Review focus | What to inspect |
|---|---|
| New packages | Maintainer reputation, source, and necessity |
| Version jumps | Major updates, abandoned packages, sudden ownership changes |
| License changes | New obligations or legal restrictions |
| Vulnerabilities | Known CVEs or malware indicators |
| Transitive risk | High-risk sub-dependencies introduced by innocent-looking packages |

### Reviewer Checklist

- [ ] Confirm every new dependency has a documented business need.
- [ ] Review manifest and lockfile changes together.
- [ ] Check for unusual post-install scripts or build hooks.
- [ ] Validate package source and maintainer trust.
- [ ] Block merges when critical vulnerabilities are introduced.

## SBOM Generation

A Software Bill of Materials (SBOM) provides inventory visibility for internal stakeholders, customers, and auditors.

### SBOM Use Cases

| Use case | Why it matters |
|---|---|
| Vulnerability response | Quickly identify affected apps when new CVEs emerge |
| Customer assurance | Demonstrate component transparency |
| Procurement/security review | Validate open source usage and ownership |
| Regulatory alignment | Supports emerging supply chain expectations |

### Implementation Guidance

- Generate SBOMs during CI for release builds.
- Store SBOMs alongside artifacts or release records.
- Version SBOMs per build or release tag.
- Use SPDX or CycloneDX consistently across the organization.

## Artifact Attestations and Provenance

Artifact attestations provide signed metadata proving where and how an artifact was produced.

| Capability | Benefit |
|---|---|
| Workflow-linked provenance | Verifies the source repo, commit, and workflow identity |
| Tamper resistance | Makes it harder to substitute untrusted artifacts |
| Policy enforcement | Lets downstream systems verify artifacts before deploy |
| Auditability | Creates durable evidence for release approvals |

### Adoption Checklist

- [ ] Identify high-value release artifacts to attest first.
- [ ] Use trusted GitHub-hosted or tightly controlled self-hosted runners.
- [ ] Enforce environment protection and reviewer gates for production release workflows.
- [ ] Verify attestations before deployment into sensitive environments.
- [ ] Retain attestation evidence with release metadata.

## Sigstore Signing

Sigstore supports modern, keyless signing models that reduce long-lived signing key exposure.

| Practice | Benefit |
|---|---|
| Keyless signing with OIDC | Avoids storing private signing keys in CI |
| Transparency log usage | Enables public verifiability and tamper evidence |
| Policy validation | Lets deploy systems reject unsigned or untrusted packages |

### Where to Start

1. Pilot signing on one container image or package ecosystem.
2. Use OIDC-backed identity assertions from GitHub Actions.
3. Add verification in staging before enforcing in production.
4. Extend to release artifacts, container images, and internal packages.

## npm and PyPI Provenance

GitHub Actions can publish packages with provenance metadata so consumers can verify origin.

| Ecosystem | Control goal |
|---|---|
| npm | Prove that published packages came from a trusted workflow and source repository |
| PyPI | Attach build provenance to published Python packages |

### Publishing Guardrails

- [ ] Use environment protection rules for publish jobs.
- [ ] Limit publishing to tagged releases or approved branches.
- [ ] Use OIDC or short-lived tokens instead of stored credentials where supported.
- [ ] Require review for workflow file changes that affect release paths.

## Securing the Build Pipeline

### Actions Hardening Checklist

- [ ] Use least-privilege `permissions:` in every workflow.
- [ ] Pin third-party actions to commit SHAs.
- [ ] Restrict self-hosted runner scope and network reachability.
- [ ] Separate build, test, and release environments.
- [ ] Require approvals for protected environments.
- [ ] Prevent untrusted forks from accessing secrets.
- [ ] Log and review workflow changes affecting deployment or publishing.

### Pipeline Threats and Mitigations

| Threat | Mitigation |
|---|---|
| Malicious dependency update | Dependency review, allowlists, manual approval for risky changes |
| Compromised action | SHA pinning, approved-actions policy, internal mirrors |
| Credential theft in CI | OIDC federation, ephemeral tokens, reduced secret scope |
| Artifact substitution | Artifact attestations and downstream verification |
| Runner compromise | Ephemeral runners, network segmentation, re-imaging |

## Executive Scorecard Ideas

| Metric | Target |
|---|---|
| % repos with dependency review required | 95%+ |
| % release artifacts with attestations | 100% for tier-1 apps |
| % publish workflows using OIDC or provenance | 100% for supported ecosystems |
| Mean time to remediate critical dependency CVEs | < 7 days |
| % workflows pinned to SHAs | 90%+ |

## Reference Links

- [GitHub supply chain security](https://docs.github.com/en/code-security/supply-chain-security)
- [Dependency review](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-dependency-review)
- [Dependency graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)
- [Artifact attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations-to-establish-provenance-for-builds)
- [Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [Trusted publishing for npm](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-npm)
- [Trusted publishing for PyPI](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-pypi)
