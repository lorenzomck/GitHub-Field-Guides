# 06 - Security and Effectiveness Measurement

## Objective

Equip security and platform teams with a concrete risk model for AI agents, alongside a measurement framework that proves — with evidence, not anecdotes — whether agentic workflows are delivering real engineering value.

## Part 1: Security Considerations for AI Agents

### Threat Model Overview

| Threat category | Description |
|---|---|
| Prompt injection | Malicious or misleading content in a repository, issue, or tool output attempts to manipulate agent behavior |
| Excessive permission scope | Agent or its service identity has broader access than the task requires |
| Data exfiltration via tool use | A compromised or overly permissive MCP tool could be used to move sensitive data outside intended boundaries |
| Supply chain risk in agent-authored code | Agent introduces a vulnerable dependency or insecure pattern without sufficient review catching it |
| Credential exposure | Agent execution environment has access to secrets it doesn't need, increasing blast radius if compromised |
| Unauthorized or unreviewed changes | Weak governance allows agent-authored changes to bypass standard review gates |

### Prompt Injection: A Closer Look

Prompt injection is one of the most agent-specific risks. Because agents read repository content, issue text, and tool output as part of their context, an attacker who can influence any of that content (e.g., a malicious issue comment, a crafted file, or manipulated tool output) may attempt to steer the agent's behavior.

| Mitigation | Description |
|---|---|
| Treat all external content as untrusted | Repository content, issue text, and MCP tool responses should never be treated as trusted instructions |
| Constrain tool permissions | Limit what actions a tool can perform so even a successfully manipulated agent has limited blast radius |
| Require human review of all output | The PR review gate is your primary backstop against a successfully injected task |
| Monitor for anomalous tool call patterns | Unexpected sequences of tool calls can indicate an injection attempt worth investigating |

### Security Controls Checklist

- [ ] Agent execution environments use least-privilege, scoped credentials — never broad admin/production secrets.
- [ ] All agent-authored changes flow through standard branch protection and human review.
- [ ] MCP tools are limited to the approved registry with documented data sensitivity classifications.
- [ ] Logging captures task assignment, tool calls, and resulting changes for audit purposes.
- [ ] Security team has visibility into which repositories and domains have agent access enabled.
- [ ] Incident response process includes a defined path for agent-related security concerns (see [04-Agent-Guardrails-and-Governance](./04-Agent-Guardrails-and-Governance.md)).

### Data Handling Considerations

| Consideration | Guidance |
|---|---|
| Sensitive repository content | Confirm your organization's data handling policy for what repository content agents may process |
| Third-party/open-source repositories | Apply extra scrutiny before enabling agent task assignment on public or externally contributed repositories |
| Cross-repository context | Understand whether and how agent context can span multiple repositories, and constrain accordingly |
| Retention of task transcripts/logs | Confirm retention periods for agent session logs align with your data governance policy |

## Part 2: Measuring Agent Effectiveness

### Why Measurement Matters

Without a measurement framework, agentic adoption becomes anecdote-driven — a handful of good or bad experiences shape the entire narrative. A structured metrics program lets you make evidence-based decisions about where to expand, where to pull back, and how to communicate value to leadership.

### Core Metrics Categories

| Category | What it measures |
|---|---|
| Adoption | How much the organization is using agentic workflows |
| Quality | Whether agent output meets the bar without excessive rework |
| Efficiency | How much developer time is saved or reallocated |
| Cost | What agentic workflows cost relative to the value delivered (see the Usage-Based Billing pack) |

### Adoption Metrics

| Metric | Definition |
|---|---|
| Tasks assigned per week | Volume trend of agent task assignment |
| Repositories with agent access enabled | Breadth of rollout |
| Active task assigners | Number of distinct users/teams actively using the capability |

### Quality Metrics

| Metric | Definition |
|---|---|
| First-pass merge rate | % of agent PRs merged with no or minimal requested changes |
| Review iteration count | Average number of review rounds before merge, compared to human-authored PRs |
| Post-merge defect rate | Rate of bugs/regressions traced back to agent-authored changes, compared to baseline |
| Reviewer-reported usefulness | Qualitative feedback score from reviewers on agent PR quality |

### Efficiency Metrics

| Metric | Definition |
|---|---|
| Time-to-PR | Elapsed time from task assignment to PR creation |
| Reviewer time per PR | Time spent reviewing agent PRs vs. comparable human-authored PRs |
| Backlog throughput | Change in the rate at which a defined backlog category (e.g., minor bugs) is resolved |

### Cost/Value Metrics

| Metric | Definition |
|---|---|
| Premium requests consumed per completed task | Efficiency of agent token usage relative to output delivered |
| Cost per merged PR | Total agent-related cost divided by successfully merged PRs |
| Estimated developer time saved | Comparable human effort estimate for equivalent tasks, used directionally rather than precisely |

## Building an Effectiveness Scorecard

| Metric | Baseline | Current | Target | Trend |
|---|---|---|---|---|
| First-pass merge rate | | | | |
| Reviewer time per agent PR vs. human PR | | | | |
| Tasks assigned per week | | | | |
| Post-merge defect rate (agent vs. baseline) | | | | |
| Premium requests per completed task | | | | |

## Establishing a Feedback Loop

1. **Collect** — Gather quantitative metrics (adoption, quality, efficiency, cost) on a consistent schedule.
2. **Gather qualitative input** — Survey reviewers and task assigners on perceived value and pain points.
3. **Review** — Bring metrics and qualitative feedback to the recurring governance review (see [04-Agent-Guardrails-and-Governance](./04-Agent-Guardrails-and-Governance.md)).
4. **Act** — Expand successful use cases, refine or retire underperforming ones, and update instructions/guardrails based on findings.
5. **Communicate** — Share a concise effectiveness summary with engineering leadership on a regular cadence.

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Measuring only volume (tasks assigned, PRs opened) | Balance volume metrics with quality and reviewer-burden metrics |
| No security review before broad rollout | Complete the security controls checklist before expanding beyond a pilot |
| Anecdote-driven expansion decisions | Require a metrics review before adding new repositories/domains to agent scope |
| Ignoring reviewer sentiment | Treat qualitative reviewer feedback as a first-class input, not an afterthought |

## Reference Links

- [Responsible use of GitHub Copilot Coding Agent](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features/responsible-use-of-github-copilot-coding-agent)
- [GitHub Copilot Trust Center](https://resources.github.com/copilot-trust-center/)
- [About audit log events for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/about-the-audit-log-for-your-enterprise)
- [Best practices for using Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks)
