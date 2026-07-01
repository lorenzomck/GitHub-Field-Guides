# 02. Rollout Strategy

## Objective

Roll out GitHub Copilot in phases so adoption, governance, support, and measurable outcomes stay aligned.

## Phased Rollout Model

| Phase | Goal | Duration | Primary Outputs |
| --- | --- | --- | --- |
| Pilot | Validate value and identify blockers | 30 days | Baseline, training, initial metrics |
| Expand | Add adjacent teams and refine enablement | 30 days | Champion network, playbooks, support model |
| Scale | Operationalize broad adoption | 30-90 days | Standard policies, reporting, seat governance |

## Phase 1: Pilot

### Selecting Pilot Teams

Choose teams that are:

- Actively shipping code every sprint
- Open to experimentation and feedback
- Working across representative tech stacks
- Able to provide baseline metrics
- Supported by a manager who will reinforce adoption

### Good Pilot Candidate Profiles

| Team Type | Why They Work Well |
| --- | --- |
| Platform/tooling | High scripting, automation, docs, and infrastructure tasks |
| Product engineering | Frequent feature delivery and PR activity |
| QA/SDET | Strong fit for test generation and automation |
| Support engineering/SRE | Good fit for debugging, scripts, and incident documentation |

### Pilot Success Criteria

| Category | Example Target |
| --- | --- |
| Adoption | 80% of pilot users active weekly by end of month one |
| Usage quality | Meaningful acceptance rate and repeated usage in core workflows |
| Productivity | 10%+ reduction in time on selected tasks |
| Experience | Positive sentiment from 70%+ of participants |
| Governance | No unresolved critical security or policy issues |

## Phase 2: Expand

### Expand to Teams That Have

- Similar workflow patterns to successful pilot groups
- Identified local champions
- Basic training availability
- Manager support for behavior change

### What to Improve Before Expanding

- Update FAQ using pilot feedback
- Add team-specific prompting examples
- Refine office hours and support routing
- Standardize measurement dashboards
- Create seat reassignment rules for low usage

## Phase 3: Scale

At scale, treat Copilot as a product capability:

- Published ownership model
- Standard onboarding for new hires
- Regular reporting to leadership
- Policy review cadence
- Champion community and refresh training

## Communication Plan

### Audience Matrix

| Audience | Message | Channel | Cadence |
| --- | --- | --- | --- |
| Executives | ROI, risk management, milestone progress | Steering review, email summary | Monthly |
| Engineering managers | Adoption expectations, success criteria, seat use | Manager syncs | Biweekly |
| Developers | What Copilot helps with, how to access it, best practices | Slack, docs, workshops | Weekly during rollout |
| Security/legal | Controls, exclusions, privacy, escalation path | Review meetings | At launch + as needed |

### Sample Launch Message

```text
We are launching a GitHub Copilot pilot to help developers reduce repetitive work, accelerate learning in unfamiliar code, and improve overall flow. Pilot participants will receive training, office hours support, and clear guidance on acceptable use. We will measure outcomes over 30 days and use the results to guide broader rollout.
```

## Training Schedule

| Timeframe | Session | Duration |
| --- | --- | ---: |
| Week 0 | Manager and champion briefing | 45 min |
| Week 1 | Developer kickoff workshop | 90 min |
| Week 1 | IDE setup clinic | 30 min |
| Week 2 | Prompting and workflow lab | 60 min |
| Week 3 | Office hours / Q&A | 45 min |
| Week 4 | Pilot retrospective | 60 min |

## Seat Management Strategy

| Practice | Guidance |
| --- | --- |
| Start small | Assign only to pilot users and champions first |
| Review weekly | Check activation and reassign dormant seats |
| Reserve buffer | Keep a small reserve for high-priority teams |
| Document ownership | Make platform/admin team responsible for seat audits |
| Expand intentionally | Only add seats when training/support capacity exists |

## 30/60/90 Day Milestones

### Day 30

- Pilot launched and baseline established
- First usage and satisfaction data collected
- Initial prompt examples and FAQs published

### Day 60

- 1-3 additional teams onboarded
- Champions identified in each major engineering group
- Reporting cadence and seat governance operational

### Day 90

- Leadership review completed
- Scaled enablement plan approved
- Governance, exclusions, and acceptable use policy embedded
- Success stories documented for broader promotion

## RACI Template

| Workstream | Exec Sponsor | Platform/Admin | Enablement | Security/Legal | Team Leads |
| --- | --- | --- | --- | --- | --- |
| Funding approval | A | C | I | C | I |
| Technical setup | I | A/R | C | C | I |
| Training delivery | I | C | A/R | I | C |
| Policy review | I | C | I | A/R | I |
| Team adoption | I | C | C | I | A/R |
| Reporting | A | R | C | I | C |

## Reference Links

- GitHub Copilot docs: https://docs.github.com/copilot
- GitHub Copilot rollout resources: https://resources.github.com/copilot
- Change management guidance: https://resources.github.com/learn/pathways/copilot
