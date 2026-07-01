# 05 - Security and Best Practices

Security in GitHub Actions is not a single control; it is a layered operating model spanning identity, permissions, action provenance, environments, runner isolation, and auditability.

> Official docs: <https://docs.github.com/actions/security-guides/security-hardening-for-github-actions>

## Core Security Principles

- Use **least privilege** for tokens, secrets, and runners.
- Prefer **short-lived identity** via OIDC over long-lived cloud credentials.
- Treat workflow changes as **privileged code changes**.
- Isolate **untrusted code paths** from sensitive environments.
- Pin dependencies and actions to **immutable versions** where possible.
- Centralize governance for **required controls**.

## Securing Secrets

### Secret scope options

| Secret type | Best for | Notes |
|---|---|---|
| Repository secrets | App-specific credentials | Easy to start, harder to scale |
| Organization secrets | Shared non-production or common credentials | Scope carefully to selected repos |
| Environment secrets | Deployment credentials for protected targets | Best paired with approvals and gates |

### Secret handling checklist

- [ ] Eliminate secrets where OIDC can be used instead
- [ ] Scope secrets to the narrowest possible boundary
- [ ] Avoid echoing secrets or writing them to artifacts
- [ ] Rotate existing secrets on a defined schedule
- [ ] Separate build credentials from deployment credentials

## OpenID Connect (OIDC)

OIDC lets workflows request short-lived cloud credentials from providers such as AWS, Azure, GCP, or Vault without storing long-lived static secrets in GitHub.

### Why OIDC is preferred

- Reduces secret sprawl
- Enables short-lived, audience-bound credentials
- Improves traceability through cloud-side federation policies
- Supports environment-specific trust conditions

### Implementation guidance

1. Configure an identity provider relationship in the target cloud.
2. Constrain trust using repository, branch, environment, or workflow claims.
3. Grant only the minimum cloud role permissions required.
4. Use environment protections for sensitive deployment jobs.

## Workflow Permissions

The `GITHUB_TOKEN` should be explicitly scoped.

```yaml
permissions:
  contents: read
```

Grant elevated permissions only where needed, such as `id-token: write` for OIDC or `packages: write` for package publishing.

### Recommended pattern

| Job type | Common permissions |
|---|---|
| Build/test | `contents: read` |
| Security scan | `contents: read`, sometimes `security-events: write` |
| Cloud deploy with OIDC | `contents: read`, `id-token: write` |
| Release publishing | `contents: write`, optionally `packages: write` |

## Supply Chain Security for Actions

### Pin actions to immutable references

Pin third-party actions to full commit SHAs rather than floating tags when possible.

```yaml
- uses: actions/checkout@8ade135a41bc03ea155e62e844d188df1ea18608
```

### Action trust policy checklist

- [ ] Prefer GitHub-authored, verified, or internally maintained actions
- [ ] Pin external actions to commit SHAs
- [ ] Review action source before adopting it
- [ ] Monitor deprecations and archived repositories
- [ ] Limit who can introduce new external actions

## Required Workflows and Policy Controls

Required workflows and branch protections help ensure baseline controls run consistently.

### Good candidates for required workflows

- Dependency scanning or SBOM generation
- Secret scanning validation steps
- License or provenance checks
- Standard build and unit test gates
- Artifact signing or attestations

## Environment Protections and Deployment Gates

Environments allow you to define rules before deployment jobs can access secrets or proceed.

### Common protections

| Control | Purpose |
|---|---|
| Required reviewers | Human approval for sensitive environments |
| Wait timers | Cooling period before production deployment |
| Branch restrictions | Prevent unauthorized refs from deploying |
| Environment secrets | Scope deployment credentials to approved jobs |

### Recommended deployment model

- Lower environments deploy automatically where appropriate.
- Production deployments require environment protections.
- Deployment jobs run on isolated runners with minimal permissions.
- Release workflows produce auditable artifacts before deployment approval.

## Audit Logging and Monitoring

Enterprise teams should monitor Actions usage and administrative changes.

### What to track

- Workflow file changes
- Secret creation and updates
- Runner registration and removal
- Environment policy changes
- Failed and rerun deployment jobs
- Unusual spikes in workflow minutes or external network activity

## Secure Workflow Authoring Tips

| Practice | Why it matters |
|---|---|
| Quote and sanitize untrusted inputs | Prevents command/script injection |
| Avoid `pull_request_target` unless truly needed | It executes in a more privileged context |
| Separate CI for forks from deployment jobs | Protects secrets and production access |
| Use ephemeral runners for privileged jobs | Reduces persistence risk |
| Limit artifact retention | Minimizes sensitive data exposure |

## Enterprise Best Practices Checklist

- [ ] Set organization defaults for restricted `GITHUB_TOKEN` permissions.
- [ ] Enforce branch protection on workflow changes.
- [ ] Use CODEOWNERS for `.github/workflows/`.
- [ ] Prefer OIDC to static cloud credentials.
- [ ] Pin third-party actions to SHAs.
- [ ] Segregate untrusted and privileged workloads.
- [ ] Protect production environments with reviewers and branch rules.
- [ ] Review audit logs regularly.
- [ ] Document incident response for compromised workflows or runners.

## Reference Links

- [Security hardening for GitHub Actions](https://docs.github.com/actions/security-guides/security-hardening-for-github-actions)
- [Using secrets in GitHub Actions](https://docs.github.com/actions/security-guides/using-secrets-in-github-actions)
- [About security hardening with OIDC](https://docs.github.com/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Managing environments for deployment](https://docs.github.com/actions/deployment/targeting-different-environments/using-environments-for-deployment)
- [Audit log events for GitHub Actions](https://docs.github.com/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/auditing-the-audit-log-for-your-enterprise)
