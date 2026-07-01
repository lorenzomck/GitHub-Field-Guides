# 01 - Migration Planning

A successful migration starts well before the first repository is moved. This guide helps you assess the current environment, choose the right migration pattern, align stakeholders, and reduce delivery risk.

## Objectives

- Build a complete inventory of source control, CI/CD, planning, package, and identity dependencies.
- Choose a migration approach that fits your scale, compliance posture, and release cadence.
- Create a realistic timeline with clear gates, owners, and rollback plans.
- Ensure developers, administrators, and leadership understand what changes and when.

## Pre-Migration Assessment Checklist

| Area | Questions to Answer | Typical Outputs |
|------|---------------------|-----------------|
| Repositories | How many repos, branches, tags, protected branches, and archived projects exist? | Repository inventory, criticality tiers |
| Pipelines | Which builds, releases, approvals, secrets, and self-hosted agents are in use? | CI/CD migration matrix |
| Work tracking | Are boards, issues, epics, milestones, or external integrations in scope? | Planning system mapping |
| Users and teams | Which users, groups, contractors, and service accounts need access? | Access model and team structure |
| Security and compliance | What policies exist for SSO, SCIM, branch protection, code scanning, and audit logs? | Control mapping to GitHub |
| Integrations | Which webhooks, bots, package feeds, and ITSM tools are attached? | Integration remediation list |

## Repository and Platform Inventory

### Recommended Inventory Fields

| Field | Description |
|-------|-------------|
| Repository name | Current repo/project identifier |
| Business owner | Team or function accountable for the codebase |
| Technical owner | Maintainer or platform contact |
| Language/stack | Primary technology and build system |
| Size | Repository size including Git LFS or binaries |
| Activity level | Commits, pull requests/MRs, and releases per month |
| Dependencies | External services, packages, deployment targets |
| Migration wave | Pilot, wave 1, wave 2, long tail, archive |
| Special handling | Monorepo, submodules, secrets cleanup, compliance review |

### Inventory Process

1. Export repository/project lists from the source platform.
2. Tag each repository by **business criticality**: mission-critical, important, or standard.
3. Identify repos with **large binaries**, **LFS**, **submodules**, or **history anomalies**.
4. Document branch policies, merge rules, and required reviewers.
5. Confirm whether code, work items, pipelines, artifacts, wikis, and test results all need migration.

## Pipeline and Delivery Assessment

| Current Capability | What to Review | GitHub Target |
|--------------------|----------------|---------------|
| Build pipelines | Triggers, matrix builds, templates, secrets | [GitHub Actions workflows](https://docs.github.com/en/actions/using-workflows) |
| Release pipelines | Environments, approvals, gates, manual interventions | [Deployments and environments](https://docs.github.com/en/actions/deployment/targeting-different-environments) |
| Agent pools/runners | OS images, network access, caching, scale | [Self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners) |
| Test reporting | Unit, integration, security, quality gates | [Workflow artifacts and summaries](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts) |
| Secrets | Variable groups, key vault links, service connections | [Actions secrets and variables](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) |

## Users, Teams, and Identity Planning

### Identity Topics to Resolve

- Enterprise Managed Users vs standard users.
- SAML SSO and SCIM provisioning requirements.
- Team hierarchy and repository access model.
- Admin roles, billing roles, and audit responsibilities.
- Service account strategy for bots and automation.

### Suggested Access Planning Table

| Persona | GitHub Role | Notes |
|---------|-------------|-------|
| Enterprise admins | Enterprise owner/admin | Limited, break-glass controlled |
| Org admins | Organization owner or admin role | Separate from day-to-day repo maintainers |
| Team maintainers | Team maintainer + repo admin where needed | Prefer least privilege |
| Developers | Write/Triage/Read depending on repo | Map from current project permissions |
| Auditors/Security | Security manager / read access | Include audit log review paths |

## Choosing a Migration Strategy

### Big Bang Migration

Best when tight alignment is possible and the environment is relatively uniform.

**Pros**
- Single cutover date.
- Less duplicated administration across two platforms.
- Faster consolidation of tooling and governance.

**Cons**
- Higher coordination overhead.
- Larger blast radius if issues occur.
- Tougher for organizations with many custom integrations.

### Phased Migration

Best when you have many teams, different compliance needs, or significant delivery variability.

**Pros**
- Lower operational risk.
- Lessons from early waves improve later waves.
- Easier change management and training.

**Cons**
- Longer period of dual-platform support.
- More coordination for dependencies spanning migrated and non-migrated teams.
- Requires strong governance to avoid inconsistent standards.

### Strategy Decision Matrix

| Factor | Big Bang | Phased |
|--------|----------|--------|
| Number of teams | Small to medium | Medium to very large |
| Platform complexity | Low to moderate | Moderate to high |
| Regulatory constraints | Lower | Higher |
| Tolerance for dual-running | Low | High |
| Need for iterative learning | Lower | Higher |

## Timeline Planning

### Sample Migration Timeline

| Phase | Activities | Exit Criteria |
|------|------------|---------------|
| Discovery | Inventory, interviews, dependency mapping | Approved scope and inventory |
| Foundation | Org setup, identity, runner strategy, policies | GitHub landing zone ready |
| Pilot | Migrate a few representative teams | Pilot success review completed |
| Waves | Execute migration batches | Each wave validated and signed off |
| Cutover | Freeze, final sync, DNS/tooling updates | Teams active in GitHub |
| Stabilization | Hypercare, support, cleanup, training | KPIs trending positively |

### Timeline Planning Tips

1. Schedule pilot migrations away from critical product release windows.
2. Include time for **identity testing**, **network approvals**, and **runner hardening**.
3. Reserve a hypercare window after each migration wave.
4. Build rollback criteria into every wave plan.
5. Coordinate communications, training, and documentation updates ahead of cutover.

## Stakeholder Communication Plan

| Audience | Message | Timing | Owner |
|----------|---------|--------|-------|
| Executives | Why the migration matters, milestones, risk posture | Monthly / phase gates | Program sponsor |
| Engineering managers | Team impact, required prep, cutover dates | Biweekly / wave planning | Program manager |
| Developers | Tooling changes, onboarding steps, support channels | 2–4 weeks before wave | Enablement lead |
| Security/Compliance | Policy mapping, controls, audit requirements | Early and ongoing | Security lead |
| Help desk / IT | Access and authentication changes | Before user onboarding | IT operations |

## Risk Mitigation

### Common Risks and Responses

| Risk | Impact | Mitigation |
|------|--------|------------|
| Incomplete inventory | Missed repos or pipelines | Validate exports with team owners |
| Permission mismatches | Blocked developers or over-entitlement | Run access dry-runs and audits |
| Pipeline parity gaps | Release delays | Migrate and test critical workflows first |
| Secrets/config drift | Failed builds and security exposure | Centralize secret review before cutover |
| User adoption issues | Low productivity after migration | Deliver training, office hours, quick-reference guides |
| Integration failures | Broken notifications or deployments | Test webhooks and service connections in pilot |

## Step-by-Step Planning Workflow

1. **Define scope**: repositories, pipelines, work items, registries, identities, and integrations.
2. **Build the inventory**: export platform data and validate it with technical owners.
3. **Segment the estate**: classify by risk, criticality, and migration complexity.
4. **Design the target state**: organizations, teams, rulesets, runners, and security controls.
5. **Choose the migration model**: big bang or phased, with pilot success metrics.
6. **Create the wave plan**: dates, owners, communication checkpoints, rollback criteria.
7. **Prepare enablement**: documentation, office hours, support routes, and onboarding guides.
8. **Run rehearsals**: dry-run repository and pipeline migrations for representative teams.

## Recommended GitHub References

- [About migrations to GitHub](https://docs.github.com/en/migrations/overview/about-migrations-to-github)
- [Using GitHub Enterprise Importer](https://docs.github.com/en/migrations/using-github-enterprise-importer)
- [Managing teams and people with access](https://docs.github.com/en/organizations/organizing-members-into-teams)
- [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [Planning and tracking with Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
