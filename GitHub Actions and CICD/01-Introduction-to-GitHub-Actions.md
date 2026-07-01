# 01 - Introduction to GitHub Actions

GitHub Actions is GitHub's native automation and orchestration platform for software delivery and operational workflows. It allows teams to respond to repository events, execute CI/CD pipelines, enforce governance, and integrate external systems without leaving the GitHub platform.

> Official docs: <https://docs.github.com/actions>

## Why Teams Adopt GitHub Actions

- **Native to GitHub:** Source, pull requests, checks, packages, releases, environments, and security features are tightly integrated.
- **Event-driven:** Automations can trigger from pushes, pull requests, schedules, labels, releases, deployments, discussions, and manual dispatches.
- **Flexible compute:** Use GitHub-hosted runners, self-hosted runners, larger runners, or Kubernetes-backed scale sets.
- **Composable:** Build from marketplace actions, reusable workflows, and composite actions.
- **Governable:** Fine-grained permissions, environment protections, required workflows, OIDC federation, and auditability support enterprise use.

## Core Concepts

| Concept | What it is | Why it matters |
|---|---|---|
| **Workflow** | A YAML file in `.github/workflows/` that defines automation | Primary unit of CI/CD logic |
| **Event / trigger** | The activity that starts a workflow | Controls when automation runs |
| **Job** | A group of steps executed on the same runner | Enables fan-out, fan-in, and isolation |
| **Step** | An individual command or action invocation inside a job | Smallest executable unit |
| **Action** | A reusable automation component | Reduces duplication and standardizes logic |
| **Runner** | The machine or environment that executes jobs | Determines OS, tooling, network reach, and scaling |
| **Artifact** | File output persisted from a workflow | Used for build outputs, reports, and handoff |
| **Environment** | A deployment target with protections | Adds approvals, secrets, and gates |

## Execution Model

At a high level, GitHub Actions works like this:

1. A configured event occurs, such as a push or pull request.
2. GitHub evaluates workflows whose `on:` clause matches the event.
3. Each selected workflow creates one or more jobs.
4. Jobs are assigned to compatible runners based on `runs-on`.
5. Steps execute in order within each job.
6. Status, logs, annotations, artifacts, and outputs are captured in GitHub.

## YAML Syntax Basics

A workflow file is standard YAML with GitHub Actions-specific keys.

### Minimal structure

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Run a shell command
        run: echo "Hello from GitHub Actions"
```

### Common top-level keys

| Key | Purpose |
|---|---|
| `name` | Human-readable workflow name |
| `on` | Events that trigger the workflow |
| `env` | Shared environment variables |
| `permissions` | Token scopes granted to `GITHUB_TOKEN` |
| `concurrency` | Controls parallelism and cancellation behavior |
| `jobs` | Collection of job definitions |

### Common job-level keys

| Key | Purpose |
|---|---|
| `runs-on` | Runner label or group selection |
| `needs` | Declares job dependencies |
| `if` | Conditional execution |
| `strategy.matrix` | Executes the same job across variable combinations |
| `permissions` | Overrides token scopes for a specific job |
| `services` | Sidecar containers such as databases |
| `environment` | Associates the job to a deployment environment |
| `outputs` | Exposes values to downstream jobs |

## Key Triggers

| Trigger | Example use case |
|---|---|
| `push` | Run CI for direct commits and merges |
| `pull_request` | Validate proposed changes before merge |
| `workflow_dispatch` | Manually start release, hotfix, or backfill jobs |
| `schedule` | Nightly scans, dependency refresh, or housekeeping |
| `release` | Publish packages or deployment manifests |
| `workflow_call` | Enable reusable workflows |
| `repository_dispatch` | Trigger workflows from external systems |

## Simple Example Workflow

```yaml
name: Node CI

on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [18, 20]

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

## Comparing GitHub Actions to Other CI/CD Platforms

| Capability | GitHub Actions | Jenkins | Azure DevOps Pipelines | GitLab CI/CD |
|---|---|---|---|---|
| Hosting model | SaaS-native with optional self-hosted runners | Self-managed first, plugin-heavy | SaaS + Microsoft ecosystem integration | SaaS or self-managed GitLab |
| Pipeline definition | YAML workflows | Jenkinsfile / UI jobs | YAML pipelines | `.gitlab-ci.yml` |
| Ecosystem model | Actions marketplace + reusable workflows | Plugin ecosystem | Tasks/extensions | Templates/includes |
| Governance | GitHub org/repo controls, environments, required workflows | Depends on Jenkins architecture | Strong enterprise controls | Strong within GitLab platform |
| Operational overhead | Low for GitHub-hosted | High for controller, agents, plugins | Moderate | Moderate |
| Best fit | GitHub-centric developer platforms | Highly customized legacy estates | Microsoft-centric delivery | GitLab-centric end-to-end DevOps |

### Migration summary by platform

- **From Jenkins:** Expect reduced operational overhead and fewer plugin dependencies, but plan to re-express bespoke shared-library logic.
- **From Azure DevOps:** Variable groups, environments, and approvals map well, but terminology and marketplace tasks differ.
- **From GitLab CI:** Jobs/stages map conceptually, though Actions uses job dependency graphs instead of stage-only sequencing.
- **From CircleCI:** Parallelism and orbs map to matrices, reusable workflows, and composite actions.

## Best Practices for Beginners

- Keep workflows in source control and review them like application code.
- Start with CI validation before adding deployments.
- Set explicit `permissions` instead of relying on broad defaults.
- Prefer official or verified actions where possible.
- Use matrices for supported language/runtime versions.
- Break repeated logic into reusable workflows or composite actions.
- Add `workflow_dispatch` for safe manual operations.

## Adoption Checklist

- [ ] Standardize required CI checks for pull requests.
- [ ] Define runner strategy: GitHub-hosted, self-hosted, or hybrid.
- [ ] Establish action trust policy and SHA pinning standards.
- [ ] Create starter workflows for common stacks.
- [ ] Train teams on logs, artifacts, and rerun behavior.
- [ ] Document deployment approvals and environment ownership.

## Reference Links

- [Understanding GitHub Actions](https://docs.github.com/actions/learn-github-actions/understanding-github-actions)
- [Workflow syntax](https://docs.github.com/actions/using-workflows/workflow-syntax-for-github-actions)
- [Events that trigger workflows](https://docs.github.com/actions/using-workflows/events-that-trigger-workflows)
- [Choosing runners](https://docs.github.com/actions/using-github-hosted-runners/about-github-hosted-runners)
