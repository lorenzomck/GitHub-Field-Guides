# 04 - Auth Strategy: GitHub Apps vs. PATs vs. OAuth

## Objective

Give platform and security teams a clear decision framework for choosing the right authentication mechanism — GitHub App, personal access token (PAT), or OAuth App — for any given automation, integration, or user-facing scenario.

## The Three Mechanisms at a Glance

| Mechanism | Acts as | Best for | Key characteristic |
|---|---|---|---|
| GitHub App | Its own identity, installed on organizations/repositories | Automation, integrations, and services that need scoped, non-personal access | Fine-grained permissions, short-lived installation tokens, independent of any individual user |
| Personal access token (PAT) | The issuing individual user | Personal scripts, quick automation, individual developer workflows | Tied to a person; classic PATs are broad-scoped, fine-grained PATs are narrower |
| OAuth App | The authorizing user, via a third-party application | User-facing applications where the app needs to act on behalf of whoever logs in | Broad org-level scopes historically; increasingly superseded by GitHub Apps for new integrations |

## Decision Framework

### Step 1: Who or what is performing the action?

| Answer | Direction |
|---|---|
| A specific automation/service that should not be tied to any one person | GitHub App |
| An individual developer doing personal, ad hoc work | Fine-grained PAT |
| A third-party product where each user needs to authorize it individually with their own permissions | OAuth App (or a GitHub App with user-to-server tokens) |

### Step 2: Does the action need to survive an individual's employment?

| Answer | Direction |
|---|---|
| Yes — this must keep working regardless of who's employed | GitHub App (identity is independent of any person) |
| No — this is explicitly personal/individual work | PAT is acceptable, with proper scoping and expiration |

### Step 3: What's the required permission granularity?

| Answer | Direction |
|---|---|
| Needs precise, resource-specific permissions (e.g., only "read contents, write issues" on 3 repos) | GitHub App or fine-grained PAT |
| Broad, coarse access is genuinely required across many resources | Reassess — this is a signal to narrow scope, not a justification for broad classic PAT/OAuth scopes |

## GitHub Apps: Why They're the Preferred Default for Automation

| Advantage | Description |
|---|---|
| Scoped permissions | Grant only the specific permissions and repository access the integration needs |
| Short-lived tokens | Installation access tokens expire quickly (typically within an hour), limiting exposure if leaked |
| Independent identity | Not tied to any individual user's account, avoiding disruption on offboarding |
| Clear audit attribution | Actions are attributed to the App's identity, making automated vs. human activity easy to distinguish |
| Rate limit benefits | GitHub Apps often have more favorable, predictable rate limit behavior for high-volume automation |
| Webhook and event support | Built-in support for subscribing to organization/repository events relevant to automation use cases |

### When to Build a GitHub App

- CI/CD pipelines needing repository access without a personal token.
- Internal platform tooling (e.g., automated labeling, policy enforcement bots).
- Third-party integrations and marketplace products.
- Any Copilot Extension or MCP-adjacent service requiring GitHub API access (see the Agentic Workflows pack).

## When a PAT Is Still Appropriate

| Scenario | Guidance |
|---|---|
| Individual developer's personal script or local tooling | Fine-grained PAT with minimum necessary scope and expiration |
| Quick, one-off administrative task | Fine-grained PAT, revoked immediately after use |
| Prototyping before building a proper GitHub App | Acceptable temporarily; plan a migration path before production use |

## When OAuth Apps Are Appropriate

| Scenario | Guidance |
|---|---|
| A user-facing product where individual users must explicitly authorize access under their own identity | OAuth App, or a GitHub App using user-to-server tokens for a similar effect with finer-grained permissions |
| Legacy integrations already built on OAuth | Evaluate migration to GitHub Apps as part of a broader hardening roadmap |

## Migration Roadmap: PATs/OAuth Apps → GitHub Apps

### Step 1: Inventory

- [ ] Identify all automation currently authenticated via PATs or OAuth Apps.
- [ ] Classify by criticality and by how broad the current scope/permissions are.

### Step 2: Prioritize

| Priority | Criteria |
|---|---|
| High | Broad-scoped classic PATs powering critical CI/CD or production automation |
| Medium | Fine-grained PATs with reasonable scope but tied to an individual who may leave |
| Low | Genuinely personal, low-risk developer scripts |

### Step 3: Build and Cut Over

1. Register a GitHub App with the minimum permissions required to replace the existing token's function.
2. Install the App on the relevant organization/repositories.
3. Update automation to authenticate using the App's installation token instead of the PAT/OAuth token.
4. Validate functionality in a non-production environment before full cutover.
5. Revoke the replaced PAT/OAuth token once migration is confirmed successful.

## Permission Comparison Example

| Permission need | Classic PAT | Fine-grained PAT | GitHub App |
|---|---|---|---|
| Read issues on one repository | Grants `repo` scope (access to all repos) | Grants read-only issues access, scoped to one repo | Grants read-only issues permission, installed only on that repo |
| Write commit statuses | Grants `repo` scope | Grants commit status write, scoped to selected repos | Grants commit status write, installed only where needed |
| Manage organization webhooks | Requires broad `admin:org` scope | Not always available at fine-grained PAT level depending on resource | Grants organization webhook permission, scoped precisely |

## Governance Recommendations

- [ ] Set an organizational default: new automation must use GitHub Apps unless there's a documented reason otherwise.
- [ ] Require security review/approval for any new OAuth App requesting organization-wide access.
- [ ] Track GitHub App installations and their granted permissions in a central registry, alongside your MCP server registry if applicable.
- [ ] Periodically review installed GitHub Apps and OAuth Apps for continued necessity and appropriate scope.

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Defaulting to PATs because they're the fastest to set up | Invest in GitHub App tooling/templates to make the secure path the easy path |
| Over-permissioning a GitHub App "to be safe" | Request only the permissions the integration actually uses; expand later if genuinely needed |
| No inventory of what's authenticated via which mechanism | Maintain a central registry of GitHub Apps, OAuth Apps, and long-lived PATs in use |
| Treating OAuth Apps as equivalent to GitHub Apps for internal automation | Prefer GitHub Apps for anything that isn't genuinely a user-facing, per-user authorization flow |

## Reference Links

- [About creating GitHub Apps](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/about-creating-github-apps)
- [Differences between GitHub Apps and OAuth Apps](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/differences-between-github-apps-and-oauth-apps)
- [Managing your personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [About authentication with a GitHub App](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/authenticating-as-a-github-app)
