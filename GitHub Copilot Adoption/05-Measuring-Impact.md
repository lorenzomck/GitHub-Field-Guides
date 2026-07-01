# 05. Measuring Impact

## Objective

Measure GitHub Copilot using a combination of telemetry, workflow metrics, and developer sentiment so leaders can make confident scaling decisions.

## Key Metrics

| Metric | Definition | Why It Matters |
| --- | --- | --- |
| Acceptance rate | % of suggestions accepted by users | Signal of usefulness |
| Lines suggested | Volume of AI assistance offered | Indicates reach and usage intensity |
| Active users | Unique users engaging over time | Tracks adoption consistency |
| Time saved | Self-reported or observed reduction in task duration | Connects usage to productivity |
| Developer satisfaction | Survey-based sentiment | Predicts sustainability of adoption |
| PR lead time | Time from work start to merge | Delivery efficiency |
| Review turnaround | Time waiting for review completion | Collaboration efficiency |

## Using the Copilot Usage Dashboard

### Questions to Answer Monthly

- How many assigned users are active weekly?
- Which teams have strong adoption or low activation?
- Is suggestion acceptance improving with enablement?
- Are there dormant seats to reassign?
- Do heavy users report higher perceived value?

### Dashboard Review Template

| Area | Observation | Action |
| --- | --- | --- |
| Weekly active users | Example: 72% active | Coach low-usage teams |
| Acceptance rate | Example: 28% | Provide better prompt training |
| Seat utilization | Example: 14 inactive seats | Reclaim and reassign |
| Team outliers | Example: SRE high value, QA low value | Tailor workflows by team |

## Survey Templates

### Pulse Survey (1-5 Scale)

1. GitHub Copilot helps me complete routine coding tasks faster.
2. GitHub Copilot helps me understand unfamiliar code more quickly.
3. I trust Copilot suggestions when paired with normal review.
4. I know how to prompt Copilot effectively for my work.
5. I would recommend continued use of Copilot on my team.

### Open-Ended Questions

- What tasks is Copilot most helpful for?
- Where does Copilot underperform or create friction?
- What examples or training would improve your usage?
- What concerns remain about quality, privacy, or workflow fit?

## Calculating ROI

### Practical Approach

Use three evidence layers:

1. **Telemetry** - active users, acceptance, usage trends
2. **Workflow outcomes** - cycle time, throughput, incident/script/documentation efficiency
3. **Sentiment** - developer time-saved estimates and satisfaction

### ROI Calculation Worksheet

| Input | Example |
| --- | ---: |
| Licensed developers | 200 |
| Weekly active rate | 78% |
| Avg. hours saved/week/active dev | 1.6 |
| Loaded hourly cost | $90 |
| Annualized benefit | $1,168,128 |
| Annual program cost | $92,000 |
| Net benefit | $1,076,128 |

Formula:

```text
Active Developers = Licensed Developers x Weekly Active Rate
Annualized Benefit = Active Developers x Hours Saved per Week x 52 x Loaded Hourly Cost
Net Benefit = Annualized Benefit - Annual Program Cost
```

## Benchmarking Against Industry

Use external benchmarks carefully:

- Position industry data as directional, not deterministic.
- Compare your pilot against conservative ranges first.
- Segment by role and workflow because gains vary widely.
- Avoid over-claiming based on suggestion volume alone.

## Reporting to Leadership

### Monthly Leadership Summary

| Section | Content |
| --- | --- |
| Adoption | Seat count, active rate, trends |
| Productivity | Time saved estimates, task studies, cycle time shifts |
| Quality | Defect trends, review observations, testing usage |
| Sentiment | Survey summary and champion insights |
| Risks | Policy issues, low-usage pockets, support needs |
| Next actions | Reassign seats, train teams, expand or pause |

### Example Narrative

> In month two, weekly active usage increased from 61% to 77%, supported by prompt training and champion-led office hours. Developers reported the strongest value in test creation, documentation, and understanding unfamiliar code. Early task studies show 12-18% faster completion times on selected work items, with no negative quality signal observed so far.

## Measurement Cadence

| Cadence | Activity |
| --- | --- |
| Weekly | Check active users, seat utilization, support issues |
| Monthly | Review dashboard, sentiment, and productivity indicators |
| Quarterly | Reassess ROI model, policy posture, and expansion plan |

## Reference Links

- GitHub Copilot documentation: https://docs.github.com/copilot
- GitHub resources and customer stories: https://resources.github.com/copilot
- Developer productivity measurement guidance: https://queue.acm.org/detail.cfm?id=3454124
