# 05 - Internal Developer Portal

An internal developer portal helps teams discover the golden path, understand service ownership, request platform capabilities, and access documentation without needing tribal knowledge. On GitHub, a portal can begin with README-driven documentation and evolve into a richer catalog and self-service experience.

## What a Developer Portal Should Do

| Goal | Example portal capability |
|---|---|
| Discoverability | Find templates, services, standards, and owners |
| Self-service | Request new repos, environments, access, or scaffolding |
| Documentation | Surface setup guides, runbooks, and architecture notes |
| Governance | Show required controls, exceptions, and service metadata |
| Measurement | Track adoption, developer satisfaction, and golden path usage |

## Building a Portal with GitHub-Native Components

| Component | GitHub feature | Portal use |
|---|---|---|
| Landing page | README in a central repo | Start page for standards and quick links |
| Documentation site | GitHub Pages | Publish curated docs, standards, and playbooks |
| Collaboration | Issues and Discussions | Intake, FAQs, and feedback loops |
| Knowledge base | Wiki or docs repo | Deeper procedural content |
| Automation entry point | Actions + issue forms | Self-service requests and approvals |

### README-driven portal pattern

A simple but effective first step is a central `platform-engineering` repository with:

- A README that acts as the home page.
- Linked docs for standards, templates, and onboarding.
- Issue forms for new repository, exception, or access requests.
- A GitHub Pages site for browsable documentation.

## Backstage.io Integration

Many organizations use [Backstage](https://backstage.io/) as the primary internal developer portal and GitHub as the system of record for repository metadata, documentation, and workflows.

| Backstage concept | GitHub mapping |
|---|---|
| Software catalog entity | Repository plus `catalog-info.yaml` |
| TechDocs | Markdown docs in repo rendered in portal |
| Scaffolder template | GitHub template repo or workflow-backed automation |
| Ownership | GitHub teams, CODEOWNERS, repo metadata |
| Lifecycle and status | GitHub releases, environments, workflow runs |

### Example `catalog-info.yaml`

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: payments-api
  description: Internal API for payment orchestration
  tags:
    - nodejs
    - tier1
  annotations:
    github.com/project-slug: your-org/payments-api
spec:
  type: service
  lifecycle: production
  owner: team-platform-payments
```

## Service Catalog Design

A useful service catalog goes beyond repository links.

| Field | Why it matters |
|---|---|
| Service name | Primary identifier |
| Owning team | Support and accountability |
| System/domain | Business alignment |
| Lifecycle | Experimental, production, deprecated |
| Tier/criticality | Operational and governance expectations |
| Runbook link | Faster incident response |
| API/docs link | Easier integration |
| Repo link | Source of truth for code and changes |

## Developer Self-Service Patterns

| Self-service need | GitHub implementation pattern |
|---|---|
| Create a new service | Issue form or Backstage Scaffolder invoking repo template automation |
| Request environment access | Issue form + approval workflow |
| Ask for policy exception | Standard exception issue template with review labels |
| Register ownership | PR to metadata file or automated form-backed workflow |
| Start from a template | Template repository chooser or portal button |

### Example issue form flow

```text
Developer opens "New Service Request"
   -> Provides service name, language, owner, data classification
   -> Workflow validates inputs
   -> Platform automation creates repo from template
   -> Rulesets and metadata applied
   -> Catalog entry created or PR opened
```

## Measuring Platform Adoption

A portal should help you answer whether the platform is working.

| Metric | What it indicates |
|---|---|
| Template adoption rate | How often teams choose the golden path |
| Time to provision repo | Efficiency of self-service flow |
| Ruleset compliance rate | Governance coverage |
| Documentation usage | Discoverability and usefulness |
| Exception volume | Whether defaults are too rigid or incomplete |
| Developer satisfaction | Experience quality |

### Sample scorecard

| Metric | Target | Data source |
|---|---|---|
| New repos created from approved templates | >80% | Repo creation workflow logs |
| Repos with catalog metadata | >90% | Scheduled metadata scan |
| Repos passing required workflows | >95% | Actions / check results |
| Mean provisioning time | <15 minutes | Automation timestamps |

## Maturity Model

| Stage | Characteristics |
|---|---|
| Level 1 - Documented | Standards live in markdown and README pages |
| Level 2 - Discoverable | Pages/wiki/catalog make standards easy to find |
| Level 3 - Self-service | Repo creation, access, and policy requests are automated |
| Level 4 - Integrated | Backstage or equivalent aggregates GitHub data and workflows |
| Level 5 - Measured | Adoption, satisfaction, and delivery metrics guide investment |

## Implementation Tips

| Tip | Reason |
|---|---|
| Start with markdown and metadata | Lowest-friction path to value |
| Use GitHub as source of truth | Avoid duplicate ownership records |
| Keep requests standardized | Easier automation and reporting |
| Publish support expectations | Developers know where to go for help |
| Review portal analytics quarterly | Portal relevance degrades without active curation |

## Reference Links

- [GitHub Pages documentation](https://docs.github.com/pages)
- [GitHub Wikis](https://docs.github.com/communities/documenting-your-project-with-wikis/about-wikis)
- [GitHub Issues forms](https://docs.github.com/issues/building-community/using-issue-forms-to-encourage-useful-issues)
- [Backstage](https://backstage.io/)
- [TechDocs in Backstage](https://backstage.io/docs/features/techdocs/)
