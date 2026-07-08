# 02 - Token Budget Management

## Objective

Give platform and engineering leaders a repeatable model for managing GitHub Copilot's token and premium-request consumption so AI-assisted development stays affordable, predictable, and high-value as adoption scales from pilot teams to the full organization.

## How Copilot Consumption Works

| Concept | Description |
|---|---|
| Premium request | A unit of consumption charged when a Copilot interaction uses a premium/non-default model (e.g., advanced chat, agent tasks, certain model tiers) |
| Included allowance | Business/Enterprise plans include a monthly allowance of premium requests per seat |
| Token usage | The underlying unit models consume (input + output tokens); premium request accounting is typically the customer-facing abstraction over token usage |
| Model tiering | Not all models consume premium requests at the same rate — lighter models may be "included" while frontier models draw down the allowance faster |

> Confirm current model-to-request multipliers in your tenant's admin console, since GitHub periodically updates which models are included vs. premium and at what consumption ratio.

## Why Token Budgets Matter

- Costs scale with **how** Copilot is used, not just **how many seats** are assigned.
- Agentic workflows (multi-step reasoning, longer context, tool calls) consume meaningfully more tokens per task than single-turn autocomplete.
- Without visibility, a small number of power users or automated agents can consume a disproportionate share of the organization's allowance.

## Consumption Patterns by Use Case

| Use case | Typical consumption profile |
|---|---|
| Inline code completion | Low — short context, frequent small requests |
| Chat Q&A | Moderate — depends on conversation length and repository context size |
| Code review summarization | Moderate — diff size drives context length |
| Coding agent / autonomous tasks | High — multi-turn reasoning, tool calls, and large repository context |
| Custom extensions / MCP tool chains | Variable — depends on how much context and how many round trips each tool call requires |

## Building a Token Budget Model

### Step 1: Establish a Baseline

- [ ] Pull premium request usage by organization, team, and top individual consumers for the last 60-90 days.
- [ ] Segment usage by interaction type (chat, completions, agent tasks) where reporting allows.
- [ ] Identify seasonal or sprint-driven spikes (e.g., release weeks, hackathons).

### Step 2: Set Allocation Tiers

| Tier | Profile | Suggested allowance approach |
|---|---|---|
| Standard developer | Day-to-day completions and chat | Default seat allowance |
| Power user / platform team | Heavy chat and code review usage | Monitor; escalate only if consistently maxing allowance |
| Agent/automation workloads | Coding agent, scheduled agent tasks | Separate budget line, tracked independently from human seats |

### Step 3: Define Escalation Paths

- [ ] Document what happens when a user or team approaches their allowance (notification, soft block, or approval-based increase).
- [ ] Assign an owner who can approve incremental premium-request budget for specific initiatives.
- [ ] Avoid ad hoc, unlimited increases — treat additional allowance as a reviewed exception.

## Optimization Strategies

### Prompt and Workflow Efficiency

| Strategy | Impact |
|---|---|
| Scope chat context to relevant files/folders | Reduces unnecessary context tokens per request |
| Use repository custom instructions (`.github/copilot-instructions.md`) | Reduces the need to repeat context in every prompt |
| Prefer targeted questions over open-ended exploration | Fewer follow-up turns, lower cumulative token use |
| Close out long chat threads and start fresh sessions per task | Prevents unbounded context growth in a single conversation |
| Right-size agent task scope | Narrow, well-defined agent tasks avoid runaway multi-turn loops |

### Model Selection Guidance

| Scenario | Recommended approach |
|---|---|
| Routine completions | Use default/included models |
| Complex reasoning or architecture questions | Reserve premium models for genuinely hard problems |
| Bulk/automated tasks (agents, batch triage) | Evaluate whether a lighter model meets quality bar before defaulting to the most capable one |
| Experimentation | Pilot new models on a small population before wide rollout |

### Governance Controls

- [ ] Enable admin policies that control which models are available to which teams.
- [ ] Restrict agent/automation access to premium models to approved workflows only.
- [ ] Require review before enabling new MCP tools or extensions that trigger high-frequency automated requests.

## Monitoring Dashboard

| Metric | Why it matters | Suggested cadence |
|---|---|---|
| Premium requests consumed vs. allowance | Core budget health signal | Weekly |
| Top 10 consumers (users/teams/agents) | Identify concentration risk | Weekly |
| Requests by interaction type | Understand what's driving consumption | Monthly |
| Trend vs. prior period | Detect acceleration before it becomes a budget problem | Monthly |
| Agent/automation share of total consumption | Separate human vs. automated demand | Monthly |

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Treating agent workloads the same as human seats | Track agent/automation consumption as its own budget line |
| No visibility until the invoice arrives | Set up proactive usage dashboards and alerts (see [04-Spending-Limits-and-Budget-Alerts](./04-Spending-Limits-and-Budget-Alerts.md)) |
| Defaulting every workflow to the most capable model | Establish model-selection guidance tied to task complexity |
| Unlimited scheduled agent runs | Cap frequency and scope of scheduled/automated agent tasks |
| No owner for the allowance | Assign a named budget owner per organization or business unit |

## Building a Token Budget Playbook

Turn the practices above into a living internal document that new teams can reference when they start using Copilot.

### Playbook Contents

| Section | Purpose |
|---|---|
| Current allowance summary | Plain-language explanation of what's included per seat and what triggers overage |
| Model selection guidance | Which models to use for which task types, and why |
| Escalation contacts | Who to reach out to for additional budget or policy questions |
| Optimization tips | The prompt/workflow efficiency practices most relevant to that team's stack |
| Reporting links | Where to find live dashboards for the team's own consumption |

### Rollout Checklist

- [ ] Publish the playbook to a discoverable internal location (engineering wiki, onboarding docs).
- [ ] Reference it in new-hire onboarding for any role with a Copilot seat.
- [ ] Update it whenever GitHub changes model tiering or allowance policy.
- [ ] Solicit feedback from heavy users on what additional guidance would help them self-optimize.

## Balancing Cost Control with Developer Experience

Token budget management should never feel like it's actively fighting developers trying to do good work. Keep these principles in mind:

- **Guardrails, not gates.** Prefer notifications and visibility over hard blocks wherever possible.
- **Fast exceptions.** Make it easy for a team with a legitimate, temporary need to get an approved increase.
- **Celebrate efficiency, not just savings.** Recognize teams that adopt custom instructions and scoped prompts well, not only teams that consume less overall.
- **Revisit thresholds regularly.** As adoption matures and instructions improve, expect the "normal" consumption baseline to shift — recalibrate accordingly.

## Reference Links

- [About billing for GitHub Copilot](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-github-copilot-for-your-enterprise/about-billing-for-github-copilot)
- [Managing policies for Copilot in your enterprise](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-policies-for-copilot-in-your-enterprise)
- [Configuring custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/custom-instructions)
- [About GitHub Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/about-copilot-coding-agent)
