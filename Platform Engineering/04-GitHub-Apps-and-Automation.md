# 04 - GitHub Apps and Automation

Automation is how platform engineering turns standards into self-service experiences. GitHub Apps, Actions, and webhooks let platform teams encode workflows such as repository provisioning, ownership assignment, compliance checks, and lifecycle management.

## Why GitHub Apps Matter for Platform Teams

| Capability | GitHub App advantage |
|---|---|
| Fine-grained permissions | Apps request only the repository/org permissions they need |
| Event-driven automation | Apps react to webhook events in near real time |
| Multi-repository operation | Apps can manage standards centrally across many repos |
| Identity and auditability | Actions are attributable to the app installation |
| Safer automation | Better than long-lived personal access tokens for platform tooling |

## Common Platform Engineering Use Cases

| Use case | Trigger | Automated action |
|---|---|---|
| Repository creation | Issue form, workflow dispatch, portal request | Create repo from template, set teams, add rulesets |
| Team assignment | Repo created or metadata updated | Add admin/maintain/write teams automatically |
| Compliance checks | Pull request or scheduled scan | Validate labels, metadata, workflows, CODEOWNERS |
| Service registration | New repo or release | Update catalog, README badges, ownership index |
| Lifecycle hygiene | Schedule | Archive stale repos, alert on missing owners |

## Building a Custom GitHub App

A custom GitHub App usually includes:

1. App registration and permission model.
2. Webhook receiver.
3. Business logic for events or API-driven actions.
4. Secure secret handling and deployment.
5. Observability and retry handling.

### Minimal webhook handler example with Probot

```javascript
module.exports = (app) => {
  app.on('repository.created', async (context) => {
    const owner = context.payload.repository.owner.login;
    const repo = context.payload.repository.name;

    await context.octokit.repos.createOrUpdateFileContents({
      owner,
      repo,
      path: '.github/ISSUE_TEMPLATE/platform-request.yml',
      message: 'Bootstrap platform request template',
      content: Buffer.from('name: Platform Request').toString('base64')
    });
  });
};
```

## Probot Framework

[Probot](https://probot.github.io/) is a useful framework for building GitHub Apps quickly. It handles authentication, event routing, and local development ergonomics.

| Probot benefit | Why it helps platform teams |
|---|---|
| Event abstraction | Faster webhook development |
| Local simulator | Easier testing of automation flows |
| Built-in GitHub client | Simplifies API interactions |
| Community examples | Faster prototyping for common automation patterns |

## Webhooks and Event Patterns

| Event | Example platform response |
|---|---|
| `repository.created` | Apply labels, teams, README defaults, environments |
| `pull_request.opened` | Validate template usage, metadata, linked issue requirements |
| `push` | Run drift detection, scan for policy violations |
| `release.published` | Update catalog and deployment status |
| `installation_repositories.added` | Backfill standards to newly installed repositories |

### Event-driven repo bootstrap flow

```text
Portal request submitted
   -> GitHub Action or App validates request
   -> Repository created from template
   -> Rulesets and custom properties applied
   -> Teams and CODEOWNERS configured
   -> Welcome issue opened with next steps
```

## Automating Common Platform Tasks

| Task | GitHub-native option | App-based option |
|---|---|---|
| Create repo from template | Workflow dispatch + CLI/API | App with approval workflow |
| Assign teams | Actions + REST API | App listening to repo events |
| Enforce metadata | Required workflow | App comments/fails check if missing |
| Compliance evidence | Scheduled workflow | App writes evidence to issue/check run |
| Drift remediation | Actions cron | App periodically reconciles desired state |

### Example workflow dispatch for self-service repo creation

```yaml
name: create-service-repo
on:
  workflow_dispatch:
    inputs:
      repo_name:
        required: true
        type: string
      visibility:
        required: true
        type: choice
        options: [private, internal]

jobs:
  provision:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - name: Create repository from template
        env:
          GH_TOKEN: ${{ secrets.PLATFORM_BOT_TOKEN }}
        run: |
          gh repo create your-org/${{ inputs.repo_name }} \
            --private \
            --template your-org/service-template
```

## Marketplace Apps for Platform Engineering

Not every capability requires a custom build. Marketplace apps can accelerate platform maturity.

| App category | Platform use |
|---|---|
| Security/scanning apps | Expand policy and risk controls |
| Dependency management | Improve update hygiene and supply chain visibility |
| Policy and compliance | Validate controls and evidence |
| Backstage/GitHub integrations | Sync catalogs and developer portals |
| Incident/ops integrations | Connect releases and service ownership to operational tooling |

## Design Principles for Automation

| Principle | Practical guidance |
|---|---|
| Least privilege | Use GitHub App permissions instead of broad PATs |
| Idempotency | Re-running automation should converge, not duplicate work |
| Explainability | Open issues, PR comments, or checks that tell users what happened |
| Safe failure modes | Avoid partially configured repositories where possible |
| Auditability | Log changes and tie them to app installations or workflows |

## Build vs Buy Decision Table

| Scenario | Prefer custom app | Prefer marketplace or native feature |
|---|---|---|
| Organization-specific provisioning logic | Yes | No |
| Standard SAST or dependency management | No | Yes |
| Internal approval workflows | Yes | Sometimes |
| Simple CI enforcement | No, use rulesets/workflows | Yes |
| Deep portal integration | Often yes | Sometimes |

## Reference Links

- [GitHub Apps documentation](https://docs.github.com/apps)
- [Webhook events and payloads](https://docs.github.com/webhooks-and-events/webhooks/webhook-events-and-payloads)
- [Probot](https://probot.github.io/)
- [GitHub Marketplace](https://github.com/marketplace)
- [REST API for repositories](https://docs.github.com/rest/repos)
