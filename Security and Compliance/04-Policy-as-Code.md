# 04 - Policy as Code

## Objective

Translate governance requirements into enforceable, version-aware controls so repository health, review discipline, and release requirements are implemented consistently at scale.

## Policy Layers in GitHub

| Layer | Example controls | Scope |
|---|---|---|
| Enterprise/organization policies | Allowed actions, repository creation policies, security defaults | Broadest governance layer |
| Repository rulesets | Branch/tag restrictions, status checks, signed commits, required reviews | Standardized enforcement for groups of repos |
| CODEOWNERS | Mandatory review by designated experts | Path-level ownership and approval |
| Actions workflows | Automated validation and compliance checks | Technical enforcement in CI/CD |
| Repository properties | Metadata for policy targeting and reporting | Enables dynamic governance |

## Repository Rulesets

Rulesets provide centralized, reusable governance compared with one-off branch settings.

### Recommended Ruleset Controls

| Control | Why it matters |
|---|---|
| Restrict direct pushes to protected branches | Prevents bypass of review and CI |
| Require pull requests before merge | Enforces auditable change flow |
| Require status checks | Ensures quality/security signals are present |
| Require linear history or merge policy | Improves traceability |
| Block force pushes and deletions | Protects source integrity |
| Require signed commits/tags where appropriate | Strengthens change authenticity |

### Ruleset Rollout Checklist

- [ ] Define baseline rulesets by repo tier (critical, standard, experimental).
- [ ] Pilot in monitor-only mode where available before strict enforcement.
- [ ] Document exception handling and temporary bypass approvals.
- [ ] Review bypass permissions quarterly.

## Branch Protection and Required Status Checks

| Check category | Examples |
|---|---|
| Build verification | Compile, unit tests, integration tests |
| Security | Code scanning, dependency review, secret scanning gates |
| Quality | Linting, coverage thresholds, license checks |
| Compliance | Commit signature checks, change ticket validation, SBOM generation |

### Status Check Design Principles

1. Keep required checks stable and well named.
2. Separate fast feedback checks from long-running release validations.
3. Fail closed for security-critical workflows on protected branches.
4. Publish SLA and ownership for broken required checks.

## CODEOWNERS Enforcement

CODEOWNERS helps encode accountability for sensitive files and systems.

| Sensitive area | Suggested owner groups |
|---|---|
| `.github/workflows/` | Platform engineering + security |
| `infra/` or IaC | Cloud platform team |
| `auth/`, `security/` | AppSec or service security champions |
| `payments/`, `regulated/` | Domain owners + compliance stakeholders |

### CODEOWNERS Checklist

- [ ] Require owner review for security-sensitive paths.
- [ ] Keep team names current with actual operating ownership.
- [ ] Protect the CODEOWNERS file itself with required review.
- [ ] Align owner coverage to on-call or support responsibilities.

## Organization Policies and Standards

| Policy area | Example implementation |
|---|---|
| Approved Actions usage | Allow only GitHub-owned or approved internal actions |
| Default branch standards | Apply standard branch names and protections |
| Secret handling | Require environment secrets for deployments |
| Repository creation | Mandate templates, labels, and properties |
| Security enablement | Require dependency graph, Dependabot, and secret scanning |

## Custom Repository Properties

Custom properties make policy targeting and reporting easier.

### Useful Property Examples

| Property | Example values | Usage |
|---|---|---|
| `data_classification` | public, internal, confidential, regulated | Apply stricter rules to sensitive repos |
| `service_tier` | tier1, tier2, tier3 | Vary evidence and approval rigor |
| `deployment_target` | internal, external, production | Drive environment protection rules |
| `compliance_scope` | none, pci, hipaa, sox | Filter control reports and workflow logic |

## Automated Compliance Checks via Actions

Use GitHub Actions to validate policies that cannot be fully expressed in native settings.

| Check | Example automation |
|---|---|
| Required files | Ensure CODEOWNERS, SECURITY.md, and workflow templates exist |
| Metadata checks | Validate repository properties or labels |
| Branch hygiene | Verify required ruleset assignment |
| Deployment gating | Check change tickets or approvals before prod deploy |
| Supply chain | Generate SBOMs, verify attestations, check pinned actions |

### Automation Checklist

- [ ] Put policy workflows in reusable templates.
- [ ] Limit write permissions in compliance workflows.
- [ ] Log results in a machine-readable format for reporting.
- [ ] Escalate repeated failures to repo owners automatically.

## Example Governance Matrix

| Repo tier | Review requirement | Required checks | Extra controls |
|---|---|---|---|
| Tier 1 / regulated | 2 reviewers incl. CODEOWNER | Build, tests, code scanning, dependency review | Protected environments, signed tags |
| Tier 2 | 1-2 reviewers | Build, tests, dependency review | Standard ruleset |
| Tier 3 / sandbox | 1 reviewer | Basic CI | Fewer restrictions, no prod deploy |

## Operational Considerations

| Challenge | Recommendation |
|---|---|
| Policy sprawl | Prefer reusable org-level rulesets over repo-specific drift |
| Developer friction | Pair every control with documentation and fast remediation tips |
| Exceptions | Time-box waivers and track them centrally |
| M&A or legacy repos | Use phased adoption with monitor-first modes |

## Reference Links

- [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [About CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Manage custom properties](https://docs.github.com/en/organizations/managing-organization-settings/managing-custom-properties-for-repositories-in-your-organization)
- [Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
