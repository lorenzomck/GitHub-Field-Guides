# 01 - GHAS Rollout Plan

## Objective

Roll out GitHub Advanced Security (GHAS) in a measurable, low-friction way that improves developer security outcomes without overwhelming teams. The program should establish a repeatable adoption model that starts with a representative pilot and scales to the full organization.

## Outcomes to Target

| Outcome | Why it matters | Example KPI |
|---|---|---|
| Secret exposure reduction | Prevent credential leakage before and after merge | 90% of detected secrets remediated within SLA |
| Vulnerability visibility | Surface dependency and code risks early | 100% of tier-1 repos onboarded to dependency review + scanning |
| Developer workflow integration | Embed security into pull requests and CI | 80% of PRs reviewed with security checks enabled |
| Executive reporting | Demonstrate ROI and risk reduction | Monthly trend dashboard for alerts, MTTR, and adoption |

## Phase 0 - Prepare the Program

### Stakeholders

| Role | Responsibilities |
|---|---|
| Executive sponsor | Funds rollout, removes blockers, tracks risk reduction |
| Security program lead | Defines policies, standards, and success criteria |
| Platform/DevEx team | Builds templates, automations, enablement assets |
| AppSec engineers | Tune rules, triage alerts, coach pilot teams |
| Engineering managers | Commit pilot teams, enforce remediation expectations |
| Developers | Validate usability and integrate fixes into delivery flow |

### Readiness Checklist

- [ ] Confirm GHAS licensing and target organizations.
- [ ] Classify repositories by business criticality, language, and CI maturity.
- [ ] Define severity-based remediation SLAs.
- [ ] Publish exception and risk acceptance workflow.
- [ ] Agree on dashboard metrics and review cadence.
- [ ] Identify enablement owners for docs, office hours, and training.

## Phase 1 - Select the Pilot

Choose 5-15 repositories that represent your engineering landscape.

### Pilot Selection Criteria

| Criterion | Good pilot signal |
|---|---|
| Business criticality | Includes at least one crown-jewel application |
| Tech diversity | Covers major languages and frameworks in use |
| Delivery maturity | Teams already use pull requests and CI/CD consistently |
| Team engagement | Engineering leads willing to partner and provide feedback |
| Risk profile | Repositories with dependencies, APIs, or secrets exposure |

### Pilot Mix Example

| Repo type | Count | Notes |
|---|---|---|
| Customer-facing service | 3 | Prioritize internet-facing workloads |
| Internal platform | 2 | Useful for reusable patterns and templates |
| Shared library/SDK | 2 | Helps prevent vulnerability propagation |
| Data/automation repo | 1 | Common source of secret leakage |

## Phase 2 - Enable Core Controls

### Recommended Enablement Order

| Control | Rollout guidance | Value |
|---|---|---|
| Secret scanning | Enable first, including push protection where supported | Fastest route to reducing credential leakage |
| Dependency review | Require in pull requests | Blocks risky dependency changes before merge |
| Dependabot alerts | Enable for all pilot repos | Establishes vulnerability backlog visibility |
| Code scanning | Start with CodeQL default setup, then tune | Identifies exploitable patterns in first-party code |
| Dependabot security updates | Pilot auto-remediation on low-risk ecosystems | Improves patch velocity |

### Minimum Configuration Checklist

- [ ] Enable secret scanning for supported repositories.
- [ ] Turn on push protection for users who can benefit from pre-push prevention.
- [ ] Configure dependency graph and dependency review.
- [ ] Enable Dependabot alerts and security updates.
- [ ] Turn on code scanning with CodeQL default setup.
- [ ] Define alert triage labels, owners, and escalation paths.

## Phase 3 - Operationalize Triage and Remediation

### Triage Model

| Severity | Initial response | Target remediation |
|---|---|---|
| Critical | Same business day | 24-72 hours |
| High | 1 business day | 7 days |
| Medium | Weekly backlog review | 30 days |
| Low | Monthly review | Risk-based |

### Operating Cadence

- **Daily:** Review critical and high alerts.
- **Weekly:** Pilot repo owner sync to review trends, suppressions, and false positives.
- **Monthly:** Executive scorecard review with adoption, MTTR, and outstanding risk.

### Tuning Guidance

- Suppress only with documented rationale and expiry date.
- Capture recurring false positives and convert them into org-level guidance.
- Standardize reusable CodeQL and Dependabot configuration in starter repositories.

## Phase 4 - Measure Adoption and Effectiveness

### Metrics Dashboard

| Category | Metric | Target |
|---|---|---|
| Adoption | % of repos with secret scanning enabled | 100% of scoped repos |
| Adoption | % of repos with code scanning enabled | 90%+ of supported repos |
| Developer workflow | % of PRs with dependency review checks | 95% |
| Risk reduction | Open critical alerts | Downward trend month over month |
| Remediation | Mean time to remediate high alerts | < 7 days |
| Quality | False-positive rate after tuning | < 10% for priority rules |

### Evidence to Collect

- GHAS enablement export by organization/repository.
- Samples of code scanning alerts, dismissals, and remediation pull requests.
- Screenshots or exports of dependency review and Dependabot alert dashboards.
- Records of office hours, training, and pilot retrospectives.

## Phase 5 - Scale to the Full Organization

### Scale Strategy

| Workstream | Scale action |
|---|---|
| Governance | Make minimum security controls part of repo creation standards |
| Platform | Bake scanning and policy defaults into templates and reusable workflows |
| Enablement | Publish quickstarts, FAQs, and secure coding examples |
| Reporting | Roll up org-level metrics into scorecards by business unit |
| Exceptions | Centralize waivers with expiry dates and compensating controls |

### Expansion Checklist

- [ ] Group repositories by business unit or application tier.
- [ ] Roll out waves every 2-4 weeks instead of all at once.
- [ ] Pre-stage CI changes for languages that require build customization.
- [ ] Formalize remediation SLAs in engineering scorecards.
- [ ] Train repo admins on alert ownership and suppression policies.
- [ ] Review pilot feedback and incorporate lessons before each wave.

## Sample 90-Day Timeline

| Timeframe | Milestones |
|---|---|
| Days 1-15 | Baseline inventory, pilot selection, KPI definition |
| Days 16-30 | Enable secret scanning, dependency review, Dependabot |
| Days 31-45 | Turn on code scanning, start triage workflows |
| Days 46-60 | Tune alerts, establish dashboards, publish runbooks |
| Days 61-90 | Expand to next repo wave, measure MTTR, brief leadership |

## Common Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Too many alerts at once | Start with pilot cohorts, severity filters, and triage playbooks |
| Developer resistance | Keep controls in PR flow, provide fix examples, highlight quick wins |
| CI overhead | Start with default setup and optimize heavy scans later |
| Weak ownership | Require repo owners and security champions for every pilot team |
| Poor executive visibility | Use a compact monthly scorecard tied to risk language |

## Reference Links

- [About GitHub Advanced Security](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [Configuring code scanning](https://docs.github.com/en/code-security/code-scanning/enabling-code-scanning)
- [About secret scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)
- [About dependency review](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-dependency-review)
- [Dependabot best practices](https://docs.github.com/en/code-security/dependabot)
