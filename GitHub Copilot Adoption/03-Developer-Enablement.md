# 03. Developer Enablement

## Goal

Help developers move from basic access to confident, effective day-to-day usage of GitHub Copilot across editor, chat, CLI, and agent-assisted workflows.

## Getting Started Workshop Outline

### 90-Minute Workshop Agenda

| Time | Topic | Outcome |
| --- | --- | --- |
| 0-10 min | What Copilot is and is not | Sets expectations |
| 10-25 min | IDE installation and sign-in | Everyone is configured |
| 25-45 min | Feature walkthrough | Users see practical workflows |
| 45-70 min | Hands-on exercises | Users practice prompting and review |
| 70-80 min | Team-specific examples | Connects to real work |
| 80-90 min | Q&A and next steps | Reinforces adoption |

## IDE Setup Guides

### VS Code

1. Install the **GitHub Copilot** and **GitHub Copilot Chat** extensions.
2. Sign in with the approved GitHub account.
3. Verify Copilot status from the status bar.
4. Open a known project and test an inline completion.
5. Open chat and ask for help understanding a file or generating tests.

### JetBrains IDEs

1. Install the **GitHub Copilot** plugin from the JetBrains Marketplace.
2. Authenticate with GitHub.
3. Enable inline suggestions and chat features as permitted.
4. Test on a non-sensitive repository first.
5. Review keyboard shortcuts for accept/next/previous suggestions.

### Neovim

1. Install the approved GitHub Copilot plugin for Neovim.
2. Complete authentication flow.
3. Confirm keybindings for accepting suggestions.
4. Pair editor usage with terminal/CLI workflows for chat and repo assistance.

## Key Features Walkthrough

| Feature | What It Helps With | Example |
| --- | --- | --- |
| Inline completions | Boilerplate, repetitive code, patterns | Generate a serializer or DTO mapping |
| Copilot Chat | Explain, refactor, test, document | Ask for unit tests for a service class |
| Agent mode | Multi-step repo changes with validation | Implement scoped feature work with tests |
| Code review | AI-assisted pull request feedback | Get suggested improvements on a PR |
| Copilot CLI | Terminal command suggestions and debugging | Investigate failing tests or create docs |
| Cloud agent | Background autonomous coding tasks | Assign an issue for Copilot to implement |
| Custom instructions | Tailor Copilot behavior to your codebase | Define coding standards and patterns |
| MCP integrations | Connect external tools via Model Context Protocol | Integrate databases, APIs, or internal tools |

## Hands-On Exercises

### Exercise 1: Generate a Unit Test

Prompt:

```text
Write unit tests for this function covering happy path, validation errors, and edge cases. Use our existing test style and keep mocks minimal.
```

### Exercise 2: Explain a Legacy Module

Prompt:

```text
Explain this file in plain language. Identify inputs, outputs, major dependencies, and any risky assumptions.
```

### Exercise 3: Refactor for Readability

Prompt:

```text
Refactor this method into smaller functions without changing behavior. Preserve existing naming conventions and add tests if coverage is missing.
```

### Exercise 4: Create a Script

Prompt:

```text
Create a shell script that scans application logs for timeout errors, groups them by service, and outputs the top 10 affected endpoints.
```

## Effective Usage Habits

| Practice | Why It Works |
| --- | --- |
| Give context first | Better suggestions come from repo, file, and goal context |
| Ask for one step at a time when needed | Reduces hallucination and makes review easier |
| Verify all generated code | Maintains quality and security |
| Request tests and edge cases explicitly | Improves completeness |
| Use Copilot for drafts, not blind acceptance | Keeps engineering judgment in the loop |

## Tips for Maximum Effectiveness

### Prompting Tips

- Mention the language, framework, and constraints.
- Reference existing files or patterns in the codebase.
- Ask for tests, error handling, logging, and performance considerations.
- State what must **not** change.

### Review Tips

- Check business logic carefully.
- Validate imports, API usage, and generated dependencies.
- Run local tests before committing.
- Watch for subtle security issues in generated auth, SQL, or shell code.

### Team Enablement Tips

- Create a prompt cookbook for common tasks.
- Run office hours for the first month.
- Share before/after examples from real work.
- Celebrate practical wins, not hype.

## Champion Model

| Champion Responsibility | Description |
| --- | --- |
| Office hours | Help unblock day-to-day questions |
| Examples | Share prompts and workflows that worked |
| Feedback loop | Surface friction to platform/admin owners |
| Metrics advocacy | Encourage usage while reinforcing quality |

## New Hire Onboarding Checklist

- Seat assigned and access confirmed
- IDE plugin installed
- Chat and editor features tested
- Prompting guide shared
- First task completed with Copilot support
- Team policy and review expectations understood

## Reference Links

- GitHub Copilot docs: https://docs.github.com/copilot
- VS Code extension marketplace: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot
- JetBrains plugin marketplace: https://plugins.jetbrains.com/plugin/17718-github-copilot
- Neovim plugin docs: https://docs.github.com/copilot/configuring-github-copilot/configuring-github-copilot-in-your-environment
