# Agent Configuration Reference

Configuration schema + prompt templates for custom GitHub Copilot agents (VS Code and GitHub Copilot Coding Agent). This is a consolidated reference intended to be easy to skim and safe to copy/paste.

## Contents

- [Where agent files live](#where-agent-files-live)
- [YAML frontmatter schema](#yaml-frontmatter-schema)
- [Body prompt structure](#body-prompt-structure)
- [Templates](#templates)
- [Prompt size and complexity](#prompt-size-and-complexity)
- [Skills integration](#skills-integration)
- [Verification (cargo-cult detection)](#verification-cargo-cult-detection)
- [Validation checklist](#validation-checklist)
- [Cross-platform notes](#cross-platform-notes)
- [Limits](#limits)

## Where agent files live

| Type | Location | Scope |
|------|----------|-------|
| Repository agents | `.github/agents/*.agent.md` | Per-repository |
| Personal agents | `~/.copilot/agents/*.agent.md` | Per-user local VS Code |
| Legacy format | `*.chatmode.md` | Deprecated; still recognized |

**Naming convention:** `<agent-name>.agent.md`

- Filename (minus extension) becomes the agent identifier.
- More specific configs override broader ones (repo > org > enterprise).

## YAML frontmatter schema

### Properties

| Property | Type | VS Code | Coding Agent | Description |
|----------|------|---------|--------------|-------------|
| `name` | string | ✅ | ✅ | Display name. Optional; defaults to filename. |
| `description` | string | ✅ | ✅ Required | Purpose + when to use. Used for agent selection UI. |
| `tools` | string[] or string | ✅ | ✅ | Tool allowlist. Omit for all tools; empty `[]` for none. |
| `infer` | boolean | ✅ | ✅ | If `false`, agent cannot be auto-selected as a subagent. Default: `true`. |
| `target` | string | ✅ | ✅ | Scope: `vscode`, `github-copilot`, or omit for both. |
| `model` | string | ✅ | ❌ Ignored | Preferred model (VS Code only). |
| `mcp-servers` | object | ❌ | ✅ Org/Enterprise | MCP server config (not supported at repo level). |
| `metadata` | object | ✅ | ❌ | Annotation key/value pairs (VS Code only). |

### Tools allowlist

```yaml
# Allow a specific tool
tools: ["azure.some-extension/some-tool"]

# Allow a tool from an MCP server
tools: ["custom-mcp/tool-name"]

# Allow all tools from an MCP server
tools: ["github/*"]

# Allow no tools
tools: []
```

### Surface differences

- `model:` is respected in VS Code; ignored by Coding Agent.
- MCP server configuration is org/enterprise-scoped in Coding Agent; not repo-scoped.

## Body prompt structure

The agent *body* (below frontmatter) is free-form Markdown. Keep it structured and consistent; prefer a small number of predictable tags/sections.

Recommended sections:

```markdown
<mission>
[What this agent is responsible for and what success looks like]
</mission>

<scope>
In scope:
- ...

Out of scope:
- ...
</scope>

<tool_policy>
- ...
</tool_policy>

<workflow>
1. ...
2. ...
</workflow>

## Output Format
- ...

<halt_conditions>
- ...
</halt_conditions>

<done_when>
- ...
</done_when>
```

## Templates

### Minimal agent

```markdown
---
name: <agent-name>
description: "<one sentence purpose and when to use>"
tools: ["read", "search", "edit", "agent", "todo"]
infer: true
---

<mission>
[Primary responsibility + definition of success]
</mission>

<scope>
In scope:
- ...

Out of scope:
- ...
</scope>

<workflow>
1. ...
2. ...
3. ...
</workflow>

## Output Format
- ...

<halt_conditions>
- ...
</halt_conditions>

<done_when>
- ...
</done_when>
```

### Canonical agent (full)

```markdown
---
name: <agent-name>
description: "<one sentence purpose and when to use>"
tools: ["read", "search", "edit", "agent", "todo"]
infer: true
---

<mission>
[Primary objective + definition of done]
</mission>

<scope>
In scope:
- [specific responsibilities]
- [file types or domains]

Out of scope:
- [delegate/ignore]
- [requires escalation]
</scope>

<trigger_keywords>
Invoke when:
- [explicit criteria]
- [keywords/patterns]
</trigger_keywords>

<required_context>
Read before acting:
- [repo instruction files]
- [constraints]
</required_context>

<skills_awareness>
Check `.github/skills/` for reusable workflows before implementing ad-hoc procedures.

| Skill | Trigger | Use Case |
|-------|---------|----------|
| `<skill-name>` | `<trigger>` | `<use-case>` |

Before executing a workflow covered by a skill:
1. Read `.github/skills/<skill-name>/SKILL.md`
2. Follow the skill's workflow as written
3. Denote usage in output: `Skills Applied: [SKILL] <skill-name>`
</skills_awareness>

<tool_policy>
- [when to use which tools]
- [parallel tool call guidance]
</tool_policy>

<workflow>
1. Load constraints and create todo list
2. Discovery (if needed)
3. Planning (if needed)
4. Execute
5. Verify
6. Report
</workflow>

## Output Format
- [required sections/artifacts]

<halt_conditions>
- [missing inputs]
- [validation failures]
</halt_conditions>

<done_when>
- [completion criteria]
</done_when>

<handoffs_required>
[Optional: include only if this agent delegates]
</handoffs_required>
```

### Orchestrator

```markdown
---
name: orchestrator
description: "Coordinates multi-step workflows by delegating to specialists and synthesizing results."
tools: ["read", "search", "edit", "agent", "todo"]
infer: true
---

<mission>
Own the control loop for complex tasks: classify, delegate, verify, synthesize.
</mission>

<tool_policy>
- Use `todo` for end-to-end progress.
- Delegate only when it reduces risk or improves quality.
- Prefer parallel tool calls for independent operations.
</tool_policy>

<workflow>
1. Load constraints
2. Create todo list
3. Classify: simple vs complex
4. Delegate (if complex)
5. Verify results
6. Synthesize and report
</workflow>
```

### Subagent

```markdown
---
name: <specialist-name>
description: "<focused purpose>"
tools: ["read", "search", "todo"]
infer: true
---

<mission>
[Focused responsibility + definition of success]
</mission>

<tool_policy>
- Use `todo` to track subtasks.
- Use `read`/`search` for investigation.
- Do NOT spawn subagents (single-level nesting only).
</tool_policy>

<workflow>
1. Parse objective
2. Create todo list
3. Execute focused work
4. Return results in required format
</workflow>
```

### Handoff payload template

```markdown
<handoffs_required>
### Example Target Agent: <target-agent>
**Trigger:** ALWAYS | IF <condition>

**Payload (copy/paste):**
## Handoff
- **From:** <this-agent>
- **To:** <target-agent>
- **Objective:** <one sentence>
- **Inputs:**
  - <data to pass>
  - Directory: <path>
- **Context already gathered:**
  - <facts the subagent should not re-discover>
- **Constraints:**
  🚨 <hard rules>
  Maximum <N> files
- **Required outputs:**
  Return ONLY this structure:

  | Column A | Column B |
  |----------|----------|

- **Stop rules:**
  - Stop when: <condition>
  - Return partial results if incomplete
</handoffs_required>
```

## Prompt size and complexity

| Lines | Typical behavior | Action |
|-------|------------------|--------|
| ≤150 | Best compliance | None |
| 150–300 | Usually fine | Monitor for drift |
| 300–500 | Degraded | Simplify or extract to skills/reference |
| 500+ | Poor | Mandatory refactor |

### Separation of concerns

| What | Where |
|------|-------|
| Orchestration + tool policy | `.github/agents/` |
| Output templates | `Reference/` |
| Reusable procedures + thresholds | `.github/skills/` |

## Skills integration

- List only the relevant skills in `<skills_awareness>`.
- Require reading `.github/skills/<skill-name>/SKILL.md` before executing.
- Denote usage only when the skill was actually followed.

## Verification (cargo-cult detection)

### Common failure signals

| Signal | Likely meaning |
|--------|----------------|
| Diagnostic output matches a template exactly | Fabricated, not measured |
| "I followed all steps" with no evidence | Cargo-cult compliance |
| Headers present but thin content | Format compliance, content skipped |

## Validation checklist

- [ ] `name` is kebab-case (e.g., `repo-auditor`)
- [ ] `description` is present and accurate
- [ ] `tools` is intentional (omit for all tools; `[]` for none) and includes `todo` for repo agents
- [ ] `infer` is set appropriately (`false` for side-effectful/coordinator agents)
- [ ] Prompt body has `<mission>`, `<scope>`, and `<workflow>`
- [ ] If delegating, `<handoffs_required>` exists with explicit output format + stop rules
- [ ] No huge inline templates (move to `Reference/`)

## Cross-platform notes

| Aspect | VS Code | Coding Agent | Guidance |
|--------|---------|--------------|----------|
| Delegation API | `runSubagent` | `agent` | Keep `tools` lists using `agent` |
| Model pinning | `model:` honored | `model:` ignored | Don't depend on it |
| MCP servers | Not repo-configurable | Org/enterprise-scoped | Document externally |

## Limits

| Component | Limit |
|-----------|-------|
| Agent body (prompt) | 30,000 characters |
| Description | 1,024 characters |
| Name | 64 characters |
