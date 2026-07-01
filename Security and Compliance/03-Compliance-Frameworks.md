# 03 - Compliance Frameworks

## Objective

Use GitHub enterprise capabilities as part of a broader control environment that supports audit readiness, policy enforcement, and evidence collection across common frameworks.

## Important Framing

GitHub features do not by themselves make an organization compliant. They help implement and evidence controls that map to your internal policies and external frameworks.

## Framework Mapping Overview

| Framework | Typical focus areas | Relevant GitHub capabilities |
|---|---|---|
| SOC 2 | Security, availability, confidentiality, change management | Audit log, SSO/SAML, repo permissions, branch protections, Actions approvals |
| ISO 27001 | ISMS governance, access control, secure development, supplier management | Org policies, rulesets, CODEOWNERS, issue/PR history, secret scanning |
| FedRAMP | Access restrictions, logging, authorization boundaries, vulnerability management | Enterprise managed users, audit logs, IP allow lists, GHAS, runner controls |
| HIPAA | Access control, auditability, minimum necessary access, incident response | Fine-grained roles, environment protections, audit events, secret management |
| PCI-DSS | Access control, secure configuration, vulnerability management, logging | Branch protections, required reviews, dependency scanning, audit logs, Actions controls |

## Control Families and GitHub Support

### Access Control

| Control question | GitHub capability | Evidence example |
|---|---|---|
| Who can access code and admin settings? | Teams, roles, repository permissions, enterprise/org policies | Access review export, team membership record |
| Is strong authentication enforced? | SAML SSO, enterprise managed users, 2FA requirements | Identity provider settings, policy screenshots |
| Is privileged access limited? | Custom repository roles, environment reviewers, admin scoping | Role matrix and approval workflow |

### Change Management

| Control question | GitHub capability | Evidence example |
|---|---|---|
| Are changes reviewed before production? | Pull requests, required reviewers, CODEOWNERS, status checks | Merged PR samples with approvals |
| Are deployments gated? | Protected environments, required reviewers, deployment logs | Environment rule configuration |
| Are policy exceptions tracked? | Issues, discussions, external GRC system links | Waiver record with expiry |

### Logging and Monitoring

| Control question | GitHub capability | Evidence example |
|---|---|---|
| Can security-relevant events be reconstructed? | Enterprise and organization audit logs | Audit log export or SIEM ingestion record |
| Can repo configuration changes be tracked? | Audit events for policy, permissions, and workflow changes | Configuration change report |
| Are code risks monitored? | GHAS alerts, Dependabot alerts, secret scanning | Alert dashboards and remediation tickets |

## Framework-Specific Notes

### SOC 2

**Useful control themes:** logical access, SDLC controls, incident management, vendor oversight.

- Use required pull request reviews and status checks to support change approval controls.
- Use audit logs and retained PR history as evidence of review and segregation of duties.
- Use secret scanning and dependency alerts as evidence of proactive security monitoring.

### ISO 27001

**Useful annex themes:** access control, secure development, vulnerability management, logging, supplier relationships.

- GitHub rulesets can operationalize secure development policy requirements.
- CODEOWNERS and protected branches help enforce accountability and review discipline.
- Dependabot and code scanning support vulnerability identification and treatment.

### FedRAMP

**Useful themes:** authorization boundaries, logging, least privilege, vulnerability management, secure configuration.

- Confirm whether your GitHub deployment model aligns with boundary and residency requirements.
- Export audit logs to a central SIEM for retention and monitoring.
- Use strict runner controls, network segmentation, and environment approvals for deployment workflows.

### HIPAA

**Useful themes:** access control, audit controls, integrity, transmission security, incident response.

- Minimize PHI exposure in repositories and issues; establish data handling rules.
- Use secret scanning and push protection to reduce accidental exposure of credentials tied to regulated systems.
- Restrict environment secrets and deployments to approved maintainers.

### PCI-DSS

**Useful themes:** least privilege, secure system development, vulnerability management, logging, change control.

- Require review and tests on all changes affecting cardholder data environments.
- Use dependency scanning and code scanning to support secure coding and vulnerability tracking.
- Protect workflow files and deployment branches with stronger review rules.

## Audit Log Capabilities

| Capability | Value for compliance |
|---|---|
| User/admin event history | Demonstrates access and configuration activity |
| Filterable exports | Supports investigations and evidence requests |
| SIEM integration workflows | Improves retention and centralized monitoring |
| Org/repo scope visibility | Helps map events to control ownership |

### Audit Log Checklist

- [ ] Define retention requirements with legal/compliance teams.
- [ ] Export or integrate logs into the enterprise SIEM.
- [ ] Create saved queries for admin changes, permission changes, and workflow edits.
- [ ] Test quarterly evidence retrieval for audit readiness.

## Data Residency, Encryption, and Platform Assurance

| Topic | Questions to validate |
|---|---|
| Data residency | Where is metadata/content stored and what contractual options are available? |
| Encryption | How is data encrypted in transit and at rest? |
| Backup/recovery | What recovery and retention commitments are relevant? |
| Shared responsibility | Which controls are handled by GitHub vs your organization? |

## Evidence Collection Model

### Suggested Evidence Table

| Evidence type | Source | Cadence |
|---|---|---|
| Access reviews | IdP + GitHub team/role exports | Quarterly |
| Branch/ruleset configuration | Repository settings export/screenshots | Quarterly |
| Workflow protection settings | Actions environment and workflow configs | Quarterly |
| Audit logs | Enterprise/org audit log exports | Monthly |
| Security alerts | GHAS and Dependabot dashboards | Monthly |
| Incident records | Advisory, issue tracker, or IR platform | As needed |

### Audit Readiness Checklist

- [ ] Maintain a control-to-capability mapping owned by compliance.
- [ ] Preserve sample PRs showing approval, test, and deployment gates.
- [ ] Export audit evidence on a schedule rather than waiting for audit requests.
- [ ] Document compensating controls for unsupported requirements.
- [ ] Review new GitHub features for control impact at least twice per year.

## Reference Links

- [GitHub Trust Center](https://github.com/trust-center)
- [GitHub compliance and security resources](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement)
- [About audit logs](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise)
- [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [Enterprise managed users](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-iam/understanding-iam-for-enterprises/about-enterprise-managed-users)
