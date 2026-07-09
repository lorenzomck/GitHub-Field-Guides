# GitHub Customer Newsletter - July 2026

## Introduction

This is a personally curated newsletter for my customers, focused on the most relevant updates and resources from GitHub this month. Highlights for this month include **Copilot's evolution into a full agent platform with sandboxed execution**, **usage-based billing going live for all Copilot users**, and **critical Actions security hardening including the pwn request attack block**. If you have any feedback or want to dive deeper into any topic, please let me know. Feel free to share this newsletter with others on your team as well. You can find an archive of past newsletters [here](https://github.com/lorenzomck/Customer-Newsletter).

---

# Copilot Evolves into an Agent Platform

**June marks Copilot's transformation from coding assistant to developer workflow platform.** With sandboxed execution, usage-based billing, and a wave of security hardening for Actions, this period delivers major operational changes for engineering teams. If you only read one section, focus on the billing changes and the Actions security updates, both require near-term action.

---

# Copilot

## Latest Releases

- **Copilot Agent Platform with Sandboxes** (GA) - Copilot now runs tools in isolated local or cloud sandboxes, limiting file and host access for tests, dependency installs, and code changes. Security-conscious teams can enforce sandboxes for all repository-modifying tasks. [Changelog](https://github.blog/changelog/label/copilot/)

- **Rubber Duck Mode** (GA) - The new `/rubber-duck` command in the CLI gives you a second-opinion agent that reviews your plans, highlighting blind spots or design flaws before Copilot takes automated action. Think of it as a code review for your prompts. [Changelog](https://github.blog/changelog/label/copilot/)

- **Voice Input for CLI** (PREVIEW) - Dictate prompts using a privacy-focused, local-only speech-to-text engine. No audio leaves your machine. [Changelog](https://github.blog/changelog/label/copilot/)

- **Prompt Scheduling** (PREVIEW) - The CLI now supports `/every` and `/after` commands for recurring background tasks like maintenance checks, dependency audits, or scheduled code reviews. [Changelog](https://github.blog/changelog/label/copilot/)

- **Session-Level Cost Tracking** (GA) - VS Code now shows total cost per Copilot chat session, not just individual turns. The status dashboard displays your budget consumption percentage, helping teams manage AI Credits proactively. [Release Notes](https://code.visualstudio.com/updates/v1_126)

- **Multiple Chats in One Agent Session** (GA) - Run several conversations side-by-side in the Agents window, keeping code generation, review, and documentation workflows in parallel. [Release Notes](https://code.visualstudio.com/updates/v1_125)

- **Model Providers Marketplace** (GA) - Discover and install third-party or BYOK models directly from the VS Code Marketplace. A combined picker adjusts both context size and thinking effort for model tuning. [Release Notes](https://code.visualstudio.com/updates/v1_125)

- **Claude Sonnet 5 and Claude Opus 4.8** (GA) - Expanded model availability across all Copilot surfaces, giving teams more choices for quality, speed, and cost tradeoffs. [Changelog](https://github.blog/changelog/label/copilot/)

- **Code Review 20% Cheaper** (GA) - Built-in source exploration tools (grep, rg, glob) replace older plumbing. Organization-level review defaults and medium-depth review labeling added for better cost visibility at scale. [Changelog](https://github.blog/changelog/label/copilot/)

- **BYOK (Bring Your Own Key)** (PREVIEW) - Run Copilot agent sessions via your own AI model providers. Useful for teams with existing model contracts or data residency requirements. [Docs](https://docs.github.com/en/copilot)

## Copilot at Scale

- **Usage-Based Billing Now Live** (GA) - Every completion, chat, and review consumes AI Credits. User-level budget controls and context window upgrades are available. Premium models are restricted by plan level. **Action required**: Review your org's Copilot budget settings and communicate the change to developers. [Docs](https://docs.github.com/en/copilot)

- **Enterprise Managed Settings** (GA) - Deliver Copilot configuration through device management tools (MDM, GPO). IT admins get tighter control over features and permissions across the org. [Release Notes](https://code.visualstudio.com/updates/v1_125)

- **Copilot for Jira** (GA) - Integration now generally available for teams using Jira alongside GitHub. [Changelog](https://github.blog/changelog/label/copilot/)

- **Agent Finder and Auto Mode** (GA) - Users can discover and invoke specialized agents automatically in Copilot Chat, reducing the learning curve for new capabilities. [Changelog](https://github.blog/changelog/label/copilot/)

- **Granular Usage Metrics** (PREVIEW) - More detailed consumption reporting helps admins identify heavy usage patterns, optimize model selection, and track adoption across teams. [Docs](https://docs.github.com/en/copilot)

**Source links**: [Copilot Feature Matrix](https://docs.github.com/en/copilot) | [GitHub Copilot Changelog](https://github.blog/changelog/label/copilot/) | [VS Code Release Notes](https://code.visualstudio.com/updates) | [Copilot CLI Releases](https://github.com/github/copilot-cli/releases)

---

# Enterprise and Security Updates

- **actions/checkout Blocks Pwn Request Attacks** (GA) - Starting June 18, actions/checkout refuses to fetch fork PR code in `pull_request_target` and `workflow_run` by default. This eliminates a major class of secret exfiltration attacks. Override is possible but discouraged. **Action required**: Test workflows that rely on fork PRs before July 16 backport completes. [Announcement](https://thehackernews.com/2026/06/github-updates-actionscheckout-to-block.html)

- **Native Egress Firewall for Actions** (GA) - A new Layer 7 firewall blocks unauthorized outbound connections from workflows, preventing exfiltration or credential leaks without explicit approval. [Announcement](https://dev.to/x4nent/complete-guide-to-github-actions-2026-security-roadmap-dependency-locking-native-egress-5aap)

- **Dependency Locking for Workflows** (GA) - Pin workflow dependencies to deterministic commits (similar to go.mod). Prevents attackers from mutating tags to inject malicious code at runtime. [Announcement](https://dev.to/x4nent/complete-guide-to-github-actions-2026-security-roadmap-dependency-locking-native-egress-5aap)

- **Scoped Secrets** (GA) - GitHub Actions secrets can now be tightly scoped to specific workflows or jobs, minimizing blast radius from leaks. [Changelog](https://github.blog/changelog/label/actions/)

- **Runner Enforcement v2.329.0** - Self-hosted runners must be at least v2.329.0 by June 29, 2026. Brownout periods will disrupt CI/CD for non-compliant runners. **Action required**: Verify all self-hosted runners are current, especially containerized runners with disabled auto-update. [Announcement](https://github.blog/changelog/2026-06-12-github-actions-minimum-version-enforcement-timeline-for-self-hosted-runners/)

- **GHES Key Rotation Required** - Following a May 2026 security incident, all GitHub Enterprise Server customers must rotate GPG signing keys using the provided script. Failure blocks future upgrades. **Action required**: Complete rotation immediately if not already done. [Advisory](https://github.blog/security/investigating-unauthorized-access-to-githubs-internal-repositories/)

- **CVE-2026-45793: Token Disclosure in Logs** - High-severity vulnerability where GITHUB_TOKENs could leak in build logs. **Action required**: Review workflow log output and secret usage patterns. [Advisory](https://integsec.com/blog/cve-2026-45793-github-actions-token-disclosure-in-logs-what-it-means-for-your-business-and-how-to-respond)

---

# Resources and Best Practices

- **GitHub Actions Security Checklist** - A new hardening guide covering 25 controls: pin all action refs, use CODEOWNERS, OIDC, environment reviewers, restrict to verified sources, and monitor outbound connections. [Read guide](https://www.stingrai.io/blog/github-actions-security-checklist)

- **Copilot Fridays On-Demand** - Full back catalog of Copilot training sessions available for self-paced learning. [Watch recordings](https://resources.github.com/copilot-fridays-english-on-demand/)

---

# Webinars, Events, and Recordings

- [Copilot Tips and Training Video](https://www.youtube.com/playlist?list=PLCiDM8_DsPQ1WJ5Ss3e0Lsw8EaijUL_6D)
- [GitHub Enterprise, Actions, and GHAS videos](https://www.youtube.com/playlist?list=PLCiDM8_DsPQ3wk4atKpN-yOW1FtyxN48W)
- [How GitHub GitHubs videos](https://www.youtube.com/playlist?list=PLCiDM8_DsPQ1nWhqxi-UQF_O-gYWo5jpG)

## Virtual Events

| Date | Event | Categories |
|------|-------|------------|
| Jul 15 | [Security Assessments: How to Measure and Improve your Code Security](https://github.com/resources/events) | Enterprise, GHAS |
| Jul 16 | [Let's Learn GitHub Copilot App](https://github.com/resources/events) | Copilot |
| Jul 29 | [From Issue to Merge: A Live Demo of the GitHub Copilot App](https://github.com/resources/events) | Copilot |
| Jul 30 | [Measuring AI Adoption, Impact, and Value with GitHub Copilot](https://github.com/resources/events) | Copilot, Enterprise |
| Aug 12 | [Copilot in Action: Best Practices & Use Cases](https://github.com/resources/events) | Copilot |

## In-Person Events

| Date | Location | Event |
|------|----------|-------|
| Aug 1 | Chennai, India | [SheBuilds Hack](https://githubcampus.expert/events) |
| Aug 22 | Kharagpur, India | [BuildX'26 Hackathon](https://githubcampus.expert/events) |

## Behind the scenes

- Copilot Fridays on-demand back catalog: [Watch recordings](https://resources.github.com/copilot-fridays-english-on-demand/)
- GitHub Actions Security Checklist (25 controls): [Read guide](https://www.stingrai.io/blog/github-actions-security-checklist)

---

## Closing

If you have any questions or want to discuss these updates in detail, feel free to reach out. As always, I'm here to help you and your team stay informed and get the most value from GitHub. I welcome your feedback, and please let me know if you would like to add or remove anyone from this list.
