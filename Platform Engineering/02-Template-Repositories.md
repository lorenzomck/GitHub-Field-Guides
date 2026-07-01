# 02 - Template Repositories

Template repositories are one of the fastest ways to create consistent golden paths. A template turns proven repository structure, automation, and governance into a reusable starting point that teams can adopt in minutes instead of days.

## Why Template Repositories Matter

| Benefit | Description | Outcome |
|---|---|---|
| Faster onboarding | New repos start from a known baseline | Teams ship earlier |
| Better consistency | Standard layouts and automation across services | Easier support and auditing |
| Built-in controls | Security checks and ownership defaults are present from day one | Lower compliance risk |
| Easier upgrades | Shared patterns can be versioned and rolled out systematically | Less drift |

## What to Include in a Template Repository

A strong template should contain the minimum viable set of files needed to support delivery, operations, and governance.

| Category | Recommended contents |
|---|---|
| Source structure | `src/`, `test/`, `docs/`, `.github/` |
| CI/CD | Pull request checks, build, publish, deployment workflows |
| Quality | Linting, formatting, unit test examples, coverage reporting |
| Security | Code scanning, secret scanning guidance, Dependabot, dependency review |
| Ownership | `CODEOWNERS`, team mappings, labels, issue templates |
| Documentation | README, architecture notes, runbook, contribution guide |
| Configuration | `.editorconfig`, `.gitignore`, license, devcontainer/Codespaces setup |

### Example template layout

```text
service-template/
├── .github/
│   ├── CODEOWNERS
│   ├── dependabot.yml
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── release.yml
│   │   └── scorecard.yml
│   └── ISSUE_TEMPLATE/
├── docs/
│   ├── architecture.md
│   └── runbook.md
├── src/
├── test/
├── Dockerfile
├── README.md
├── catalog-info.yaml
└── package.json
```

## Org-Level Templates and `.github` Defaults

GitHub supports organization-level defaults through a dedicated `.github` repository. This is a useful companion to templates.

| Pattern | Use case |
|---|---|
| `.github` repo | Shared issue templates, pull request templates, reusable workflows, community health files |
| Template repo | Full project archetype with code, configuration, docs, and automation |
| Reusable workflow | Shared workflow logic invoked from many repositories |

### Example reusable workflow call from a template

```yaml
name: ci
on:
  pull_request:
  push:
    branches: [main]

jobs:
  verify:
    uses: your-org/.github/.github/workflows/reusable-ci.yml@main
    with:
      runtime: node20
      enable-codeql: true
```

## Archetype Templates

Most organizations benefit from multiple templates for different delivery patterns rather than one universal scaffold.

| Archetype | Use when | Typical inclusions |
|---|---|---|
| Microservice | Deployable API or background service | Dockerfile, deploy workflow, API docs, runbook, observability config |
| Library | Shared package or SDK | Semantic release, package publishing, API compatibility tests |
| Frontend app | Web UI or SPA | UI test workflow, preview deployment, accessibility checks |
| Infrastructure-as-Code | Terraform or platform modules | Policy checks, module docs, examples, release tagging |
| Data/ML service | Pipelines or model services | Notebook hygiene, artifact tracking, scheduled workflows |

### Comparison table for three common archetypes

| Capability | Microservice | Library | Frontend app |
|---|---|---|---|
| Container build | Yes | Optional | Optional |
| Deployment environments | Yes | No | Often preview + prod |
| Package publishing | Rare | Primary | Rare |
| API contract tests | Common | Rare | Sometimes |
| Visual regression tests | Rare | Rare | Common |

## Managing Template Lifecycle

Templates should be treated like products.

| Lifecycle stage | Platform team activity |
|---|---|
| Design | Work with early adopters to identify common patterns |
| Publish | Mark repo as template, document supported use cases |
| Adopt | Make creation self-service and provide migration guides |
| Evolve | Version key changes, deprecate outdated patterns, collect feedback |
| Measure | Track usage, drift, and exception requests |

### Suggested versioning approach

| Approach | When to use | Trade-off |
|---|---|---|
| Branch/tag releases | Stable, controlled template revisions | Requires consumers to select version |
| Changelog in repo | Small teams, lower complexity | Easier, but less rigorous |
| Release notes + migration guides | High adoption templates | Best for governed changes |

## Automating Repository Setup with Actions

A template becomes more powerful when combined with post-creation automation. That setup can populate repository metadata, configure environments, and open onboarding tasks.

### Example setup workflow

```yaml
name: bootstrap-repository
on:
  workflow_dispatch:
    inputs:
      service_name:
        required: true
        type: string
      owning_team:
        required: true
        type: string

jobs:
  bootstrap:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      actions: write
    steps:
      - uses: actions/checkout@v4
      - name: Replace placeholders
        run: |
          sed -i.bak "s/__SERVICE_NAME__/${{ inputs.service_name }}/g" README.md catalog-info.yaml
          sed -i.bak "s/__TEAM__/${{ inputs.owning_team }}/g" .github/CODEOWNERS
      - name: Commit updates
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add .
          git commit -m "Bootstrap repository" || exit 0
          git push
```

## Template Governance Checklist

| Control | Why it matters |
|---|---|
| Template owner assigned | Someone must maintain the paved road |
| Supported use cases documented | Teams know when to adopt or request exceptions |
| Security defaults enabled | Prevents insecure repositories from being created |
| Upgrade guidance published | Makes it easier to evolve adopters over time |
| Health metrics reviewed | Usage and drift become visible |

## Example Service README Sections

When you create templates, standardize the README sections teams should fill in:

1. Purpose and business capability
2. Local development setup
3. Test strategy
4. Deployment process
5. Observability and alerting
6. Ownership and escalation
7. Security considerations

## Reference Links

- [Creating a template repository](https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository)
- [About reusable workflows](https://docs.github.com/actions/using-workflows/reusing-workflows)
- [Creating starter workflows for your organization](https://docs.github.com/actions/using-workflows/creating-starter-workflows-for-your-organization)
- [About community health files](https://docs.github.com/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
