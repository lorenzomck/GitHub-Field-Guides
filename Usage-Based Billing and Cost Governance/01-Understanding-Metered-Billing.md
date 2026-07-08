# 01 - Understanding Metered Billing

## Objective

Build a shared, accurate mental model of how GitHub's usage-based billing works so that finance, platform, and engineering leadership can plan, forecast, and govern spend with confidence. Metered billing means cost scales with consumption rather than a flat per-seat fee, which requires different planning habits than traditional software licensing.

## Why Usage-Based Billing Matters

| Driver | Why it matters |
|---|---|
| Elastic consumption | Compute-heavy workloads (CI, AI inference, storage) don't fit a fixed-fee model |
| Pay-for-value | Teams pay in proportion to what they actually use, which rewards efficiency |
| Forecasting complexity | Unlike flat licensing, usage-based costs require trend-based forecasting |
| Governance need | Without guardrails, usage-based spend can grow faster than budget cycles anticipate |

## Metered Products on GitHub

| Product | Unit of measure | Included allowance | Overage behavior |
|---|---|---|---|
| GitHub Copilot | Premium requests / seats | Varies by plan (Business, Enterprise) | Additional premium requests billed per-unit or blocked based on policy |
| GitHub Actions | Minutes (by runner type) + storage (GB) | Included minutes/storage vary by plan | Billed per minute beyond allowance; larger runners consume multipliers |
| Codespaces | Compute hours + storage (GB-month) | Included hours vary by plan | Billed per hour/GB beyond allowance |
| Packages | Storage (GB) + data transfer | Included storage varies by plan | Billed per GB beyond allowance |
| Git LFS | Storage (GB) + bandwidth (GB) | Included storage/bandwidth by plan | Billed per GB beyond allowance |

> Treat this table as a starting framework — confirm current allowances and unit pricing against your organization's live billing settings, since GitHub periodically updates plan inclusions.

## How a Bill Comes Together

1. **Base subscription** — per-seat or per-enterprise licensing (e.g., GitHub Enterprise, Copilot Business/Enterprise seats).
2. **Included usage allowances** — a bundled amount of Actions minutes, storage, or Copilot premium requests included at no extra cost.
3. **Metered overage** — consumption beyond the included allowance, billed at the applicable per-unit rate.
4. **Marketplace/add-on charges** — optional GitHub Marketplace apps or additional product add-ons.
5. **Proration and credits** — mid-cycle seat changes, promotional credits, or enterprise agreement adjustments.

### Runner Multipliers (Actions Example)

| Runner type | Relative minute multiplier |
|---|---|
| Linux (standard) | 1x |
| Windows (standard) | 2x |
| macOS (standard) | 10x |
| Larger Linux runners | Varies by CPU/memory tier |

Multipliers mean a 10-minute macOS job can consume the same budget as a 100-minute Linux job — this is one of the most common sources of unexpected spend.

## Billing Entities and Hierarchy

| Level | What it controls |
|---|---|
| Enterprise account | Consolidated billing across all organizations, enterprise-wide spending limits |
| Organization | Org-level spending limits, budgets, and usage reports |
| Repository | Where the actual consumption event occurs (a workflow run, a Copilot request, a package upload) |
| User | Individual Copilot seat assignment and personal usage attribution |

Understanding this hierarchy matters because governance controls (limits, budgets, alerts) can be applied at different levels, and the right level depends on whether you want organization-wide guardrails or fine-grained team-level control.

## Reading Your Usage Reports

- **Usage report (CSV/UI):** Enterprise and organization owners can export usage by product, SKU, and date range.
- **Cost centers:** Some enterprise tiers support grouping usage by cost center for allocation (see [03-Cost-Allocation-and-Chargeback](./03-Cost-Allocation-and-Chargeback.md)).
- **Invoice detail:** Line-item invoices break down base subscription vs. metered overage vs. add-ons.
- **API access:** Billing usage APIs allow programmatic extraction into internal FinOps tooling or data warehouses.

### Common Report Fields

| Field | Meaning |
|---|---|
| Product/SKU | Which metered product generated the line item (Actions, Copilot, Packages, etc.) |
| Quantity | Units consumed (minutes, GB, requests) |
| Unit price | Rate applied per unit beyond the included allowance |
| Net amount | Quantity × unit price, before taxes/credits |
| Organization/repository | Attribution context for the usage event |

## Common Misconceptions

| Misconception | Reality |
|---|---|
| "Copilot is a flat per-seat cost" | Business/Enterprise tiers include a request allowance; premium model usage above that allowance is metered |
| "Actions minutes reset are unlimited on paid plans" | Paid plans include a fixed minute allowance per month; overage is metered |
| "Storage cost is negligible" | Package and Actions artifact/cache storage can accumulate significantly if retention policies aren't tuned |
| "Only usage costs money" | Idle storage (artifacts, caches, packages) also accrues cost even without active usage |

## Building a Baseline

Before you can govern cost, you need an accurate baseline.

### Baseline Checklist

- [ ] Export the last 3 months of usage reports across all metered products.
- [ ] Identify the top 10 repositories/teams driving Actions minutes and storage.
- [ ] Identify Copilot seat assignment vs. active usage (inactive seats waste budget).
- [ ] Document current spending limits and whether they've ever been hit.
- [ ] Confirm who has billing admin access at the enterprise and organization level.

## Forecasting Approach

| Method | Best for | Limitation |
|---|---|---|
| Trailing 3-month average | Stable, mature usage patterns | Misses seasonal or onboarding-driven spikes |
| Growth-adjusted trend | Teams actively scaling headcount or CI usage | Requires headcount/roadmap input from engineering leadership |
| Scenario modeling | New product launches, planned Copilot rollouts | Requires collaboration between finance and platform teams |

## Key Roles

| Role | Responsibility |
|---|---|
| Enterprise billing admin | Owns spending limits, budgets, and invoice reconciliation |
| FinOps/finance partner | Translates usage trends into forecasts and chargeback |
| Platform engineering | Owns Actions/runner configuration and storage retention policy |
| Engineering managers | Accountable for their team's consumption patterns |

## Reference Links

- [About billing for GitHub Accounts](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/about-billing-for-github-accounts)
- [About billing for GitHub Copilot](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-github-copilot-for-your-enterprise/about-billing-for-github-copilot)
- [About billing for GitHub Actions](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions)
- [About billing for GitHub Packages](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-packages/about-billing-for-github-packages)
- [Viewing your GitHub Actions usage](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/viewing-your-github-actions-usage)
