## Claude 4.5 Prompting Best Practices

This reference codifies prompting guidance optimized for Claude Opus 4.5. It is written for AI agents to consume and apply when writing or improving prompts for custom agents, prompt files, and instructions.

## Core Principles

### Be Explicit with Instructions

Claude 4.5 follows instructions precisely and does not "go above and beyond" unless explicitly requested.

```xml
<!-- GOOD: Explicit instruction -->
<default_to_action>
Implement changes rather than only suggesting them. If the user's intent is unclear,
infer the most useful likely action and proceed, using tools to discover missing details.
</default_to_action>

<!-- BAD: Vague expectation -->
"Help the user with their code."
```

### Add Context and Motivation

Providing *why* an instruction matters helps Claude generalize correctly.

```xml
<!-- GOOD: Instruction + motivation -->
Use parallel tool calls whenever possible. This dramatically improves response latency
and reduces the number of API round-trips, which improves user experience.

<!-- BAD: Instruction without motivation -->
Use parallel tool calls.
```

### Be Vigilant with Examples

Claude 4.5 pays close attention to examples. Ensure examples:
- Model the exact behavior you want
- Avoid patterns you want to discourage
- Use placeholders `[like this]` for variable content

## Tool Usage Patterns

### Parallel Tool Calls

```xml
<use_parallel_tool_calls>
If you intend to call multiple tools and there are no dependencies between the calls,
make all independent calls in parallel. However, if some calls depend on previous results,
call them sequentially. Never use placeholders or guess missing parameters.
</use_parallel_tool_calls>
```

### Tool Triggering

Avoid aggressive capitalized language like "CRITICAL: You MUST use this tool when..." which may cause overtriggering. Use natural phrasing:

```xml
<!-- GOOD: Natural phrasing -->
Use the read_file tool when you need to see the contents of a file.

<!-- BAD: Aggressive emphasis -->
CRITICAL: You MUST ALWAYS use read_file before any discussion of file contents.
```

### Investigate Before Acting

```xml
<investigate_before_answering>
Always read and understand relevant files before proposing code edits. Do not speculate
about code you have not inspected.
</investigate_before_answering>
```

### Tool Permission Generosity

```xml
<tool_philosophy>
Include tools the agent might need, even if not used every time. Missing tools cause
failures; extra tools in the allowlist do not hurt.

Default generous: read, edit, search, web, agent, todo
Only restrict for safety reasons.
</tool_philosophy>
```

## Subagent Orchestration

### Delegation Tools by Surface

| Surface | Delegation Tool | Notes |
|---------|-----------------|-------|
| VS Code Copilot | `runSubagent` | Use `agent` alias in tools list |
| Coding Agent | `agent` | Use `agent` alias in tools list |

### Subagent Prompt Requirements

When spawning a subagent, the prompt must be self-contained:
- Include complete task description
- Specify exactly what information to return
- Define success criteria
- State whether the subagent should write code or only research

## Subagent Handoff Best Practices

### The 3 Golden Rules

| Rule | Confidence | Why It Matters |
|------|------------|----------------|
| 📋 Specify return format | 95% | Produces parseable responses |
| 🛑 Set stop rules | 90% | Creates bounded execution |
| 📦 Pass gathered context | 80% | Prevents redundant research |

### Optimized Handoff Template

```markdown
## Handoff
- **From:** <orchestrator>
- **To:** <subagent>
- **Objective:** <one sentence task>
- **Inputs:**
  - <data to pass>
  - Directory: <path>
- **Context already gathered:**
  - <fact - don't re-discover>
- **Constraints:**
  🚨 Read-only; do NOT make file changes
  Maximum <N> files
- **Required outputs:**
  Return ONLY this structure:

  | Column A | Column B |
  |----------|----------|

- **Stop rules:**
  - Stop when: <condition>
  - Return partial results if incomplete
```

### Validated Return Format Patterns

| Format Type | Preservation | Use When |
|-------------|--------------|----------|
| Simple table | 60% | Never (insufficient) |
| Rich table | 90% | Default for structured data |
| Narrative | 85% | Summary/analysis tasks |

### Key Columns That Preserve Nuance

| Column Type | Impact | Why It Works |
|-------------|--------|--------------|
| Evidence Quote | +20% | Grounds claims, enables verification |
| Source File | +10% | Attribution enables follow-up |
| Confidence | +10% | Calibrates orchestrator trust |
| Last Contact | +5% | Temporal context for recency |

## Context Management

### Long-Horizon State Tracking

```xml
<state_management>
- Use JSON or structured formats for tracking status
- Use freeform text for progress notes
- Use git for checkpointing across multiple sessions
- Emphasize incremental progress over completing everything at once
</state_management>
```

## Avoiding Common Issues

### Overeagerness and Overengineering

```xml
<minimal_scope>
Avoid over-engineering. Only make changes directly requested or clearly necessary.
Keep solutions simple and focused.
</minimal_scope>
```

### Reducing Hallucinations

```xml
<grounded_answers>
Never speculate about code you have not opened. Make sure to investigate and read
relevant files before answering questions about the codebase.
</grounded_answers>
```

## Agentic Coding Guidance

### Test-Focused Solutions

```xml
<robust_solutions>
Write high-quality, general-purpose solutions. Do not hard-code values for specific
test cases. Implement actual logic that solves the problem generally.
</robust_solutions>
```

### Code Exploration

```xml
<code_exploration>
Thoroughly review the style, conventions, and abstractions of the codebase before
implementing new features. Be rigorous and persistent in searching code for key facts.
</code_exploration>
```
