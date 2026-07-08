# 04 - Spending Limits and Budget Alerts

## Objective

Prevent bill shock and give finance/platform teams early warning before usage-based spend exceeds expectations, by combining hard spending limits with proactive budget alerting across GitHub's metered products.

## Controls Available

| Control | What it does | Where configured |
|---|---|---|
| Spending limit | A hard cap on metered overage spend (e.g., for Actions minutes/storage beyond included allowance) | Enterprise or organization billing settings |
| Budget | A soft threshold used to track and alert on spend without necessarily blocking usage | Enterprise or organization billing settings |
| Budget alert | Notification sent when spend crosses a defined percentage of a budget | Tied to a configured budget |
| Copilot policy controls | Restrict which models/features are available, indirectly controlling premium request consumption | Enterprise/organization Copilot policy settings |

> Exact control names and configuration surfaces evolve over time — validate current options in your enterprise's billing settings before publishing internal runbooks.

## Spending Limits vs. Budgets: Choosing the Right Tool

| Use spending limits when... | Use budgets/alerts when... |
|---|---|
| You need a hard ceiling that cannot be exceeded (e.g., a sandbox org for experimentation) | You want visibility and escalation without blocking legitimate work |
| The consuming team has no other cost accountability mechanism | The team is already accountable via chargeback and just needs early warning |
| Risk of runaway automation (e.g., misconfigured workflow in a loop) is high | Spend patterns are well understood and predictable |

Most mature organizations use **both**: a generous spending limit as a safety net, paired with tighter budget alerts as the operational signal.

## Step 1: Set a Baseline Spending Limit

1. Review the last 3-6 months of metered overage spend per organization.
2. Set an initial spending limit at roughly 120-150% of the highest observed monthly overage, to avoid immediately blocking legitimate usage while still capping the worst case.
3. Document the rationale and review date for every limit set — spending limits should not be "set and forgotten."

### Spending Limit Checklist

- [ ] Confirm who holds billing admin rights to change spending limits (should be a small, named group).
- [ ] Set limits at both enterprise and organization level where relevant.
- [ ] For sandbox/experimentation organizations, set conservative limits by default.
- [ ] Require a change-approval step (ticket or admin review) before raising a limit.

## Step 2: Configure Budgets and Alerts

### Recommended Alert Thresholds

| Threshold | Action |
|---|---|
| 50% of budget | Informational notification to team lead |
| 75% of budget | Notification + review of usage trend, no action required if trend is expected |
| 90% of budget | Escalation to budget owner, investigate root cause |
| 100%+ of budget | Formal review; determine if this is a one-time spike or a new sustained baseline |

### Who Should Receive Alerts

| Alert level | Recipient |
|---|---|
| Team-level budget | Engineering manager / team lead |
| Organization-level budget | Organization owner + FinOps partner |
| Enterprise-level budget | Enterprise billing admin + finance leadership |

## Step 3: Build a Response Runbook

A budget alert without a defined response is just noise. Define what happens next.

### Sample Response Runbook

1. **Triage** — Identify which product (Actions, Copilot, storage, packages) and which team/repository drove the spike.
2. **Classify** — Is this expected (planned launch, hackathon, onboarding wave) or unexpected (misconfigured workflow, runaway agent loop, leaked credential triggering automated jobs)?
3. **Contain** — For unexpected/anomalous spend, pause the offending workflow, scheduled job, or agent task while investigating.
4. **Resolve** — Fix the root cause (e.g., add concurrency limits, fix an infinite retry loop, adjust artifact retention).
5. **Document** — Log the incident, root cause, and resolution for future reference and to refine alert thresholds.

## Common Sources of Unexpected Spend

| Source | Mitigation |
|---|---|
| Workflow retry loops or missing concurrency controls | Add `concurrency` groups and sane retry/backoff limits |
| Long-running or stuck jobs | Set explicit `timeout-minutes` on all jobs |
| Expensive runner types used unnecessarily (e.g., macOS for a Linux-compatible build) | Right-size runner selection per job |
| Artifact/cache accumulation | Set retention policies and clean up stale artifacts/caches |
| Scheduled agent tasks running too frequently or with too broad a scope | Narrow schedule and scope; review before enabling new scheduled agents |
| Seat sprawl (assigned but inactive Copilot seats) | Pair spending limits with the seat audit in [05-Seat-and-Minutes-Optimization](./05-Seat-and-Minutes-Optimization.md) |

## Guardrails at the Workflow Level

- [ ] Require `timeout-minutes` on all Actions jobs via reusable workflow templates or org policy.
- [ ] Require `concurrency` groups on workflows triggered by pushes/PRs to cancel superseded runs.
- [ ] Restrict which runner types (especially macOS/larger runners) are available to which repositories.
- [ ] Set default artifact and cache retention periods at the organization level.

## Building an Early-Warning Dashboard

| Panel | Purpose |
|---|---|
| Spend vs. budget (current month, trending) | Primary at-a-glance health check |
| Day-over-day spend velocity | Detect anomalies before month-end |
| Top 5 movers (teams/repos with biggest week-over-week change) | Focus investigation effort |
| Open alerts and their resolution status | Track accountability and follow-through |

## Tabletop Exercise: Practicing the Response Runbook

Before relying on the runbook during a real incident, walk through it as a team exercise:

1. Simulate a budget alert firing at the 90% threshold for a specific organization.
2. Have the on-call/budget owner walk through triage, classification, and containment steps live.
3. Time how long it takes to identify the root cause using available dashboards and logs.
4. Identify any gaps — missing dashboard data, unclear ownership, slow escalation — and fix them before the next real incident.
5. Repeat the exercise periodically (e.g., every 6 months) as tooling and ownership change.

## Escalation Path Template

| Step | Owner | Expected response time |
|---|---|---|
| Initial alert triage | On-call platform engineer | Within 1 business day |
| Root cause investigation | Team lead of the affected organization/repository | Within 2 business days |
| Budget owner notification | FinOps/platform lead | Same day as root cause confirmed |
| Spending limit adjustment (if warranted) | Billing admin | Within 1 business day of approval |

## Reference Links

- [Managing your spending limit for GitHub Actions](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/managing-your-spending-limit-for-github-actions)
- [About budgets](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/about-budgets)
- [Usage limits, billing, and administration for GitHub Actions](https://docs.github.com/en/actions/administering-github-actions/usage-limits-billing-and-administration)
- [Managing policies for Copilot in your enterprise](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-policies-for-copilot-in-your-enterprise)
