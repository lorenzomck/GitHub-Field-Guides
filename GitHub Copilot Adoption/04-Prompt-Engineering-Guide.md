# 04. Prompt Engineering Guide

## Purpose

Teach developers to get better results from GitHub Copilot Chat, CLI, and agent workflows by providing the right context, structure, and constraints.

## Anatomy of an Effective Prompt

A strong prompt usually includes:

1. **Goal** - what you want to achieve
2. **Context** - repo, file, architecture, ticket, or environment details
3. **Constraints** - coding style, performance, security, dependencies, no-go areas
4. **Output format** - code, explanation, diff, checklist, test cases, commands
5. **Validation** - tests, edge cases, risks, or acceptance criteria

### Example

```text
Update this Express route to validate input with our existing zod schema pattern. Keep the response shape unchanged, add unit tests, and explain any edge cases you handled.
```

## Context-Setting Techniques

| Technique | Example |
| --- | --- |
| Reference local patterns | "Follow the same structure as src/services/accountService.ts" |
| Provide acceptance criteria | "Must preserve API contract and return 400 on validation errors" |
| Narrow the scope | "Only update this function, not the rest of the file" |
| Add environmental constraints | "Node 20, TypeScript, no new dependencies" |
| Include audience | "Explain this for a new team member" |

## Prompt Patterns for Copilot Chat

### Generate

```text
Create a starter implementation for [feature] using [language/framework]. Include error handling, logging, and tests.
```

### Explain

```text
Explain how this module works, identify dependencies, and list the top three risks or assumptions.
```

### Refactor

```text
Refactor this code for readability and maintainability without changing behavior. Preserve public interfaces and add tests if needed.
```

### Debug

```text
Analyze this failing test output. Suggest the likely root cause, the files to inspect, and the smallest fix.
```

## Slash Commands and Mentions

> Exact availability can vary by client and plan; confirm in your environment.

### Common Slash Command Uses

| Pattern | Intent |
| --- | --- |
| `/explain` | Get a structured explanation of selected code |
| `/fix` | Ask for a fix based on code or errors |
| `/tests` | Generate tests for selected code |
| `/doc` | Create or improve documentation/comments |

### Using `@workspace`

Use `@workspace` when you want Copilot Chat to reason over your repository context.

Example:

```text
@workspace find where authentication tokens are created and validated, then summarize the flow and suggest where to add audit logging.
```

### Using `@terminal`

Use `@terminal` when you want help interpreting commands, shell output, or debugging terminal issues.

Example:

```text
@terminal explain why this build command failed and suggest the next three checks to run.
```

## Code Generation Patterns

### Pattern: Scaffold + Review

```text
Generate the initial implementation for this handler using our existing conventions. Then list any assumptions and missing tests.
```

### Pattern: Tests First

```text
Write tests first for this function based on the described behavior. After that, propose the implementation.
```

### Pattern: Migration Support

```text
Convert this legacy utility to TypeScript. Keep runtime behavior identical and highlight any typing tradeoffs.
```

## Debugging with Copilot

### Recommended Flow

1. Paste the error or summarize the failure.
2. Ask Copilot for likely root causes.
3. Provide the relevant file or stack trace section.
4. Ask for the **smallest safe fix**.
5. Request tests or verification steps.

### Example Debug Prompt

```text
This API test started failing after a refactor. Here is the error and the updated service code. Identify the likely regression, propose a minimal patch, and suggest regression tests.
```

## Agent Mode Best Practices

| Practice | Why It Matters |
| --- | --- |
| Be explicit about scope | Prevents unrelated edits |
| Require verification | Ensures tests/builds are run |
| Mention constraints | Avoids dependency or architecture drift |
| Ask for a summary of changes | Makes review faster |
| Prefer small tasks | Easier to validate and rollback |

### Example Agent Prompt

```text
Implement support for filtering archived projects in the dashboard API. Update only the backend route, service, and tests. Do not change the frontend. Run the relevant test suite and summarize the result.
```

## Prompt Anti-Patterns

| Anti-Pattern | Better Alternative |
| --- | --- |
| "Fix this" | "Fix this null-pointer issue in the payment retry path and keep the public API unchanged" |
| "Write code for login" | "Create a login handler in FastAPI using our auth service abstraction and return the existing response format" |
| "Make this faster" | "Optimize this query path for large result sets; avoid schema changes and preserve output" |

## Reusable Prompt Template

```text
Task:
Context:
Constraints:
Relevant files/patterns:
Expected output:
Validation needed:
```

## Reference Links

- GitHub Copilot docs: https://docs.github.com/copilot
- Prompting guidance: https://resources.github.com/learn/pathways/copilot
- Responsible AI guidance: https://docs.github.com/copilot/responsible-use-of-github-copilot-features
