# 03 - Repository Rulesets and Policies

Rulesets give platform teams a scalable way to enforce repository and branch protections without relying on one-off manual configuration. They are especially valuable for organizations that need consistent compliance across many repositories while still allowing for exceptions where justified.

## Why Rulesets Matter

| Problem | Traditional approach | Ruleset-based approach |
|---|---|---|
| Inconsistent branch protection | Configure every repo separately | Apply org-wide rules to matching repositories |
| Weak auditability | Manual settings changes are hard to track | Central policy definition is easier to review |
| Slow compliance rollout | Teams must self-configure controls | Platform team pushes guardrails centrally |
| Exception handling | Ad hoc admin overrides | Scoped targeting and explicit bypass permissions |

## Ruleset Types and Common Controls

| Control area | Example rules |
|---|---|
| Pull request quality | Require PRs, approvals, code owner review, resolved conversations |
| Commit integrity | Require signed commits, linear history |
| CI enforcement | Require status checks or required workflows |
| Deployment safety | Gate merges on successful environment workflows |
| Branch safety | Restrict deletions, force pushes, branch creation |
| Tag integrity | Protect release tags from modification |

## Org-Wide vs Repo-Level Rulesets

| Scope | Best for | Example |
|---|---|---|
| Organization-wide | Baseline security and compliance | Require pull requests and signed commits for all production repos |
| Repository-level | Team- or product-specific controls | Extra approvals for a regulated payment service |
| Branch-targeted | Different standards for different branches | `main` stricter than `release/*` |
| Tag-targeted | Release integrity | Protect `v*` tags from updates or deletion |

## Using Custom Properties for Repository Classification

Custom properties help classify repositories so policies can be targeted intelligently instead of universally.

| Property | Example values | Why it helps |
|---|---|---|
| `data_classification` | public, internal, confidential, regulated | Apply tighter rules to sensitive repos |
| `service_tier` | tier1, tier2, sandbox | Match operational and review expectations |
| `system_type` | service, library, frontend, infra | Route archetype-specific workflows |
| `deployment_criticality` | low, medium, high | Tailor release controls |

### Example classification model

```json
{
  "business_unit": "platform",
  "system_type": "service",
  "data_classification": "regulated",
  "service_tier": "tier1"
}
```

With repository metadata in place, platform teams can map controls to risk rather than forcing identical controls on every repository.

## Required Workflows as Policy

Required workflows are a strong pattern for platform engineering because they centralize critical checks.

| Workflow type | Typical purpose |
|---|---|
| Security baseline | Dependency review, CodeQL, secret scanning validation |
| Build verification | Lint, unit tests, packaging |
| Compliance evidence | SBOM generation, artifact signing, provenance |
| Documentation checks | README, ownership, catalog metadata validation |

### Example required workflow pattern

```yaml
name: required-platform-checks
on:
  pull_request:
  merge_group:

jobs:
  baseline:
    uses: your-org/.github/.github/workflows/required-checks.yml@main
    with:
      enforce-codeql: true
      enforce-sbom: true
```

## Tag Protection Rules

Tag protection is often overlooked but critical for release integrity.

| Tag pattern | Recommended control |
|---|---|
| `v*` | Restrict creation/deletion to release automation and platform admins |
| `prod-*` | Lock down to deployment service account |
| `hotfix-*` | Require elevated approval process |

### Benefits of tag protection

- Prevents accidental or malicious retagging of releases.
- Preserves traceability between builds, releases, and deployed artifacts.
- Supports supply chain integrity practices.

## Push Restrictions and Bypass Strategy

Push restrictions are useful for high-sensitivity branches, but they should be designed carefully to avoid slowing teams unnecessarily.

| Strategy | Good use case | Risk if overused |
|---|---|---|
| Restrict direct pushes to `main` | Standard engineering baseline | Minimal |
| Restrict force pushes to everyone | Protect history integrity | Minimal |
| Restrict pushes to release branches | Managed release trains | Can slow emergency response |
| Narrow bypass list | Auditability for emergencies | Requires clear ownership |

## Scaling Compliance with Rulesets

A mature platform team uses layered governance:

1. **Org baseline** for all production repositories.
2. **Risk-based overlays** using repo metadata.
3. **Repository-specific exceptions** documented and approved.
4. **Automated evidence collection** through required workflows and audit logs.

### Example policy matrix

| Repo class | PR approvals | Signed commits | Required workflows | Direct push |
|---|---|---|---|---|
| Sandbox | 1 | Optional | Basic CI | Limited |
| Internal service | 1-2 | Yes | CI + security | No |
| Regulated service | 2 + CODEOWNERS | Yes | CI + security + compliance | No |
| Release automation | 2 | Yes | CI + provenance | Restricted to bot |

## Operating Model Tips

| Recommendation | Why |
|---|---|
| Publish policy intent in docs | Teams understand the reason for controls |
| Use dry-run/pilot where possible | Reduces rollout surprises |
| Track exception volume | High exception rates may indicate poor golden path design |
| Pair rulesets with templates | Guardrails work best when defaults already comply |
| Review bypass permissions quarterly | Prevents governance drift |

## Reference Links

- [About rulesets](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [Available rules for rulesets](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)
- [About custom properties](https://docs.github.com/organizations/managing-organization-settings/managing-custom-properties-for-repositories-in-your-organization)
- [Protected tags](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
