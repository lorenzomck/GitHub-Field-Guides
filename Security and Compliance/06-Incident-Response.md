# 06 - Incident Response

## Objective

Prepare engineering, security, and platform teams to respond quickly when source code, credentials, repositories, or package release processes are suspected to be compromised.

## Incident Categories

| Incident type | Example triggers |
|---|---|
| Leaked secret | Secret scanning alert, external report, accidental commit |
| Compromised repository | Unauthorized pushes, malicious workflow edits, deleted branches |
| Dependency/package issue | Malicious package update, typosquat, compromised maintainer |
| Vulnerability disclosure | Researcher report, customer report, internal finding |
| Account misuse | Suspicious token use, impossible travel, admin action anomaly |

## Core Response Workflow

| Step | Goal |
|---|---|
| Detect | Confirm signal and establish severity |
| Contain | Stop further damage or abuse |
| Eradicate | Remove malicious access, code, or artifacts |
| Recover | Restore trusted state and resume delivery safely |
| Learn | Improve controls, training, and telemetry |

## Responding to Leaked Secrets

### Immediate Actions

- [ ] Validate whether the secret is real and still active.
- [ ] Revoke or rotate the secret immediately.
- [ ] Identify where the secret was used and what systems it could access.
- [ ] Remove the secret from active branches and affected artifacts.
- [ ] Determine whether history rewrite is required.
- [ ] Document the timeline, owner, and containment status.

### Secret Leak Decision Table

| Question | Action |
|---|---|
| Is the credential active? | Revoke first, investigate second |
| Does it grant privileged or production access? | Escalate severity and broaden review |
| Was it exposed publicly? | Assume compromise and rotate immediately |
| Does it appear in release artifacts or container layers? | Rebuild and republish trusted artifacts |

## Responding to a Compromised Repository

### Containment Checklist

- [ ] Freeze releases from affected branches or environments.
- [ ] Review recent pushes, force pushes, workflow edits, and admin changes.
- [ ] Disable or rotate affected tokens, deploy keys, and app credentials.
- [ ] Restrict repo access to a smaller trusted group during investigation.
- [ ] Review whether self-hosted runners or environments were abused.

### Investigation Focus Areas

| Area | Questions |
|---|---|
| Git history | What unexpected commits, tags, or deletions occurred? |
| Workflow files | Were CI/CD workflows modified to exfiltrate secrets or tamper artifacts? |
| Access records | Did a user, app, or token perform suspicious admin actions? |
| Releases/packages | Were untrusted artifacts published downstream? |

## Vulnerability Disclosure and Advisories

GitHub security advisories help coordinate private remediation before public disclosure.

| Practice | Why it matters |
|---|---|
| Private intake path | Gives researchers a safe reporting channel |
| Severity triage | Aligns response effort to exploitability and impact |
| Coordinated disclosure timeline | Balances customer protection and transparency |
| Advisory publication | Communicates fixed versions and mitigation guidance |

### Disclosure Process Checklist

- [ ] Publish a SECURITY.md with reporting instructions.
- [ ] Acknowledge inbound reports promptly.
- [ ] Reproduce, scope, and prioritize the issue.
- [ ] Create a private fix branch or advisory workflow.
- [ ] Coordinate release timing, customer comms, and CVE issuance if needed.

## Audit Log Investigation

Audit logs are critical when establishing what changed, who changed it, and whether the issue spread.

### High-Value Audit Questions

| Question | Audit clues |
|---|---|
| Who changed repo permissions? | Membership, role, and admin setting events |
| Were workflows altered? | Workflow file changes, Actions policy changes |
| Was a token or app abused? | API/auth events, app installation events |
| Were secrets or environments modified? | Secret, environment, and deployment events |

### Investigation Checklist

- [ ] Pull audit logs for the suspected time window.
- [ ] Correlate GitHub events with IdP, cloud, and CI/CD logs.
- [ ] Preserve copies of relevant logs and PR/issue discussions.
- [ ] Identify initial access vector and blast radius.
- [ ] Record all remediation actions with timestamps.

## Communications Plan

| Audience | What they need |
|---|---|
| Security leadership | Severity, impact, containment status, next decisions |
| Engineering leaders | Affected repos/services, required team actions |
| Legal/compliance | Notification obligations and evidence preservation |
| Customers/partners | Clear impact statement, mitigation, next steps when applicable |

## Recovery and Hardening

- [ ] Rebuild artifacts from a trusted commit and trusted workflow.
- [ ] Re-enable releases only after verification steps pass.
- [ ] Backfill missing controls such as push protection, rulesets, or OIDC.
- [ ] Review access scopes and remove stale admin privileges.
- [ ] Conduct a post-incident review with owners and deadlines.

## Post-Incident Review Template

| Section | Prompts |
|---|---|
| Summary | What happened and what was affected? |
| Detection | How was it discovered and how long did it take? |
| Response | What actions contained and remediated the issue? |
| Gaps | Which controls or decisions failed? |
| Follow-up | What policy, tooling, or training changes are required? |

## Reference Links

- [About security advisories](https://docs.github.com/en/code-security/security-advisories/working-with-repository-security-advisories/about-repository-security-advisories)
- [Best practices for writing a security policy](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository)
- [Reviewing audit logs for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise)
- [Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)
- [Secret scanning alerts](https://docs.github.com/en/code-security/secret-scanning/managing-alerts-from-secret-scanning)
