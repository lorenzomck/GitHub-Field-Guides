# 05 - Developer Onboarding Guide

Use this guide to help developers become productive on GitHub from day one. It covers account access, Git basics, tooling, authentication, and the first pull request workflow.

## Day-One Checklist

| Task | Description | Status |
|------|-------------|--------|
| Join organization | Accept invite or sign in through enterprise identity | ☐ |
| Confirm team membership | Verify correct repositories and teams | ☐ |
| Set up authentication | SSH or PAT/device/browser auth as required | ☐ |
| Install IDE tooling | VS Code, Git, GitHub CLI, recommended extensions | ☐ |
| Clone a repository | Verify read/write access and branch protection behavior | ☐ |
| Run local build/test | Ensure dev environment works end-to-end | ☐ |
| Create first branch and PR | Validate contribution workflow | ☐ |

## Git Basics Refresher

### Core Concepts

| Concept | Summary |
|---------|---------|
| Repository | Project history and files tracked by Git |
| Clone | Local copy of a remote repository |
| Branch | Isolated line of development |
| Commit | Snapshot of changes with history |
| Push | Send local commits to GitHub |
| Pull request | Propose, review, and merge changes |

### Basic Workflow

1. Clone the repository.
2. Create a new branch.
3. Make changes and commit them locally.
4. Push the branch to GitHub.
5. Open a pull request.
6. Address feedback and merge when approved.

Reference: [GitHub Git cheatsheet](https://docs.github.com/en/get-started/using-git/about-git)

## Recommended IDE Setup (VS Code)

### Essential Tools

| Tool | Purpose |
|------|---------|
| Visual Studio Code | Editor and GitHub integration |
| Git | Source control CLI |
| GitHub CLI (`gh`) | Repository, PR, and auth workflows |
| Language SDKs | Language-specific build and test support |

### Suggested VS Code Extensions

| Extension | Benefit |
|-----------|---------|
| GitHub Pull Requests and Issues | Review PRs and manage issues from VS Code |
| GitHub Actions | Workflow visibility and YAML support |
| EditorConfig | Consistent formatting behavior |
| Language-specific linters | Faster feedback while coding |

Reference: [VS Code and GitHub](https://docs.github.com/en/get-started/getting-started-with-git/set-up-git)

## Installing GitHub CLI

### Common First Commands

```bash
gh auth login
gh repo clone ORG/REPO
gh pr create
gh pr status
```

Reference: [GitHub CLI manual](https://cli.github.com/manual/)

## Authentication: SSH and Personal Access Tokens

### SSH Setup Steps

1. Generate an SSH key if you do not already have one.
2. Add the public key to your GitHub account.
3. Test the connection.
4. Clone repositories using the SSH URL.

Reference: [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

### PAT / Browser / Device Auth Notes

| Method | Best For | Notes |
|--------|----------|-------|
| Browser/device flow | Most developers | Simplest for GitHub CLI |
| Personal access token | Automation or specific HTTPS policies | Use least privilege and expiration |
| SSH | Frequent Git operations | Great developer ergonomics |

Reference: [About authentication to GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github)

## First Pull Request Workflow

### Step-by-Step

1. Clone the repository:

   ```bash
   gh repo clone ORG/REPO
   ```

2. Create a feature branch:

   ```bash
   git checkout -b my-first-change
   ```

3. Make your changes.
4. Run the project’s local checks.
5. Commit clearly:

   ```bash
   git add .
   git commit -m "Add initial improvement"
   ```

6. Push the branch:

   ```bash
   git push -u origin my-first-change
   ```

7. Open a pull request:

   ```bash
   gh pr create
   ```

8. Request reviewers, respond to feedback, and update the branch as needed.
9. Merge according to repository policy.

### Pull Request Checklist

| Check | Why |
|-------|-----|
| Branch is up to date | Avoid merge conflicts |
| Tests pass | Maintain build health |
| Description is clear | Helps reviewers understand intent |
| Linked issue/task included | Improves traceability |
| Review comments addressed | Speeds approval |

## Branch Protection and Review Expectations

Developers should understand that many repositories enforce:

- Required pull request reviews.
- Required status checks.
- Conversation resolution before merge.
- Restricted direct pushes to protected branches.

Reference: [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)

## Recommended Learning Path

| Stage | Focus |
|-------|-------|
| Day 1 | Access, auth, clone, local build |
| Week 1 | Pull requests, reviews, issue workflows |
| Week 2 | Actions visibility, code owners, security practices |
| First month | Team conventions, reusable workflows, advanced GitHub features |

## Helpful GitHub Docs

- [Getting started with GitHub](https://docs.github.com/en/get-started)
- [Creating a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
- [Reviewing proposed changes](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests)
- [Authenticating with the GitHub CLI](https://docs.github.com/en/github-cli/github-cli/quickstart)
