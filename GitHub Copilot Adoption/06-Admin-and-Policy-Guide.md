# 06. Admin and Policy Guide

## Objective

Provide administrators, security stakeholders, and engineering leaders with a practical guide for enabling GitHub Copilot responsibly across the organization.

## Enabling Copilot for Your Organization

### Core Setup Steps

1. Confirm the right Copilot plan and licensing model (Business at $19/user/mo or Enterprise at $39/user/mo).
2. Enable Copilot for the organization or enterprise scope.
3. Define who can request or receive seats.
4. Configure AI model availability policies (which models developers can use).
5. Configure any available privacy, policy, and exclusion settings.
6. Set up MCP (Model Context Protocol) integrations if applicable.
7. Review AI credit allocation (Business: 1,900/user/mo, Enterprise: 3,900/user/mo) and set spending limits.
8. Publish support and acceptable use guidance before broad rollout.

## Seat Assignment Strategies

| Strategy | Best For | Considerations |
| --- | --- | --- |
| Named pilot seats | Early rollout | Easier to measure and support |
| Role-based assignment | Platform, product, QA, SRE groups | Good for phased scaling |
| Manager-request workflow | Controlled expansion | Needs SLA and ownership |
| Champion-first model | Peer enablement | Useful when training capacity is limited |

## Content Exclusions

Use content exclusions for repositories or paths that should not be included in Copilot context according to your policy posture.

### Typical Exclusion Candidates

- Highly regulated or restricted codebases
- Repositories containing sensitive algorithms or data-handling logic
- Vendor code, mirrors, or generated assets
- Temporary migration or incident repositories

### Exclusion Review Checklist

| Question | Owner |
| --- | --- |
| Does this repo contain regulated or highly sensitive logic? | Security |
| Is it broadly used by developers who still need Copilot elsewhere? | Engineering leadership |
| Can path-level exclusions reduce risk without blocking productivity? | Platform/Admin |
| Has the exclusion decision been documented? | Program owner |

## IP Indemnity and Legal Considerations

Review the current commercial terms from GitHub with legal counsel. Capture the answers to:

- What indemnity applies to our plan?
- What responsibilities remain with our developers and reviewers?
- What review evidence should be retained for high-risk systems?
- How should third-party license obligations be handled in normal review?

> Policy principle: AI-assisted code should follow the same approval, testing, and attribution rules as any other code contribution.

## Privacy Settings and Data Handling

| Topic | Recommended Guidance |
| --- | --- |
| Prompt hygiene | Do not paste secrets, credentials, or unnecessary sensitive data into prompts |
| Access control | Align GitHub org membership, SSO, and seat assignment |
| Auditability | Maintain normal PR review and commit history |
| Sensitive systems | Apply exclusions and additional human review where needed |

## Acceptable Use Policy Template

### Required Behaviors

- Use Copilot to assist development, not to bypass engineering review.
- Verify generated code before committing.
- Follow secure coding, testing, and documentation standards.
- Avoid sharing secrets, tokens, customer data, or regulated information in prompts.

### Prohibited Behaviors

- Accepting generated code without understanding it
- Using Copilot outputs to bypass licensing, privacy, or security obligations
- Entering confidential information not approved for tool use
- Treating AI output as authoritative without validation

## Handling Developer Concerns

| Concern | Suggested Response |
| --- | --- |
| "Will this replace developers?" | Position Copilot as an augmentation tool focused on reducing low-value toil. |
| "I do not trust generated code." | Emphasize review, testing, and targeted usage patterns where value is highest. |
| "It does not help in my stack." | Gather team-specific examples and adjust rollout rather than forcing uniform expectations. |
| "I am worried about privacy." | Share the configured settings, exclusions, and acceptable use guidance transparently. |

## Governance Operating Model

| Area | Owner | Cadence |
| --- | --- | --- |
| Licensing and seat audit | Platform/Admin | Monthly |
| Security/policy review | Security + Legal | Quarterly |
| Enablement content refresh | Dev enablement/champions | Monthly |
| ROI and adoption review | Engineering leadership | Monthly/Quarterly |

## Admin Readiness Checklist

- Licensing model confirmed
- Sponsorship identified
- Pilot scope approved
- Seat assignment workflow documented
- Exclusions reviewed
- Privacy and acceptable use guidance published
- Support channel established
- Reporting cadence scheduled

## Reference Links

- GitHub Copilot docs: https://docs.github.com/copilot
- Managing policies and settings: https://docs.github.com/en/copilot/how-tos/administering-github-copilot
- Responsible use guidance: https://docs.github.com/copilot/responsible-use-of-github-copilot-features
- GitHub Trust Center: https://github.com/trust-center
