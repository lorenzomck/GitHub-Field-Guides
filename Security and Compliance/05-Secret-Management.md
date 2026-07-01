# 05 - Secret Management

## Objective

Reduce credential exposure by eliminating hardcoded secrets, enforcing secure storage patterns, and shifting toward short-lived, federated identity for automation.

## Secret Hierarchy in GitHub

| Secret type | Best use case | Scope considerations |
|---|---|---|
| Repository secrets | App-specific automation | Good for isolated repos, harder to scale |
| Organization secrets | Shared automation across many repos | Use repo allowlists and ownership controls |
| Environment secrets | Deployment credentials and production controls | Best when paired with protected environments |

## Recommended Secret Placement Model

| Scenario | Preferred approach |
|---|---|
| Shared non-production credential | Org secret with limited repo access |
| Production deployment auth | Environment secret behind approvals |
| Cloud resource access from CI | OIDC federation instead of stored secret |
| Local developer auth | Use developer-managed secret stores, not repos |

## Avoiding Hardcoded Credentials

### Engineering Checklist

- [ ] Prohibit plaintext credentials in source, scripts, and IaC.
- [ ] Scan commit history and pull requests for leaked secrets.
- [ ] Externalize configuration using environment variables or secret managers.
- [ ] Review sample apps, test fixtures, and docs for placeholder misuse.
- [ ] Train developers on safe token generation, storage, and revocation.

### Common Hardcoding Hotspots

| Location | Typical issue |
|---|---|
| `.env` files committed by mistake | Local credentials leaked to repo |
| CI workflows | API tokens embedded inline |
| Test fixtures | Real service keys reused for convenience |
| Terraform/Kubernetes manifests | Static secrets committed as config |
| Documentation | Copy/paste examples include real values |

## OIDC for Cloud Providers

OIDC allows GitHub Actions workflows to obtain short-lived cloud credentials without storing long-lived secrets.

| Provider | Benefit |
|---|---|
| AWS | Assume roles with trust policies tied to repo, branch, or environment |
| Azure | Federated credentials reduce secret sprawl in service principals |
| GCP | Workload identity federation eliminates static service account keys |

### OIDC Adoption Checklist

- [ ] Inventory workflows that currently use cloud access keys.
- [ ] Create cloud roles/identities with least privilege.
- [ ] Bind trust conditions to repository, ref, and environment where possible.
- [ ] Test token issuance and failure behavior in non-production first.
- [ ] Remove replaced long-lived secrets after successful migration.

## Secret Scanning and Push Protection

| Capability | Purpose |
|---|---|
| Secret scanning | Detects committed secrets in repositories and history |
| Push protection | Stops supported secrets before they land in the repo |
| Custom patterns | Extends detection to internal token formats |
| Alert workflow | Routes findings to owners for revocation and cleanup |

### Secret Scanning Program Checklist

- [ ] Enable secret scanning for all eligible repositories.
- [ ] Turn on push protection where available.
- [ ] Add custom patterns for internal credential formats.
- [ ] Define severity, ownership, and revocation playbooks.
- [ ] Track mean time to revoke and remove exposed secrets.

## Custom Patterns

Use custom patterns when your organization issues tokens or identifiers that public detectors will not recognize.

| Pattern source | Example |
|---|---|
| Internal APIs | Proprietary API keys |
| Legacy platforms | Older token prefixes not covered by default patterns |
| Partner integrations | Shared identifiers with known structure |

### Pattern Design Tips

- Prefer precise prefixes and checksums to reduce false positives.
- Validate patterns against historical samples before enforcement.
- Pair custom patterns with immediate revocation procedures.

## Rotation Strategies

| Secret class | Rotation guidance |
|---|---|
| Human access tokens | Minimize use, prefer SSO-backed flows, short expiry |
| CI/CD credentials | Replace with OIDC where possible, otherwise rotate automatically |
| Database credentials | Rotate on schedule and after staff changes or incidents |
| Third-party API keys | Track owner, usage, expiry, and break-glass process |

### Rotation Playbook Checklist

- [ ] Maintain an inventory with owner, system, and expiry date.
- [ ] Automate rotation for high-value secrets.
- [ ] Test rollover procedures before incident-driven rotations.
- [ ] Revoke leaked secrets immediately, even if exposure seems low risk.
- [ ] Remove inactive secrets from GitHub regularly.

## Governance and Reporting

| Metric | Why track it |
|---|---|
| Open secret alerts by business unit | Identify lagging teams |
| Mean time to revoke leaked secrets | Measure response speed |
| % cloud workflows migrated to OIDC | Measure reduction in static credentials |
| # repos with push protection enabled | Track prevention coverage |

## Reference Links

- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/using-secrets-in-github-actions)
- [About security hardening with OIDC](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [OpenID Connect for AWS](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)
- [OpenID Connect for Azure](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-azure)
- [OpenID Connect for GCP](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-google-cloud-platform)
- [Secret scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)
