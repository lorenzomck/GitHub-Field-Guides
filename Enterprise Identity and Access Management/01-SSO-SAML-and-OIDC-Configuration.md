# 01 - SSO, SAML, and OIDC Configuration

## Objective

Centralize authentication for GitHub Enterprise under your organization's identity provider (IdP), so that access is governed by the same identity lifecycle, MFA policy, and conditional access rules used everywhere else in the enterprise — rather than by GitHub-local credentials.

## Why Centralized SSO Matters

| Without centralized SSO | With centralized SSO |
|---|---|
| GitHub credentials/MFA managed separately from the rest of the enterprise | Single identity lifecycle governs GitHub access alongside all other enterprise apps |
| Offboarding requires a separate manual step to remove GitHub access | Deprovisioning in the IdP can automatically revoke GitHub access |
| Inconsistent MFA/conditional access enforcement | IdP-level conditional access (device compliance, location, risk signals) applies uniformly |
| Harder to produce a clean access audit trail | IdP sign-in logs plus GitHub audit logs provide a unified access story |

## SAML vs. OIDC: Choosing an Approach

| Aspect | SAML | OIDC |
|---|---|---|
| Protocol maturity | Long-established, widely supported by legacy and modern IdPs | Modern, REST/JSON-based, increasingly preferred for new integrations |
| Token format | XML assertions | JSON Web Tokens (JWT) |
| Typical enterprise fit | Common in enterprises with established SAML-based IdP configurations | Growing default choice for new enterprise identity configurations |
| GitHub support | Supported for enterprise-managed authentication | Supported for enterprise-managed authentication as an alternative to SAML |

> Confirm current protocol support and configuration steps in GitHub's enterprise identity and access management documentation, since capabilities evolve across GitHub Enterprise Cloud and Server.

## Step 1: Plan Before You Configure

- [ ] Confirm your enterprise plan supports enterprise-managed authentication (SAML/OIDC at the enterprise level).
- [ ] Identify the IdP application/integration to use (most major IdPs provide a pre-built GitHub Enterprise integration).
- [ ] Decide whether you're enabling SSO at the enterprise level (recommended for centralized control) or organization level.
- [ ] Plan a communication and rollback strategy before enforcing SSO — a misconfiguration can lock out users.
- [ ] Identify a small group of break-glass/emergency access administrators who retain access if SSO configuration issues arise.

## Step 2: Configure the IdP Side

### Typical IdP-Side Configuration Steps

1. Create/select the GitHub Enterprise application in your IdP's application catalog.
2. Configure the entity ID, ACS URL (SAML) or client ID/redirect URI (OIDC) as provided by GitHub's enterprise settings.
3. Map required claims/attributes (e.g., name identifier, email) according to GitHub's expected schema.
4. Assign the appropriate user/group scope to the application (who is allowed to authenticate via this integration).
5. Configure conditional access policies (MFA requirement, device compliance, risk-based sign-in) at the IdP level.

### Attribute/Claim Mapping Checklist

- [ ] NameID/subject claim maps to a stable, unique user identifier (not something that changes, like display name).
- [ ] Email claim is mapped and verified to match the email GitHub will use for account linking.
- [ ] Group claims (if using group-based team sync) are configured and scoped appropriately.

## Step 3: Configure the GitHub Side

1. Navigate to enterprise settings and enable SAML/OIDC single sign-on.
2. Enter the IdP-provided metadata (issuer URL, sign-on URL, certificate/public key as applicable).
3. Test the configuration with a pilot user or test account before enforcing organization-wide.
4. Enable enforcement once testing confirms successful sign-in flows.
5. Communicate the change to all affected users with clear instructions and a support contact.

## Step 4: Test Before Enforcing

### Pre-Enforcement Test Checklist

- [ ] Test sign-in with at least one account from each major user population (employees, contractors if applicable).
- [ ] Verify that existing GitHub accounts correctly link to IdP identities (avoid creating duplicate/orphaned accounts).
- [ ] Test the sign-out and re-authentication flow.
- [ ] Confirm break-glass administrator accounts retain access if SSO enforcement needs to be rolled back.
- [ ] Validate that CI/CD and automation using service accounts are not inadvertently broken by enforcement (service accounts typically use separate authentication, e.g., GitHub Apps — see [04-Auth-Strategy-GitHub-Apps-vs-PATs-vs-OAuth](./04-Auth-Strategy-GitHub-Apps-vs-PATs-vs-OAuth.md)).

## Step 5: Enforce and Monitor

- [ ] Enable SSO enforcement at the enterprise level once testing is complete.
- [ ] Monitor sign-in success/failure rates in both the IdP and GitHub audit logs during the first days after enforcement.
- [ ] Establish a support path for users who encounter sign-in issues post-enforcement.
- [ ] Document the configuration (entity IDs, claim mappings, admin contacts) for future audits and disaster recovery.

## Session and Token Considerations

| Consideration | Guidance |
|---|---|
| SSO session duration | Align session lifetime with your enterprise security policy; shorter sessions increase security but add friction |
| SAML/OIDC and PATs/SSH keys | Enterprise-managed authentication may also require authorizing PATs and SSH keys for SSO-protected organizations |
| Re-authentication for sensitive actions | Consider whether step-up authentication is needed for particularly sensitive administrative actions |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Enforcing SSO before testing all user populations | Always pilot with a representative group before enterprise-wide enforcement |
| No break-glass access plan | Maintain a small set of emergency administrator accounts outside the SSO-only flow |
| Automation/service accounts broken by enforcement | Migrate automation to GitHub Apps or properly authorized tokens before enforcing SSO broadly |
| Claim mapping mismatches causing duplicate accounts | Carefully map and test the identifying claim (NameID/subject) before rollout |
| No documented rollback plan | Document exact steps to disable enforcement in case of a lockout scenario |

## Multi-IdP and Multi-Enterprise Considerations

Larger organizations sometimes operate multiple enterprises, multiple IdP tenants (e.g., following a merger or acquisition), or a mix of cloud and on-premises identity systems. Plan for this complexity early rather than retrofitting it later.

| Scenario | Guidance |
|---|---|
| Multiple GitHub enterprises under one company | Decide whether to consolidate into a single enterprise account or maintain separate SSO configurations per enterprise, based on organizational boundaries |
| IdP migration (e.g., legacy IdP to a new provider) | Plan a phased cutover with a defined overlap window, and test thoroughly with a pilot group on the new IdP before full migration |
| Federated subsidiaries with separate IdP tenants | Consider whether enterprise-level or organization-level SSO configuration better matches the federation model |

## Rollback Plan Template

Document this before enforcing SSO, not after something goes wrong.

| Step | Action | Owner |
|---|---|---|
| 1 | Disable SSO enforcement at the enterprise level | Break-glass administrator |
| 2 | Notify affected users of temporary reversion and expected timeline | Communications/IT support |
| 3 | Investigate root cause using IdP and GitHub audit logs | Identity/platform team |
| 4 | Fix configuration issue in a test environment before re-enforcing | Identity/platform team |
| 5 | Re-pilot with a small group before re-enforcing broadly | Identity/platform team |

## Reference Links

- [About SAML for enterprise IAM](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-saml-for-enterprise-iam/about-saml-for-enterprise-iam)
- [About OIDC for enterprise IAM](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-oidc-for-enterprise-iam/about-oidc-for-enterprise-iam)
- [Configuring SAML single sign-on for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-saml-for-enterprise-iam/configuring-saml-single-sign-on-for-your-enterprise)
- [Enforcing security settings in your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/enforcing-policies/enforcing-policies-for-security-settings-in-your-enterprise)
