<p align="center">
  <img src="./assets/banner.svg" alt="Security and Compliance Resource Pack" width="100%"/>
</p>

<p align="center">
  <img src="https://octodex.github.com/images/scottocat.jpg" alt="Scottocat" width="140"/>
</p>

<h1 align="center">Security &amp; Compliance Resource Pack</h1>

<p align="center">
  A practical guide for rolling out GitHub-native security controls, strengthening software supply chain integrity, and aligning engineering practices to enterprise compliance requirements.
</p>

## Contents

| # | Document | Description |
|---|----------|-------------|
| 01 | [GHAS Rollout Plan](./01-GHAS-Rollout-Plan.md) | Phased strategy for piloting, enabling, measuring, and scaling GitHub Advanced Security |
| 02 | [Supply Chain Security](./02-Supply-Chain-Security.md) | Controls for dependencies, provenance, attestations, SBOMs, and build pipeline hardening |
| 03 | [Compliance Frameworks](./03-Compliance-Frameworks.md) | Mapping GitHub capabilities to SOC 2, ISO 27001, FedRAMP, HIPAA, and PCI-DSS requirements |
| 04 | [Policy as Code](./04-Policy-as-Code.md) | Governance guardrails with rulesets, branch protections, CODEOWNERS, and automated policy checks |
| 05 | [Secret Management](./05-Secret-Management.md) | Secrets hierarchy, OIDC federation, push protection, detection, and rotation strategies |
| 06 | [Incident Response](./06-Incident-Response.md) | Playbooks for leaked secrets, repo compromise, advisories, disclosures, and audit investigations |

## How to Use

1. **Starting a security program?** Begin with [01-GHAS-Rollout-Plan](./01-GHAS-Rollout-Plan.md) to define scope, pilots, and executive measures.
2. **Hardening the SDLC?** Use [02-Supply-Chain-Security](./02-Supply-Chain-Security.md) and [05-Secret-Management](./05-Secret-Management.md) to reduce dependency and credential risk.
3. **Preparing for audits?** Review [03-Compliance-Frameworks](./03-Compliance-Frameworks.md) and adapt the evidence checklists to your control library.
4. **Enforcing standards at scale?** Apply [04-Policy-as-Code](./04-Policy-as-Code.md) to convert governance requirements into repository-level controls.
5. **Building response readiness?** Operationalize [06-Incident-Response](./06-Incident-Response.md) with clear owners, escalation paths, and investigation workflows.

## Suggested Rollout Sequence

- **Week 1-2:** Align on business drivers, compliance obligations, and pilot repositories.
- **Week 3-6:** Turn on foundational controls such as secret scanning, code scanning, and dependency review.
- **Week 6-10:** Introduce provenance, attestation, and OIDC-based workload identity.
- **Week 10+:** Standardize rulesets, automate evidence capture, and publish incident response runbooks.

## Reference Sources

- [GitHub Advanced Security](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [GitHub security features](https://docs.github.com/en/code-security/getting-started/github-security-features)
- [GitHub Actions security hardening](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [OpenID Connect in GitHub Actions](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Artifact attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations-to-establish-provenance-for-builds)
