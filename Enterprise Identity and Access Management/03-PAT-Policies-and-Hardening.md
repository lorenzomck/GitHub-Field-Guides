# 03 - PAT Policies and Hardening

## Objective

Reduce the security risk posed by personal access tokens (PATs) through organization-wide policy, scoping discipline, and lifecycle controls — while giving developers a clear migration path toward more secure authentication patterns.

## Why PATs Are a Common Risk Vector

| Risk factor | Description |
|---|---|
| Broad scope by default | Classic PATs can be granted broad, long-lived access across many resources |
| Long or indefinite lifetime | Tokens without expiration remain valid indefinitely if not manually rotated |
| Credential sprawl | Tokens are easily copied into scripts, CI configuration, or local environment files, increasing exposure surface |
| Attribution ambiguity | A PAT acts as the issuing user, making it harder to distinguish human vs. automated activity in audit logs |
| Leakage risk | Tokens committed to code, shared in chat, or exposed via misconfigured systems are a leading cause of unauthorized access incidents |

## Classic PATs vs. Fine-Grained PATs

| Aspect | Classic PAT | Fine-grained PAT |
|---|---|---|
| Scope | Broad OAuth-style scopes (e.g., `repo` grants access to all repositories the user can access) | Explicit repository selection and granular permission set |
| Expiration | Optional; can be set to "no expiration" | Expiration required (with a maximum allowed lifetime) |
| Owner approval | Not natively supported | Organizations can require approval before a fine-grained PAT can access organization resources |
| Least privilege | Difficult to achieve — scopes are coarse | Purpose-built for least privilege |

**Recommendation:** Treat fine-grained PATs as the default going forward, and actively work to reduce reliance on classic PATs across the organization.

## Step 1: Establish Organization/Enterprise Token Policy

### Policy Controls to Configure

| Control | Recommended setting |
|---|---|
| Require expiration for all PATs | Enabled — disallow indefinite-lifetime tokens |
| Maximum token lifetime | Set to the shortest period that's operationally reasonable (e.g., 30-90 days) |
| Restrict fine-grained PAT access to organization resources | Require administrator approval before a fine-grained PAT can access org repositories |
| Restrict classic PAT access | Where supported, restrict or disable classic PAT access to organization resources in favor of fine-grained PATs |
| IP allow list (if applicable) | Restrict API/token usage to expected network ranges where feasible |

### Policy Rollout Checklist

- [ ] Audit current PAT usage across the organization/enterprise before changing policy (avoid breaking existing automation blind).
- [ ] Communicate upcoming policy changes with a clear timeline and migration guidance.
- [ ] Enable expiration requirements first (lowest disruption), then move to approval requirements for org resource access.
- [ ] Provide a self-service path for developers to request approval or migrate to GitHub Apps.

## Step 2: Inventory and Classify Existing Tokens

- [ ] Export the list of active PATs with access to organization resources (available to organization/enterprise owners via token request review features where supported).
- [ ] Classify each token by purpose: personal developer use, CI/CD automation, third-party integration, or unknown/orphaned.
- [ ] Flag tokens with no expiration or unusually broad scope for immediate review.
- [ ] Identify tokens with unclear ownership (e.g., tied to a departed employee) for immediate revocation.

## Step 3: Migrate Automation Off PATs Where Possible

| Current pattern | Migration target | Why |
|---|---|---|
| CI/CD pipeline using a personal PAT | GitHub App or OIDC-based workload identity | Removes dependency on an individual's credential; improves attribution and control |
| Third-party integration using a shared PAT | Dedicated GitHub App installation | Scoped permissions, independent lifecycle, doesn't tie to a person |
| Script/cron job using a long-lived PAT | Short-lived fine-grained PAT or GitHub App | Reduces blast radius if leaked |

See [04-Auth-Strategy-GitHub-Apps-vs-PATs-vs-OAuth](./04-Auth-Strategy-GitHub-Apps-vs-PATs-vs-OAuth.md) for a full decision framework.

## Step 4: Reduce Leakage Risk

| Control | Description |
|---|---|
| Secret scanning | Enable secret scanning (and push protection) to catch tokens committed to repositories before or shortly after exposure |
| No tokens in plaintext config | Require tokens to be stored in a secrets manager or CI secret store, never in checked-in configuration |
| Rotate on suspected exposure | Establish a fast rotation/revocation path when a token is suspected to be exposed |
| Scope tokens to the minimum needed | Avoid granting `repo`-wide or admin scopes when a narrower fine-grained scope would suffice |

## Step 5: Build a Token Lifecycle Process

### Token Request and Renewal Workflow

1. Developer requests a token with a stated purpose and minimum required scope.
2. Token is created as fine-grained with an expiration date, scoped only to the necessary repositories/permissions.
3. Organization administrator approves access to organization resources (if policy requires approval).
4. Token usage is periodically reviewed; renewal requires re-justification, not automatic extension.
5. Token is revoked immediately upon project completion, role change, or departure.

### Renewal and Expiration Handling

- [ ] Notify token owners in advance of expiration to avoid unplanned automation breakage.
- [ ] Require a renewal justification rather than automatic reissue.
- [ ] Track tokens nearing expiration in a shared dashboard for proactive management.

## Monitoring and Auditing

| Signal | Where to look |
|---|---|
| Token creation events | Enterprise/organization audit log |
| Token usage patterns | API access logs, where available |
| Fine-grained PAT access requests | Organization token request review interface |
| Anomalous token activity (unusual IP, volume, or scope) | Audit log combined with any SIEM integration |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Long-lived classic PATs powering critical automation | Migrate to GitHub Apps or short-lived fine-grained PATs |
| No expiration policy | Require expiration for all PATs at the organization/enterprise level |
| Tokens shared across multiple systems/people | Issue individual, purpose-specific tokens rather than shared credentials |
| No process for revoking tokens on offboarding | Tie token revocation to the broader deprovisioning workflow (see [02-Entra-ID-Integration-and-SCIM-Provisioning](./02-Entra-ID-Integration-and-SCIM-Provisioning.md)) |
| Overly broad scopes "just in case" | Default to the narrowest fine-grained scope that accomplishes the task |

## Reference Links

- [Managing your personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [About fine-grained personal access tokens](https://docs.github.com/en/rest/authentication/permissions-required-for-fine-grained-personal-access-tokens)
- [Setting a personal access token policy for your organization](https://docs.github.com/en/organizations/managing-programmatic-access-to-your-organization/setting-a-personal-access-token-policy-for-your-organization)
- [About secret scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)
