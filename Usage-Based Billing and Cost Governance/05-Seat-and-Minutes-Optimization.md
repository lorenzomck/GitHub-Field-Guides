# 05 - Seat and Minutes Optimization

## Objective

Ensure Copilot seats, Actions minutes, and storage are consumed efficiently — so the organization pays for value delivered rather than idle capacity, unused licenses, or unoptimized pipelines.

## Part 1: Copilot Seat Optimization

### Why Seats Go Unoptimized

| Cause | Impact |
|---|---|
| Seats auto-assigned at hire without validation of need | Inactive seats accumulate cost with no usage |
| No offboarding process tied to seat reclamation | Departed employees' seats remain assigned |
| No visibility into per-seat activity | Impossible to distinguish power users from non-users |
| Fear of under-provisioning | Teams over-request seats "just in case" |

### Seat Lifecycle Management

| Stage | Action |
|---|---|
| Provisioning | Assign seats through group-based access (e.g., IdP group sync) rather than manual one-off assignment |
| Active monitoring | Review seat activity reports monthly; flag seats with no activity in 30-60 days |
| Reclamation | Automatically or manually unassign seats after a defined inactivity period, with a grace-period notification |
| Offboarding | Tie seat removal to the existing employee offboarding/deprovisioning workflow (see [SCIM provisioning](../Enterprise%20Identity%20and%20Access%20Management/02-Entra-ID-Integration-and-SCIM-Provisioning.md)) |

### Seat Audit Checklist

- [ ] Export the Copilot seat activity report for the organization/enterprise.
- [ ] Identify seats with zero or near-zero activity over the last 30/60/90 days.
- [ ] Cross-reference inactive seats against active employee/contractor status.
- [ ] Reclaim seats for departed or long-term-inactive users.
- [ ] Establish a recurring (monthly or quarterly) seat review cadence with a named owner.

### Right-Sizing Seat Assignment

| Approach | Description |
|---|---|
| Role-based assignment | Assign seats via engineering role/group rather than ad hoc individual requests |
| Trial-then-commit | New teams get a time-boxed trial before being counted against permanent budget |
| Usage-based renewal | Seat renewal at each billing cycle requires evidence of recent activity |
| Self-service with guardrails | Allow developers to request seats but require manager approval above a threshold |

## Part 2: Actions Minutes Optimization

### Reducing Minute Consumption

| Technique | Effect |
|---|---|
| Cache dependencies (`actions/cache`, language-specific caching) | Avoids redundant download/build steps on every run |
| Use path filters (`paths:`, `paths-ignore:`) | Skip workflow runs unaffected by a given change |
| Use `concurrency` to cancel superseded runs | Avoids paying for runs that are immediately obsoleted by a newer push |
| Split workflows by change scope | Only run expensive integration/E2E suites when relevant paths change |
| Use matrix builds judiciously | Avoid combinatorial explosion of jobs that don't add proportional value |
| Choose the smallest sufficient runner | Avoid defaulting to large or macOS runners when a standard Linux runner suffices |
| Consider self-hosted runners for high-volume, steady workloads | Shifts cost from per-minute metering to fixed infrastructure cost, but adds operational overhead |

### Runner Selection Guidance

| Workload | Recommended runner |
|---|---|
| Linting, unit tests, small builds | Standard Linux runner |
| Cross-platform validation | Use platform-specific runners only for the jobs that need them, not the whole pipeline |
| GPU/ML workloads | Larger/specialized runners, scoped to only the jobs that require them |
| High-frequency, steady-state CI at scale | Evaluate self-hosted runners or runner groups for cost predictability |

### Workflow Efficiency Audit Checklist

- [ ] Identify the top 10 workflows by total minutes consumed over the last month.
- [ ] Check for missing caching in build-heavy workflows.
- [ ] Check for missing `concurrency` groups on PR/push-triggered workflows.
- [ ] Check for missing `timeout-minutes` (a stuck job can silently burn minutes for hours).
- [ ] Review matrix build dimensions for unnecessary combinations.
- [ ] Evaluate whether expensive runner types (macOS, larger runners) are used only where required.

## Part 3: Storage Optimization

### Storage Cost Drivers

| Source | Optimization |
|---|---|
| Actions artifacts | Set shorter default retention (e.g., 7-14 days) unless compliance requires longer |
| Actions cache | Caches are automatically evicted over time, but oversized caches still add cost pressure — keep cache scope tight |
| GitHub Packages | Set retention/cleanup policies for old package versions, especially pre-release/snapshot builds |
| Git LFS | Periodically audit large binary assets; consider external storage for very large files |
| Codespaces | Set idle timeout and auto-delete policies for unused Codespaces |

### Storage Governance Checklist

- [ ] Set an organization-wide default artifact retention policy.
- [ ] Set a package version retention/cleanup policy for high-churn packages (e.g., nightly builds).
- [ ] Set Codespaces idle timeout and retention defaults.
- [ ] Review Git LFS usage for repositories with unexpectedly large storage footprints.

## Reporting on Optimization Impact

| Metric | Before/after comparison |
|---|---|
| Active seat utilization rate | % of assigned seats with recent activity |
| Average workflow duration | Trend after caching/runner optimizations |
| Minutes consumed per merged PR | Efficiency metric that normalizes for team growth |
| Storage footprint by product (artifacts, packages, LFS) | Trend after retention policy changes |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Optimizing once and never revisiting | Bake seat and workflow audits into a recurring quarterly cadence |
| Cutting retention too aggressively | Balance cost savings against compliance/debugging needs before shortening retention |
| Moving everything to self-hosted runners without an operational owner | Evaluate total cost of ownership, not just per-minute savings |
| Removing seats without a communication plan | Notify users before reclaiming seats to avoid support friction |

## Reference Links

- [Managing access to GitHub Copilot in your enterprise or organization](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-access-to-github-copilot-in-your-organization)
- [Viewing your GitHub Actions usage](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/viewing-your-github-actions-usage)
- [Caching dependencies to speed up workflows](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows)
- [About artifacts, logs, and caches in GitHub Actions](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/storing-and-sharing-data-from-a-workflow)
- [About billing for GitHub Packages](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-packages/about-billing-for-github-packages)
