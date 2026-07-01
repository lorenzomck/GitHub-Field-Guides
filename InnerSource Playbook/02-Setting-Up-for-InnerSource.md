# 02 — Setting Up for InnerSource

Making a repository open to internal contribution starts with **discoverability** and **clarity**. If teams cannot find a repo, understand what it does, or see how to contribute safely, InnerSource stalls before the first pull request.

## Repository discoverability fundamentals

### Minimum discoverability checklist

| Element | What to include | Why it matters |
| --- | --- | --- |
| Repository name | Clear, descriptive, product-agnostic if possible | Helps teams find the right asset quickly |
| Description | What the repo is, who it serves, and scope boundaries | Sets expectations before opening the README |
| Topics | Domain, language, platform, business capability | Improves GitHub search and filtering |
| README | Purpose, setup, usage, ownership, support, contribution path | Converts curiosity into adoption |
| Visibility metadata | Owners, code owners, support channel, lifecycle status | Signals trust and support level |

### Suggested repository topics

| Repo type | Useful topics |
| --- | --- |
| Shared frontend library | `innersource`, `frontend`, `design-system`, `react`, `ui-components` |
| Platform tooling | `innersource`, `platform`, `ci-cd`, `developer-experience`, `automation` |
| API standards | `innersource`, `api`, `standards`, `governance`, `schemas` |
| Data utilities | `innersource`, `data-platform`, `etl`, `python`, `observability` |

## README structure for InnerSource repos

A strong README lowers the cost of first contact.

### Recommended README outline

1. **What this repo is**
2. **Who should use it**
3. **Architecture or component overview**
4. **Quick start**
5. **Common workflows**
6. **How to request changes or contribute**
7. **Ownership and support expectations**
8. **Status** (active, experimental, deprecated, etc.)

### Example README ownership block

```md
## Ownership

- Maintainer team: `@acme-platform/devex`
- Slack: `#devex-support`
- Review SLA: 3 business days
- Escalation: engineering manager for Developer Experience
```

## CONTRIBUTING.md templates

A `CONTRIBUTING.md` file should explain how outside contributors can succeed without guesswork.

### Recommended sections

| Section | Purpose |
| --- | --- |
| Scope | What kinds of changes are welcome |
| Setup | Local development and test steps |
| Workflow | Branch naming, PR expectations, commit conventions |
| Quality gates | Tests, linting, docs updates, security review |
| Review process | Who reviews, expected timelines, how feedback works |
| Communication | Where to ask questions or discuss larger changes |

### Lightweight template example

```md
# Contributing

## Before you start
Open an issue for large changes so maintainers can confirm scope and design.

## Development flow
1. Create a branch from `main`.
2. Make the smallest useful change.
3. Run tests and linters locally.
4. Update docs if user-facing behavior changes.
5. Open a pull request using the template.

## Review expectations
Maintainers aim to respond within 3 business days.
```

## Issue templates and intake hygiene

Issue templates reduce ambiguity and help maintainers triage efficiently.

### Common template types

| Template | Use case | Key fields |
| --- | --- | --- |
| Bug report | Something is broken | Environment, steps, expected vs actual behavior |
| Feature request | New capability | Problem statement, proposed outcome, alternatives considered |
| Contribution proposal | Cross-team enhancement | Consumer team, use case, design notes, rollout impact |
| Documentation improvement | Docs are unclear or missing | Current gap, suggested change, affected audience |

### Example labels for InnerSource repos

| Label | Meaning |
| --- | --- |
| `good first issue` | Safe entry point for new contributors |
| `help wanted` | Maintainers welcome outside support |
| `needs-design` | Discuss approach before implementation |
| `cross-team-impact` | Change affects multiple consumers |
| `maintainer-needed` | Waiting on repo owner response |

## Creating effective “good first issues”

A good first issue is not merely small; it is **bounded, documented, and safe**.

| Attribute | Good example | Poor example |
| --- | --- | --- |
| Scope | Add missing validation to one endpoint | Improve developer experience |
| Context | Links to code, screenshots, and affected tests | No pointers provided |
| Outcome | Clear definition of done | "Refactor this" |
| Risk | Low blast radius | Deep architectural dependency |

### Good first issue template example

```md
## Goal
Add a missing timeout configuration option to the internal SDK client.

## Where to start
- `src/client.ts`
- `src/config.ts`
- `test/client.test.ts`

## Definition of done
- New config option is documented
- Tests cover default and override behavior
- No breaking changes to existing callers
```

## Internal visibility settings

Not every repo can be visible to every employee, but InnerSource works best when restrictions are **intentional and minimal**.

### Visibility guidance

| Scenario | Recommended setting |
| --- | --- |
| General reusable engineering asset | Broad internal visibility |
| Sensitive business logic but safe collaboration | Internal visibility with team-based write permissions |
| Regulated or confidential data handling | Limited visibility with documented request path |
| Security-sensitive infrastructure | Restricted visibility plus public-facing catalog entry if possible |

### Practical rule

If a repository must be restricted, still consider publishing a **directory entry** with:

- purpose
- owning team
- support path
- request process for access

That preserves discoverability even when source access is constrained.

## GitHub search and explore features

GitHub features can make InnerSource participation much easier when consistently used.

### High-value capabilities

| Feature | InnerSource use |
| --- | --- |
| Repository search | Find relevant repos by name, topic, language, and README content |
| Code search | Discover APIs, patterns, and examples across internal repos |
| Topics | Filter reusable assets by domain or platform |
| Saved searches | Help teams monitor ownership areas or contribution opportunities |
| Discussions / Issues | Centralize design questions and adoption feedback |
| Insights | Track participation, PR throughput, and contributor trends |

### Example search queries

| Goal | Search example |
| --- | --- |
| Find reusable Node libraries | `topic:innersource topic:nodejs` |
| Find API client examples | `"createClient(" org:your-org language:typescript` |
| Find onboarding-friendly issues | `label:"good first issue" org:your-org is:open` |
| Find a team’s shared repos | `org:your-org topic:innersource team-name` |

## Recommended rollout plan

| Phase | Actions | Output |
| --- | --- | --- |
| Pilot | Select 1–3 repos with active maintainers and multiple consumers | Initial templates and ownership docs |
| Standardize | Reuse shared README, issue, and contribution patterns | Consistent contributor experience |
| Promote | Publish catalogs, topics, and onboarding links | Increased discovery |
| Optimize | Review search behavior and contribution friction | Better conversion from users to contributors |

## Example repository scorecard

| Criterion | Yes/No | Notes |
| --- | --- | --- |
| Clear description and topics |  |  |
| README explains who it is for |  |  |
| CONTRIBUTING.md exists |  |  |
| Issue templates support common requests |  |  |
| Good first issues are labeled |  |  |
| Owners and support channel are documented |  |  |
| Review SLA is visible |  |  |
| Repo visibility is justified and documented |  |  |

## References

- [GitHub Docs: Adding a repository description and website](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics)
- [GitHub Docs: About issue and pull request templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests)
- [GitHub Docs: Searching on GitHub](https://docs.github.com/en/search-github)
- [InnerSource Commons Patterns](https://patterns.innersourcecommons.org/)
