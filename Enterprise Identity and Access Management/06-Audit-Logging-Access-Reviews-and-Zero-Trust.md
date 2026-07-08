# 06 - Audit Logging, Access Reviews, and Zero Trust

## Objective

Build continuous visibility and periodic assurance into who has access to what on GitHub, and apply zero-trust principles so that access decisions are continuously validated rather than granted once and forgotten.

## Part 1: Audit Logging

### What the Audit Log Captures

| Category | Example events |
|---|---|
| Authentication | Sign-in success/failure, SSO enforcement changes |
| Authorization/access | Team membership changes, repository permission changes, custom role assignment |
| Repository administration | Repository creation/deletion, visibility changes, branch protection changes |
| Token and credential activity | PAT creation, GitHub App installation, SSH key addition |
| Security controls | Secret scanning/code scanning configuration changes, security alert dismissals |
| Enterprise/organization settings | SAML/OIDC configuration changes, SCIM provisioning events, policy changes |

### Audit Log Access Levels

| Level | Scope |
|---|---|
| Enterprise audit log | All organizations within the enterprise account |
| Organization audit log | A single organization's events |
| Repository-level events | Surfaced within organization/enterprise logs, filterable by repository |

## Step 1: Centralize Audit Log Collection

- [ ] Configure audit log streaming to a central SIEM or log management platform rather than relying solely on the GitHub UI.
- [ ] Confirm retention in the central log system meets your compliance/audit requirements (native retention windows may be shorter than what compliance requires).
- [ ] Validate that log streaming covers both enterprise-level and organization-level events as needed.

## Step 2: Define What to Monitor Actively

### High-Priority Events for Alerting

| Event category | Why it matters |
|---|---|
| SSO/SAML/OIDC configuration changes | Could indicate an attempt to weaken authentication controls |
| SCIM provisioning failures | May indicate broken deprovisioning, leaving stale access in place |
| New PAT creation with broad scope or no expiration | Signals potential policy violation |
| New GitHub App installations with broad permissions | Requires review to confirm legitimacy and scope |
| Repository visibility changes (private → public) | High-risk event requiring immediate review |
| Bulk permission changes | Could indicate compromised admin credentials or a misconfigured automation |
| Security control disablement (e.g., secret scanning turned off) | Direct reduction in security posture |

### Alerting Checklist

- [ ] Route high-priority events to a monitored alerting channel, not just a passive log.
- [ ] Define an on-call or triage owner for security-relevant audit log alerts.
- [ ] Document expected response actions for each high-priority event type.

## Part 2: Access Reviews

### Why Periodic Access Review Matters

Provisioning automation (SSO, SCIM) handles the "should this person have access at all" question well, but it doesn't answer "does this person still need this specific level of access to this specific resource." That requires a recurring, deliberate review process.

### Access Review Cadence

| Review type | Frequency | Scope |
|---|---|---|
| Sensitive repository access review | Quarterly | Repositories with production, security, or compliance-sensitive content |
| Team membership review | Quarterly | Confirm team membership still matches active responsibilities |
| Admin/owner role review | Quarterly | Organization and enterprise owner/admin lists — keep these lists as small as possible |
| Custom role and permission catalog review | Semi-annually | Confirm custom roles haven't accumulated unnecessary permissions |
| Full access certification | Annually | Comprehensive review across all organizations, teams, and sensitive repositories |

### Access Review Process

1. **Generate** a current access report (team membership, repository permissions, custom role assignments, admin/owner lists).
2. **Distribute** the report to resource owners (engineering managers, repository maintainers) for validation.
3. **Certify** — owners confirm each access grant is still appropriate or flag it for removal.
4. **Remediate** — remove access that's no longer justified.
5. **Document** — record the review's completion, findings, and remediation actions for audit evidence.

### Access Review Template

| Reviewer | Resource | Current access | Still needed? | Action |
|---|---|---|---|---|
| | | | Yes / No | Keep / Downgrade / Remove |

## Part 3: Zero-Trust Access Patterns for GitHub

### Core Zero-Trust Principles Applied to GitHub

| Principle | GitHub application |
|---|---|
| Never trust, always verify | Enforce SSO with strong authentication (MFA/conditional access) for every sign-in, every time |
| Least privilege | Use fine-grained PATs, custom repository roles, and scoped GitHub Apps as the default (see [05-Fine-Grained-Permissions-and-Custom-Roles](./05-Fine-Grained-Permissions-and-Custom-Roles.md)) |
| Assume breach | Design token/credential scope so that a single leaked credential has limited blast radius |
| Continuous validation | Replace one-time access grants with recurring access reviews rather than "set and forget" |
| Strong identity for automation | Prefer GitHub Apps with short-lived tokens over long-lived personal credentials (see [04-Auth-Strategy-GitHub-Apps-vs-PATs-vs-OAuth](./04-Auth-Strategy-GitHub-Apps-vs-PATs-vs-OAuth.md)) |

### Building a Zero-Trust Posture: Practical Steps

- [ ] Enforce SSO with MFA/conditional access as a non-negotiable baseline (see [01-SSO-SAML-and-OIDC-Configuration](./01-SSO-SAML-and-OIDC-Configuration.md)).
- [ ] Automate provisioning/deprovisioning via SCIM so access reflects current employment/role status continuously (see [02-Entra-ID-Integration-and-SCIM-Provisioning](./02-Entra-ID-Integration-and-SCIM-Provisioning.md)).
- [ ] Default all automation to GitHub Apps with scoped, short-lived tokens.
- [ ] Apply custom repository roles and fine-grained PATs so every credential and role reflects least privilege.
- [ ] Run recurring access reviews rather than treating provisioning as a one-time event.
- [ ] Centralize and actively monitor audit logs for high-risk events.
- [ ] Require additional approval/step-up verification for particularly sensitive actions (e.g., changing security settings, modifying branch protection on critical repositories).

### Zero-Trust Maturity Model

| Level | Characteristics |
|---|---|
| Level 1 — Basic | SSO enforced, but manual provisioning and infrequent access reviews |
| Level 2 — Automated identity | SCIM provisioning in place, PAT policy enforced, but access reviews are ad hoc |
| Level 3 — Governed | Regular access reviews, custom roles and fine-grained tokens standard practice, centralized audit log monitoring |
| Level 4 — Continuous assurance | Automated anomaly detection on audit logs, access reviews tightly integrated with HR/IdP events, minimal standing privilege anywhere |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Audit logs collected but never reviewed | Define specific high-priority events and route them to an active alerting channel |
| Access reviews treated as a check-the-box exercise | Require resource owners to actively certify access, not just passively acknowledge a report |
| Zero trust treated as a one-time project | Treat it as an ongoing operating model with recurring reviews and continuous monitoring |
| Break-glass accounts excluded from all review/logging | Ensure emergency access accounts are still logged and reviewed, just with an appropriately fast-access path |

## Reference Links

- [About the audit log for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/about-the-audit-log-for-your-enterprise)
- [Streaming the audit log for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/streaming-the-audit-log-for-your-enterprise)
- [Reviewing your organization's installed integrations](https://docs.github.com/en/organizations/managing-programmatic-access-to-your-organization/reviewing-your-organizations-installed-integrations)
- [About identity and access management for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/understanding-iam-for-enterprises/about-identity-and-access-management-for-your-enterprise)
