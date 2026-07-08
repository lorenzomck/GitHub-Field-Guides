# 03 - MCP Overview and Registry Setup

## Objective

Explain the Model Context Protocol (MCP) at a level useful for platform teams, and provide a concrete approach for standing up a governed internal MCP registry so that Copilot and other AI agents can safely access additional tools and data sources.

## What Is MCP?

The Model Context Protocol (MCP) is an open protocol that standardizes how AI applications (like Copilot Chat or Coding Agent) connect to external tools, data sources, and services. Instead of every AI product building bespoke integrations to every system, MCP defines a common interface: an **MCP server** exposes a set of tools/resources, and any **MCP client** (an AI assistant) that speaks the protocol can discover and call them.

| Concept | Description |
|---|---|
| MCP server | A process that exposes tools, resources, and/or prompts over the protocol (e.g., a server that queries an internal ticketing system) |
| MCP client | The AI application (Copilot Chat, Coding Agent, an IDE extension) that connects to and invokes MCP servers |
| Tool | A discrete callable capability exposed by a server (e.g., "search internal knowledge base," "create a ticket") |
| Resource | Read-only context a server can provide to the model (e.g., a document, a config file) |
| Transport | The mechanism connecting client and server (local stdio process or remote HTTP/SSE endpoint) |

## Why MCP Matters for Agentic Workflows

Without MCP, extending an agent's capabilities means custom point-to-point integration work for every tool and every AI surface. MCP gives you:

- **Reusability** — one MCP server can serve Copilot Chat, Coding Agent, and any other MCP-compatible client.
- **Separation of concerns** — tool/data owners maintain their own MCP server; the AI platform doesn't need bespoke code per integration.
- **A natural governance boundary** — because servers are discrete, addressable components, you can apply access control, logging, and approval processes at the server level.

## Local vs. Remote MCP Servers

| Type | Description | Best for |
|---|---|---|
| Local (stdio) | Runs as a subprocess on the same machine as the client | Individual developer tools, local file/dev-environment integrations |
| Remote (HTTP/SSE) | Runs as a hosted service the client connects to over the network | Shared internal tools, org-wide data sources, centrally governed integrations |

For enterprise-wide agentic workflows (especially Coding Agent, which runs in a managed cloud environment), remote MCP servers are typically the relevant pattern.

## Designing a Governed MCP Registry

An MCP registry is your organization's curated, vetted catalog of MCP servers that agents are permitted to use — analogous to an internal package/artifact registry, but for AI tool integrations.

### Registry Components

| Component | Purpose |
|---|---|
| Catalog/inventory | List of approved MCP servers, their owners, and their exposed tools |
| Review/approval workflow | Process for vetting a new MCP server before it's added to the catalog |
| Access policy | Which teams/repositories/agents are permitted to use which servers |
| Monitoring/logging | Visibility into which tools are called, how often, and by which agent/task |

### Registry Entry Template

| Field | Description |
|---|---|
| Server name | Human-readable identifier |
| Owner team | Who maintains and is accountable for this server |
| Purpose | What capability it provides |
| Exposed tools | List of callable tools/resources and what they do |
| Data sensitivity | Classification of any data the server can read or write |
| Transport | Local or remote, and connection details |
| Review status | Approved / pending review / deprecated |
| Last security review date | Tracks review recency |

## Step-by-Step: Standing Up a Registry

1. **Inventory existing/desired integrations.** Identify what internal systems teams already want to connect (ticketing, internal docs search, deployment tooling, observability).
2. **Define a review process.** Establish minimum requirements before a server is added: authentication, least-privilege scopes, logging, and a named owner.
3. **Pilot with a small number of low-risk servers.** Start with read-only, low-sensitivity tools (e.g., internal documentation search) before write-capable tools.
4. **Publish the catalog.** Make the approved server list discoverable to engineering teams, with clear instructions on how to request access or propose a new server.
5. **Wire up access controls.** Ensure only approved teams/repositories can configure and use registry servers, consistent with your governance policy (see [04-Agent-Guardrails-and-Governance](./04-Agent-Guardrails-and-Governance.md)).
6. **Monitor and iterate.** Review tool call logs regularly and re-evaluate servers that see heavy use or trigger unexpected errors.

## Security Considerations for MCP Servers

| Risk | Mitigation |
|---|---|
| Overly broad tool permissions | Design tools with the narrowest useful scope (e.g., "search tickets" rather than "execute arbitrary query") |
| Prompt injection via tool output | Treat all data returned from MCP tools as untrusted input; sanitize/validate before it influences further tool calls |
| Credential sprawl | Use scoped service credentials per server; never share broad admin credentials across multiple tools |
| Unvetted third-party servers | Require the same review process for third-party/community MCP servers as internally built ones |
| Silent tool failures | Ensure servers fail loudly and log errors so agents don't silently proceed on bad data |

## Sample Approval Checklist for a New MCP Server

- [ ] Server has a named owning team and on-call/support path.
- [ ] Authentication uses scoped, least-privilege credentials — not shared admin accounts.
- [ ] Data sensitivity of exposed tools/resources is documented and reviewed by security.
- [ ] Tool call logging is enabled and routed to central observability.
- [ ] Server has been tested for basic resilience (timeouts, malformed input handling).
- [ ] Server is added to the registry catalog with a review date and renewal cadence.

## Common Pitfalls

| Pitfall | Mitigation |
|---|---|
| Every team builds ad hoc integrations without a registry | Centralize discovery and review through a single governed catalog |
| Write-capable tools approved without extra scrutiny | Apply a higher review bar for any tool that can mutate state (create, update, delete) |
| No visibility into which agents call which tools | Require logging as a condition of registry approval |
| Registry becomes stale | Set a recurring review cadence and deprecate unused/unmaintained servers |

## Sample Registry Catalog Structure

Maintaining the registry as a structured, version-controlled document (rather than tribal knowledge) makes review and onboarding easier.

```markdown
# MCP Server Registry

## <server-name>
- Owner: <team>
- Purpose: <one-line description>
- Tools exposed: <tool-1>, <tool-2>, ...
- Data sensitivity: <public | internal | confidential>
- Transport: <local | remote>
- Review status: <approved | pending | deprecated>
- Last security review: <date>
```

### Registry Maintenance Checklist

- [ ] Store the registry in a version-controlled repository so changes are reviewable via pull request.
- [ ] Require a review from the owning security/platform team before a new entry is merged.
- [ ] Set a recurring reminder (e.g., every 6 months) to re-validate each entry's review status.
- [ ] Deprecate and remove entries for servers that are no longer maintained or used.

## Onboarding a New Team to the Registry

1. The requesting team documents the desired integration using the registry entry template.
2. Platform/security reviews the request against the approval checklist.
3. Once approved, the entry is merged into the registry and access is provisioned to the requesting team's repositories/agents.
4. The requesting team is added to a lightweight support channel in case of issues.
5. The entry is scheduled for its first periodic review alongside the rest of the catalog.

## Next Steps

With a governed set of tools available, move to [04-Agent-Guardrails-and-Governance](./04-Agent-Guardrails-and-Governance.md) to define the broader policy framework that constrains how agents operate.

## Reference Links

- [Extending Copilot Chat with the Model Context Protocol (MCP)](https://docs.github.com/en/copilot/customizing-copilot/using-model-context-protocol/extending-copilot-chat-with-mcp)
- [About MCP servers in GitHub Copilot](https://docs.github.com/en/copilot/customizing-copilot/using-model-context-protocol/about-mcp-servers)
- [Extending the GitHub MCP Server](https://docs.github.com/en/copilot/customizing-copilot/using-model-context-protocol/extending-the-github-mcp-server)
- [Responsible use of GitHub Copilot](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features)
