# 03 - Cost Allocation and Chargeback

## Objective

Design a defensible, transparent model for attributing GitHub usage costs to the teams, organizations, and business units that actually generate them — so that spend accountability shifts from a central platform team to the consumers of the platform.

## Why Allocation Matters

| Without allocation | With allocation |
|---|---|
| Platform team absorbs all cost centrally with no accountability signal | Teams see and own their consumption footprint |
| Budget conversations are reactive and adversarial | Budget conversations are data-driven and proactive |
| No incentive to optimize usage | Teams are incentivized to right-size Actions minutes, storage, and Copilot usage |
| Hard to fund growth requests | Easy to justify incremental budget with usage trend data |

## Allocation Models

| Model | Description | Best for |
|---|---|---|
| Direct attribution | Cost mapped 1:1 to the organization/repository that generated it | Enterprises with clean org-per-business-unit structures |
| Cost center tagging | Usage tagged with a cost center/business unit identifier independent of org structure | Enterprises where multiple teams share organizations |
| Proportional/shared allocation | Shared infrastructure costs (e.g., a shared CI org) split by a formula (headcount, seat count, historical usage %) | Shared platform or DevEx-owned resources |
| Hybrid | Direct attribution where possible, proportional allocation for shared services | Most large enterprises in practice |

## Step 1: Define the Allocation Hierarchy

Map your GitHub structure to your finance structure before choosing a model.

| GitHub level | Typical finance mapping |
|---|---|
| Enterprise account | Company-wide GitHub agreement |
| Organization | Business unit, division, or product line |
| Team | Cost center or department |
| Repository | Project or product |
| User | Individual (rarely billed directly, but used for seat right-sizing) |

### Mapping Checklist

- [ ] Document which organizations map to which business units.
- [ ] Identify organizations that are genuinely shared across business units (platform, security, DevEx).
- [ ] Confirm whether cost centers are tracked as GitHub organization/team metadata or only in an external finance system.

## Step 2: Choose Your Tagging Convention

Consistent tagging is the foundation of any allocation model.

| Tag dimension | Example values | Where applied |
|---|---|---|
| Business unit | `platform`, `payments`, `mobile` | Organization or team name/description |
| Cost center | `CC-1042` | Team custom property or naming convention |
| Environment | `prod`, `shared`, `sandbox` | Repository topic or naming convention |
| Owner | Named engineering manager or director | Team metadata |

### Tagging Guidance

- Use GitHub's custom properties (organization-level) to attach structured metadata to repositories for cost-center mapping.
- Keep the taxonomy small and stable — a tagging scheme that changes every quarter breaks trend analysis.
- Require tagging as part of repository/organization creation templates so new resources aren't left unattributed.

## Step 3: Extract and Join Usage Data

1. Export usage reports (Actions minutes, storage, Copilot premium requests, packages) at the organization/repository level.
2. Join usage data against your tagging/metadata table (business unit, cost center, owner).
3. Aggregate by the allocation dimension your finance team consumes (usually cost center or business unit).
4. Publish to a shared dashboard or data warehouse table refreshed on a fixed cadence (weekly/monthly).

### Data Pipeline Reference Architecture

| Stage | Tooling pattern |
|---|---|
| Extraction | Billing usage API / scheduled export → object storage or data warehouse staging table |
| Enrichment | Join with organization/team metadata table maintained by platform team |
| Aggregation | Scheduled query/job rolling up usage by business unit and cost center |
| Publication | BI dashboard or finance-facing report, refreshed on a fixed cadence |

## Step 4: Design the Chargeback (or Showback) Model

| Approach | Description | Trade-off |
|---|---|---|
| Showback | Report cost by team without moving money | Low friction, good starting point, weaker cost discipline |
| Soft chargeback | Cost reported against team budgets, tracked but not journal-entried | Builds accountability without finance system complexity |
| Hard chargeback | Actual cost transferred via internal billing/journal entries | Highest accountability, requires finance system integration |

### Recommended Maturity Path

1. **Start with showback** to validate data accuracy and get teams comfortable seeing their own usage.
2. **Move to soft chargeback** once data is trusted, tying usage to team-level budget targets.
3. **Graduate to hard chargeback** only for organizations with mature FinOps practices and finance system integration.

## Handling Shared/Platform Costs

Not every cost has a single clear owner. Common shared cost categories:

| Shared cost | Suggested allocation approach |
|---|---|
| Shared CI runners/org | Allocate by proportional minutes consumed per team |
| Enterprise-wide security scanning | Allocate evenly across all onboarded repos, or exclude from chargeback as a platform investment |
| Shared Copilot pilot seats | Track separately as an enablement investment until steady-state rollout |
| Platform team's own tooling repos | Attribute to the platform/DevEx cost center, not distributed |

## Governance and Dispute Resolution

- [ ] Publish the allocation methodology so teams understand how their number is calculated.
- [ ] Provide a lightweight dispute process for teams that believe their allocation is inaccurate (e.g., mis-tagged repository).
- [ ] Review and recalibrate the allocation formula at least annually as org structure changes.
- [ ] Keep a change log of methodology updates so historical comparisons remain explainable.

## Reporting Cadence

| Report | Audience | Frequency |
|---|---|---|
| Team-level usage summary | Engineering managers | Monthly |
| Business unit rollup | Directors/VPs | Monthly or quarterly |
| Enterprise-wide trend and forecast | Finance/FinOps | Quarterly |
| Ad hoc variance investigation | Platform + finance | As needed when a team spikes unexpectedly |

## Reference Links

- [About billing for GitHub Accounts](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/about-billing-for-github-accounts)
- [Managing custom properties for repositories in your organization](https://docs.github.com/en/organizations/managing-organization-settings/managing-custom-properties-for-repositories-in-your-organization)
- [Viewing your GitHub Actions usage](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/viewing-your-github-actions-usage)
- [About enterprise accounts](https://docs.github.com/en/enterprise-cloud@latest/admin/overview/about-enterprise-accounts)
