# 05 - Fine-Grained Permissions and Custom Repository Roles

## Objective

Move beyond broad, all-or-nothing access grants by applying least-privilege principles at the repository and organization level — using fine-grained permission tokens, custom repository roles, and team-based access design.

## The Problem with Broad Default Roles

GitHub's standard organization roles (Read, Triage, Write, Maintain, Admin) cover most needs, but real organizations often have access requirements that don't map cleanly to any single standard role:

| Scenario | Why standard roles fall short |
|---|---|
| A team needs to manage issues and review PRs but should never be able to change repository settings | "Write" grants push access; "Maintain" grants settings access neither may be exactly right |
| A compliance team needs read access plus the ability to dismiss specific security alerts | No standard role isolates this narrow combination |
| A release manager needs to manage releases and tags without full write access to code | Standard roles don't separate "release management" from general write access |

Custom repository roles solve this by letting you compose a permission set tailored to a specific responsibility.

## Custom Repository Roles

### How They Work

Custom repository roles let organization owners define a named role built from a granular list of available permissions (e.g., "manage issues," "view security alerts," "manage releases"), then assign that role to teams or individuals per repository — instead of forcing a fit into a standard role.

### Example Custom Roles

| Custom role name | Included permissions | Use case |
|---|---|---|
| Issue Triager | Read code, manage issues, manage labels | Support/triage teams that shouldn't have code write access |
| Security Reviewer | Read code, view/dismiss security alerts | Security team members auditing findings without needing write access |
| Release Manager | Read code, manage releases, manage tags | Release engineering without full write access to source |
| Docs Contributor | Write access scoped conceptually to documentation workflows, paired with path-based branch protection | Technical writers contributing without broader code responsibilities |

### Building a Custom Role: Process

1. Identify the specific responsibility that doesn't map to a standard role.
2. List the exact permissions required — resist the temptation to add "just in case" permissions.
3. Create the custom role at the organization level.
4. Assign the role to the relevant team or individuals on the specific repositories where it applies.
5. Document the role's purpose and intended assignees for future audits.

### Custom Role Governance Checklist

- [ ] Maintain a documented catalog of custom roles and their intended use, owned by a named team.
- [ ] Review custom roles periodically for permission creep (roles quietly accumulating more access over time).
- [ ] Avoid creating near-duplicate custom roles — consolidate where possible to reduce management overhead.
- [ ] Require justification before adding a new permission to an existing custom role.

## Fine-Grained Personal Access Tokens (Recap and Application)

Fine-grained PATs (introduced in [03-PAT-Policies-and-Hardening](./03-PAT-Policies-and-Hardening.md)) are the token-level counterpart to custom repository roles — applying least privilege to individual and automation access rather than team-based access.

### Permission Design Principles for Fine-Grained PATs

| Principle | Application |
|---|---|
| Scope to specific repositories | Never select "all repositories" when a specific list will do |
| Grant only required permission categories | e.g., grant "Issues: write" without also granting "Administration: write" |
| Prefer read over write where possible | Only escalate to write permissions when the task genuinely requires it |
| Set the shortest reasonable expiration | Pair narrow scope with a short lifetime for defense in depth |

## Designing Team-Based Access Architecture

### Recommended Pattern

| Layer | Purpose |
|---|---|
| Parent teams | Map to broad organizational units (e.g., "Engineering") |
| Child teams | Map to specific squads/services with their own repository access |
| Custom roles per child team | Grant precisely the permissions that squad's responsibilities require |
| Break-glass/admin team | Small, tightly controlled group with elevated access for emergencies, subject to extra logging/review |

### Team Access Review Checklist

- [ ] Map every team to a clear owning group and purpose.
- [ ] Confirm each team's repository access uses the narrowest role/custom role that satisfies its function.
- [ ] Remove stale team-repository associations no longer tied to an active responsibility.
- [ ] Ensure sensitive repositories (production infra, security tooling, secrets management) restrict access to explicitly named, small teams.

## Repository-Level Hardening

| Control | Guidance |
|---|---|
| Branch protection | Require reviews and status checks on protected branches, regardless of who holds write access |
| CODEOWNERS | Route changes to sensitive paths to the appropriate reviewers automatically |
| Environment protection rules | Require approval for deployments to sensitive environments, independent of general repository write access |
| Visibility settings | Confirm repository visibility (private/internal/public) matches the intended audience, reviewed periodically |

## Least-Privilege Design Checklist

- [ ] Every team/individual's access is tied to a documented, current responsibility.
- [ ] No standing "admin for everything" grants beyond a small, audited set of platform administrators.
- [ ] Custom roles are used where standard roles would otherwise force over-provisioning.
- [ ] Fine-grained PATs are the default for any token-based access, with narrow repository and permission scope.
- [ ] Sensitive repositories have additional protections (CODEOWNERS, environment approval, restricted team access) beyond baseline permissions.

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Defaulting to "Write" or "Admin" because it's simpler | Invest the time to define a custom role that matches actual responsibility |
| Custom roles created ad hoc with no catalog or ownership | Maintain a central, documented catalog of approved custom roles |
| Permission creep on long-lived roles | Schedule recurring reviews of custom role definitions and their assignees |
| Broad token scope "to avoid future token errors" | Issue a new, narrowly scoped token when requirements change rather than over-provisioning upfront |

## Reference Links

- [About custom repository roles](https://docs.github.com/en/organizations/managing-peoples-access-to-your-organization-with-roles/managing-custom-repository-roles-for-an-organization)
- [Repository permission levels for an organization](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/repository-permission-levels-for-an-organization)
- [About fine-grained personal access tokens](https://docs.github.com/en/rest/authentication/permissions-required-for-fine-grained-personal-access-tokens)
- [About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
