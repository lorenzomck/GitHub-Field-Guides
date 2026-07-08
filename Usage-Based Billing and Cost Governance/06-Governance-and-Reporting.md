# 06 - Governance and Reporting

## Objective

Establish a durable, FinOps-style governance framework that keeps GitHub usage-based spend visible, accountable, and continuously optimized — turning the practices from the previous documents into an operating rhythm rather than a one-time project.

## The FinOps-for-DevOps Operating Model

| Phase | Focus | Cadence |
|---|---|---|
| Inform | Make cost and usage visible to the people who influence it | Continuous (dashboards) |
| Optimize | Identify and act on efficiency opportunities | Monthly |
| Operate | Formalize policy, budgets, and accountability into standard process | Quarterly review, continuously enforced |

This mirrors established cloud FinOps practice, applied specifically to GitHub's metered products (Copilot, Actions, storage, packages, Codespaces).

## Governance Framework Components

### 1. Roles and Ownership

| Role | Responsibility |
|---|---|
| Executive sponsor | Owns overall GitHub spend accountability at the leadership level |
| FinOps/platform lead | Runs the recurring governance cadence, owns dashboards and reporting |
| Business unit budget owners | Accountable for their allocated spend, review variances |
| Engineering managers | First line of accountability for team-level usage patterns |
| Billing admins | Execute spending limit/budget configuration changes |

### 2. Policy Library

Maintain a small set of clear, written policies rather than ad hoc decisions:

| Policy area | Example policy statement |
|---|---|
| Spending limits | "All organizations must have a spending limit set; sandbox orgs default to a conservative cap." |
| Runner usage | "macOS and larger runners require justification and are opt-in per repository, not default." |
| Retention | "Actions artifacts default to 14-day retention unless a compliance exception is documented." |
| Seat management | "Copilot seats inactive for 60+ days are flagged for review; 90+ days triggers reclamation." |
| Agent/automation budgets | "Scheduled or autonomous agent workloads must have a named owner and a defined premium-request budget." |
| New org/repo creation | "New organizations must be tagged with a cost center at creation time." |

### 3. Decision Rights (RACI Example)

| Decision | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|
| Raise an organization's spending limit | Billing admin | FinOps lead | Business unit owner | Finance |
| Approve additional Copilot premium-request budget | FinOps lead | Executive sponsor | Engineering manager | Finance |
| Change default artifact retention policy | Platform engineering | FinOps lead | Security/compliance | All engineering teams |
| Approve a new scheduled agent workload | Platform engineering | Engineering manager | FinOps lead | — |

## Reporting Framework

### Report Types and Audiences

| Report | Audience | Frequency | Key content |
|---|---|---|---|
| Operational usage dashboard | Platform/FinOps team | Real-time / daily | Spend vs. budget, top movers, anomalies |
| Team scorecard | Engineering managers | Monthly | Team-level usage, trend, efficiency metrics |
| Business unit rollup | Directors/VPs | Monthly or quarterly | Allocated cost, variance vs. plan, notable initiatives |
| Executive summary | Executive sponsor, finance leadership | Quarterly | Total spend trend, ROI narrative, upcoming risk/opportunities |

### Executive Scorecard Template

| Metric | This period | Prior period | Trend | Commentary |
|---|---|---|---|---|
| Total metered spend | | | | |
| Spend vs. budget | | | | |
| Copilot seat utilization rate | | | | |
| Actions minutes per merged PR | | | | |
| Top 3 cost drivers | | | | |
| Notable optimizations completed | | | | |
| Upcoming risks (planned launches, new teams) | | | | |

## Building the Recurring Cadence

### Monthly Governance Review Agenda

1. Review spend vs. budget across all tracked organizations/business units.
2. Review top movers (week-over-week or month-over-month changes) and root causes.
3. Review Copilot seat activity and reclaim inactive seats.
4. Review any budget alert incidents and confirm resolution/documentation.
5. Approve or deny any pending budget increase requests.

### Quarterly Business Review Agenda

1. Present the executive scorecard and cost trend narrative.
2. Revisit the allocation/chargeback methodology for continued accuracy.
3. Review policy library for needed updates (new products, changed pricing, org restructuring).
4. Set goals for the next quarter (e.g., target seat utilization rate, target minutes-per-PR efficiency).

## Maturity Model

| Level | Characteristics |
|---|---|
| Level 1 — Reactive | Spend is only reviewed when an invoice surprises someone; no dashboards or alerts |
| Level 2 — Visible | Dashboards and budget alerts exist, but no formal allocation or recurring review |
| Level 3 — Accountable | Chargeback/showback model in place, monthly reviews happen, policies are documented |
| Level 4 — Optimized | Recurring optimization initiatives run continuously, efficiency metrics trend positively, forecasting is proactive |

### Moving Up the Maturity Curve

- [ ] **Level 1 → 2:** Stand up usage dashboards and configure budget alerts (see [04-Spending-Limits-and-Budget-Alerts](./04-Spending-Limits-and-Budget-Alerts.md)).
- [ ] **Level 2 → 3:** Implement cost allocation/chargeback and a monthly review cadence (see [03-Cost-Allocation-and-Chargeback](./03-Cost-Allocation-and-Chargeback.md)).
- [ ] **Level 3 → 4:** Run continuous optimization initiatives (see [05-Seat-and-Minutes-Optimization](./05-Seat-and-Minutes-Optimization.md)) and tie efficiency metrics to team goals.

## Tooling Considerations

| Need | Approach |
|---|---|
| Automated usage extraction | Scheduled export via billing usage APIs into a data warehouse |
| Dashboarding | BI tool of choice connected to the warehouse tables |
| Alerting | Native budget alerts plus custom threshold alerts built on top of warehouse data for finer-grained triggers |
| Policy enforcement | Combination of native admin policy settings and repository/organization templates |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Governance framework exists on paper but no one runs the cadence | Assign a named owner and put the review on a recurring calendar |
| Policies are unwritten tribal knowledge | Maintain a living policy library accessible to all stakeholders |
| Reporting only goes to engineering, never finance | Include finance/FinOps partners in the monthly and quarterly cadences |
| No feedback loop from optimization work back into policy | Update the policy library whenever a new pitfall or best practice is discovered |

## Reference Links

- [About billing for GitHub Accounts](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/about-billing-for-github-accounts)
- [About budgets](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/about-budgets)
- [Roles for an enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/user-management/managing-users-in-your-enterprise/roles-in-an-enterprise)
- [Managing policies for Copilot in your enterprise](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-policies-for-copilot-in-your-enterprise)
