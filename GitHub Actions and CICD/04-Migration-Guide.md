# 04 - Migration Guide

Migrating to GitHub Actions is both a technical conversion and an operating model change. Successful programs start with inventory, mapping, pilot migrations, and governance rather than rewriting every pipeline at once.

> Official docs: <https://docs.github.com/actions/migrating-to-github-actions>

## Migration Principles

- Start with **high-value, low-complexity** applications first.
- Standardize on **reusable workflows** during migration rather than after it.
- Separate **pipeline conversion** from **runner/infrastructure redesign**.
- Build a **golden path** for onboarding additional teams.
- Validate security controls before migrating production deployments.

## Concept Mapping

| Source platform | Source concept | GitHub Actions equivalent |
|---|---|---|
| Jenkins | Jenkinsfile / pipeline | Workflow YAML |
| Jenkins | Stage | Job or logical section of a job |
| Jenkins | Agent / node | Runner |
| Jenkins | Shared library | Reusable workflow / composite action |
| Azure DevOps | Pipeline | Workflow |
| Azure DevOps | Stage / job | Job / environment gate |
| Azure DevOps | Service connection | OIDC or secrets-based credential |
| GitLab CI | Stage | Job graph using `needs` |
| GitLab CI | Include/template | Reusable workflow / template |
| CircleCI | Orb | Composite action / reusable workflow |
| CircleCI | Executor | Runner environment |

## GitHub Actions Importer

GitHub Actions Importer helps assess and automate migrations from supported CI/CD systems.

### What it can do

- Analyze source pipelines
- Estimate migration complexity
- Convert supported definitions to GitHub Actions workflows
- Produce audit reports and recommendations

### What it does not replace

- Business logic validation
- Secret and credential redesign
- Runner architecture decisions
- Platform team approval workflows

## Platform-Specific Guidance

### Migrating from Jenkins

**Common characteristics:** plugin sprawl, custom agents, shared libraries, tightly coupled controller logic.

**Key gotchas:**
- Shared libraries often hide critical business logic.
- Agents may assume persistent workspaces or preinstalled tools.
- Plugins may have no direct Actions equivalent.
- Freestyle jobs and UI-only configuration need discovery before migration.

**Recommended approach:**
1. Inventory Jenkinsfiles, shared libraries, plugins, credentials, and agents.
2. Identify reusable patterns to convert into reusable workflows.
3. Replace plugin-specific logic with native actions or scripts.
4. Pilot with non-production apps first.

### Migrating from Azure DevOps Pipelines

**Common characteristics:** YAML pipelines, variable groups, environments, approvals, Microsoft ecosystem integrations.

**Key gotchas:**
- Variable groups need a new secrets/configuration strategy.
- Service connections should usually become OIDC federation or environment-scoped secrets.
- Stage approvals map differently using environments and protection rules.

**Recommended approach:**
1. Map variable groups and service connections.
2. Recreate approval flows with GitHub environments.
3. Normalize build/test templates into reusable workflows.

### Migrating from GitLab CI/CD

**Common characteristics:** stages, includes, runners, environment deployments.

**Key gotchas:**
- Stage-based execution may need conversion to DAG-style dependencies with `needs`.
- GitLab predefined variables differ from GitHub context variables.
- Runner tags and protected runners need a runner group strategy.

**Recommended approach:**
1. Convert stage order to explicit job dependencies.
2. Map variables and secrets carefully.
3. Validate artifacts and cache behavior on representative builds.

### Migrating from CircleCI

**Common characteristics:** orbs, executors, workflows, contexts.

**Key gotchas:**
- Orbs may bundle more opinionated behavior than expected.
- Contexts need a replacement strategy using org/repo/environment secrets.
- Parallelism features may need matrix or job-level redesign.

**Recommended approach:**
1. Expand orb behavior and understand hidden steps.
2. Recreate executors with runner/image selection.
3. Translate contexts into least-privilege secret scopes.

## Step-by-Step Migration Approach

### Phase 1: Discover

- [ ] Inventory all pipelines, schedules, and manual triggers
- [ ] Catalog secrets, credentials, and external dependencies
- [ ] Identify deployment approvals and compliance controls
- [ ] Map runner requirements and network dependencies
- [ ] Group pipelines by stack, complexity, and business criticality

### Phase 2: Design

- [ ] Define target workflow standards and repository layout
- [ ] Decide default runner strategy
- [ ] Establish reusable workflows and templates
- [ ] Define action trust, pinning, and permissions policy
- [ ] Map credential strategy to OIDC where possible

### Phase 3: Pilot

- [ ] Migrate 1–3 low-risk applications
- [ ] Compare duration, reliability, and developer experience
- [ ] Validate logs, artifacts, approvals, and rollback steps
- [ ] Capture lessons learned into templates and standards

### Phase 4: Scale

- [ ] Onboard by application archetype
- [ ] Track migration status centrally
- [ ] Provide office hours and enablement support
- [ ] Sunset legacy pipelines gradually, not immediately

### Phase 5: Optimize

- [ ] Reduce duplication using reusable workflows
- [ ] Tune caching, matrices, and concurrency
- [ ] Review runner cost and utilization trends
- [ ] Enforce required workflows and deployment protections

## Common Migration Gotchas

| Gotcha | Why it matters | Mitigation |
|---|---|---|
| Hidden behavior in plugins/orbs/shared libraries | Conversion misses critical logic | Expand and document source behavior first |
| Static long-lived credentials | Security risk | Replace with OIDC or short-lived tokens |
| Persistent runner assumptions | Causes flaky workflows | Move to ephemeral execution and explicit setup |
| Approval model mismatch | Can block releases unexpectedly | Design environments and gate ownership early |
| Naming mismatch on required checks | Branch protection failures | Standardize job and workflow names |

## Migration Scorecard Template

| Area | Questions to answer |
|---|---|
| Pipeline logic | Can we express the workflow natively or via reusable workflows? |
| Secrets | Can static credentials be eliminated or reduced? |
| Infrastructure | Do we need GitHub-hosted, self-hosted, or hybrid runners? |
| Governance | What approvals, protections, and required checks must exist on day one? |
| Support | Who owns the shared automation after cutover? |

## Reference Links

- [Migrating to GitHub Actions](https://docs.github.com/actions/migrating-to-github-actions)
- [About GitHub Actions Importer](https://docs.github.com/actions/migrating-to-github-actions/automated-migrations/about-github-actions-importer)
- [CI/CD migration examples](https://docs.github.com/actions/migrating-to-github-actions/manually-migrating-to-github-actions)
- [Reusable workflows](https://docs.github.com/actions/using-workflows/reusing-workflows)
