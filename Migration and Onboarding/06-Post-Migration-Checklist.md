# 06 - Post-Migration Checklist

Migration success is determined after cutover, not at the moment repositories land in GitHub. Use this checklist to validate the platform, support teams, and confirm adoption goals over the first 90 days.

## Immediate Validation Checklist

| Area | Validation Step | Status |
|------|-----------------|--------|
| Repositories | Confirm branches, tags, default branch, and protections | ☐ |
| CI/CD | Run critical workflows and verify deploy paths | ☐ |
| Access | Validate team membership, SSO, and least-privilege permissions | ☐ |
| Packages/artifacts | Publish and consume at least one package/image | ☐ |
| Integrations | Test webhooks, chatops, ITSM, and notification flows | ☐ |
| Developer experience | Verify clone, branch, PR, review, and merge workflows | ☐ |

## Repository and Data Validation

### Validation Table

| Item | What to Check | Evidence |
|------|----------------|----------|
| Commit history | Sample commit counts and dates align | Git log comparison |
| Tags/releases | Historical release tags exist | Release/tag audit |
| LFS files | Binary assets download correctly | Clone/open test |
| Branch settings | Protection/rulesets match policy | Admin review |
| CODEOWNERS/templates | Review routing and templates function | PR dry run |

## CI/CD Verification

### Critical Questions

- Do pull request checks trigger correctly?
- Do environment approvals and deployment protections work as expected?
- Can self-hosted runners reach required internal resources?
- Are secrets, variables, and OIDC integrations functioning correctly?

### CI/CD Verification Steps

1. Trigger at least one **pull request workflow** and one **main branch workflow**.
2. Execute a **deployment workflow** to each target environment.
3. Validate artifact retention, caches, test reports, and logs.
4. Confirm rollback or redeploy procedures are documented and tested.

Reference: [Monitoring your current jobs in the UI](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows)

## Access Audit

### Access Review Table

| Check | Goal |
|-------|------|
| SSO login test | Users can authenticate successfully |
| Team membership review | Users have correct repo visibility |
| Admin role audit | Privileged roles are limited |
| Service account review | Automation identities are documented and scoped |
| Token/SSH review | Credentials comply with security policy |

Reference: [Reviewing your organization’s audit log](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization)

## Decommissioning the Old Platform

Do not decommission the source platform until validation is complete and business owners sign off.

### Decommissioning Checklist

1. Freeze source repositories or set them read-only.
2. Communicate the authoritative source of truth is now GitHub.
3. Export/archive required compliance records.
4. Disable legacy integrations gradually.
5. Retire unused agent pools, package feeds, and service hooks.
6. Update internal docs, onboarding portals, and support scripts.

## Training and Hypercare Plan

| Audience | Support Activity | Timing |
|----------|------------------|--------|
| Developers | Office hours, quick-start docs, PR coaching | Week 1–4 |
| Team leads | Migration retrospectives, workflow tuning | Week 2–6 |
| Admins | Access audits, runner tuning, policy adjustments | Week 1–8 |
| Executives | Adoption and delivery KPI reviews | Monthly |

## Success Metrics

### Suggested KPIs

| Metric | Why It Matters |
|--------|----------------|
| Active users in GitHub | Confirms adoption |
| PR throughput and cycle time | Measures developer flow |
| Workflow success rate | Indicates CI/CD stability |
| Mean time to restore/fix failed workflows | Measures operational resilience |
| Open support tickets | Shows friction during adoption |
| Legacy platform usage | Indicates readiness for full decommissioning |

## 30 / 60 / 90 Day Plan

| Timeframe | Focus Areas | Sample Outcomes |
|-----------|-------------|-----------------|
| First 30 days | Stabilization and support | Access issues resolved, workflows green, office hours active |
| 60 days | Optimization and standardization | Shared workflow patterns, refined permissions, improved onboarding |
| 90 days | Consolidation and measurement | Legacy platform retirement plan complete, KPI review delivered |

## Step-by-Step Post-Migration Workflow

1. Run repository and history validation for each migration wave.
2. Execute critical CI/CD and deployment smoke tests.
3. Audit access, SSO, teams, and privileged roles.
4. Confirm packages, registries, and integrations work in production.
5. Hold retrospective sessions with pilot and wave teams.
6. Publish enablement updates and targeted training.
7. Track KPIs weekly through the stabilization period.
8. Decommission legacy systems only after formal sign-off.

## Recommended GitHub Docs

- [Migrations documentation](https://docs.github.com/en/migrations)
- [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [Audit log documentation](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization)
- [GitHub Actions troubleshooting](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows)
