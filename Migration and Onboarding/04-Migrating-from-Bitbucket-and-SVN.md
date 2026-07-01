# 04 - Migrating from Bitbucket and SVN

Bitbucket and SVN migrations often require more source-specific planning than GitLab or Azure DevOps, especially when repositories vary between cloud and server models or when centralized version control history must be transformed into Git.

## Bitbucket Migration Paths

### Bitbucket Capability Map

| Bitbucket Source | GitHub Target | Notes |
|------------------|---------------|-------|
| Bitbucket Cloud repos | GitHub repositories | Git history migration is straightforward |
| Bitbucket Server/Data Center | GitHub repositories | Validate auth, network, and plugin dependencies |
| Pipelines | GitHub Actions | Rebuild pipeline logic |
| Pull requests | Pull requests or historical reference | Review metadata portability varies |
| LFS | Git LFS | Confirm object migration |

### Bitbucket Cloud vs Server Considerations

| Topic | Bitbucket Cloud | Bitbucket Server/Data Center |
|-------|------------------|------------------------------|
| Connectivity | Internet-accessible APIs | May require internal network access |
| Scale | Often easier to enumerate | Can include more custom workflows/plugins |
| Auth | App passwords / tokens | Internal SSO or service credentials |
| Metadata export | Usually API-friendly | Depends on version and tooling |

## Bitbucket Repository Migration Steps

1. Export a repository inventory including owners, projects, default branches, and LFS usage.
2. Create target GitHub repositories or automate creation by naming convention.
3. Mirror-clone each Bitbucket repository:

   ```bash
   git clone --mirror https://bitbucket.example.com/scm/project/repo.git
   ```

4. Repoint origin to GitHub and push all refs:

   ```bash
   git remote set-url origin https://github.com/ORG/REPO.git
   git push --mirror
   ```

5. If LFS is in use, fetch and push all LFS content.
6. Recreate branch protections, CODEOWNERS, webhooks, and deploy keys.

## SVN-to-Git Conversion Overview

Subversion migrations require converting a centralized history model into Git’s distributed model. The most common tools are `git svn` and `svn2git`.

### Tool Comparison

| Tool | Best For | Notes |
|------|----------|-------|
| `git svn` | Flexible/manual conversions | Native Git tooling, more control |
| `svn2git` | Standard trunk/branches/tags layouts | Faster setup for conventional repos |

## SVN Discovery Checklist

| Item | Why It Matters |
|------|----------------|
| Repository layout | Need to confirm `trunk`, `branches`, `tags` patterns |
| Author mapping | Converts SVN usernames to Git authors |
| Large binaries | Likely need Git LFS after conversion |
| Externals | Must be redesigned, vendored, or replaced |
| Monorepo layout | Decide whether to keep whole tree or split by directory |

## Using `git svn`

### Step-by-Step Example

1. Inspect the SVN repository structure.
2. Build an authors mapping file.
3. Clone with standard layout if applicable:

   ```bash
   git svn clone URL --stdlayout --authors-file=authors.txt repo-name
   ```

4. Review branches and tags after conversion.
5. Clean up tag references if needed.
6. Convert large binary patterns to Git LFS tracking.
7. Push the final Git repository to GitHub.

## Using `svn2git`

### Step-by-Step Example

1. Install `svn2git` in an approved environment.
2. Prepare an author mapping file.
3. Run conversion for standard layouts:

   ```bash
   svn2git URL --authors authors.txt
   ```

4. Review branch names, tag fidelity, and ignored paths.
5. Add Git LFS tracking where required.
6. Push to the destination GitHub repository.

## Preserving History and Tags

### Best Practices

- Validate commit counts before and after migration.
- Compare representative historical tags and branches.
- Preserve author identity with a reviewed mapping file.
- Document any paths intentionally excluded from the conversion.

### Validation Table

| Validation Item | Method |
|-----------------|--------|
| Commit history depth | Compare counts and sample commit SHAs/messages |
| Tags | Check historical releases and annotated tags |
| Branches | Confirm active and long-lived branches exist |
| Binary assets | Open and clone representative LFS-managed files |
| Buildability | Run at least one build/test workflow post-migration |

## Large Files and Git LFS

For both Bitbucket and SVN sources, large binary assets should usually be migrated to Git LFS where appropriate.

Reference: [About Git Large File Storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage)

## Handling Monorepos

### Key Questions

- Keep the monorepo intact, or split by service/domain?
- Will teams share release cadence and permissions?
- Are build times manageable with current tooling?
- Does migration create an opportunity to standardize layouts?

### Monorepo Decision Table

| Option | When to Choose It |
|--------|-------------------|
| Keep as monorepo | Tight coupling, shared tooling, coordinated releases |
| Split into multiple repos | Clear boundaries, independent teams, smaller pipelines |
| Hybrid | Preserve history centrally, then gradually extract services |

## Step-by-Step Migration Workflow

1. Identify whether the source is Bitbucket Cloud, Bitbucket Server/Data Center, or SVN.
2. Inventory repositories, branches, tags, binaries, and integrations.
3. Pilot one straightforward repo and one complex repo.
4. Migrate Git history and LFS content.
5. Rebuild CI/CD in GitHub Actions.
6. Recreate permissions, reviews, and automation.
7. Validate history, artifacts, and developer workflows.
8. Cut over teams with onboarding and post-migration support.

## Reference Links

- [About migrations to GitHub](https://docs.github.com/en/migrations/overview/about-migrations-to-github)
- [Duplicating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository)
- [Managing large files](https://docs.github.com/en/repositories/working-with-files/managing-large-files)
- [GitHub Actions docs](https://docs.github.com/en/actions)
