# 02 - Migrating from Azure DevOps

This guide covers the most common Azure DevOps to GitHub migration paths: repositories, pipelines, work tracking, artifacts, and identity integration.

## Migration Scope Map

| Azure DevOps Capability | GitHub Equivalent | Notes |
|-------------------------|-------------------|-------|
| Azure Repos | GitHub repositories | Preserve branches, tags, and history |
| Azure Pipelines | GitHub Actions | Convert YAML and release logic |
| Azure Boards | GitHub Projects + Issues | Decide whether full work-item migration is needed |
| Azure Artifacts | GitHub Packages | Map feeds and package permissions |
| Azure AD / Entra ID | SAML SSO, SCIM, Enterprise Managed Users | Align with enterprise identity design |

## Prerequisites

- GitHub enterprise or organization target prepared.
- Azure DevOps organization/project admin permissions.
- Access to GitHub Enterprise Importer if moving repositories at scale.
- Runner strategy decided for GitHub Actions.
- Service connections, secrets, and package feeds documented.

## Repositories: Preserving Git History

### Options

| Method | Best For | Comments |
|--------|----------|----------|
| `git clone --mirror` + `git push --mirror` | Straight Git repo transfer | Simple and reliable |
| [GitHub Enterprise Importer](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-repositories-from-azure-devops-to-github-enterprise-cloud) | Large-scale migrations | Supports automation and tracking |
| GitHub UI import | Small/simple repos | Usually not preferred for enterprise waves |

### Manual Repository Migration Steps

1. Create the destination repository in GitHub.
2. Mirror-clone the Azure DevOps repository:

   ```bash
   git clone --mirror https://dev.azure.com/ORG/PROJECT/_git/REPO
   ```

3. Change into the mirrored repo directory.
4. Add the GitHub remote:

   ```bash
   git remote set-url origin https://github.com/ORG/REPO.git
   ```

5. Push all refs and tags:

   ```bash
   git push --mirror
   ```

6. Validate branches, tags, default branch, and protected branch requirements.

## Migrating Pipelines to GitHub Actions

### Capability Mapping

| Azure Pipelines | GitHub Actions |
|-----------------|----------------|
| `azure-pipelines.yml` | `.github/workflows/*.yml` |
| Agent pools | GitHub-hosted or self-hosted runners |
| Variable groups | Organization/repository/environment variables and secrets |
| Stages/approvals | Jobs, environments, protection rules |
| Pipeline artifacts | Artifacts / Packages / Releases |

### Migration Workflow

1. Inventory pipeline files, templates, service connections, and variable groups.
2. Identify triggers: PR, CI, schedule, manual, release.
3. Rebuild one representative pipeline in GitHub Actions.
4. Map secrets to GitHub Actions secrets or environment secrets.
5. Recreate approval flows with environments and required reviewers.
6. Test runner compatibility, caching, and network access.
7. Parallel-run key pipelines before cutover.

### Helpful Documentation

- [Migrating to GitHub Actions](https://docs.github.com/en/actions/migrating-to-github-actions)
- [Azure Pipelines migration examples](https://docs.github.com/en/actions/migrating-to-github-actions/manually-migrating-to-github-actions/migrating-from-azure-pipelines-to-github-actions)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments)

## Azure Boards to GitHub Projects

### Planning Questions

- Are historical work items required, or only active backlog data?
- Do teams need roadmap/portfolio features beyond GitHub Projects?
- Will issues be the source of truth going forward?

### Mapping Table

| Azure Boards | GitHub |
|-------------|--------|
| Epic/Feature/User Story/Bug | Issue types / labels / Projects fields |
| Iteration paths | Milestones or custom Project fields |
| Area paths | Labels, teams, or project views |
| Boards | Project views |

### Recommended Approach

1. Decide on a minimal issue taxonomy before importing data.
2. Create issue templates, labels, and project fields first.
3. Import only current or relevant work if historical fidelity is not essential.
4. Train teams on GitHub Issues and Projects workflows.

Reference: [About Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)

## Azure Artifacts to GitHub Packages

| Artifact Type | GitHub Packages Option |
|---------------|------------------------|
| NuGet | GitHub Packages NuGet registry |
| npm | GitHub Packages npm registry |
| Maven | GitHub Packages Maven registry |
| Container images | GitHub Container Registry |

### Migration Steps

1. Export the list of package feeds and consumers.
2. Create destination package scopes and permissions in GitHub.
3. Update package manager authentication.
4. Republish packages or configure upstream/rebuild strategy.
5. Update CI workflows and developer instructions.

Reference: [Working with GitHub Packages](https://docs.github.com/en/packages/learn-github-packages/introduction-to-github-packages)

## Azure AD / Microsoft Entra ID Integration

GitHub can integrate with enterprise identity providers through SAML SSO, SCIM, and Enterprise Managed Users.

### Identity Migration Checklist

| Task | Why It Matters |
|------|----------------|
| Confirm user lifecycle model | Avoid orphaned accounts |
| Validate group-to-team mapping | Preserve least-privilege access |
| Test SSO login flows | Reduce day-one login failures |
| Plan service account alternatives | Human SSO may not suit automation |
| Document PAT/SSH policy | Ensure developers can authenticate safely |

References:
- [About Enterprise Managed Users](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-iam/understanding-iam-for-enterprises/about-enterprise-managed-users)
- [About identity and access management with SAML single sign-on](https://docs.github.com/en/enterprise-cloud@latest/organizations/managing-saml-single-sign-on-for-your-organization/about-identity-and-access-management-with-saml-single-sign-on)

## GitHub Enterprise Importer Walkthrough

### When to Use It

Use GitHub Enterprise Importer (GEI) when migrating many Azure DevOps repositories and you need repeatability, visibility, and enterprise-scale execution.

### High-Level Steps

1. Prepare the target enterprise and organizations.
2. Grant required permissions in Azure DevOps and GitHub.
3. Install and authenticate the GitHub CLI with GEI extensions if used in your process.
4. Build migration source/target maps.
5. Run a pilot migration.
6. Review migration logs and remediate errors.
7. Execute wave migrations and monitor completion.

Reference: [Migrating repositories from Azure DevOps to GitHub Enterprise Cloud](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-repositories-from-azure-devops-to-github-enterprise-cloud)

## End-to-End Step-by-Step Walkthrough

1. **Assess** Azure Repos, Pipelines, Boards, Artifacts, users, and integrations.
2. **Prepare GitHub** organizations, teams, runners, policies, and security settings.
3. **Migrate a pilot repo** with full history and branch protections.
4. **Rebuild one CI/CD workflow** in GitHub Actions and validate parity.
5. **Map work tracking** to GitHub Issues/Projects and migrate only required data.
6. **Migrate packages** and update developer/CI credentials.
7. **Test SSO and team access** with representative users.
8. **Run a cutover rehearsal** including freeze, final sync, and validation.
9. **Execute production waves** with communications and support coverage.
10. **Perform post-cutover validation** using the post-migration checklist.

## Azure DevOps Gotchas

| Gotcha | Recommendation |
|--------|----------------|
| Release pipelines with manual stages | Rebuild using environments and reusable workflows |
| Variable groups linked to Key Vault | Reassess secrets ownership and runtime access |
| Boards-heavy teams | Pilot Projects and issue field design early |
| Service connections | Replace with GitHub OIDC or scoped credentials where possible |
| Classic pipelines | Convert to YAML before or during migration if feasible |

## Reference Links

- [Migrations to GitHub](https://docs.github.com/en/migrations)
- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [Projects documentation](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
- [GitHub Packages documentation](https://docs.github.com/en/packages)
