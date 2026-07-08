# 04 - Agent Guardrails and Governance

## Objective

Define the policy, permission, and approval framework that allows agentic workflows to scale safely — ensuring autonomy grows in proportion to demonstrated trust, and that every agent action remains attributable, reviewable, and reversible.

## Governance Principles

| Principle | Description |
|---|---|
| Least privilege | Agents should have only the access and tool permissions required for their assigned task scope |
| Human-in-the-loop by default | Agent output (PRs, changes) requires human review before merge, especially early in adoption |
| Auditability | Every agent action should be traceable to a task, a requester, and a resulting change |
| Proportional autonomy | Expand what agents can do without direct oversight only as trust is earned through track record |
| Fail-safe defaults | When in doubt, restrict scope rather than grant broad access "to be safe later" |

## Governance Framework Layers

| Layer | Controls |
|---|---|
| Identity and access | Which agents/service identities exist, and what repository/organization access they hold |
| Task scope | What kinds of tasks can be assigned to agents, and which domains are off-limits |
| Tool access | Which MCP servers/tools an agent can call (see [03-MCP-Overview-and-Registry-Setup](./03-MCP-Overview-and-Registry-Setup.md)) |
| Change control | Branch protection, required reviewers, and merge policies applied to agent-authored PRs |
| Monitoring | Logging, alerting, and periodic review of agent activity |

## Step 1: Define Permission Boundaries

### Repository and Organization Scope

- [ ] Maintain an explicit list of repositories where agent task assignment is enabled.
- [ ] Apply the same branch protection rules to agent-authored PRs as to human-authored PRs — no bypass.
- [ ] Require at least one human approval before merge, regardless of CI status, during initial rollout phases.
- [ ] Exclude repositories handling secrets management, IAM configuration, or production infrastructure from early-phase agent access.

### Task Domain Restrictions

| Domain | Recommended posture |
|---|---|
| Bug fixes with clear repro steps | Generally appropriate for agent assignment |
| Test coverage improvements | Generally appropriate |
| Documentation updates | Generally appropriate |
| Authentication/authorization logic | Require explicit security review before and after; consider excluding initially |
| Payment/financial processing logic | Exclude from agent scope unless under strict, specialized review |
| Infrastructure-as-code affecting production | Require additional approval gate beyond standard code review |

## Step 2: Establish Approval Workflows

### Task Assignment Approval

| Trust level | Assignment process |
|---|---|
| Pilot phase | Named pilot leads only, manual assignment, every task pre-reviewed for scope appropriateness |
| Expanding phase | Engineering managers can assign tasks within their team's approved repositories |
| Mature phase | Broader self-service assignment within pre-approved domains, with periodic sampling audits |

### Change Approval

- [ ] Require standard PR review (same reviewers/CODEOWNERS rules as human contributions).
- [ ] For sensitive repositories, require a second reviewer specifically for agent-authored changes during the trust-building period.
- [ ] Never enable auto-merge for agent PRs without an explicit, deliberate policy decision and monitoring in place.

## Step 3: Apply Technical Guardrails

| Guardrail | Implementation |
|---|---|
| Branch protection | Require PR review, status checks, and disallow direct pushes to protected branches — applies equally to agents |
| CODEOWNERS | Route agent PRs touching sensitive paths to the appropriate human reviewers automatically |
| Environment secrets scoping | Agent execution environments use least-privilege, non-production credentials only |
| MCP tool allowlisting | Agents can only call tools present in the approved registry (see [03-MCP-Overview-and-Registry-Setup](./03-MCP-Overview-and-Registry-Setup.md)) |
| Rate/frequency limits | Cap how many tasks can be auto-assigned or scheduled within a given time window to avoid runaway automation |

## Step 4: Build the Governance Review Cadence

### Monthly Agent Governance Review

1. Review the volume and type of tasks assigned to agents.
2. Sample a subset of agent-authored PRs for quality and scope adherence.
3. Review any incidents (rejected PRs, scope violations, unexpected tool calls).
4. Reassess whether any repositories/domains should be added to or removed from the approved scope.

### Quarterly Policy Review

- [ ] Revisit task domain restrictions based on accumulated track record.
- [ ] Review MCP registry entries for continued relevance and security posture.
- [ ] Update training/enablement materials based on lessons learned.
- [ ] Report governance posture and incident summary to security/compliance stakeholders.

## Incident Response for Agent-Related Issues

| Scenario | Response |
|---|---|
| Agent proposes a change touching an out-of-scope sensitive area | Reject the PR, document the gap, tighten task domain restrictions or instructions |
| Agent calls an unexpected or unapproved tool | Investigate MCP configuration, remove/restrict the tool, review logs for any data exposure |
| Agent-authored PR merged without adequate review | Treat as a process failure; reinforce branch protection and reviewer requirements |
| Runaway or excessive task volume from automation | Apply rate limits, pause the triggering automation, review root cause |

## Governance Maturity Model

| Level | Characteristics |
|---|---|
| Level 1 — Ad hoc | Agents used informally with no defined policy or approval process |
| Level 2 — Controlled pilot | Named pilot leads, restricted repository scope, manual task assignment |
| Level 3 — Managed rollout | Written policies, defined approval workflows, MCP registry in place |
| Level 4 — Scaled governance | Self-service within guardrails, recurring review cadence, mature metrics program (see [06-Security-and-Effectiveness-Measurement](./06-Security-and-Effectiveness-Measurement.md)) |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Treating agent PRs with less scrutiny than human PRs | Apply identical or stricter review standards, especially early on |
| No documented task domain restrictions | Publish and enforce a clear list of in-scope vs. out-of-scope task types |
| Ungoverned MCP tool sprawl | Route all tool access through the approved registry, no ad hoc connections |
| Governance policy written once and never revisited | Put governance review on a recurring calendar with a named owner |

## Reference Links

- [Responsible use of GitHub Copilot Coding Agent](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features/responsible-use-of-github-copilot-coding-agent)
- [About branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Managing policies for Copilot in your enterprise](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-policies-for-copilot-in-your-enterprise)
