# Total Cost of Ownership

## Executive Summary

License price rarely determines the true cost of a developer platform. The bigger drivers are infrastructure, maintenance, workflow fragmentation, retraining, migration effort, security tooling overlap, and the productivity tax of poor developer experience. This document provides a practical TCO framework for comparing GitHub with GitLab, Azure DevOps, and Bitbucket.

## The TCO Model

### Core Cost Buckets

| Cost Bucket | GitHub | GitLab | Azure DevOps | Bitbucket |
| --- | --- | --- | --- | --- |
| Platform licensing | Direct subscription cost | Direct subscription cost | Direct subscription cost | Direct subscription cost |
| Infrastructure | Lower for cloud-first model; higher only for self-hosted options | Can increase if self-managed | Lower for SaaS; rises with add-ons and agent footprint | Lower for cloud; can rise with Atlassian stack dependencies |
| Administration | Centralized platform admin | Can be higher in self-managed estates | Often split across ADO, Azure, and legacy tooling | Often coupled with Jira/Atlassian admin overhead |
| CI/CD compute | Actions minutes/runners | CI runners | Pipelines agents | Pipelines minutes/runners |
| Security add-ons | GHAS and ecosystem tools | Often higher tiers for full security | Often requires extra Microsoft tooling | Often external tools required |
| Integration/maintenance | Broad marketplace reduces custom work | Moderate | Often strong in Microsoft-centric environments | Strong in Atlassian-centric environments |
| Training/change | Lower due to familiarity | Moderate | Higher when teams prefer modern GitHub workflows | Moderate |
| Opportunity cost | Strong due to talent familiarity and AI adoption | Moderate | Can be high if platform feels legacy to developers | Moderate to high at enterprise scale |

## The Hidden Costs Leaders Miss

### 1. Infrastructure and Reliability Overhead

Self-managed or hybrid deployments introduce ongoing costs:

- compute and storage
- high availability architecture
- backups and disaster recovery
- patching and upgrades
- security hardening
- incident response and support coverage

These are often booked outside the platform budget, masking the real cost of ownership.

### 2. Platform Engineering Time

A platform that needs constant customization can consume expensive engineering hours in:

- runner/agent lifecycle management
- pipeline troubleshooting
- permissions administration
- integration scripting
- migration of templates between business units

### 3. Training and Talent Friction

The less intuitive or less familiar the platform, the more organizations pay in:

- slower onboarding
- reduced mobility between teams
- more internal documentation
- lower adoption of advanced capabilities

### 4. Tool Sprawl

When the core platform is incomplete, enterprises buy adjacent products for:

- code scanning
- secret scanning
- dependency security
- artifact management
- workflow orchestration
- AI coding assistance

Every added product increases vendor management, integration burden, and policy complexity.

## Comparison Framework

### Weighted TCO Evaluation Matrix

Use a weighted scoring model to complement raw spend.

| Dimension | Weight | What to Measure |
| --- | --- | --- |
| Licensing | 20% | Subscription and add-on costs |
| Administration | 15% | Internal FTE time to operate the platform |
| Infrastructure | 10% | Hosting, storage, backup, networking |
| CI/CD compute | 10% | Build/test/deploy minutes, agents, runner support |
| Security tooling overlap | 15% | External spend that can be avoided or reduced |
| Developer enablement | 10% | Training time and onboarding speed |
| Integration complexity | 10% | Custom glue code and support burden |
| Opportunity cost | 10% | Delayed modernization, weaker AI leverage, slower delivery |

## Pricing Inputs to Check

> Pricing changes frequently. Validate current rates before final procurement.

| Platform | Public pricing pointer | Notable public signals |
| --- | --- | --- |
| GitHub | https://github.com/pricing | Team and Enterprise plans; Actions/GHAS may be separate considerations depending on package |
| GitLab | https://about.gitlab.com/pricing/ | Premium and Ultimate tiers; Ultimate often needed for broad security/compliance capabilities |
| Azure DevOps | https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/ | Basic plan commonly listed at about **$6/user/month**; Basic + Test Plans materially higher |
| Bitbucket | https://www.atlassian.com/software/bitbucket/pricing | Cloud tiers typically scale from free to Standard/Premium; enterprise value often depends on broader Atlassian stack |

## Sample TCO Calculation Method

### Assumptions

These sample models are **illustrative**, not vendor quotes.

- Fully loaded developer cost: **$180,000/year**
- Platform admin/SRE cost: **$200,000/year**
- Work year: **220 days**
- Consolidation opportunity: retire 1-3 adjacent tools depending on current state
- Security incident avoidance modeled separately from pure TCO

### Example Cost Components

| Component | Formula |
| --- | --- |
| License cost | users × monthly price × 12 |
| CI/CD cost | annual minutes/runners/agents spend |
| Admin cost | platform FTEs × loaded cost |
| Integration cost | annualized engineering/support effort |
| Training cost | users × training hours × hourly loaded rate |
| Retired tools savings | eliminated licenses + eliminated admin burden |

## Illustrative TCO Scenarios

### Scenario A: 100 Developers

| Cost Category | GitHub | GitLab | Azure DevOps | Bitbucket |
| --- | ---: | ---: | ---: | ---: |
| Platform licenses | $28,800 | $28,800-$57,600 | $7,200-$14,400 | $4,380-$8,700 |
| CI/CD compute and runners | $18,000 | $24,000 | $22,000 | $20,000 |
| Platform admin | $100,000 | $140,000 | $120,000 | $120,000 |
| Integration and support | $35,000 | $55,000 | $50,000 | $50,000 |
| Training/change | $16,000 | $24,000 | $28,000 | $22,000 |
| Less consolidation savings | **($45,000)** | **($25,000)** | **($20,000)** | **($22,000)** |
| **Illustrative annual TCO** | **$152,800** | **$246,800-$275,600** | **$207,200-$214,400** | **$194,380-$198,700** |

### Scenario B: 500 Developers

| Cost Category | GitHub | GitLab | Azure DevOps | Bitbucket |
| --- | ---: | ---: | ---: | ---: |
| Platform licenses | $144,000 | $144,000-$288,000 | $36,000-$72,000 | $21,900-$43,500 |
| CI/CD compute and runners | $95,000 | $120,000 | $115,000 | $105,000 |
| Platform admin | $300,000 | $420,000 | $360,000 | $360,000 |
| Integration and support | $120,000 | $180,000 | $160,000 | $155,000 |
| Training/change | $48,000 | $72,000 | $84,000 | $68,000 |
| Less consolidation savings | **($180,000)** | **($100,000)** | **($85,000)** | **($90,000)** |
| **Illustrative annual TCO** | **$527,000** | **$836,000-$980,000** | **$670,000-$706,000** | **$619,900-$641,500** |

### Scenario C: 1,000 Developers

| Cost Category | GitHub | GitLab | Azure DevOps | Bitbucket |
| --- | ---: | ---: | ---: | ---: |
| Platform licenses | $288,000 | $288,000-$576,000 | $72,000-$144,000 | $43,800-$87,000 |
| CI/CD compute and runners | $210,000 | $260,000 | $245,000 | $225,000 |
| Platform admin | $500,000 | $700,000 | $600,000 | $620,000 |
| Integration and support | $220,000 | $320,000 | $290,000 | $285,000 |
| Training/change | $90,000 | $130,000 | $150,000 | $125,000 |
| Less consolidation savings | **($380,000)** | **($210,000)** | **($175,000)** | **($185,000)** |
| **Illustrative annual TCO** | **$928,000** | **$1.49M-$1.78M** | **$1.18M-$1.25M** | **$1.11M-$1.16M** |

## How to Interpret the Samples

The tables are designed to highlight three executive realities:

1. **License deltas are only part of the picture.** Lower sticker price can still produce higher total cost.
2. **GitHub tends to outperform when consolidation is real.** The more tools you can retire, the stronger the case.
3. **Scale magnifies platform friction.** Admin complexity and integration drag become more expensive than seat cost.

## Licensing Optimization Levers

### GitHub-Specific Optimization Ideas

- standardize on a single enterprise agreement where possible
- use enterprise-managed users / centralized identity to simplify governance
- rationalize third-party CI/CD or security spend where GitHub capabilities are sufficient
- segment premium add-ons to the highest-value repositories or teams first
- use policy and reusable workflows to avoid duplicative platform engineering effort

## CFO-Friendly Talking Points

- We should evaluate **cost-to-ship software**, not just cost-to-buy licenses.
- Platform sprawl creates hidden labor costs that rarely appear on one invoice.
- Standardizing on a developer-preferred platform reduces change resistance and accelerates benefit realization.
- The best TCO outcome typically comes from consolidation plus productivity, not from the cheapest nominal seat price.

## Recommended Next Step

Run a 60-minute finance/platform workshop and replace the sample assumptions with:

- current user counts
- current vendor invoices
- admin FTE allocation
- annual CI/CD usage
- security tool overlap
- onboarding/training burden
- migration and integration backlog

## Reference Links

- GitHub Pricing: https://github.com/pricing
- GitLab Pricing: https://about.gitlab.com/pricing/
- Azure DevOps Pricing: https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/
- Bitbucket Pricing: https://www.atlassian.com/software/bitbucket/pricing
