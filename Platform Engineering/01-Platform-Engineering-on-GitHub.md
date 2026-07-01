# 01 - Platform Engineering on GitHub

Platform engineering is the practice of building and operating internal products that make software delivery safer, faster, and easier for development teams. Instead of asking every team to solve CI/CD, security, repository governance, secrets management, and service onboarding from scratch, a platform team provides reusable capabilities and a curated experience.

GitHub is a strong foundation for that platform because it combines source control, collaboration, automation, policy enforcement, documentation, and developer workflows in one place. For many organizations, GitHub becomes the control plane for the software delivery lifecycle and a lightweight Internal Developer Platform (IDP).

## Why Platform Engineering Matters

| Challenge | What developers feel | Platform engineering response | GitHub capability |
|---|---|---|---|
| Tool sprawl | Too many systems and inconsistent workflows | Consolidate common tasks behind standard paths | Repositories, Actions, Issues, Projects, Codespaces |
| Compliance overhead | Manual approvals and audit preparation | Shift controls into templates and policy | Rulesets, branch protection, audit log, required workflows |
| Slow onboarding | New teams reinvent scaffolding | Provide ready-to-use starting points | Template repositories, starter workflows, docs |
| Operational inconsistency | Services look and behave differently | Standardize delivery contracts | Actions, environments, reusable workflows, CODEOWNERS |
| Cognitive overload | Engineers must understand every platform detail | Hide complexity behind opinionated defaults | Golden paths, automation, portal experiences |

### Key outcomes

- Reduce time-to-first-commit for new services and teams.
- Increase consistency across repositories and delivery pipelines.
- Improve developer satisfaction by removing repetitive setup work.
- Encode security and compliance into the default path.
- Create fast feedback loops for both platform teams and product teams.

## GitHub as an Internal Developer Platform

An IDP is not a single product screen. It is a collection of capabilities that provide self-service with guardrails. On GitHub, those capabilities often include:

| IDP capability | GitHub building blocks | Example use case |
|---|---|---|
| Source scaffolding | Template repos, `.github` defaults, starter workflows | Create a new microservice repo with baseline CI, docs, and CODEOWNERS |
| Delivery automation | GitHub Actions, reusable workflows, environments | Standard build-test-deploy pipeline reused across all services |
| Governance | Rulesets, required workflows, branch protection, custom properties | Enforce signed commits and SAST scans on regulated repositories |
| Documentation | README-driven docs, Wikis, Pages | Publish platform standards and service runbooks |
| Service ownership | CODEOWNERS, teams, labels, repo metadata | Identify service owner, business domain, and support tier |
| Self-service operations | GitHub Apps, Actions workflows, issue forms | Request a new repository, team access, or environment promotion |

### Example IDP architecture on GitHub

```text
Developer Request
   |
   v
Issue Form / Portal / Backstage Plugin
   |
   v
GitHub App or Workflow Dispatch
   |
   +--> Create repository from template
   +--> Apply custom properties and rulesets
   +--> Add CODEOWNERS, teams, environments
   +--> Open onboarding issue or PR
   |
   v
Golden path repository ready for use
```

## The Golden Path Concept

A **golden path** is the recommended way to build and operate a class of software in your organization. It is not the only path, but it is the path that is most supported, best documented, and easiest to adopt.

A good golden path usually includes:

1. **A starting point** - template repository or scaffold.
2. **A delivery path** - reusable CI/CD workflows.
3. **Guardrails** - rulesets, required checks, and security defaults.
4. **A support model** - docs, ownership, and escalation guidance.
5. **A feedback loop** - telemetry, adoption metrics, and backlog intake.

### Golden path example

| Layer | Standard for a backend microservice |
|---|---|
| Repository scaffold | Service template with Dockerfile, README, API spec, tests |
| CI | Reusable workflow for lint, unit test, dependency review, code scanning |
| CD | Environment-based deploy workflow with approvals for production |
| Security | Secret scanning, Dependabot, code scanning, artifact provenance |
| Ownership | CODEOWNERS, service tier, support team, on-call metadata |
| Docs | Runbook, architecture summary, local setup, operational checklist |

## Reducing Cognitive Load for Developers

One of the primary goals of platform engineering is to reduce **extraneous cognitive load** so teams can focus on domain problems instead of delivery plumbing.

### Practical ways to reduce load on GitHub

| Pattern | How it helps |
|---|---|
| Opinionated templates | Removes blank-page setup decisions |
| Reusable workflows | Avoids copying pipeline logic into every repo |
| Environment protection rules | Makes release expectations clear and consistent |
| Repository metadata | Helps teams discover ownership and lifecycle quickly |
| README conventions | Reduces time spent searching for setup and operational context |
| Automated issue/PR generation | Converts process steps into guided workflows |

### Example: replace manual setup with a template

```yaml
name: service-ci
on:
  pull_request:
  push:
    branches: [main]

jobs:
  verify:
    uses: your-org/.github/.github/workflows/reusable-service-ci.yml@main
    with:
      language: node
      run-integration-tests: false
```

Instead of asking each team to design a CI workflow, the platform team publishes a supported default and teams opt into well-defined parameters.

## Team Topologies Alignment

Platform engineering aligns closely with the ideas in *Team Topologies*. In that model, the platform team exists to provide internal services that reduce cognitive load for stream-aligned teams.

| Team Topologies concept | Platform engineering interpretation |
|---|---|
| Stream-aligned team | Product or service team delivering business outcomes |
| Platform team | Team building reusable capabilities, templates, and self-service tooling |
| Enabling team | Team helping adoption of new workflows, security practices, or developer tools |
| Complicated subsystem team | Specialist team owning deep technical domains like data platforms or ML infrastructure |

### Interaction modes

- **X-as-a-Service:** Platform team provides reusable templates, workflows, and automation.
- **Collaboration:** Platform team pairs with early adopters to shape the golden path.
- **Facilitating:** Platform team enables teams through office hours, docs, and migration guides.

## Implementation Checklist

| Area | Questions to answer |
|---|---|
| Personas | Which developer groups will the platform serve first? |
| Archetypes | Which service types need distinct golden paths? |
| Controls | Which security and compliance rules must be defaulted? |
| Self-service | What should teams be able to request without tickets? |
| Support | How do users discover docs, owners, and escalation paths? |
| Metrics | How will you measure adoption, lead time, and experience? |

## Example 90-Day Platform Roadmap

| Phase | Focus | Example deliverables |
|---|---|---|
| Days 0-30 | Discovery and baseline | Inventory repo patterns, define personas, choose first golden path |
| Days 31-60 | Standardization | Publish templates, reusable workflows, rulesets, and docs |
| Days 61-90 | Self-service and measurement | Automate provisioning, launch portal views, track adoption metrics |

## Reference Links

- [GitHub Actions documentation](https://docs.github.com/actions)
- [Managing repository rulesets](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [Creating a template repository](https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository)
- [GitHub Pages documentation](https://docs.github.com/pages)
- [Team Topologies](https://teamtopologies.com/)
