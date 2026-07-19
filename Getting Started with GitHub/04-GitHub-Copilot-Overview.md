# GitHub Copilot Overview

## What is GitHub Copilot?

GitHub Copilot is an AI-powered developer tool that helps write code faster, understand codebases, and automate repetitive tasks. It integrates directly into popular IDEs (VS Code, JetBrains, Neovim, Xcode) and the GitHub platform.

---

## Core Capabilities

| Capability | Description |
|-----------|-------------|
| **Code Completions** | Real-time suggestions as you type – single lines to entire functions |
| **Chat** | Natural language conversations about code, debugging, and architecture |
| **Agent Mode** | Autonomous multi-step coding tasks (create files, run tests, fix errors) |
| **Code Review** | AI-assisted pull request reviews with suggested improvements |
| **CLI Assistance** | Terminal command suggestions and explanations |
| **Cloud Agent** | Background tasks that run in GitHub-hosted environments |
| **MCP Support** | Extensibility via Model Context Protocol for custom tool integrations |

---

## Plans Comparison

| Feature | Free | Pro | Pro+ | Max | Business | Enterprise |
|---------|------|-----|------|-----|----------|------------|
| **Price** | $0 | $10/user/mo | $39/user/mo | $100/user/mo | $19/user/mo | $39/user/mo |
| **Code completions** | 2,000/mo | Unlimited | Unlimited | Unlimited | Unlimited | Unlimited |
| **AI credits** | Limited | ~$15/mo | ~$70/mo | ~$200/mo | 1,900/user/mo pooled | 3,900/user/mo pooled |
| **Premium AI models** | Basic only | Broad selection | Advanced selection | Full selection | Broad selection | Advanced + Custom |
| **Cloud agent** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Code review** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **MCP support** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Centralized billing** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Admin/Policy controls** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| **SAML SSO** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Internal code context** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Custom models** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Enterprise support** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |

> **Note:** All paid Copilot plans use monthly **GitHub AI Credits** for chat, agent, and code review interactions. Code completions remain unlimited and do not consume credits.

---

## For Organizations: Business vs Enterprise

### Copilot Business ($19/user/mo)
- Centralized seat management and billing
- Organization-wide policy controls (enable/disable features)
- IP indemnity and content exclusions
- Audit log integration
- SSO-backed access
- Best for: Teams that want managed AI assistance with admin controls

### Copilot Enterprise ($39/user/mo)
- Everything in Business, plus:
- **Knowledge Bases** – Index internal repositories for context-aware responses
- **Custom Models** – Fine-tuned models for your codebase and patterns
- **Bing integration** – Web-grounded answers for documentation questions
- **Priority support** – Dedicated onboarding and enterprise SLAs
- Best for: Large orgs wanting AI deeply integrated with their internal codebase

---

## Admin Controls (Business & Enterprise)

Administrators can:
- Enable/disable Copilot for specific teams or users
- Set content exclusion policies (prevent Copilot from using specific repos)
- Configure which AI models are available
- View usage analytics and adoption metrics
- Manage seat assignments and spending

---

## Privacy and IP Protection

- **No training on your code** – Business and Enterprise data is never used to train models
- **Content exclusions** – Specify files/repos that Copilot should not reference
- **IP indemnity** – Microsoft provides intellectual property protection for Copilot output (Business/Enterprise)
- **Data handling** – Prompts and suggestions are not stored beyond the session

---

## Getting Started with Copilot

1. **Admin enables Copilot** in the enterprise/org settings
2. **Assign seats** to users or teams
3. **Users install the extension** in their IDE (VS Code, JetBrains, etc.)
4. **Start coding** – suggestions appear automatically

---

## References

- [GitHub Copilot Plans](https://github.com/features/copilot/plans)
- [Copilot for Business Documentation](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-enterprise)
- [Copilot Trust Center](https://resources.github.com/copilot-trust-center/)
- [Getting Started Guide](https://docs.github.com/en/copilot/getting-started)
