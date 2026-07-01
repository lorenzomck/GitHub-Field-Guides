# 01 — What is InnerSource?

## Definition

**InnerSource** is the practice of applying open source ways of working inside an organization. Teams build and maintain code in internal repositories that are visible and open to contribution from other teams, subject to company-specific governance, security, and compliance controls.

At its core, InnerSource changes the default answer from **"that repo belongs only to that team"** to **"that team maintains it, but others can discover it, use it, and contribute to it."**

## Why organizations adopt InnerSource

InnerSource helps companies scale knowledge and software reuse without forcing every dependency into a centralized platform backlog.

### Primary benefits

| Benefit | What it means in practice | Typical outcome |
| --- | --- | --- |
| Reduced duplication | Teams extend or improve existing internal components instead of rebuilding them | Fewer overlapping services, libraries, and utilities |
| Faster innovation | Contributors can ship improvements directly to shared assets | Shorter cycle time for cross-team needs |
| Knowledge sharing | Design, code review, and documentation happen more transparently | Stronger engineering resilience and onboarding |
| Better quality | More eyes on important codebases surface bugs and edge cases earlier | More reliable shared systems |
| Stronger ownership signals | Maintainers define standards in the open | Clearer roadmap and support expectations |

## InnerSource vs. open source

InnerSource borrows the **process model** of open source, not necessarily the public distribution model.

| Dimension | InnerSource | Open source |
| --- | --- | --- |
| Audience | Internal employees and approved contractors | Anyone on the internet |
| Visibility | Internal by default | Public by default |
| Licensing | Governed by company policy | Governed by open source licenses |
| Security model | Company access controls, data classification, SSO | Public review plus project-specific controls |
| Incentives | Business goals, platform reuse, internal reputation | Community value, adoption, ecosystem growth |
| Compliance | Must fit internal legal and regulatory obligations | Must fit public distribution and license obligations |

## Core InnerSource principles

1. **Transparency by default** — repositories, docs, and backlog items are visible unless there is a valid reason to restrict them.
2. **Self-service discovery** — teams can find code, owners, and usage guidance without relying on private tribal knowledge.
3. **Contribution over ticket queues** — consumers can propose changes rather than always waiting for maintainers.
4. **Maintainer-led governance** — openness does not eliminate ownership; it clarifies it.
5. **Merit and trust** — repeated, high-quality contributions can expand influence and responsibility over time.

## Common InnerSource patterns

| Pattern | Description | Example |
| --- | --- | --- |
| Shared library model | One team maintains a reusable package; others contribute bug fixes and extensions | UI component library used by multiple product teams |
| Platform service model | Platform team owns a service and accepts improvements from consuming teams | CI templates, deployment tooling, observability SDKs |
| Reference implementation model | A repo provides a canonical pattern teams can copy or extend | Authentication starter app, service boilerplate |
| Community-owned standard | Several teams co-maintain a common asset | API guidelines, schema registry tooling |

## InnerSource Commons framework

The [InnerSource Commons](https://innersourcecommons.org/) community provides patterns, learning paths, and a shared vocabulary for building internal open collaboration. It is often the best starting point for formal programs because it offers:

- **Pattern catalog** for proven practices
- **Learning pathways** for contributors, maintainers, and leaders
- **Community stories** from organizations at different scales
- **Maturity guidance** to phase adoption realistically

### Useful InnerSource Commons references

| Resource | Why it matters |
| --- | --- |
| [Patterns](https://patterns.innersourcecommons.org/) | Playbooks for contribution, governance, communication, and adoption |
| [Learning Path](https://innersourcecommons.org/learn/) | Role-based guidance for starting and scaling programs |
| [Maturity Model](https://innersourcecommons.org/learn/path/leadership/maturity-model/) | Helps teams assess current state and next-step practices |

## InnerSource maturity model

A maturity model helps organizations avoid treating InnerSource as a binary switch.

| Maturity stage | Characteristics | Next milestone |
| --- | --- | --- |
| Ad hoc | Individual teams occasionally accept outside pull requests | Document contribution basics and repo ownership |
| Emerging | A few strategic repos are discoverable and open to internal contribution | Standardize templates, labels, and review expectations |
| Operational | InnerSource practices are repeatable across domains | Track adoption metrics and establish committer paths |
| Scaled | Cross-team contribution is normal; governance is well understood | Optimize incentives, staffing, and portfolio health |
| Strategic | InnerSource shapes platform strategy and engineering culture | Tie metrics to business outcomes and leadership planning |

## Example: from silo to shared ownership

### Before InnerSource

- Team A owns an internal SDK.
- Team B needs one feature and files a ticket.
- Team A has competing priorities, so the feature waits six weeks.
- Team B builds a workaround and divergence begins.

### After InnerSource

- The SDK repo has clear docs, owners, labels, and contribution rules.
- Team B opens an issue, proposes a design, and submits a pull request.
- Team A reviews for quality and compatibility.
- The shared SDK improves once, and both teams benefit.

## Anti-patterns to avoid

| Anti-pattern | Why it hurts | Better alternative |
| --- | --- | --- |
| "Open" repos with no responding maintainers | Contributors lose trust quickly | Publish review SLAs and escalation contacts |
| Invisible ownership | No one knows who can approve or roadmap work | Add CODEOWNERS, team labels, and support channels |
| Contribution by exception only | Every external change feels political | Normalize small fixes and documentation PRs first |
| InnerSource without documentation | Discovery stops at the repo name | Add purpose, setup, architecture, and contribution guidance |
| Measuring only repo count | Quantity hides whether collaboration is actually working | Track contributors, reuse, response time, and merged PRs |

## Getting started checklist

| Question | Why ask it? |
| --- | --- |
| Which internal repos are already reused by multiple teams? | These are your best pilot candidates |
| Which teams frequently wait on shared components? | High-friction dependencies benefit first |
| Can contributors find ownership and setup information in under 5 minutes? | Discoverability predicts contribution volume |
| Do maintainers have protected time for reviews? | Openness fails without capacity |
| Are incentives aligned with shared outcomes? | Teams need recognition for enabling others |

## References

- [InnerSource Commons](https://innersourcecommons.org/)
- [InnerSource Patterns](https://patterns.innersourcecommons.org/)
- [GitHub: About collaborative development](https://docs.github.com/)
- [GitHub CODEOWNERS documentation](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
