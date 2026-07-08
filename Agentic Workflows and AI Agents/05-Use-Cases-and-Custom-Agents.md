# 05 - Use Cases and Custom Agents

## Objective

Provide a practical catalog of high-value agentic use cases teams can adopt quickly, along with guidance for building custom agents and Copilot Extensions when off-the-shelf capabilities need to be extended for organization-specific workflows.

## Part 1: High-Value Use Cases

### Automated Pull Request Generation

| Aspect | Detail |
|---|---|
| Pattern | A triaged issue or task description is assigned to the agent, which produces a complete PR |
| Best fit | Bug fixes, dependency upgrades, test coverage gaps, documentation updates |
| Success signal | Low reviewer rework rate, PRs merge with minimal back-and-forth |
| Getting started | Pick a backlog of 10-15 well-scoped issues and assign them over 2-3 weeks; track review effort compared to human-authored equivalents |

### Automated Issue Triage

| Aspect | Detail |
|---|---|
| Pattern | An agent (or a lighter-weight automation) reviews incoming issues, applies labels, requests missing information, and suggests severity/priority |
| Best fit | High-volume issue trackers where manual triage is a bottleneck |
| Success signal | Reduced time-to-first-response, more consistent labeling |
| Getting started | Start with label suggestion and missing-info follow-up only; avoid auto-closing or auto-prioritizing until trust is established |

### Automated Code Review Assistance

| Aspect | Detail |
|---|---|
| Pattern | An agent reviews a PR diff and surfaces potential bugs, style issues, or missing tests as review comments |
| Best fit | Supplementing (not replacing) human review, especially for large or unfamiliar diffs |
| Success signal | Reviewers report the comments are high-signal and reduce review time |
| Getting started | Enable on a subset of repositories and gather reviewer feedback on comment usefulness before wider rollout |

### Dependency and Framework Migration

| Aspect | Detail |
|---|---|
| Pattern | Agent applies a well-defined migration pattern (e.g., library major version upgrade) across multiple call sites |
| Best fit | Repetitive, pattern-based changes with a clear "before/after" transformation |
| Success signal | High first-pass build/test success rate on generated PRs |
| Getting started | Pilot on a single repository with strong test coverage before scaling to a fleet-wide migration |

### Test Coverage Expansion

| Aspect | Detail |
|---|---|
| Pattern | Agent identifies undertested code paths and generates targeted unit/integration tests |
| Best fit | Legacy code with test debt, or new code needing coverage before a release gate |
| Success signal | New tests catch real regressions over time, not just coverage percentage increases |
| Getting started | Pair with a code coverage report to prioritize the highest-value gaps first |

## Use Case Prioritization Framework

| Criterion | Question |
|---|---|
| Scope clarity | Can the task be described with clear acceptance criteria? |
| Verifiability | Can success be validated automatically (tests, build, lint)? |
| Blast radius | If the agent gets it wrong, how contained is the impact? |
| Volume | Is this a repetitive task type worth automating, or a one-off? |

Prioritize use cases that score well on all four dimensions before tackling ambiguous, high-blast-radius, or low-volume tasks.

## Part 2: Building Custom Agents and Extensions

### When to Build vs. Use Off-the-Shelf Capability

| Scenario | Recommended approach |
|---|---|
| Standard coding tasks within a repository | Use Copilot Coding Agent directly — no custom build needed |
| Need to expose an internal tool/data source to agents | Build an MCP server (see [03-MCP-Overview-and-Registry-Setup](./03-MCP-Overview-and-Registry-Setup.md)) |
| Need a packaged, distributable AI experience integrated into GitHub (chat participant, custom skill) | Build a Copilot Extension |
| Need a fully custom automation outside the Copilot surface | Consider a standalone GitHub App or Action combined with your own orchestration |

### Copilot Extensions Overview

A Copilot Extension packages a capability (often backed by an MCP server or a custom API) so it can be invoked directly within Copilot Chat, typically via an `@` mention, giving users a discoverable, named integration point.

| Component | Purpose |
|---|---|
| GitHub App | Provides the identity, permissions, and installation model for the extension |
| Backend service | Implements the extension's logic and integrates with Copilot's chat completion API |
| Agent/skill definition | Describes what the extension does and how it should respond to user requests |

### Custom Agent/Extension Development Checklist

- [ ] Define a narrow, clear purpose — avoid building a generic "do anything" extension.
- [ ] Design the GitHub App with least-privilege permissions scoped to what the extension actually needs.
- [ ] Implement clear error handling so failures produce useful feedback rather than silent failures.
- [ ] Write documentation so users know what the extension can and cannot do.
- [ ] Include the extension in your organization's MCP/extension registry and governance review (see [04-Agent-Guardrails-and-Governance](./04-Agent-Guardrails-and-Governance.md)).
- [ ] Pilot with a small user group before organization-wide rollout.

## Building an Internal Use Case Playbook

As adoption grows, capture what's working in a lightweight internal playbook:

| Field | Purpose |
|---|---|
| Use case name | e.g., "Dependency upgrade automation" |
| Owning team | Who maintains this pattern |
| Task template | Reusable prompt/issue template for this use case |
| Success metrics | What "working well" looks like for this specific use case |
| Known limitations | Edge cases where this use case doesn't perform well |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Starting with ambiguous, high-stakes use cases | Prioritize using the four-criterion framework above; start narrow |
| Building custom extensions before exhausting off-the-shelf capability | Confirm Coding Agent or existing MCP servers can't already solve the problem |
| No internal knowledge sharing across teams piloting agents | Maintain a shared use case playbook to avoid duplicated effort |
| Measuring only volume (PRs opened) rather than quality | Track review effort and defect rates, not just throughput (see [06-Security-and-Effectiveness-Measurement](./06-Security-and-Effectiveness-Measurement.md)) |

## Reference Links

- [Best practices for using Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks)
- [About building Copilot Extensions](https://docs.github.com/en/copilot/building-copilot-extensions/about-building-copilot-extensions)
- [Extending Copilot Chat with the Model Context Protocol (MCP)](https://docs.github.com/en/copilot/customizing-copilot/using-model-context-protocol/extending-copilot-chat-with-mcp)
- [About GitHub Apps](https://docs.github.com/en/apps/overview)
