# 05 — Measuring Success

If InnerSource is treated as a cultural aspiration without measurement, it is hard to improve or defend. Effective metrics show whether repositories are becoming easier to discover, easier to contribute to, and more valuable across teams.

## Measurement principles

| Principle | Why it matters |
| --- | --- |
| Measure outcomes and activity | PR counts alone do not prove reuse or impact |
| Compare owner-team vs outside-team behavior | InnerSource exists to enable cross-team collaboration |
| Use trends, not snapshots | A single month may be misleading |
| Combine quantitative and qualitative signals | Speed without trust is not success |
| Publish results transparently | Shared visibility encourages accountability |

## Core KPIs for InnerSource

### Contribution KPIs

| KPI | Definition | Why it matters |
| --- | --- | --- |
| Cross-team PR count | Number of PRs opened by contributors outside the owner team | Direct signal of collaboration |
| Cross-team merge rate | Percentage of outside-team PRs that merge | Indicates whether contribution paths are viable |
| Time to first review | Median time from PR open to first maintainer response | Reflects responsiveness |
| Time to merge | Median time from PR open to merge | Shows process efficiency |
| Repeat contributors | Number of people contributing more than once | Measures community stickiness |

### Community KPIs

| KPI | Definition | Why it matters |
| --- | --- | --- |
| Contributor diversity | Distinct teams contributing to a repo or portfolio | Avoids concentration in one org slice |
| Maintainer diversity | Number of active reviewers/maintainers | Reduces bottlenecks and risk |
| Trusted committer growth | Contributors promoted into broader stewardship | Measures sustainable scaling |
| Documentation engagement | README views, doc PRs, or FAQ traffic | Signals discoverability and onboarding quality |

### Reuse and platform value KPIs

| KPI | Definition | Why it matters |
| --- | --- | --- |
| Consumer count | Teams or services depending on the asset | Tracks reach |
| Reuse vs rebuild ratio | Number of adopters compared with duplicate implementations retired or avoided | Connects to business value |
| Shared component adoption | Usage of templates, SDKs, workflows, or libraries | Quantifies standardization |
| Defect reduction in shared paths | Incident or bug trend after consolidation | Tests whether reuse improves quality |

## Example KPI dashboard

| Metric | Target | Current | Trend |
| --- | --- | --- | --- |
| Outside-team PRs / quarter | 25 | 18 | ↑ |
| Median first review time | < 2 business days | 1.6 days | → |
| Teams contributing | 8 | 6 | ↑ |
| Reusable repo adoption rate | 70% of target teams | 54% | ↑ |
| Good first issue completion rate | 60% | 48% | ↓ |

## GitHub Insights for InnerSource

GitHub provides useful built-in views for measuring collaboration.

| GitHub capability | What to monitor |
| --- | --- |
| Pull request insights | Opened, merged, review turnaround, author/reviewer distribution |
| Contributor graphs | Whether contribution is broadening beyond maintainers |
| Traffic insights | Which repos and docs are being discovered |
| Issue dashboards | Demand patterns, unresolved backlog, cross-team requests |
| Projects / labels | Cross-team work types, onboarding issues, governance bottlenecks |

## Using the GitHub API for custom dashboards

Built-in views often need enrichment with organizational metadata such as team ownership, business domain, or repo classification.

### Useful API dimensions

| Data source | Example use |
| --- | --- |
| Repositories API | Pull descriptions, topics, visibility, default branch |
| Pull Requests API | Measure author team vs owner team behavior |
| Issues API | Track requests, triage age, and labels |
| Teams / org metadata | Group activity by org unit |
| Code search / dependency data | Estimate reuse of libraries, actions, or templates |

### Example dashboard questions

1. Which shared repos have the most cross-team contributors?
2. Which repos have the highest outside-team PR rejection rate?
3. Where are review SLAs being missed?
4. Which teams consume shared assets but never contribute back?
5. Which good first issue programs convert new contributors into repeat contributors?

### Example pseudo-query design

| Metric | Inputs | Derived logic |
| --- | --- | --- |
| Cross-team PR rate | PR author, repo owner team | PR author team != repo owner team |
| Review SLA compliance | PR created time, first review time | Compare against repo SLA target |
| Contributor diversity | Unique teams per repo | Count distinct contributing teams over period |
| Reuse metric | Dependency graph or usage registry | Count repos/services importing shared asset |

## Case study patterns from successful InnerSource programs

While implementations vary, successful programs usually share a few measurable traits.

| Pattern | Observable metric |
| --- | --- |
| A small number of well-supported pilot repos first | Higher merge rates in pilot repos |
| Strong documentation investment | Faster time from first issue to first PR |
| Named maintainers with review capacity | Lower PR abandonment rate |
| Recognition for contributors | Higher repeat-contributor percentage |
| Governance plus flexibility | Lower conflict rate on cross-team changes |

### Illustrative mini case study

| Situation | Action | Result |
| --- | --- | --- |
| Three teams built separate auth middleware | One shared repo was opened with docs, templates, and SLA-backed maintainers | Duplicate work stopped; two teams contributed tests and extensibility features within one quarter |
| Platform team received too many feature tickets | Introduced contribution guidelines and labeled good first issues | 30% of accepted enhancements came from consuming teams |
| Shared CI templates were underused | Added topics, examples, and internal search enablement | Adoption increased across new services |

## Qualitative success signals

Quantitative metrics should be paired with human feedback.

| Signal | How to gather it |
| --- | --- |
| Contributor confidence | Short surveys after merged PRs |
| Maintainer load perception | Monthly maintainer retrospectives |
| Discovery quality | Ask engineers how long it took to find the right repo |
| Trust in governance | Review decision disputes and escalations |

## Common metric pitfalls

| Pitfall | Why it misleads | Better approach |
| --- | --- | --- |
| Counting repos marked as InnerSource | Labels do not prove participation | Measure contribution and reuse |
| Rewarding only merged PR volume | Encourages shallow or noisy changes | Pair with quality and impact indicators |
| Ignoring maintainer load | Increased contribution can hide burnout | Track queue depth and reviewer spread |
| Comparing unrelated repos directly | Repo size and mission vary widely | Benchmark by repo category |

## Operating cadence for measurement

| Cadence | Focus |
| --- | --- |
| Weekly | SLA breaches, stalled PRs, urgent maintainership issues |
| Monthly | Contributor trends, diversity, adoption signals |
| Quarterly | Business impact, duplicate reduction, portfolio health |
| Biannually | Maturity assessment and governance redesign |

## References

- [GitHub REST API docs](https://docs.github.com/en/rest)
- [GitHub GraphQL API docs](https://docs.github.com/en/graphql)
- [GitHub Insights documentation](https://docs.github.com/)
- [InnerSource Commons Patterns](https://patterns.innersourcecommons.org/)
