# Classroom Transition Guide

## Overview

GitHub Classroom is sunsetting and transitioning to partner solutions. This guide helps educators and organizations understand the timeline, available alternatives, and how to migrate existing workflows.

---

## GitHub Classroom Sunset

**Official Announcement:** [gh.io/classroom-sunset](https://gh.io/classroom-sunset)

### What's Happening

GitHub Classroom — the tool for distributing coding assignments and auto-grading student work — is being retired. GitHub is partnering with specialized education platforms to provide continued (and improved) functionality.

### Key Dates

Refer to the [official sunset page](https://gh.io/classroom-sunset) for the latest timeline, including:

- End of new feature development
- Read-only period for existing classrooms
- Final data export deadline
- Service shutdown date

---

## What to Do Now

### 1. Export Your Data

- Download all assignment repositories and student work
- Export grading records and feedback
- Save any custom autograding configurations

### 2. Evaluate Partner Solutions

GitHub is recommending partner platforms that integrate with GitHub repositories. Check the [sunset announcement](https://gh.io/classroom-sunset) for the current list of recommended partners.

### 3. Plan Your Migration

| Step | Action | Timeline |
|------|--------|----------|
| 1 | Audit current Classroom usage | Immediately |
| 2 | Export assignment data | Before read-only date |
| 3 | Evaluate partner platforms | 2-4 weeks |
| 4 | Pilot new platform with one course | 1-2 weeks |
| 5 | Migrate remaining courses | Before shutdown |

---

## Alternatives for Training and Enablement

If you were using GitHub Classroom for internal developer training (not just academic courses), consider these alternatives:

### GitHub Skills + Exercise Builder

- **Best for:** Self-paced learning with automated validation
- **How it works:** Learners fork a template repo and complete steps validated by GitHub Actions
- **Tool:** [GitHub Skills Exercise Builder](https://github.com/arilivigni/gh-skills-builder) for creating custom exercises

### Template Repositories + GitHub Actions

- **Best for:** Standardized project scaffolding with automated checks
- **How it works:** Create template repos that students/trainees use as starting points; Actions workflows validate their work

### GitHub Codespaces for Education

- **Best for:** Consistent development environments for workshops
- **How it works:** Provide pre-configured dev containers that learners launch in one click

---

## For Educators

### What Changes

| Before (Classroom) | After (Alternatives) |
|--------------------|---------------------|
| Centralized roster management | Partner platform or manual team management |
| Automatic repo creation per student | Template repositories + fork/clone |
| Built-in autograding | GitHub Actions workflows |
| Deadline enforcement | Branch protection rules + Actions |
| Plagiarism detection | Partner platform feature |

### What Stays the Same

- Student work lives in GitHub repositories
- Git workflows (branches, PRs, commits) remain the same
- GitHub Actions can still validate and test student code
- Codespaces provides consistent environments

---

## Questions?

- Review the [official sunset FAQ](https://gh.io/classroom-sunset)
- Discuss with peers in the [GitHub Community](https://github.com/orgs/community/discussions)
- Contact your GitHub account team for enterprise training alternatives
