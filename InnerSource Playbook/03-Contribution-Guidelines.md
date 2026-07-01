# 03 — Contribution Guidelines

InnerSource thrives when contribution is **safe, respectful, and predictable**. Clear guidelines reduce friction for new contributors while protecting maintainers from chaotic review queues and unclear expectations.

## What good contribution guidelines should achieve

| Goal | Why it matters |
| --- | --- |
| Lower activation energy | Contributors should know how to start in minutes, not days |
| Protect quality | Shared repos need guardrails for tests, compatibility, and documentation |
| Respect maintainer capacity | Good process reduces rework and review churn |
| Support cross-team trust | Teams need a fair, consistent path to contribute |

## Recommended contribution guideline structure

| Section | Questions it answers |
| --- | --- |
| Welcome and scope | What contributions are encouraged? |
| Ways to contribute | Can I file issues, improve docs, propose design changes, or submit code? |
| Development setup | How do I run, test, and validate changes locally? |
| Change sizing | What is considered small, medium, or large? |
| Review expectations | Who reviews and how long does it take? |
| Merge policy | Who can approve and what checks must pass? |
| Communication norms | Where should design discussion happen? |

## Example contribution contract

```md
## Accepted contribution types
- Bug fixes
- Documentation improvements
- Backward-compatible feature additions
- Test coverage improvements

## Discuss first
Please open an issue before starting:
- breaking API changes
- new dependencies
- schema or data model changes
- architecture refactors
```

## Code review culture for InnerSource

Code review is where InnerSource culture becomes visible.

### Healthy review behaviors

| Practice | What it looks like |
| --- | --- |
| Assume positive intent | Review focuses on code and impact, not team boundaries |
| Explain the why | Feedback includes rationale, not just directives |
| Separate blockers from suggestions | Contributors know what must change vs what is optional |
| Review for maintainability | Consider docs, ownership, and operability—not just syntax |
| Close the loop quickly | Even a partial response builds trust |

### Review comment examples

| Weak comment | Stronger InnerSource-friendly alternative |
| --- | --- |
| "Wrong approach." | "This repo avoids request-time caching here because it breaks tenant isolation. Could we move the cache to the service layer instead?" |
| "Needs tests." | "Please add coverage for the null configuration path; this code is used by three teams with different defaults." |
| "Not in scope." | "Thanks for the suggestion. We’re declining this part because it expands the public API surface. We would accept the validation fix in this PR." |

## Pull request workflows for cross-team contributions

A predictable PR workflow helps external contributors navigate internal ownership structures.

### Recommended workflow

| Step | Maintainer expectation | Contributor expectation |
| --- | --- | --- |
| 1. Issue or proposal | Clarify scope and constraints | Describe use case and impact |
| 2. Branch and implementation | Keep docs/templates current | Build the smallest viable change |
| 3. PR submission | Review against published standards | Fill out PR template completely |
| 4. Review and iteration | Respond within SLA | Address feedback or ask for clarification |
| 5. Merge and follow-up | Communicate rollout notes | Help validate downstream behavior if needed |

### Example PR template fields

```md
## Summary
What problem does this solve?

## Consumer impact
Which teams, services, or workflows benefit?

## Validation
- [ ] Tests added or updated
- [ ] Docs updated
- [ ] Backward compatibility considered

## Rollout notes
Any migration, feature flag, or operational follow-up?
```

## CODEOWNERS in cross-team repositories

`CODEOWNERS` makes approval paths explicit. In InnerSource, it should clarify ownership without discouraging external contributions.

### Good CODEOWNERS practices

| Practice | Reason |
| --- | --- |
| Map critical paths to responsible teams | Ensures experts review sensitive changes |
| Keep ownership broad enough to avoid bottlenecks | Single-person ownership is fragile |
| Align with real maintenance responsibilities | Stale ownership erodes trust |
| Pair CODEOWNERS with published SLAs | Ownership alone does not guarantee responsiveness |

### Example CODEOWNERS file

```txt
# Global maintainers
* @acme-platform/devex

# Security-sensitive integration layer
/integrations/auth/ @acme-security/identity @acme-platform/devex

# Documentation can be approved by docs champions
/docs/ @acme-platform/docs
```

## Fork vs. branch model for InnerSource

Both models can work internally. The choice depends on access boundaries, audit requirements, and contributor experience.

| Model | Best when | Trade-offs |
| --- | --- | --- |
| Branch model | Contributors have write access to create branches | Fastest path, simpler CI, less duplication |
| Fork model | Teams need stronger isolation or broader read-only access | More explicit boundary, but extra synchronization overhead |

### Practical guidance

- Prefer the **branch model** for trusted internal contributors in most repos.
- Use the **fork model** when:
  - contractors or partner teams need isolation,
  - security policies limit branch creation in the source repo,
  - CI and secrets policies differ for externalized contributors.

## Review SLAs and contributor expectations

Publishing expectations prevents frustration.

| PR type | Suggested first response SLA | Suggested merge target |
| --- | --- | --- |
| Docs-only | 1 business day | 2 business days |
| Small bug fix | 2 business days | 5 business days |
| Feature enhancement | 3 business days | agreed during design review |
| Breaking change | 5 business days | milestone-driven |

## Handling cross-team disagreements

Not every contribution should be accepted, but every decision should be **legible**.

### Decision framework

| Question | Accept signal | Reject or redesign signal |
| --- | --- | --- |
| Does it serve multiple consumers? | Reusable, configurable behavior | Highly team-specific customization |
| Is compatibility preserved? | Backward-compatible default | Breaking change without migration plan |
| Is maintenance understood? | Owner agrees to support surface | No clear maintainer commitment |
| Is the design aligned? | Fits roadmap and architecture | Duplicates an existing planned pattern |

## Example maintainer response for a declined change

```md
Thanks for the contribution. We’re not merging this as proposed because it adds a workflow specific to one product team into a repo used by six others.

We do want to support your use case. The maintainers suggest either:
1. a pluggable extension point in the shared module, or
2. a separate adapter owned by your team.

If you’d like, we can help shape one of those alternatives in a follow-up issue.
```

## Contribution checklist

| Check | Contributor | Maintainer |
| --- | --- | --- |
| Scope discussed for larger changes | ✅ | ✅ |
| Tests and docs updated | ✅ | Review |
| Labels and owners assigned |  | ✅ |
| Response provided within SLA |  | ✅ |
| Rollout risk assessed | ✅ | ✅ |
| Decision rationale recorded |  | ✅ |

## References

- [GitHub Docs: About pull requests](https://docs.github.com/en/pull-requests)
- [GitHub Docs: About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [InnerSource Commons Patterns](https://patterns.innersourcecommons.org/)
