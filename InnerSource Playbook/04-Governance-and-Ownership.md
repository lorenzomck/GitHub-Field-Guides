# 04 — Governance and Ownership

InnerSource is not ownerless. In fact, the more open a repository becomes, the more important it is to define **who decides, who reviews, and how trust is earned**.

## Why governance matters

Without explicit governance, shared repositories can become:

- overloaded with unprioritized requests,
- blocked by invisible decision makers,
- vulnerable to quality drift,
- or abandoned despite high internal dependence.

Governance ensures openness scales without sacrificing reliability.

## Trusted committer model

A trusted committer model gives recurring contributors a path from consumer to steward.

### Typical progression

| Role | Characteristics | Typical permissions |
| --- | --- | --- |
| User | Consumes the project | Read, file issues |
| Contributor | Submits docs, fixes, enhancements | Branch or fork contribution |
| Trusted contributor | Repeatedly contributes high-quality changes | Broader review participation |
| Trusted committer | Has demonstrated technical judgment and collaboration | Merge rights in defined areas |
| Maintainer | Sets standards and direction | Full stewardship responsibility |

### Criteria for trusted committers

| Criterion | Evidence |
| --- | --- |
| Consistent contribution quality | PR history, low rework, clear tests |
| Good review behavior | Constructive comments, design awareness |
| Reliability | Responds to follow-up and operational issues |
| Shared context | Understands consumer needs across teams |
| Alignment with governance | Follows documented decision process |

## Maintainer responsibilities

Maintainers are enablers, not gatekeepers for gatekeeping’s sake.

| Responsibility | What good looks like |
| --- | --- |
| Roadmap stewardship | Priorities are visible and trade-offs are explained |
| Review quality | Feedback is timely, actionable, and consistent |
| Backward compatibility | Shared consumers are protected from avoidable breakage |
| Documentation upkeep | Setup, support, and contribution docs remain current |
| Community health | New contributors are welcomed and recurring ones are recognized |
| Operational safety | Security, reliability, and compliance concerns are managed explicitly |

## Review SLAs for shared repos

Shared assets need response-time commitments that reflect their importance.

### Example SLA matrix

| Item | First response SLA | Resolution target | Owner |
| --- | --- | --- | --- |
| Documentation PR | 1 business day | 2 business days | Repo maintainer |
| Bug fix PR | 2 business days | 5 business days | Repo maintainer + code owner |
| Urgent production fix | Same day during business hours | As incident requires | On-call or designated maintainer |
| Design proposal | 3 business days | Depends on scope | Maintainer lead |

### Tips for making SLAs real

- Publish them in the README and CONTRIBUTING docs.
- Monitor them in GitHub Insights or custom dashboards.
- Escalate systematically when breached.
- Staff maintenance explicitly; do not assume it happens off the side of someone’s desk.

## Escalation paths

Escalation should reduce ambiguity, not increase politics.

| Scenario | First escalation | Second escalation |
| --- | --- | --- |
| No response to a PR | Repo support channel or maintainer alias | Engineering manager of owner team |
| Repeated review stalemate | Technical design review forum | Director or architecture council |
| Ownership unclear | Platform governance owner | Engineering leadership |
| Cross-team priority conflict | Product/engineering triage meeting | Portfolio or platform steering group |

## Accepting or rejecting cross-team contributions

Maintainers should use documented criteria so decisions feel principled rather than territorial.

### Acceptance criteria

| Accept when... | Examples |
| --- | --- |
| It improves reuse for multiple consumers | Generalized configuration, better docs, safer defaults |
| It preserves compatibility | Opt-in feature flags, additive API changes |
| It aligns with repository purpose | Enhancement fits documented scope |
| Ongoing support is clear | Owners agree who maintains new surface area |

### Reject or redirect when...

| Reject signal | Better path |
| --- | --- |
| Team-specific customization in a shared core | Build extension points or adapters |
| Breaking change without migration path | Rework into additive change or stage rollout |
| Hidden operational cost | Request observability, support plan, or resourcing first |
| Misaligned architecture | Discuss in design issue before implementation |

## Measuring InnerSource health from a governance perspective

Governance quality is visible through both quantitative and qualitative signals.

| Metric | What it can reveal |
| --- | --- |
| Median time to first review | Whether maintainers are responsive |
| Review completion rate | Whether contribution paths actually work |
| Ratio of outside-team contributors | Whether openness extends beyond the owner team |
| Trusted committer growth | Whether the community is broadening |
| Reopened or reverted changes | Whether quality or review standards need improvement |
| Contributor satisfaction | Whether the process feels welcoming and fair |

## Governance operating model example

| Layer | Decision owner | Example decisions |
| --- | --- | --- |
| Repository | Maintainers | Merge policy, labels, roadmap priorities |
| Domain | Platform or architecture group | Shared standards, interoperability expectations |
| Organization | Engineering leadership | Funding, staffing, strategic program goals |

## Recognition and incentives

InnerSource programs scale faster when maintainers and contributors are recognized.

| Behavior to reward | Example recognition |
| --- | --- |
| Reviewing external contributions quickly | Team goals, public shout-outs, promotion evidence |
| Generalizing shared capabilities | Architecture awards, platform OKRs |
| Mentoring new contributors | Staff expectations, coaching recognition |
| Improving documentation/discoverability | Developer productivity metrics, release notes |

## Governance checklist

| Question | Yes/No | Notes |
| --- | --- | --- |
| Are maintainers and code owners clearly listed? |  |  |
| Are review SLAs published? |  |  |
| Is there a trusted committer path? |  |  |
| Are escalation routes documented? |  |  |
| Are acceptance criteria explicit for major changes? |  |  |
| Are governance metrics reviewed regularly? |  |  |

## References

- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository)
- [GitHub Docs: About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [InnerSource Commons Learning Path](https://innersourcecommons.org/learn/)
