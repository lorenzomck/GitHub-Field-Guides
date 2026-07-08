# 02 - Configuring Agentic Workflows

## Objective

Provide a concrete, repeatable setup process for configuring repositories, environments, and triggers so that agentic tasks (Copilot Coding Agent and related automation) execute reliably, safely, and with minimal human babysitting.

## The Configuration Stack

| Layer | Purpose |
|---|---|
| Repository custom instructions | Tell the agent how your codebase is organized and how to work in it |
| Development environment configuration | Ensure the agent's execution environment can build, test, and lint the project |
| Task assignment mechanism | Define how work gets routed to the agent (issue assignment, direct prompt, triggered workflow) |
| Trigger and scope controls | Constrain what kinds of tasks can be automatically or manually delegated to the agent |

## Step 1: Write Effective Repository Custom Instructions

Custom instructions (commonly `.github/copilot-instructions.md`) are the single highest-leverage configuration step. They reduce ambiguity, cut down on wasted iterations, and directly reduce token consumption per task.

### What to Include

| Section | Content |
|---|---|
| Project overview | What the repository does, its architecture at a glance |
| Build and test commands | Exact commands to build, lint, and run tests |
| Coding conventions | Style guide references, naming conventions, patterns to follow or avoid |
| Directory structure guide | Where key logic, tests, and configuration live |
| Things to avoid | Deprecated patterns, files not to touch, sensitive areas requiring extra care |
| PR conventions | Expected PR description format, required labels, commit message style |

### Example Structure

```markdown
# Copilot Instructions

## Project Overview
This service handles [domain]. Built with [language/framework].

## Build & Test
- Install: `<command>`
- Build: `<command>`
- Test: `<command>`
- Lint: `<command>`

## Conventions
- Follow existing patterns in `src/<module>` for new endpoints.
- Use the shared `Result<T>` type for error handling instead of exceptions.

## Do Not Touch
- `legacy/` is frozen pending decommission; avoid modifying unless explicitly asked.

## Pull Request Expectations
- Include a summary of changes and testing performed.
- Link the originating issue.
```

### Instruction Quality Checklist

- [ ] Instructions are kept up to date as the repository evolves (stale instructions actively mislead the agent).
- [ ] Build/test commands are verified to actually work in a clean environment.
- [ ] Instructions avoid duplicating what's already obvious from reading the code (keep it high-signal).
- [ ] Sensitive or off-limits areas of the codebase are explicitly called out.

## Step 2: Configure the Development Environment

The agent needs a working build/test environment to validate its own changes before opening a PR.

| Consideration | Guidance |
|---|---|
| Dependency installation | Ensure lockfiles/manifests are current so dependency installation is deterministic |
| Environment setup scripts | Provide a setup script or well-documented steps the agent's environment can follow |
| Secrets and credentials | Never rely on the agent having access to production secrets; use scoped, non-production credentials for any required environment variables |
| Network access | Understand and configure what external network access (if any) the agent's environment is permitted, per your security policy |
| Test suite reliability | Flaky tests undermine the agent's ability to validate its own work — stabilize the suite before heavy agent use |

## Step 3: Define Task Assignment Patterns

### Assignment Methods

| Method | Description | Best for |
|---|---|---|
| Issue assignment | Assign a GitHub issue directly to the agent | Well-scoped, triaged backlog items |
| Direct prompt | Provide a task description directly through the agent interface | Ad hoc, one-off tasks |
| Triggered by label/automation | An issue reaching a certain state (e.g., labeled `ready-for-agent`) automatically routes to the agent | Scaling triage-to-agent handoff |

### Writing a Good Task Description

| Element | Why it matters |
|---|---|
| Clear problem statement | Reduces ambiguity in planning |
| Acceptance criteria | Gives the agent (and reviewer) a concrete definition of done |
| Relevant file/module pointers | Speeds up planning and reduces exploration overhead |
| Constraints | Explicitly state what must NOT change (e.g., public API signatures) |
| Testing expectations | Specify whether new tests are required and what they should cover |

### Task Description Template

```markdown
**Problem:** <what's broken or missing>
**Acceptance criteria:**
- [ ] <criterion 1>
- [ ] <criterion 2>
**Relevant areas:** <files/modules likely involved>
**Constraints:** <what must not change>
**Testing:** <expected test coverage>
```

## Step 4: Set Scope and Trigger Controls

- [ ] Start pilots with manual task assignment only — no automatic routing until the pattern is proven.
- [ ] Limit initial task types to bug fixes, test additions, and documentation updates.
- [ ] Require a named human owner for every assigned task, accountable for reviewing the resulting PR.
- [ ] Avoid assigning tasks that touch authentication, authorization, payment, or other high-risk domains during the pilot phase.

## Step 5: Validate the Setup with a Trial Run

Before broad rollout, run a small number of trial tasks and evaluate:

| Evaluation dimension | Question to answer |
|---|---|
| Planning quality | Did the agent correctly interpret the task and scope? |
| Build/test success | Could the agent build and test its own changes without environment failures? |
| PR quality | Was the resulting PR reviewable, well-described, and reasonably scoped? |
| Iteration efficiency | How many turns/how much token consumption did the task require? |
| Review burden | How much reviewer time was needed compared to a similar human-authored PR? |

## Common Configuration Mistakes

| Mistake | Consequence | Fix |
|---|---|---|
| No custom instructions | Agent wastes turns rediscovering conventions each task | Write and maintain `.github/copilot-instructions.md` |
| Flaky test suite | Agent can't reliably validate its own changes | Stabilize tests before scaling agent usage |
| Overly broad task scope | Agent produces sprawling, hard-to-review PRs | Break large tasks into smaller, well-defined units |
| No environment setup documentation | Agent's execution environment fails to build the project | Document and verify setup steps in a clean environment |
| Immediately automating task routing | Loses the human quality gate needed early in adoption | Start with manual assignment, automate only after trust is established |

## Next Steps

Once agent tasks are running reliably on core repository operations, extend capability through [03-MCP-Overview-and-Registry-Setup](./03-MCP-Overview-and-Registry-Setup.md) to connect agents with additional tools and context sources.

## Reference Links

- [Best practices for using Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks)
- [About custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/custom-instructions)
- [Customizing the development environment for Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/customizing-the-development-environment-for-copilot-coding-agent)
- [About GitHub Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/about-copilot-coding-agent)
