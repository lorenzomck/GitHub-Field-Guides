<p align="center">
  <img src="./assets/banner.svg" alt="GitHub Actions and CI/CD Resource Pack" width="100%"/>
</p>

<p align="center">
  <img src="https://octodex.github.com/images/manufacturetocat.png" alt="Manufacturetocat" width="140"/>
</p>

<h1 align="center">GitHub Actions &amp; CI/CD Resource Pack</h1>

<p align="center">
  A practical enterprise guide to designing, securing, operating, and migrating CI/CD workflows on GitHub Actions.
</p>

GitHub Actions gives teams a unified automation platform inside GitHub for continuous integration, continuous delivery, release orchestration, policy enforcement, and operational workflows. This pack is designed for platform teams, engineering leaders, security teams, and delivery teams that need both foundational guidance and enterprise-ready implementation patterns.

## Contents

| # | Document | Description |
|---|----------|-------------|
| 01 | [Introduction to GitHub Actions](01-Introduction-to-GitHub-Actions.md) | Core concepts, YAML structure, execution model, and platform comparisons |
| 02 | [Starter Workflows and Templates](02-Starter-Workflows-and-Templates.md) | Bootstrap patterns using starter workflows, templates, reusable workflows, and composite actions |
| 03 | [Runners and Infrastructure](03-Runners-and-Infrastructure.md) | Runner strategy, scaling models, cost trade-offs, and infrastructure security |
| 04 | [Migration Guide](04-Migration-Guide.md) | Step-by-step migration guidance from Jenkins, Azure DevOps, GitLab CI, and CircleCI |
| 05 | [Security and Best Practices](05-Security-and-Best-Practices.md) | Secrets, OIDC, permissions, action trust, deployment controls, and governance |

## How to Use

1. **New to Actions?** Start with [01](01-Introduction-to-GitHub-Actions.md) to understand the execution model and vocabulary.
2. **Building standards?** Use [02](02-Starter-Workflows-and-Templates.md) and [03](03-Runners-and-Infrastructure.md) to define opinionated engineering templates.
3. **Planning a migration?** Work through [04](04-Migration-Guide.md) with one pilot application before broad rollout.
4. **Hardening for production?** Apply the controls in [05](05-Security-and-Best-Practices.md) before scaling to business-critical workloads.
5. **Running enablement sessions?** Treat each document as a workshop module for platform engineering or developer experience onboarding.

## Recommended Reading Order

- **Executives / sponsors:** 01 → 04 → 05
- **Platform engineers:** 01 → 02 → 03 → 05
- **Application teams:** 01 → 02 → 05
- **Migration teams:** 01 → 03 → 04 → 05

## Official References

- [GitHub Actions documentation](https://docs.github.com/actions)
- [Workflow syntax for GitHub Actions](https://docs.github.com/actions/using-workflows/workflow-syntax-for-github-actions)
- [Secure use reference](https://docs.github.com/actions/security-guides/security-hardening-for-github-actions)
- [Managing larger runners](https://docs.github.com/actions/using-github-hosted-runners/about-larger-runners)
- [GitHub Actions Importer](https://docs.github.com/actions/migrating-to-github-actions/automated-migrations/about-github-actions-importer)
