# 02 - Entra ID Integration and SCIM Provisioning

## Objective

Automate the full user and team lifecycle between Microsoft Entra ID (Azure AD) and GitHub Enterprise so that access provisioning, group-based team membership, and deprovisioning happen automatically — eliminating manual, error-prone identity administration.

## Why Automated Provisioning Matters

| Manual provisioning | SCIM-automated provisioning |
|---|---|
| New hires wait on manual GitHub access requests | Access provisioned automatically based on IdP group membership |
| Departed employees may retain GitHub access until manually removed | Deprovisioning happens automatically when the IdP account is disabled/removed |
| Team membership drifts from HR/IdP group structure over time | Team membership stays continuously synchronized with IdP groups |
| No consistent audit trail for access grants | Provisioning events are logged and attributable to IdP group changes |

## Microsoft Entra ID + GitHub: The Integration Model

| Component | Role |
|---|---|
| Entra ID enterprise application | The GitHub Enterprise Managed User (EMU) or standard enterprise app registered in Entra ID |
| SAML/OIDC (authentication) | Governs sign-in (see [01-SSO-SAML-and-OIDC-Configuration](./01-SSO-SAML-and-OIDC-Configuration.md)) |
| SCIM (provisioning) | Governs the lifecycle of user accounts and group/team membership |
| Entra ID groups | Mapped to GitHub organizations/teams to drive automated team sync |

> SAML/OIDC and SCIM are complementary but distinct: SAML/OIDC controls **authentication** (can this user sign in), while SCIM controls **provisioning** (does this user's GitHub account/team membership exist and stay current).

## Choosing Between Standard Enterprise and Enterprise Managed Users (EMU)

| Model | Description | Best for |
|---|---|---|
| Standard enterprise with SCIM | Existing GitHub identities are linked to IdP-managed provisioning | Enterprises wanting to keep existing GitHub usernames/identities |
| Enterprise Managed Users (EMU) | GitHub accounts are fully created, owned, and controlled by the IdP; users cannot use personal GitHub accounts | Enterprises requiring strict control over accounts, including preventing personal account use for enterprise work |

### Decision Factors

- [ ] Does policy require preventing personal GitHub accounts from being used for enterprise work? → Consider EMU.
- [ ] Do you need to preserve existing external collaboration patterns (e.g., contributing to public open source under a personal identity)? → Standard enterprise with SCIM may fit better; EMU restricts external interactions.
- [ ] Is centralized, IdP-owned identity lifecycle the top priority? → EMU offers the strongest guarantee.

## Step 1: Configure SCIM Provisioning in Entra ID

1. In Entra ID, locate the GitHub Enterprise (or EMU) enterprise application.
2. Configure the provisioning mode to "Automatic" and supply the SCIM endpoint and authentication token from GitHub's enterprise settings.
3. Map Entra ID user attributes to the SCIM schema fields GitHub expects (e.g., `userName`, `emails`, `name`).
4. Define the scope of provisioning — assign specific Entra ID groups or all users, depending on rollout strategy.
5. Start provisioning in a limited test scope before enabling for the full user population.

### Attribute Mapping Checklist

- [ ] `userName`/identifier maps to a stable, unique value.
- [ ] Email attribute is mapped and matches expected verification requirements.
- [ ] Display name and other profile attributes are mapped as needed for a good user experience.
- [ ] Group membership attributes are configured if using group-based team sync.

## Step 2: Map Entra ID Groups to GitHub Teams

| Entra ID group | GitHub team mapping | Purpose |
|---|---|---|
| `eng-platform` | `platform-team` | Automatic team membership for platform engineers |
| `eng-security` | `security-team` | Automatic team membership for security engineers |
| `contractors-eng` | `contractors` (restricted permissions) | Segregated access tier for contractor population |

### Team Sync Checklist

- [ ] Identify which Entra ID groups map to which GitHub teams.
- [ ] Confirm group ownership/membership processes in Entra ID are themselves well-governed (garbage in, garbage out).
- [ ] Test that adding/removing a user from an Entra ID group correctly reflects in GitHub team membership within the expected sync interval.
- [ ] Document the mapping so both identity and platform teams have a shared source of truth.

## Step 3: Validate Deprovisioning

Deprovisioning is the highest-value outcome of SCIM automation — validate it thoroughly.

- [ ] Disable/remove a test account in Entra ID and confirm GitHub access is revoked within the expected timeframe.
- [ ] Confirm team memberships are removed, not just sign-in access.
- [ ] Confirm any owned resources (e.g., repositories owned by a personal account, in non-EMU models) have a defined transfer/cleanup process.
- [ ] Document the end-to-end offboarding SLA (time from IdP deprovisioning action to GitHub access removal).

## Step 4: Establish Ongoing Governance

### Recurring Review Checklist

- [ ] Monthly: Reconcile Entra ID group membership against GitHub team membership for drift.
- [ ] Quarterly: Review SCIM provisioning logs for failed sync events and resolve root causes.
- [ ] Quarterly: Validate that break-glass/emergency accounts are excluded from automated deprovisioning in a way that's still auditable.
- [ ] Annually: Re-validate the full provisioning-to-deprovisioning lifecycle with a test account.

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| SCIM configured but never validated end-to-end | Run a full lifecycle test (provision → modify → deprovision) before relying on it |
| Group mapping drift over time | Schedule recurring reconciliation between IdP groups and GitHub teams |
| Treating SAML/OIDC and SCIM as the same thing | Configure and test both authentication and provisioning independently |
| No visibility into failed sync events | Monitor Entra ID provisioning logs and set up alerting for repeated failures |
| Manual overrides bypassing SCIM-managed team membership | Restrict manual team membership changes for SCIM-synced teams to avoid conflicting state |

## Handling Contractors and Guest Users

Contractor and guest populations often don't fit neatly into standard employee provisioning flows. Plan for them explicitly.

| Population | Provisioning approach |
|---|---|
| Long-term contractors on the corporate IdP | Include in standard SCIM provisioning, mapped to a restricted-access team/role |
| Short-term or occasional contractors | Consider a time-boxed access model with an explicit expiration/review date |
| External collaborators (non-EMU model) | Managed through GitHub's standard outside collaborator model, outside SCIM scope, with its own periodic review |

### Contractor Access Checklist

- [ ] Contractor accounts are tagged/grouped distinctly from full-time employees in the IdP.
- [ ] Contractor GitHub team/role assignments use restricted, least-privilege custom roles (see [05-Fine-Grained-Permissions-and-Custom-Roles](./05-Fine-Grained-Permissions-and-Custom-Roles.md)).
- [ ] Contractor access has a defined expiration or mandatory periodic renewal, independent of standard employee lifecycle timing.

## Troubleshooting Common SCIM Issues

| Symptom | Likely cause | Resolution |
|---|---|---|
| User provisioned but team membership missing | Group claim not mapped or group scope excludes the user | Verify group mapping configuration and scope in the IdP |
| Sync fails intermittently | Rate limiting or transient API errors | Review provisioning logs for patterns; contact IdP/GitHub support if persistent |
| Duplicate accounts created | Identifier attribute changed or wasn't stable | Re-map to a stable unique identifier and reconcile duplicate accounts |
| Deprovisioning doesn't remove all access | Some access (e.g., outside collaborator grants) sits outside SCIM's scope | Document and separately review any access paths not covered by SCIM |

## Reference Links

- [About SCIM for enterprise IAM](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-scim-for-enterprise-iam/about-scim-for-enterprise-iam)
- [About Enterprise Managed Users](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-enterprise-managed-users-for-iam/about-enterprise-managed-users)
- [Configuring SCIM provisioning with Microsoft Entra ID](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-scim-for-enterprise-iam/configuring-scim-provisioning-for-enterprise-managed-users)
- [Synchronizing a team with an identity provider group](https://docs.github.com/en/organizations/organizing-members-into-teams/synchronizing-a-team-with-an-identity-provider-group)
