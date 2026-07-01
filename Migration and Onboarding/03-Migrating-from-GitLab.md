# 03 - Migrating from GitLab

GitLab migrations commonly include repositories, Git LFS objects, CI/CD pipelines, issues, merge requests, and registry assets. This guide focuses on preserving development continuity while adapting GitLab patterns to GitHub-native workflows.

## Capability Mapping

| GitLab Capability | GitHub Equivalent | Notes |
|-------------------|-------------------|-------|
| GitLab projects | GitHub repositories | Preserve history, branches, tags |
| Git LFS | Git LFS | Validate quotas and object transfer |
| `.gitlab-ci.yml` | GitHub Actions workflows | Translate stages, jobs, and variables |
| Issues / Boards | Issues / Projects | Decide what history to keep |
| Merge requests | Pull requests | Review rules and templates may differ |
| Container Registry | GitHub Container Registry | Update auth and image naming |

## Repository Migration with History and LFS

### Recommended Prechecks

- Confirm whether Git LFS is enabled and list tracked patterns.
- Identify submodules, mirrors, archived repos, and protected branches.
- Verify default branch names and tag conventions.

### Basic Git Migration Steps

1. Create the destination repository in GitHub.
2. Mirror-clone the GitLab project:

   ```bash
   git clone --mirror https://gitlab.example.com/group/project.git
   ```

3. Change the origin to the GitHub repository.
4. Push all refs:

   ```bash
   git push --mirror
   ```

5. If LFS is in use, ensure LFS objects are fetched and pushed:

   ```bash
   git lfs fetch --all
   git lfs push --all origin
   ```

6. Validate tags, branches, default branch, and LFS file accessibility.

Reference: [Duplicating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository)

## Migrating CI/CD from `.gitlab-ci.yml` to GitHub Actions

### Concept Mapping

| GitLab CI/CD | GitHub Actions |
|--------------|----------------|
| Stages | Jobs / workflow dependencies |
| Runners | GitHub-hosted or self-hosted runners |
| CI variables | Secrets and variables |
| Includes/templates | Reusable workflows / composite actions |
| Environments | Environments |

### Conversion Steps

1. Parse `.gitlab-ci.yml` and list all stages, jobs, images, caches, artifacts, and rules.
2. Identify reusable patterns that should become reusable workflows or composite actions.
3. Recreate triggers (`push`, `pull_request`, `workflow_dispatch`, schedules).
4. Map CI variables to repository, organization, or environment secrets.
5. Rebuild cache and artifact handling.
6. Test one branch and one pull request flow before production cutover.

Documentation:
- [Migrating to GitHub Actions](https://docs.github.com/en/actions/migrating-to-github-actions)
- [Migrating from GitLab CI/CD to GitHub Actions](https://docs.github.com/en/actions/migrating-to-github-actions/manually-migrating-to-github-actions/migrating-from-gitlab-cicd-to-github-actions)

## Issues, Merge Requests, and Planning Data

### Decision Points

- Do you need every historical issue and merge request, or only active items?
- Should closed merge requests remain in GitLab as historical record?
- Are labels, milestones, assignees, and discussions required for compliance or reporting?

### Mapping Guidance

| GitLab | GitHub |
|--------|--------|
| Issue | Issue |
| Merge request | Pull request |
| Milestone | Milestone |
| Label | Label |
| Board | Project |

### Recommended Approach

1. Standardize labels and templates in GitHub first.
2. Import active issues and high-value historical items.
3. Decide whether to recreate merge request history or reference it externally.
4. Train reviewers on pull request semantics and branch protection.

## Container Registry Migration

GitLab Container Registry users typically move to GitHub Container Registry (GHCR).

### Step-by-Step

1. List all images, tags, retention settings, and consumers.
2. Update build pipelines to authenticate to `ghcr.io`.
3. Retag and push existing images where historical retention is required.
4. Update deployment manifests and helm values referencing old registry paths.
5. Validate pull access from runtime environments.

Reference: [Working with the Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

## GitHub Enterprise Importer for GitLab

GEI is a strong choice for scaling GitLab migrations, especially when you have many repositories and want central monitoring.

### Typical Workflow

1. Prepare GitHub organizations and access tokens.
2. Ensure the GitLab source grants the required exporter/importer permissions.
3. Build source-to-target repository mappings.
4. Pilot a small set of repos, including one with LFS and one with CI complexity.
5. Review logs, fix edge cases, then execute migration waves.

Reference: [Migrating repositories from GitLab with GitHub Enterprise Importer](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-repositories-from-gitlab-with-github-enterprise-importer)

## Key Differences and Gotchas

| Area | GitLab Behavior | GitHub Consideration |
|------|-----------------|----------------------|
| Merge requests | Different approval and merge settings | Recreate with branch protections and rulesets |
| CI templates | `include` patterns are common | Use reusable workflows and shared actions |
| Environments | Review apps may be built differently | Re-architect with environments and deployments |
| Package/registry auth | Token model differs | Update automation credentials carefully |
| LFS | May be large and slow to validate | Test representative binary assets early |

## Practical Migration Checklist

1. Inventory projects, runners, registries, variables, and integrations.
2. Validate Git and LFS transfer for a pilot project.
3. Translate `.gitlab-ci.yml` to one or more GitHub Actions workflows.
4. Recreate branch protection, CODEOWNERS, and reviewer expectations.
5. Migrate or map active issues and planning artifacts.
6. Update registry paths and deployment automation.
7. Run dual-platform validation during pilot and early waves.
8. Communicate pull request workflow changes to engineering teams.

## Reference Links

- [GitHub Enterprise Importer docs](https://docs.github.com/en/migrations/using-github-enterprise-importer)
- [Git LFS on GitHub](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage)
- [About pull request reviews](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/code-review/about-pull-request-reviews)
- [About Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)
