# 02 - Starter Workflows and Templates

Standardization is one of the fastest ways to scale GitHub Actions successfully. GitHub provides several reuse mechanisms that help teams bootstrap quickly while still letting platform teams enforce good defaults.

> Official docs: <https://docs.github.com/actions/using-workflows>

## Reuse Options at a Glance

| Mechanism | Best for | Shared where | Inputs | Secrets | Notes |
|---|---|---|---|---|---|
| **Starter workflows** | Fast project onboarding | Repo creation / authoring time | No | No | Great starting point for common stacks |
| **Organization workflow templates** | Opinionated enterprise defaults | Across org repositories | Limited via template variables | No | Ideal for governed bootstrap |
| **Reusable workflows (`workflow_call`)** | End-to-end job orchestration reuse | Across repositories | Yes | Yes | Best for standard CI/CD flows |
| **Composite actions** | Reusing a set of steps | Any workflow that can reference the action | Yes | Indirect | Good for procedural step bundles |

## Starter Workflows

Starter workflows are curated examples GitHub exposes in the workflow creation experience. Teams commonly use them to generate first-pass CI pipelines for popular languages and frameworks.

### When to use starter workflows

- Rapid onboarding for new repositories
- Proof-of-concept pipelines
- Training and enablement
- A baseline to customize into an internal standard

### What to review before production use

- `permissions` scope
- Caching strategy
- Artifact retention
- Test and build commands
- Branch filters
- Deployment protections

## Organization-Level Workflow Templates

Organization workflow templates let platform teams publish pre-approved workflow scaffolds that appear to repository maintainers.

### Good uses for templates

- Standard CI for approved language stacks
- Repository bootstrap for regulated environments
- Standard security scanning workflows
- Release automation with required checks already wired in

### Template design checklist

- [ ] Include minimal required `permissions`
- [ ] Reference approved actions only
- [ ] Add comments for team-owned customization points
- [ ] Include branch and path filters where appropriate
- [ ] Align job names with required status checks
- [ ] Link to internal engineering standards

## Reusable Workflows with `workflow_call`

Reusable workflows package one or more jobs into a centrally maintained workflow that other workflows can call.

### Why reusable workflows matter

- Enforces consistent CI/CD behavior across repositories
- Simplifies updates to shared release logic
- Enables platform teams to embed policy, scanning, and provenance controls
- Reduces copy/paste drift

### Example: reusable Node CI workflow

```yaml
name: reusable-node-ci

on:
  workflow_call:
    inputs:
      node-version:
        required: false
        type: string
        default: '20'
    secrets:
      npm_token:
        required: false

jobs:
  ci:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
          cache: npm
      - run: npm ci
      - run: npm test
```

### Example: calling the reusable workflow

```yaml
name: app-ci

on:
  pull_request:
  push:
    branches: [main]

jobs:
  ci:
    uses: octo-org/platform-automation/.github/workflows/reusable-node-ci.yml@main
    with:
      node-version: '20'
    secrets:
      npm_token: ${{ secrets.NPM_TOKEN }}
```

## Composite Actions

Composite actions bundle a sequence of steps behind a single `uses:` reference. They are a strong fit when you want to standardize repeated procedural logic but do not need a full multi-job orchestration layer.

### When to prefer a composite action

- Bootstrapping toolchains
- Standard code quality steps
- Packaging version metadata
- Common setup shared by multiple workflows

### Example: composite action metadata file

```yaml
name: setup-node-and-install
description: Install Node dependencies consistently
inputs:
  node-version:
    required: true
runs:
  using: composite
  steps:
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}
        cache: npm
    - shell: bash
      run: npm ci
```

## Common Language Examples

### Node.js

```yaml
name: node-ci
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run lint --if-present
      - run: npm test
```

### Python

```yaml
name: python-ci
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.10', '3.11', '3.12']
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
          cache: pip
      - run: pip install -r requirements.txt
      - run: pytest
```

### Java

```yaml
name: java-ci
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '21'
          cache: maven
      - run: mvn --batch-mode verify
```

### .NET

```yaml
name: dotnet-ci
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.0.x'
      - run: dotnet restore
      - run: dotnet build --no-restore --configuration Release
      - run: dotnet test --no-build --configuration Release
```

## Design Guidance: Which Mechanism Should You Choose?

| If you need... | Prefer... |
|---|---|
| Fast onboarding with minimal governance | Starter workflows |
| Approved repository bootstrap patterns | Org workflow templates |
| Centralized CI/CD orchestration | Reusable workflows |
| Shared procedural step logic | Composite actions |

## Enterprise Implementation Pattern

1. Publish **organization workflow templates** for new repositories.
2. Back those templates with **reusable workflows** owned by the platform team.
3. Use **composite actions** for common setup or validation logic.
4. Apply **required workflows** or branch protections for mandatory controls.
5. Version and document breaking changes to shared workflow contracts.

## Common Pitfalls

| Pitfall | Why it happens | How to avoid it |
|---|---|---|
| Overusing copy/paste workflows | Teams start from examples and never centralize | Promote reusable workflows early |
| Hidden breaking changes | Shared workflows change without versioning | Use tags/releases and change management |
| Excessive secret sprawl | Every repo defines its own credentials | Centralize with OIDC and environment secrets |
| Unclear ownership | Nobody maintains shared automation | Assign product ownership to platform engineering |

## Rollout Checklist

- [ ] Identify top 3–5 language stacks in the organization.
- [ ] Publish one supported template per stack.
- [ ] Define support boundaries for custom modifications.
- [ ] Establish versioning for reusable workflows.
- [ ] Test templates in sample repositories before broad release.
- [ ] Document escalation paths for failing shared workflows.

## Reference Links

- [Using starter workflows](https://docs.github.com/actions/using-workflows/using-starter-workflows)
- [Creating workflow templates for your organization](https://docs.github.com/actions/learn-github-actions/creating-starter-workflows-for-your-organization)
- [Reusing workflows](https://docs.github.com/actions/using-workflows/reusing-workflows)
- [Creating a composite action](https://docs.github.com/actions/creating-actions/creating-a-composite-action)
